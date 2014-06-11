Since package names can be arbitrary and may begin with numberals (7zip)
or contain characters that are not great as classnames  (c++, C#),
a function is needed to remap those names to more "safe" alternatives,
in a way that maintains the following properties:

* readability; the new name should be readable, as much as possible
* reversibility & mapping: it should reflect well the original name
* standardization: it should follow established practice, if any
* extendibility: the logic should be extendable for future needs
* nameclash-free: class names are prefixed with "EB\_", by default

The function `encode_class_name` returns the encoded version of a class name
and is in turned based on an `EB_` prefix and the `encode_string` function.

For the programmer, this means that if, for example, you are extending
something called C++, your class name likely is `EB_C_plus__plus_`.

Samewise:

package name|class name
------------|----------
7zip|`EB_7zip`
C#|`EB_C_hash_`
Charm++|`EB_Charm_plus__plus_`
R|`EB_R`
r|`EB_r`
map\\reduce|`EB_map_backslash_reduce`

The encoding function can handle even more complicated package names, like:
 * name: `0_foo+0x0x#-$__`
  * becomes: `0_underscore_foo_plus_0x0x_hash__minus__dollar__underscore__underscore_`
    
It has been inspired by the concepts seen at: (but in lowercase style)
* http://fossies.org/dox/netcdf-4.2.1.1/escapes_8c_source.html
* http://celldesigner.org/help/CDH_Species_01.html
* http://research.cs.berkeley.edu/project/sbp/darcsrepo-no-longer-updated/src/edu/berkeley/sbp/misc/ReflectiveWalker.java
and can be extended freely as per ISO/IEC 10646:2012 / Unicode 6.1 names:
* http://www.unicode.org/versions/Unicode6.1.0/ 
For readability of >2 words, it is suggested to use \_CamelCase\_ style.
So, yes, `_GreekSmallLetterEtaWithPsiliAndOxia_` *could* indeed be a fully
valid package name; package "electron" in the original spelling anyone? ;-)

### Determining the class name for a given package

```python
>>> from easybuild.tools.filetools import encode_class_name
>>> encode_class_name('GAMESS-US')
'EB_GAMESS_minus_US'
```