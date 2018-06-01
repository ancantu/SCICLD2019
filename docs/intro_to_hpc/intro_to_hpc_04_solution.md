1) Assuming you put `blast/2.6.0` into your default modules using `module save`, how do you undo that change?

```
$ module list
Currently Loaded Modules:
  1) intel/17.0.4   3) git/2.9.0       5) python/2.7.13   7) TACC
  2) impi/17.0.3    4) autotools/1.1   6) xalt/1.7.7      8) blast/2.6.0

$ module unload blast
$ module list
Currently Loaded Modules:
  1) intel/17.0.4   2) impi/17.0.3   3) git/2.9.0   4) autotools/1.1   5) python/2.7.13   6) xalt/1.7.7   7) TACC

$ module save
```

*Log out and log back in to see if it worked.*

[Return](intro_to_hpc_04.md)
