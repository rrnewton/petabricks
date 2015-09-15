# Autotuner Usage #

The offline autotuner is `./scripts/sgatuner.py`. sgatuner takes two required arguments: the name of the program to autotune and the input size. For example:

```
 ./scripts/sgatuner.py ./examples/sort/Sort -n 1024
```

For variable accuracy benchmarks, you should add the option --`accuracy_target=...`

When complete, sgatuner will print a directory where it has saved its detailed output, e.g. "`~/tunerout/pbtunerun__CLV71Z`". The best configuration file found for each input size is located at `best/n[input size]/config` in this directory.   This directory also contains detailed logs and statistics.  You can run that config by passing the` --config=...` option to program binary.

The best configuration for the largest input size run will also be copied to `./examples/sort/Sort.cfg`.

# Options #

The offline autotuner also accepts the following options:

```
  *   -h, --help           show this help message and exit
  *  --check               check for correctness
  *  --debug               enable debugging options
  *  -n N                  input size to train for
  *  --max_time=MAX_TIME
  *  --rounds_per_input_size=ROUNDS_PER_INPUT_SIZE
  *  --mutations_per_mutator=MUTATIONS_PER_MUTATOR
  *  --output_dir=OUTPUT_DIR
  *  --population_high_size=POPULATION_HIGH_SIZE
  *  --population_low_size=POPULATION_LOW_SIZE
  *  --min_input_size=MIN_INPUT_SIZE
  *  --offset=OFFSET       
  *  --threads=THREADS     
  *  --name=NAME           
  *  --abort_on=ABORT_ON   
  *  --accuracy_target=ACCURACY_TARGET
```

Additionally, more detailed configuration changes can be made by editing ./scripts/tunerconfig.py