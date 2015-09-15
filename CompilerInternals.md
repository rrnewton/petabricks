The internals of the PetaBricks compiler are not yet fully documented (ContributeDocumentation).

The compiler source code is structured as follows:

  * ./src/compiler  -- the PetaBricks compiler (pbc)
    * pbc.cpp -- main entry point of the compiler
  * ./src/runtime   -- the runtime library (data/region handling and work stealing runtime)
  * ./src/common    -- shared files between compiler and runtime