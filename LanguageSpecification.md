The most fundamental construct in PetaBricks is the _transform_ (see LanguageTransform), a transform is like a function only it allows the user to specify multiple ways to compute different parts of the outputs using a data dependency representation.  Transforms are automatically parallelized and are the primary way of representing choices in PetaBricks.  Other ways of representing choices are reduced to transforms by the compiler.

PetaBricks files should have the extension ".pbcc".  PetaBricks has a preprocessor similar to the C preprocessor that supports constructs like #define and #include and #ifdef.  By convention, any dependencies on other PetaBricks code should be #included at the top of a PetaBricks file.  To include C and C++ header files, use LanguageEmbeddedCxx.

At the top level, a PetaBricks file can contain three constructs:
  * LanguageTransform definitions
  * LanguageFunction definitions
  * LanguageEmbeddedCxx blocks