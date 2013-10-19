EasyBuild is tested rigorously to try and ensure high-quality code.

## Unit testing

Unit tests should be added (if possible) for each added feature.
Adding more unit tests should be done by creating a new test module, adding a suite() method which returns a TestSuite object with all the testcases in it.

In `test/{framework/easyblocks/easyconfigs}/suite.py` you should add it to the list of modules then, so it will be included when somebody runs the suite.

For every push to the `master` and `develop` branches of the `easybuild-framework`, `easybuild-easyblocks` and `easybuild-easyconfigs` repositories, a set of unit tests is run, possibly triggering the unit tests in another repository (`easybuild-framework` triggers `easybuild-easyblocks`, `easybuild-easyblocks` triggers `easybuild-easyconfigs`).

### easybuild-framework

To run the `easybuild-framework` unit tests yourself, simply execute `python -m test.framework.suite` on an installed version of EasyBuild. This should give you output like below:

```bash
$ python -m test.framework.suite
Running tests...
----------------------------------------------------------------------
..............................................................
----------------------------------------------------------------------
Ran 62 tests in 609.384s

OK

Generating XML reports...
```

Note: you will need either the `json` or `simplejson` Python module.The `json` module is available since Python 2.6, for older Python versions your best bet is to install `simplejson` (see https://pypi.python.org/pypi/simplejson). For Python 2.4, you will need to install an old `simplejson` version, since the `simplejson` 3.x required at least Python 2.5 (see https://pypi.python.org/pypi/simplejson/2.1.0).

The unit tests for `easybuild-easyblocks` and `easybuild-easyconfigs` are run in a very similar way, see below.

### easybuild-easyblocks

Currently, the unit tests for `easybuild-easyblocks` consist of instantiating an easyblock object for each of the available easyblock modules. Functional testing of easyblocks is done as a part of a regression test (see below). Running this suite of unit tests requires that `easybuild-framework` is available in `$PYTHONPATH`.

```bash
$ python -m test.easyblocks.suite
Running tests...
----------------------------------------------------------------------
..................................................................................................................
----------------------------------------------------------------------
Ran 114 tests in 40.291s

OK

Generating XML reports...
```

### easybuild-easyconfigs

For the `easybuild-easyconfigs` repository, every easyconfig file is parsed and an easyblock for that easyconfig is instantiated as a sanity check. Next to this, a full dependency graph of the full set of available easyconfigs is constructed, to verify whether all dependencies can be resolved. For these unit tests, both `easybuild-framework` and `easybuild-easyblocks` need to be available in `$PYTHONPATH`.

```bash
$ python -m test.easyconfigs.suite
Running tests...
----------------------------------------------------------------------
......................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................
----------------------------------------------------------------------
Ran 950 tests in 30.966s

OK
```

### Note on older Python 2.x versions

With older Python 2.x versions, e.g. Python 2.4.x, you'll need to use a slightly different approach, since the `-m` option is not (fully) supported yet, e.g.:

```bash
python test/framework/suite.py
```


## Regression testing

A more thorough test that also requires significantly more resources is a regression test, which consists of building all easyconfig files distributed with EasyBuild, in a pristine install directory.

To run a full regression test: first append the easybuild directory to the PYTHONPATH. Also, set your MODULEPATH to something which doesn't have dependencies installed.

Output will be placed in the current directory in easybuild-test-TIMESTAMP. You can aggregate the results into a single xml file. by using the -a option.

Make sure to populate the easybuild_config.py with settings that make sense for the regression tester.

The final xml will contain JUnit-compatible xml. Per build there is either a failure (with reason).
the buildstats for each application and also a summary in a top comment. 

A regtest is submitted using the following command:

```bash
eb --regtest --robot -ld
```

**Note**: this assumes that the command is executed on a system that has can submit jobs to a PBS server (`--regtest` uses `--job` in the background).

## Test results @ Jenkins

The results for both the unit tests and regression test are available through the Jenkins continuous integration server at UGent, see http://jenkins1.ugent.be/view/EasyBuild.

#### Unit testing

Jenkins also takes care automatically running the unit tests on every push to either the `master` or `develop` branch of the `easybuild-framework`, `easybuild-easyblocks` and `easybuild-easyconfigs` repositories.

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
wget https://jenkins1.ugent.be/view/EasyBuild/job/easybuild-full-regtest_develop/build?token=TOKEN &> /dev/null
echo "Triggered Jenkins to pull in regtest results."

EOF
```

**Note**: the `TOKEN` needs to be replaced with the actual token that allows to trigger the Jenkins test `easybuild-full-regtest_develop`. Contact the EasyBuild team if you want to run a regression test and make Jenkins pull back the results.