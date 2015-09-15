# Running #

To start running the performance benchmarks run:
```
./pbbenchmark
```

There are 8 benchmarks and it takes approximately 3 hours to run the benchmark suite on a modern machine.

# Understanding Scores #

The benchmark suite outputs "scores" instead of performance, similar to the V8 benchmark suite. Scores are speedup numbers normalized to a reference system.  **Higher is better**.  A score of 100, means the raw performance was identical PetaBricks version 2.0 on our reference system.  A score of 150 means the raw performance was 50% faster than this reference system.

The formula for calculating scores is:
```
100 * REFERENCE_PERF / ACTUAL_PERF
```
Where REFERENCE\_PERF is a constant value and ACTUAL\_PERF is the measured performance on the current system.

The scores for individual benchmarks are combined using a geometric mean to produce an overall score.

For performance numbers we also report the standard deviation of 5 runs after the "+0".  Note that we only run the autotuner once, this is the standard deviation of running the same configuration 5 times.

The reference system was an 8-core system consisting of two Xeon X5460 processors running Debian 5.0.  Note that if you divide two scores the reference performance cancels out giving you a speedup.

# Output Format #

The output of the benchmark suite has two section:

  * "Fixed (no autotuning) scores:" is the performance using a fixed configuration generated on our reference system.  This score is meant to measure the performance of the PetaBricks runtime with the configuration held constant.

  * "Tuned scores:" is the performance after running the autotuner and are the more important numbers.  There is higher variance in these numbers since the autotuner is non-deterministic.  We recommend running the benchmark suite multiple times and taking the geometric mean of the scores.

For each benchmark, the output will look something like:
```
binpacking           perf: 119.5 +-  7 training: 103.1 acc: 104.3%
```
This line contains the following components:
  * binpacking is the name of the benchmark
  * "perf:" is the score of the raw performance followed by the standard deviation of 5 runs.
  * "training:" is the score for the training time (the time it took the autotuner to find a solution), normalized the reference system.  This line only appears for tuned scores.
  * "acc:" is the accuracy achieved by the configuration.  This number must be larger than 100% or the results of the benchmark suite are invalid.  This line only appears for variable accuracy benchmarks.

After each section overall scores are reported.  These are the geometric mean of the individual scores.