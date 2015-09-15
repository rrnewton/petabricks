This example program demonstrates the use of PetaBricks to compute a matrix-matrix multiplication.  We present a few simple algorithms and describe their specification for use with the PetaBricks autotuner.

# Code #

Note: The program code presented below is an abridged version.  The full matrix multiply benchmark code is located in the source tree at `$PBROOT/examples/multiply/multiply.pbcc`, where the environment variable $PBROOT contains the root directory of the PetaBricks source tree.

```
transform MatrixMultiply
from A[c,h], B[w,c] 
to AB[w,h]
{
  
  // Usual O(n^3) matrix multiply

  to(AB ab) from (A a, B b) {
    for(IndexT i=0, maxI=a.height(); i<maxI; ++i) {
      for(IndexT j=0, maxJ=b.width(); j<maxJ; ++j) {
        ab.cell(i,j) = 0;
        for(IndexT k=0, maxK=a.width(); k<maxK; ++k) {
	  ab.cell(i,j) += a.cell(i,k) * b.cell(k,j);
	}
      }
    }
  }

  // compute a cell at a time, the straightforward way

  AB.cell(x,y) from(A.row(y) a, B.column(x) b){
    ElementT sum=0;
    for(IndexT i=0, w=a.width(); i<w; ++i)
      sum+=a.cell(i)*b.cell(i);
    return sum;
  }

  // chop the A matrix in half and recurse

  recursive(h)
  to(AB.region(0, 0,   w, h/2) ab1,
     AB.region(0, h/2, w, h  ) ab2)
  from(A.region(0,   0, c,   h/2) a1,
       A.region(0, h/2, c,   h  ) a2,
       B b)
  {
    spawn MatrixMultiply(ab1, a1, b);
    spawn MatrixMultiply(ab2, a2, b);
  }
  
  // chop the B matrix in half and recurse

  recursive(w)
  to(AB.region(0,   0, w/2, h  ) ab1,
     AB.region(w/2, 0, w,   h  ) ab2)
  from( A a,
        B.region(0,   0, w/2, c  ) b1,
        B.region(w/2, 0, w,   c  ) b2)
  {
    spawn MatrixMultiply(ab1, a, b1);
    spawn MatrixMultiply(ab2, a, b2);
  }

  // chop both matrices in half and recurse

  recursive(c) 
  to(AB ab)
  from( A.region(0,   0, c/2, h  ) a1,
        A.region(c/2, 0, c,   h  ) a2,
        B.region(0,   0, w,   c/2) b1,
        B.region(0, c/2, w,   c  ) b2)
  {
    MatrixRegion2D tmp = MatrixRegion2D::allocate(w,h);
    spawn MatrixMultiply(ab,  a1, b1);
    spawn MatrixMultiply(tmp, a2, b2);
    sync;
    MatrixAdd(ab, ab, tmp);
  }
  
}
```

# Description #

Matrix-matrix multiplication is a operation that appears frequently in numerical algorithms.  While there is only one correct solution, there are multiple ways to compute the matrix product.  Each of the algorithmic variations permutes and parallelizes the memory accesses and arithmetic operations involved in different ways.  The performance differences can be dramatic depending on your machine's architecture.  In this example, we demonstrate how to use PetaBricks to specify a few of the different ways to do the computation.

The transform header:
```
transform MatrixMultiply
from A[c,h], B[w,c] 
to AB[w,h]
```
specifies that we are computing AB (a w by h matrix) from A and B (a c by h matrix and a w by c matrix, respectively).

There are five rules presented, two non-recursive and three recursive.

The first non-recursive rule:
```
  // Usual O(n^3) matrix multiply

  to(AB ab) from (A a, B b) {
    for(IndexT i=0, maxI=a.height(); i<maxI; ++i) {
      for(IndexT j=0, maxJ=b.width(); j<maxJ; ++j) {
        ab.cell(i,j) = 0;
        for(IndexT k=0, maxK=a.width(); k<maxK; ++k) {
	  ab.cell(i,j) += a.cell(i,k) * b.cell(k,j);
	}
      }
    }
  }
```
computes the matrix product sequentially in the traditional textbook manner.

