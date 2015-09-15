This short example demonstrates the basic properties and structure of a PetaBricks program.  The program displays a simple message containing the program's argument.

# Code #

Contents of helloworld.pbcc:
```
transform HelloWorld
from IN
to OUT
{
    to (OUT out)
    from (IN in)
    {
        printf("Program called with argument: %g\n", in);
        out = 7;
        return;
    }
}
```

# Description #

The first three lines specify the transform defined in this file.  The first line uses the keyword `transform` to specify the transform's name.  The second line uses the keyword `from` to specify the transform's inputs: in this case, a single input with the name `IN`.  `IN` is a scalar value since there is no size specification attached to the name (e.g. `IN[n]` would indicate an array).  The third line uses the keyword `to` to specify the transform's outputs: in this case, a single scalar value `OUT`.

The transform body contains a single rule that computes OUT from IN.  In a more complex program, we could have multiple rules with complex dependencies to compute OUT from IN.  In that case, PetaBricks's auto-tuner would help us determine which set of rules to execute.  However, in our simple example here, a single rule will suffice.

Variable names in PetaBricks are case-sensitive.  In our example, we have used upper-case `IN` and `OUT` to specify the inputs and outputs of the transform.  The rule's header associates the lower-case variables `in` and `out` with the transform's variables for use within the rule body.  In our example, `IN` and `OUT` are <i>transform variables</i>, while `in` and `out` are <i>rule variables</i>.  While this may seem like an unnecessary step here, it is a useful construct when dealing with more complicated rules and transforms (please see other examples in the documentation).

The rule body is C code that displays the transform's input and sets the transform's output to 7 before returning.

# Compile and Run #

In the following, the environment variable $PBROOT contains the root directory of the PetaBricks source tree.  Set it to the location you have PetaBricks installed on your computer.

To compile:
```
sh> $PBROOT/pbrun helloworld.pbcc
```
or
```
sh> $PBROOT/src/pbc helloworld.pbcc
```

To run:
```
sh> ./helloworld INPUT OUTPUT
```
where INPUT is a PetaBricks data file containing the input data to the program (see AsciiMatrixFileFormat.)  Example data files can be found in the testdata/ directory.  The second argument OUTPUT is the destination file where the program writes its output.  To print the output to stdout (the console), use "-".

Example:

```
sh> ./helloworld $PBROOT/testdata/Three0D -
Program called with argument: 3
SIZE
   7
```

The first line of output is from the printf statement in our transform.  The following two lines are the return value of the transform, which are printed to the console because we passed "-" as the output argument.  The SIZE line contains nothing else since the output is a scalar, and the value of the output is 7.