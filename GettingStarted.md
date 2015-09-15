# Prerequisites #

PetaBricks depends on the following packages:
  * automake/autoconf
  * gcc/g++
  * flex/bison (important: requires bison 2.x, currently broken with 3.x)
  * maxima
  * python, python-ply, python-numpy, python-scipy
  * libboost
  * libssl
  * libfftw3, liblapack, libblas (only for some benchmarks)
  * doxygen, graphviz, git (only for debugging/development)

You must install these packages to use PetaBricks.

On debian-based linux machines, run the following command to install all the required packages.
```
sudo apt-get install autoconf automake bison build-essential doxygen \
  flex g++ git-core graphviz libblas-dev libboost-all-dev libboost-dev \
  libfftw3-dev liblapack-dev libssl-dev maxima maxima-share python-numpy \
  python-ply python-scientific python-scipy libsqlite3-dev
```

PetaBricks works on Mac, however the installation process is not yet documented.  We recommend installing the dependencies using [Homebrew](http://mxcl.github.com/homebrew/).  Please ContributeDocumentation.

PetaBricks does not support Windows.  It may be possible to get it working under cygwin.

For Mac and older distributions of Linux, see MaximaNotes.

# Obtaining PetaBricks #

The most recent version of PetaBricks can be obtained [here](http://github.com/petabricks/petabricks).

# Compiling the PetaBricks Compiler #

If you obtained petabricks from the git repository, you should first run
```
./autogen.sh
```
This will generate the configure script and other files. (This is equivalent to running autoreconf.)

The standard way to build PetaBricks is:
```
./configure
make
```

To enable debugging code and (extremely verbose) debug output you can instead run configure with:  (Note that this hurts performance and should not be used when benchmarking.)
```
./configure --enable-debug
```

# Compiling a PetaBricks Program #

The PetaBricks compiler, pbc, will be located in ./src/pbc.  We recommend calling it with the wrapper script pbrun.

To compile ./examples/simple/copy.pbcc, run
```
./pbrun ./examples/simple/copy.pbcc
```

This will create the binary ./examples/simple/copy

(Note that pbrun simple wrapper script around ./src/pbc that ensures that pbc is up to date.)

# Running a PetaBricks Program #

You can run your new program using this same wrapper script:
```
./pbrun ./examples/simple/copy.pbcc ./testdata/Rand2Da -
```

This will call ./examples/simple/copy with the input "./testdata/Rand2Da" and with the output "-" (stdout).  (See AsciiMatrixFileFormat.)

The wrapper script, pbrun, will ensure that pbc and the program are up to date, recompiling them if necessary.

# Autotuning #

If your program contains choices, by default you will be running the "default" configuration, which is the first set of legal choices the pbc found.  **You must run the autotuner by hand.**

For details on running the autotuner see OfflineAutotuner.

The autotuner will create the file PROGRAM.cfg, where PROGRAM is the full that to your binary.  When you run your program, this configuration will be read and used dynamically.  You can also embed a configuration in your output binary by using the --hardcode option of pbc.  For more details on this file format, see ChoiceConfigurationFile.

# Next Steps #

Now that you have PetaBricks running, you may want to:

  * Learn the language with ExamplePrograms and the LanguageSpecification.
  * Make compiler changes with CompilerInternals and RegressionTests.
  * Make autotuner changes with AutotunerInternals and PerformanceBenchmarks.
  * Read our AcademicPublications.