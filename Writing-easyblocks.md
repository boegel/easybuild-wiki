**UNDER DEVELOPMENT** (contact Kenneth Hoste (a.k.a. _boegel_) for more information)

This page intends to provide a step-by-step guide for writing easyblocks.

For inspiration, please look at existing easyblocks: https://github.com/hpcugent/easybuild-easyblocks/tree/master/easybuild/easyblocks

A fully worked out example for WRF is available at [Tutorial: building WRF after adding support for it]

## Structure of an easyblock

## Design considerations

### Software-specific vs generic easyblocks

## Implementing an easyblock

### Step 0: figure out the build/install procedure

* read the documentation
* dependencies?
* check `configure --help`

### Step 1: create the easyblock

* create the Python module and class
* make sure EasyBuild can find it (`eb --list-easyblocks`)

### Step 2: look into building on top of existing easyblocks

building on the shoulder of giants

### Step 3: implement the custom build/install procedure steps

most commonly: `configure_step`, `build_step`, `install_step`

### Step 4: implement custom sanity check

sanity check paths, sanity check commands

## More advanced aspects

### Custom easyconfig parameters

### Custom tests

### Running commands requiring interaction

## Style considerations

