# Running Regression Tests #

You can run the regression test suite by running:
```
make check
```
or (equivalently)
```
./scripts/smoketest.py
```

To run a subset of the tests you may append a list of search strings to the command line.  For example:
```
./scripts/smoketest.py multiply ConvolutionFFT
```
will only run benchmarks containing "multiply" or "ConvolutionFFT" in their name.

To disable the "check" phase of the regression tests, you my append "nocheck" to the command line:
```
./scripts/smoketest.py nocheck
```

To add new regression tests, see the file "scripts/smoketest.tests".

There are currently 66 regression tests, the tests are automatically run in parallel and it takes about 10 minutes to complete all tests on a modern system.

# Regression Test Output #

The regression test suite outputs many lines like:
```
multigrid/Poisson2DDirect        compile PASSED run PASSED check PASSED
```

The test consists of 3 phases: compile, run, and check.

## Compile ##

The compile phase tries compiling the benchmark.  The output will be either: "compile PASSED", "compile SKIPPED", or "compile FAILED".

SKIPPED means that the output binary on disk was already up to date (according to filesystem time stamps) and so the compile test was not run.

Failed means pbc reported an error compiling the test.  To reproduce the error run:
```
./src/pbc ./examples/BENCHMARK.pbcc 
```
where BENCHMARK is the full name of the test that failed.

## Run ##

The run phase runs the bechmark on a fixed input and checks that it produces a known correct output.  These fixed inputs can be found in the folder:
```
./testdata/
```
and the "correct" outputs can be found in
```
./testdata/.output/
```

This phase of testing relies on git to test if the file in ./testdata/.output/ changed. Correctness is tested with "git diff ./testdata/.outout/...".

If a test here fails, the command line used to run will be printed to the screen.

## Check ##

The check phases uses the autotuner to make sure that the output of the benchmark is internally consistent as the ChoiceConfigurationFile changes.

It runs the autotuner as normal, only as candidates are generated checks that they agree on the output.  It tries to find two configurations that produce a different answer.

To debug a failure of the check phase, run:
```
./scripts/sgatuner.py BENCHMARK --check --debug
```

This will perform the same consistency check process.  The --debug flag causes the autotuner to pause when a inconsistent configuration is found and print the details of the offending configuration to the screen.

Some regression tests do not run a check phase because they are variable accuracy or contain configurations that abort and produce no output.