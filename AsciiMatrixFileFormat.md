# Introduction #

PetaBricks programs use the following ASCII format for representing matrix data in inputs and outputs read from disk.  We currently do not have a binary data representation on disk. (Though we plan to add one.)


# Format #

Each file consists of the following:

  * The string "SIZE ".

  * A space (" ") separate list of integers.

> Each integer represents the size of a dimension of the matrix.  The number of integers determines number of dimensions of the matrix.  The dimensions are in the order x, y, z, etc (cols, rows, depth, etc).

> Note it is valid to have zero dimensions, in which case the matrix is a single scalar value.

  * A newline character.

  * A whitespace separated list of scalar numbers.

> This is the actual data of the matrix.  Elements are ordered such that the lowest dimension is always incremented first.  (For example, a 2 by 2 matrix would be traversed in the order [0,0], [1,0], [0,1], [1,1].)  The type or whitespace must be ignored by parsers.  Our code, by convention,  produces a space when dimension 0 is incremented and a string of X newlines whenever dimension X is incremented.


# Example #

0-dimension input:
```
SIZE
0
```

1-dimension input:
```
SIZE 3
1 2 3
```

2-dimension input:
```
SIZE 16 16
21  12  39  68   2  71  18   0  82  49  72  27  78   6   8  39 
32  87  92  78   2   8  49  81  34  87  55  31   3   7  27  44 
42  81  74  95  37  98  27  77  15  45  49  79  99  32  80  14 
45  72   5  72  44  94   8   0  58  37  91  49  62   7  23  76 
67  65  48  95  51  88  96  23  73  65   1  24  42  38  93  68 
35  77  74  81  49  15   5  68  80   4  63  81  17  90  82  56 
71  89  50  47  70  13  33  30  30  76  35  55  10  99  76  80 
79  39  73  34  36  65  81   1   8   2  99  99  82  97  43  82 
46  53  57  21  58  87  10  80   7  39  79  91   9  24  38   4 
 9   1   7  66  93  90  88  67  67  81  67  78  43  61  60  28 
87  34  90  84  37   7  99  72  40  45   9  11  60  49  84  51 
99  47  62  73  80  98  73  62  41  51  42  61  51  45  58  91 
85  52   9  91  17  91  42   1  86  70  94  69  29   8  44  33 
81  98  53   3  53  55  71  21  36  39  12  79  87  23  85  43 
73  79  52  45  85  23  76  35  58  87  35  40   8  15  71  19 
59  13  34  50  52  72  77  89  64  87  47  14  43  47  64  30 
```

3-dimension input:
```
SIZE 3 2 4
1 1 1
1 1 1

2 2 2
2 2 2

3 3 3
3 3 3

4 4 4
4 4 4
```

# Performance #

This format is obviously very space inefficient and is only intended to be used for development, testing, and debugging.  For production environments we recommend interfacing with PetaBricks through the RuntimeMatrixRegion.