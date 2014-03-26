**UNDER DEVELOPMENT** (contact Kenneth Hoste (a.k.a. _boegel_) for more information)

This page intends to provide a step-by-step guide for writing easyblocks.

For inspiration, please look at existing easyblocks: https://github.com/hpcugent/easybuild-easyblocks/tree/master/easybuild/easyblocks

A fully worked out example for WRF is available at [Tutorial: building WRF after adding support for it]

## Structure of an easyblock


## Design considerations


### Software-specific vs generic easyblocks



## Implementing an easyblock



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



## Further topics


### Custom easyconfig parameters

In an easyblock, custom easyconfig parameters can be defined that can be used as build parameters
that are specific to the software supported by that easyblock. This is done via the `extra_options`
static method.

For each custom easyconfig parameter, the following specifications must be provided:

* **name**: the name of the easyconfig parameter as it will be used in easyconfig files
* **default value**: the default value for this easyconfig parameter, which will be shown
  in the help output (`eb -a -e <easyblock>`)
* **help message**: a short help message that clarifies the use of the parameter
* **parameter type**: the type of parameter, which corresponds to the group in which the parameter
  will be shown in the output of `eb -a`; commonly used types are `BUILD`, `CUSTOM`, and `MANDATORY`
  (check `ALL_CATEGORIES` in `easybuild/framework/easyconfig/default.py` for a full list)

A fictious example of an implementation of `extra_options()`, defining two custom easybuild parameters
(one mandatory named `musthave`, and one optional named `foo`):

```python
from easybuild.framework.easyblock import EasyBlock
from easybuild.framework.easyconfig import CUSTOM, MANDATORY

class SomeEasyBlock(EasyBlock):

    @staticmethod
    def extra_options():
        """Extra easyconfig parameters specific to SomeEasyBlock."""
        extra_vars = {
            'musthave': [None, "A must have build parameter", MANDATORY],
            'foo': ['bar', "Some optional build parameter", CUSTOM],
        }
        return EasyBlock.extra_options(extra_vars)

    ...
```

Exmaples:
* [MVAPICH2 easyblock](https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/m/mvapich2.py)
* [WRF easyblock](https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/w/wrf.py)

#### Inheriting custom easyconfig parameters from derived easyblocks

For (generic) easyblocks that may be used as base for other easyblocks, special care must be taken
to make sure that the custom easyconfig parameters of both easyblocks are retained.

In that case, the `extra_options` method should feature an optional named parameter (by convention `extra_vars`),
that deriving easyblocks can define in their implementation of `extra_options`.

For example, consider a fictious generic easyblock named `Base`, and another easyblock `Derived` which derives from `Base`:

```python
from easybuild.framework.easyblock import EasyBlock
from easybuild.framework.easyconfig import CUSTOM

class Base(EasyBlock):

    @staticmethod
    def extra_options(extra_vars=None):
        """Extra easyconfig parameters specific to Base."""
        # process custom easyconfig parameters defined by deriving easyblocks via EasyBlock.extra_options (if any)
        # EasyBlock.extra_options returns a list of tuples, a dict is more appropriate (will be fixed in EasyBuild v2.0)
        extra_vars = dict(EasyBlock.extra_options(extra_vars))
        # add more custom easyconfig parameters specific to Base
        extra_vars.update({
            'foo': ['bar', "Some optional build parameter", CUSTOM],
        })
        return EasyBlock.extra_options(extra_vars)

    ...
```

```python
from easybuild.easyblocks.generic.base import Base
from easybuild.framework.easyconfig import MANDATORY

class Derived(Base):

    @staticmethod
    def extra_options():
        """Extra easyconfig parameters specific to Derived."""
        extra_vars = {
            'foofoo': ['barbar', "Some optional build parameter", MANDATORY],
        }
        return Base.extra_options(extra_vars)

    ...
```

#### Note on return type of `extra_options`

The return type of `extra_options` should be a list of 2-element tuples (e.g. as returned
by the `items()` method called on dictionary values). In EasyBuild v2.x the return type will be
changed to a dictionary (`dict`), but this change can't be made in EasyBuild v1.x without breaking
backward compatibility. Passing a `dict` value to `EasyBlock.extra_options` does work already however,
and will result in the correct return type (see above).


### Custom tests


### Running commands requiring interaction



=======
## More advanced aspects

### Custom easyconfig parameters

### Custom tests

### Running commands requiring interaction

## Style considerations

