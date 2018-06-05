1) Search the module system for any application(s) that you need for your research.

*For my research, I use a molecular dynamics tool called gromacs*:
```
$ module list
Currently Loaded Modules:
   1) intel/17.0.4   2) impi/17.0.3   3) git/2.9.0   4) autotools/1.1   5) python/2.7.13   6) xalt/1.7.7   7) TACC

$ module spider gromacs
----------------------------------------------------------------------------------------------------------------------
  gromacs:
----------------------------------------------------------------------------------------------------------------------
    Description:
      molecular dynamics simulation package

     Versions:
        gromacs/5.1.2
        gromacs/2016.3
        gromacs/2016.4
        gromacs/2018.1
```


2) Figure out if any dependencies are required to load the module.
```
$ module spider gromacs/5.1.2       # carefully read output
...
You will need to load all module(s) on any one of the lines below before the "gromacs/5.1.2" module is available to load.

  intel/17.0.4  impi/17.0.3
  intel/18.0.0  impi/18.0.
...
```

*It looks like I must have `intel/17.0.4`, `intel/18.0.0`, `impi/17.0.3` and `impi/18.0` loaded in order to load `gromacs/5.1.2`. I already have those loaded, so I should be ready to go.*


3) Load the module and determine what effect it has on your environment.
```
$ module show gromacs/5.1.2         # carefully read output
...
prepend_path("PATH","/opt/apps/intel17/impi17_0/gromacs/5.1.2/bin")
prepend_path("LD_LIBRARY_PATH","/opt/apps/intel17/impi17_0/gromacs/5.1.2/lib64")
setenv("TACC_GROMACS_DIR","/opt/apps/intel17/impi17_0/gromacs/5.1.2")
setenv("TACC_GROMACS_INC","/opt/apps/intel17/impi17_0/gromacs/5.1.2/include")
setenv("TACC_GROMACS_LIB","/opt/apps/intel17/impi17_0/gromacs/5.1.2/lib64")
setenv("TACC_GROMACS_BIN","/opt/apps/intel17/impi17_0/gromacs/5.1.2/bin")
setenv("TACC_GROMACS_DOC","/opt/apps/intel17/impi17_0/gromacs/5.1.2/share")
setenv("GMXLIB","/opt/apps/intel17/impi17_0/gromacs/5.1.2/share/gromacs/top")
append_path("MANPATH","/opt/apps/intel17/impi17_0/gromacs/5.1.2/share/man")
append_path("PKG_CONFIG_PATH","/opt/apps/intel17/impi17_0/gromacs/5.1.2/lib/pkgconfig")
...
```

*It looks like the major changes are to prepend the `PATH` and `LD_LIBRARY_PATH` environment variables, and to define several new variables with `_GROMACS_` in the name (plus a few other minor changes).*

```
$ echo $PATH
$ echo $TACC_GROMACS_DIR
$ module load gromacs/5.1.2
$ module list
$ echo $PATH
$ echo $TACC_GROMACS_DIR
```


4) Make sure the desired executables (and correct version) are in your PATH.
```
$ which gmx
# gmx --version       # do not run these applications on the login node, but checking the version is okay
```


[Return](intro_to_hpc_03.md)
