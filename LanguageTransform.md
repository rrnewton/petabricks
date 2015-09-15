# Example #

```
transform Add2D
to C[w,h]
from A[w,h], B[w,h]
{
  C.cell(i,j) from (A.cell(i,j) a, B.cell(i,j) b) {
    return a + b;
  }
}
```

# Syntax #

The high level structure of a transform is as follows.

[TransformHeaderFields](LanguageTransform#Header_Fields.md)

{

> LanguageRule

> LanguageRule

> ...

}

# Calling a Transform #

Transforms are called with function syntax.  Inputs are always passed after outputs.  The example transform above can be called with the code:

```
Add2D(c, a, b);
```

# Header Fields #

  * "_transform_ IDENT" **required** (unless in a LanguageFunction)

> Specifies the name of the transform.

  * "_to_ LanguageMatrixDef, ..." **required**

> Specifies output matrices, see LanguageMatrixDef for an explanation the syntax.

  * "_from_ LanguageMatrixDef, ..."

> Specifies input matrices, see LanguageMatrixDef for an explanation the syntax.

  * "_using_ LanguageMatrixDef, ..."

> Specifies intermediate matrices, see LanguageMatrixDef for an explanation the syntax.  Note that _through_ is a legacy synonym for _using_.

  * "_generator_ IDENT"

> The name of another transform that should be used to generate training inputs for this transform during autotuning.  The other transform's _to_ line should be identical to this transforms _from_ line.  If this line is omitted, training inputs will be generated uniform randomly.

  * "_template_ < IDENT ( int, int) >"

> See LanguageTemplateTransform.

  * "_main_"

> Marks this transform as the default entry point of the program.  Only one transform may have this keyword.  If no transforms have this keyword the last transform in the file is marked as the main transform.

  * "_param_ IDENT, ..."

> Specifies an integer input parameter that may control the size of outputs.

  * "_tunable_ LanguageTunableDecl"

> Declare a value that should be set by the autotuner.  See LanguageTunableDecl for syntax.  Note that _accuracy\_variable_ is a synonym for "_tunable_ accuracy ..."


  * "_accuracy\_metric_ LanguageTunableDecl"

> The name of another transform that should be used to measure the accuracy (quality of service) of this transform.  The other transform should take the union of this transforms _from_ and _to_ lines as input.  The other transform's should output a single scalar value.


  * "_accuracy\_bins_ float, ..."

> Specifies discrete accuracy levels that should be trained for when no specific accuracy is given to the autotuner.

  * "_memoized_"

> Tell the compiler to memoize the result of this transform.  Inputs are hashed (either using SHA1 or MD5, depending on configuration) and outputs may be cached and reused if the input hashes match the cache.  The compiler may decide to ignore this keyword or clear caches at any time.