# Golang package-aware includes

```C
#include "github.com/gobpfer/gobpfer/foo/bar.h"
```

Quoted include, with at least two components ("github.com/gobpfer"), the
first one containing dot, is interpreted as golang package reference.
It doesn't matter if "github.com/gobpfer/gobpfer/foo/bar.h" when
interpreted as filesystem path yields a result or not. It is treated as
golang package reference based on the rule above.

When include is expanded by preprocessor, if the package fails to
resolve, a fatal error is signalled. Rationale: for consistency with
regular includes; if the file is missing but include is NOT expanded no
error is signalled.

FYI: golang determines whether a package is stdlib or hosted on the
Internet based on DOT in the first path component [1].

[1] https://github.com/golang/go/issues/32819


## Why overload quoted includes instead of introducing a language extension?

This is primarily to allow IDEs, syntax highlighters and language
servers to handle it out of the box.

For the language server, we can synthesize a (fake) compilation database
and manufacture `-ivfsoverlay` that will satisfy
"github.com/gobpfer/gobpfer/..." and similar includes.

