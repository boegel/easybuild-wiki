EasyBuild is tested rigorously to try and ensure high-quality code.

## Unit testing

For every push to the `master` and `develop` of `easybuild-framework`, a set of unit tests is run.

To run the unit tests yourself, simply execute `python -m easybuild.test.suite` on an installed version of EasyBuild. This should give you output like below:

```bash
$ python -m easybuild.test.suite
Running tests...
----------------------------------------------------------------------
.............................
----------------------------------------------------------------------
Ran 29 tests in 6.932s

OK

Generating XML reports...
Log available at /tmp/easybuild_tests.log , XML output of tests available in test-reports directory
```

## Regression testing

A more thorough test that also requires significantly more resources is a regression test, which consists of building all easyconfig files distributed with EasyBuild, in a pristine install directory.

A regtest is submitted using the following command:

```bash
eb --regtest --robot -ld
```

**Note**: this assumes that the command is executed on a system that has can submit jobs to a PBS server (`--regtest` uses `--job` in the background).

## Test results @ Jenkins

The results for both the unit tests and regression test are available through the Jenkins continuous integration server at UGent, see http://jenkins1.ugent.be/view/EasyBuild.

#### Unit testing

Jenkins also takes care automatically running the unit tests on every push to either the `master` or `develop` branch of the `easybuild-framework` repository.

#### Regression testing

The regression tests are triggered manually, and Jenkins is triggered to pull in the test results after the regression test has completed.

For this, the following script is used:

```bash
#!/bin/bash

if [ ! -z $PBS_OWORKDIR ]
then
    cd $PBS_OWORKDIR
fi

export PYTHONPATH=$VSC_SCRATCH/easybuild_easy_installed/lib/python2.4/site-packages
export PATH=$PATH:$VSC_SCRATCH/easybuild_easy_installed/bin

cd $EASYBUILDPREFIX
if [ -d software/WIEN2k ]
then
    chmod -R 755 software/WIEN2k
fi
rm -rf software modules ebfiles_repo

export MODULEPATH=$EASYBUILDPREFIX/modules/all

cd -

outfile="full_regtest_`date +%Y%m%d`_submission.txt"

eb --regtest --robot -ld 2>&1 | tee $outfile

results_dir=`grep "Submitted regression test as jobs, results in" $outfile | tail -1 | sed 's@.*/@@g'`

after_anys=`qstat | grep ^[0-9] | sed 's/^\([0-9]*\).*/\1/g' | tr '\n' ':' | sed 's/^/afterany:/g'`

echo "after_anys: $after_anys"

qsub -W depend=$after_anys << EOF
cd $PWD

# aggregate results
out=`eb --aggregate-regtest=$results_dir`
ec=$?
if [ $ec -ne 0 ]; then echo "Failed to aggregate regtest results!"; exit $ec; fi

fn=`echo $out | sed 's/.* //g'`
datestamp=`date +%Y%m%d`
outfn="~/easybuild-full-regtest_${datestamp}.xml"

# move to home dir with standard name
mv $fn $outfn
ec=$?
if [ $ec -ne 0 ]; then echo "Failed to move regtest result for Jenkins!"; exit $ec; fi

echo "Aggregate test results made available for Jenkins in $outfn"

# trigger Jenkins test to pull in aggregated regtest result
wget https://jenkins1.ugent.be/view/EasyBuild/job/easybuild-full-regtest_develop/build?token=3asybu1ld_t0k3n &> /dev/null
echo "Triggered Jenkins to pull in regtest results."

EOF
```

**Note**: the `TOKEN` needs to be replaced with the actual token that allows to trigger the Jenkins test `easybuild-full-regtest_develop`. Contact the EasyBuild team if you want to run a regression test and make Jenkins pull back the results.