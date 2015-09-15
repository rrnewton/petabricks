# Syntax #

```
IDENT[size1,size2, ... , sizeN]
```

IDENT is the name of a matrix. It can only include a-b, A-Z, and 0-9, and the first letter cannot be 0-9. Size1 to size N represent variable sizes of matrix in dimension1 to dimensionN respectively. Size1 to sizeN parameters will be passed in from input file (see AsciiMatrixFileFormat) or implicitly from the caller to this transform or function.

# Examples #

```
A[w,h]
```

This matix's name is A. It has w columns and h rows.

```
B
```

B matrix represent 0-dimension matrix, equivalent to a variable.