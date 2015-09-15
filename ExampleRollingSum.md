# Code #

```
transform RollingSum
from A[n]
to B[n]
{
  // Rule 0: sum all elements to the left
  to(B.cell(i) b) from(A.region(0, i+1) in){
    b=0;
    for(int j=0; j<=i; ++j)
      b += in.cell(j);
  }

  // Rule 1: use previously computed values
  to(B.cell(i) b) from(A.cell(i) a,
                       B.cell(i-1) leftSum){
    b=a+leftSum;
  }
}
```

# Description #

The transform RollingSum computes the rolling (or cumulative) sum of the input vector A, such that the output vector B satisfies B(i)=A(0)+A(1)+…+A(i).  There are two rules in this transform, each specifying a different algorithmic choice.  Rule 0 simply sums up all elements of A up to index i and stores the result to B(i).  The rule header  `to(B.cell(i) b) from(A.region(0, i+1) in)` specifies that output B(i) depends on A(0) through A(i). Rule 1 uses previously computed values of B to get B(i)=A(i)+B(i-1).  The rule header  `  to(B.cell(i) b) from(A.cell(i) a, B.cell(i-1) leftSum)` specifies the dependency of output B(i) on A(i) and B(i-1).