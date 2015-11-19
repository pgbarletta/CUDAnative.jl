# Package infrastructure

* Intrinsics clobber: importing CUDA overrides the normal stuff (ie. `floor` or
  `sin`)... Current work-around: don't export, require `CUDA.`. Make them
  conditional, based on `@target ptx`?

* Support for newer CUDA versions

* Use the existing `cfunction` functionality to generate a CUDA kernels?


# Native execution

* Pass the native codegen context into `@cuda`, allowing for multiple active
  contexts.

* Related to the point above, ff we have a single `main`, instantiating `cgctx`
  and launching a kernel (a common use-case, right?) the macro expansion happens
  before the `cgctx` instantiation is executed, but the compiler _does_ use the
  codegen context internally!

* Improve error reporting when using undefined functions. Currently, Julia just
  generates valid code, calling back into the compiler (in order to support
  calling functions defined later on). This causes the PTX back-end to generate
  a `cannot box` error, while normally Julia would report `undefined function`
  at run-time.

* Require kernel callees to have the `@target ptx` annotation.

* Refuse use of undefined entities (functions, variables). Currently, Julia
  silently boxes, and postpones any error to run-time, in case the object would
  still be defined.


# Minor

* Use `Logging.@error` instead of `error`?