The second non-recursive rule:
```
  // compute a cell at a time, the straightforward way

  AB.cell(x,y) from(A.row(y) a, B.column(x) b){
    ElementT sum=0;
    for(IndexT i=0, w=a.width(); i<w; ++i)
      sum+=a.cell(i)*b.cell(i);
    return sum;
  }
```
specifies how to compute the value of each individual element of the product matrix.  The PetaBricks autotuner can determine the order and parallelization of each of these computations, but it cannot decompose the rule further, nor rearrange its operations.

The first recursive rule:
```
  // chop the A matrix in half and recurse

  recursive(h)
  to(AB.region(0, 0,   w, h/2) ab1,
     AB.region(0, h/2, w, h  ) ab2)
  from(A.region(0,   0, c,   h/2) a1,
       A.region(0, h/2, c,   h  ) a2,
       B b)
  {
    spawn MatrixMultiply(ab1, a1, b);
    spawn MatrixMultiply(ab2, a2, b);
  }
```
splits the A matrix horizontally into two parts.  When right-multiplied by the B matrix, each of those parts forms part of the product AB.  The two multiplications are recursively spawned to be scheduled for computation by the run-time.  (Note: the syntax for calling PetaBricks transforms places the outputs as arguments before the inputs.  Calling MatrixMultiply(ab1, a1, b) multiplies a1 and b and places the result in ab1.)

The second recursive rule is similar to the first, but splits B vertically instead.

The third recursive rule:
```
  // chop both matrices in half and recurse

  recursive(c) 
  to(AB ab)
  from( A.region(0,   0, c/2, h  ) a1,
        A.region(c/2, 0, c,   h  ) a2,
        B.region(0,   0, w,   c/2) b1,
        B.region(0, c/2, w,   c  ) b2)
  {
    MatrixRegion2D tmp = MatrixRegion2D::allocate(w,h);
    spawn MatrixMultiply(ab,  a1, b1);
    spawn MatrixMultiply(tmp, a2, b2);
    sync;
    MatrixAdd(ab, ab, tmp);
  }
```
splits A <i>vertically</i> and B <i>horizontally</i>.  Multiplying the matching pairs of sub-matrices produces two partial products, each the size of the output AB, which must then be summed to get the actual product.  Since we require temporary storage space, the first line in the rule allocates a new matrix tmp.  As in the previous two rules, we spawn the recursive calls to matrix multiply, but this time we must explicitly wait for those computations to complete (using the sync keyword) before adding their results.

# Autotuning #

We have now specified five different ways to compute the matrix product when the MatrixMultiply transform is called.  The PetaBricks autotuner will help us determine what composition of these rules is best suited for different input sizes on your computer.

To invoke the autotuner, first compile:
```
sh> $PBROOT/src/pbc multiply.pbcc
```
and then run:
```
sh> $PBROOT/scripts/sgatuner.py multiply -n 1024
```
The autotuner will then try different execution paths during its learning process.

One class of choices the autotuner makes is which rule to execute for each of the input sizes up to 1024.  (Note: the maximum input size to learn is given as a parameter to sgatuner.py above.)

Different rules can be chosen for different sizes, so if recursive rules are chosen, multiple rules may be executed for a single matrix product computation as the input matrices are recursively decomposed into smaller and smaller matrices.  The decisions of <i>when</i> and between <i>which</i> rules to switch are learned by the autotuner.

Note that each of the recursive rules' headers contains the keyword `recursive`.  The argument to this keyword indicates to the autotuner which input size parameter it should use in deciding whether to use this rule.  For example, in the first recursive rule, the line `recursive(h)` indicates the program will use the height of the input matrix A to decide whether it should execute this rule or choose another rule instead.

Another class of choices the autotuner makes is execution order.  Since the second non-recursive rule is specified on a per-element basis, the iteration order over the cells of the output matrix must be determined.  The compiler's synthesized outer control flow allows the autotuner to intelligently search the space of iteration orders for an efficient solution.

For more information about the autotuner, please see our <a href='http://groups.csail.mit.edu/commit/?page=publications-static&keyword=PetaBricks'>publications</a>.