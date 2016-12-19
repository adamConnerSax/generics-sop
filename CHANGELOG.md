# 0.2.3.0 (2016-12-04)

* Add various metadata getters

* Add `hdicts`.

* Add catamorphisms and anamorphisms for `NP` and `NS`.

* TH compatibility changes for GHC 8.1 (master).

# 0.2.2.0 (2016-07-10)

* Introduced `unZ` to destruct a unary sum.

* Add Haddock `@since` annotations for various functions.

# 0.2.1.0 (2016-02-08)

* Now includes a CHANGELOG.

* Should now work with ghc-8.0.1-rc1 and -rc2 (thanks to
  Oleg Grenrus).

* Introduced `hd` and `tl` to project out of a product, and
  `Projection` and `projections` as duals of `Injection` and
  `injections`.

# 0.2.0.0 (2015-10-23)

* Now tested with ghc-7.10

* Introduced names `hmap`, `hcmap`, `hzipWith`, `hczipWith` for
  `hliftA`, `hcliftA`, `hliftA2`, `hcliftA2`, respectively.
  Similarly for the specialized versions of these functions.

* The constraint transformers `All` and `All2` are now defined
  as type classes, not type families. As a consequence, the
  partial applications `All c` and `All2 c` are now possible.

* Because of the redefinition of `All` and `All2`, some special
  cases are no longer necessary. For example, `cpure_POP` can
  now be implemented as a nested application of `pure_NP`.

* Because of the redefinition of `All` and `All2`, the functions
  `hcliftA'` and variants (with prime!) are now deprecated.
  One can easily use the normal versions instead.
  For example, the definition of `hcliftA'` is now simply

      hcliftA' p = hcliftA (allP p)
        where
	      allP :: proxy c -> Proxy (All c)
		  allP _ = Proxy

* Because `All` and `All2` are now type classes, they now have
  superclass constraints implying that the type-level lists they
  are ranging over must have singletons.

      class (SListI xs,  ...) => All c xs
      class (SListI xss, ...) => All2 c xss

  Some type signatures can be simplified due to this.

* The `SingI` typeclass and `Sing` datatypes are now deprecated.
  The replacements are called `SListI` and `SList`.
  The `sing` method is now called `sList`. The difference
  is that the new versions reveal only the spine of the list, and
  contain no singleton representation for the elements anymore.

  For one-dimensional type-level lists, replace

      SingI xs => ... 

  by

      SListI xs => ...

  For two-dimensional type-level lists, replace

      SingI xss => ...

  by

      All SListI xss => ...

  Because All itself implies `SListI xss` (see above), this
  constraint is equivalent to the old `Sing xss`.

  The old names are provided for (limited) backward
  compatibility. They map to the new constructs. This will
  work in some, but not all scenarios.

  The function `lengthSing` has also been renamed to
  `lengthSList` for consistency, and the old name is
  deprecated.

* All `Proxy c` arguments have been replaced by `proxy c`
  flexible arguments, so that other type constructors can be
  used as proxies.

* Class-level composition (`Compose`), pairing (`And`), and
  a trivial constraint (`Top`) have been added. Type-level map
  (`Map`) has been removed. Occurrences such as

      All c (Map f xs)

  should now be replaced with

      All (c `Compose` f) xs

* There is a new module called `Generics.SOP.Dict` that contains
  functions for manipulating dictionaries explicitly. These can
  be used to prove theorems about non-trivial class constraints
  such as the ones that get built using `All` and `All2`. Some
  such theorems are provided.

* There is a new TH function `deriveGenericFunctions` that
  derives the code of a datatype and conversion functions, but
  does not create a class instance. (Contributed by Oleg Grenrus.)

* There is a new TH function `deriveMetadataValue` that
  derives a `DatatypeInfo` value for a datatype, but does
  not create an instance of `HasDatatypeInfo`. (Contributed by
  Oleg Grenrus.)

* There is a very simple example file. (Contributed by Oleg
  Grenrus.)

* The function `hcollapse` for `NS` now results in an `a` rather
  than an `I a`, matching the specialized version `collapse_NS`.
  (Suggested by Roman Cheplyaka.)

# 0.1.1.2 (2015-03-27)

* Updated version bounds for ghc-prim (for ghc-7.10).

# 0.1.1.1 (2015-03-20)

* Preparations for ghc-7.10.

* Documentation fix. (Contributed by Roman Cheplyaka.)

# 0.1.1 (2015-01-06)

* Documentation fixes.

* Add superclass constraint (TODO).

* Now derive tuple instance for tuples up to 30 components.
  (Contributed by Michael Orlitzky.)

