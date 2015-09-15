The internals of the PetaBricks autotuner are not yet fully documented (ContributeDocumentation).

The autotuner source code is structured as follows:

  * ./scripts/sgatuner.py -- main entry point the current offline autotuner
  * ./scripts/candidatetester.py -- representation of candidate configurations and results
  * ./scripts/mutators.py -- mutation operators