# Example #

```
{%

  #include <math.h> 

  inline double logadd(double x, double y) {
    return log(x)+log(y);
  }

%}
```

# Details #

At the top level of a PetaBricks file one can declare a "{% ... %}" block containing raw C++ code.  This C++ code is included in the output of the PetaBricks compiler.  Note that this code is placed in common header file included by all the individual source files for each PetaBricks transform.

"#include"s that include C or C++ headers should be placed inside "{% #include <...> %}" blocks.