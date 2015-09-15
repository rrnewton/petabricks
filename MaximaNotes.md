The PetaBricks compiler, pbc, depends on [Maxima](http://maxima.sourceforge.net/), a Computer Algebra System, for reasoning about matrix indices.  This dependency is only at compile time.  Maxima is **not** used when running or autotuning PetaBricks programs.  Maxima is run in a separate process spawned by pbc.

Unfortunately there is a [bug](http://sourceforge.net/tracker/?func=detail&atid=104933&aid=2938078&group_id=4933) in (at least) Maxima versions 5.15 to 5.20.1 that prevents it from working with petabricks.  Maxima crashes in response to some of the types of queries pbc makes.  In addition maxima has broken handling of inequalities in other versions.

To test your version of maxima for compatibility run:
```
./scripts/checkmaxima.py /usr/bin/maxima
```
this is a custom test script that checks for the bugs in maxima that affect PetaBricks.

We recommend using maxima version 5.22.1, which has been the most extensively tested with PetaBricks. For debian-based linux distributions this version can downloaded directly from packages.debian.org: [maxima](http://packages.debian.org/testing/maxima) and [maxima-share](http://packages.debian.org/testing/maxima-share).  (Both maxima and maxima-share are required).

On Mac, some versions of Maxima seem to have severe performance problems that result in hour long compile times instead of seconds.  The latest version of maxima from [Homebrew](http://mxcl.github.com/homebrew/) fixes this issue.  This is an issue in the underlying lisp implementation.