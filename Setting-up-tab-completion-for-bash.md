Since EasyBuild v1.10.0, tab completion for bash shells is supported.

To make this active, you need to make sure the required bash functions are available, and that auto-completion for the `eb` command is registered.
This can be done as follows after the installation of EasyBuild

```bash
source `dirname $(which eb)`/minimal_bash_completion.bash
source `dirname $(which eb)`/optcomplete.bash
complete -F _optcomplete eb
```

Once this is done, you should have tab completion in a bash shell working, for example:

```bash
$ eb --avail-<TAB>
--avail-easyconfig-constants   --avail-easyconfig-params      --avail-module-naming-schemes  --avail-repositories           
--avail-easyconfig-licenses    --avail-easyconfig-templates   --avail-modules-tools 
```