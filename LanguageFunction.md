# Example #

```
function CopySeq
to B[n]
from A[n]
{
  for(int i=0; i<n; ++i)
  {
    B[i] = A[i];
  }
}
```

# Syntax #

function IDENT

[LanguageTransform#Header\_Fields](LanguageTransform#Header_Fields.md)

{
> LanguageRuleBody
}

# Details #

Functions in PetaBricks are syntactic sugar for a transform with only one rule.  They have all the same header fields as a LanguageTransform.

The example above would be expanded (by the compiler) to:

```
transform CopySeq
to _B[n]
from _A[n]
{
  to(_B B) from(_A A)
  {
    for(int i=0; i<n; ++i)
    {
      B[i] = A[i];
    }
  }
}
```