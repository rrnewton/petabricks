This is a program for adding 2-dimensional matrices.

# Code #

```
transform Add2D
from A[w,h], B[w,h]
to C[w,h]
{
  C.cell(i,j) from (A.cell(i,j) a, B.cell(i,j) b) {
    return a + b;
  }
}
```

# Description #

First, we specify the name for this transform.
```
transform Add2D
```

The inputs of this transform are matrices A and B, and the output is C. We also specify the size of each inputs/outputs: `A[w,h]` means a 2-dimensional matrix of size w x h.
```
from A[w,h], B[w,h]
to C[w,h]
```

Next, we define a rule for the transform. This rule tells PetaBricks to compute C(i,j) by adding A(i,j) and B(i,j). The output of this rule is cell (i,j) of matrix C (`C.cell(i,j)`). The inputs are cells from A and B, and we call them a and b, respectively.

The body of the rule can contain any C++ codes to compute the output. In this case, we just return a+b.

```
  C.cell(i,j) from (A.cell(i,j) a, B.cell(i,j) b) {
    return a + b;
  }
```