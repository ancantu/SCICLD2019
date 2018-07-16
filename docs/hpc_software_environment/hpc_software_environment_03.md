## Installing an Application

Now we will try to install our own application from the web. We are going to try to install a spliced read mapper for RNA-seq called [Tophat](https://ccb.jhu.edu/software/tophat/tutorial.shtml).

The first thing to do is decide a good place to install the application. The `$WORK` file system on TACC resources is suitable. Navigate to the `$WORK` file system, and make a new directory called `apps`, where we can put all of the applications we install:
```
$ cd $WORK
$ pwd
/work/03439/wallen/wrangler
$ mkdir apps
$ cd apps
$ pwd
/work/03439/wallen/wrangler/apps
```

According to the Tophat documentation, we need `bowtie2` and `boost` are dependencies. Check your environment and load what we know we need for Tophat:
```
$ module reset
$ module load boost/1.55.0
$ module load bowtie/2.3.4
$ module list
Currently Loaded Modules:
  1) TACC-paths   2) Linux   3) cluster-paths   4) intel/15.0.3   5) mvapich2/2.1   6) cluster   7) TACC   8) boost/1.55.0   9) bowtie/2.3.4
```


We will use the `wget` command to download the Tophat source code straight from the web, and `tar` command to unpack the file. We also make a directory called `tophat/2.1.1` which is where the installation will go:
```
$ wget https://ccb.jhu.edu/software/tophat/downloads/tophat-2.1.1.tar.gz
$ tar -xvzf tophat-2.1.1.tar.gz
$ mkdir -p tophat/2.1.1
$ cd tophat-2.1.1
```

According to the Tophat documentation, it follows a standard installation scheme of `./configure`, `make`, `make install`. The first step checks your environment for all the right dependencies, the second step compiles the code, and the third step moves the appropriate binaries and libraries to your target install location. Try running the first step with a `--help` flag to see what information you can learn:
```
$ ./configure --help
```

A few things to note from the help text:
 * We cannot install into the default `/usr/local` folder, so we will have to specify our own prefix
 * We may try using the `--enable-intel64` flag since we are compiling with intel processors
 * We may need to specify the location of boost with the `--with-boost` flag, even though it is already in our path
 * There is a helpful list of environment variables at the bottom

Try to configure using the `--prefix` and `--enable-intel64` flags:
```
$ ./configure --prefix=/work/03439/wallen/wrangler/apps/tophat/2.1.1 \
              --enable-intel64
...
checking for boostlib >= 1.38.0... configure: error: We could not detect the
boost libraries (version 1.38 or higher). If you have a staged boost library
(still not installed) please specify $BOOST_ROOT in your environment and do not
give a PATH to --with-boost option.  If you are sure you have boost installed,
then check your version number looking in <boost/version.hpp>. See
http://randspringer.de/boost for more documentation.
```

It works for a little bit before giving the boost error above. Perhaps we do need to specify the boost path after all:
```
$ ./configure --prefix=/work/03439/wallen/wrangler/apps/tophat/2.1.1 \
              --with-boost=/opt/apps/intel15/boost/1.55.0 \
              --enable-intel64
```

This time it got all the way through the `./configure` step, except there is something funny in the summary text:
```
...
-- tophat 2.1.1 Configuration Results --
  C++ compiler:        g++ -Wall -Wno-strict-aliasing -g -gdwarf-2 -Wuninitialized  -mtune=nocona -O3  -DNDEBUG -I./samtools-0.1.18 -pthread -I/opt/apps/intel16/boost/1.59/include -I./SeqAn-1.4.2
  Linker flags:        -L./samtools-0.1.18 -L/opt/apps/intel15/boost/1.55/lib
  BOOST libraries:     -lboost_thread -lboost_system
  GCC version:         gcc (GCC) 4.9.1
  Host System type:    x86_64-unknown-linux-gnu
  Install prefix:      /work/03439/wallen/lonestar/training/apps/tophat/2.1.1
  Install eprefix:     ${prefix}
...
```

It looks like it is still trying to use gcc compilers, even though we have intel compilers loaded. This is where the environment variables come in. As a reminder:
```
$ ./configure --help
Some influential environment variables:
  PYTHON      python program
  CXX         C++ compiler command
  CXXFLAGS    C++ compiler flags
  LDFLAGS     linker flags, e.g. -L<lib dir> if you have libraries in a
              nonstandard directory <lib dir>
  LIBS        libraries to pass to the linker, e.g. -l<library>
  CPPFLAGS    (Objective) C/C++ preprocessor flags, e.g. -I<include dir> if
              you have headers in a nonstandard directory <include dir>
  CC          C compiler command
  CFLAGS      C compiler flags
  CXXCPP      C++ preprocessor
```

The contents of the variables we are interested in are empty:
```
$ echo $CC
$ echo $CXX
```

We can force Tophat to use the intel compilers by pointing those variables to the appropriate C and C++ compilers:
```
$ which icc
/opt/apps/intel/15/composer_xe_2015.3.187/bin/intel64/icc
$ export CC=`which icc`
$ echo $CC
/opt/apps/intel/15/composer_xe_2015.3.187/bin/intel64/icc
$ export CXX=`which icpc`
```

Configure one more time and make sure the output makes sense:
```
$ ./configure --prefix=/work/03439/wallen/wrangler/apps/tophat/2.1.1 \
              --with-boost=/opt/apps/intel15/boost/1.55.0 \
              --enable-intel64
...
-- tophat 2.1.1 Configuration Results --
  C++ compiler:        /opt/apps/intel/15/composer_xe_2015.3.187/bin/intel64/icpc -Wall -Wno-strict-aliasing -g -gdwarf-2 -Wuninitialized  -mtune=nocona -O3  -DNDEBUG -I./samtools-0.1.18 -pthread -I/usr/include -I./SeqAn-1.4.2
  Linker flags:        -L./samtools-0.1.18 -L/usr/lib
  BOOST libraries:     -lboost_thread 
  GCC version:         icc (ICC) 15.0.3 20150407
  Host System type:    x86_64-unknown-linux-gnu
  Install prefix:      /work/03439/wallen/wrangler/apps/tophat/2.1.1
  Install eprefix:     ${prefix}
...
```

This looks better. Finish with:
```
$ make                # compiles code
$ make install        # moves binaries into $WORK/apps/tophat/2.1.1
$ ls -l ../tophat/2.1.1/
$ ls -l ../tophat/2.1.1/bin
```

The `tophat/2.1.1/bin` directory should be filled with executables.

Previous: [Wrangler Basics](hpc_software_environment_02.md) | Next: [Testing an Application](hpc_software_environment_04.md) | Top: [Course Overview](../../index.md)

