---
layout: post
title: C pointers to multi-dimensional assumed shape Fortran arrays with gfortran
category: posts
---

Interoperability between C and Fortran codes is often complicated by Fortran's
support for "true" multi-dimensional arrays. In addition, Fortran 90 supports
assumed shape (also referred to as deferred shape) arrays, where the extent
sizes are determined at runtime. Actual arguments for array dummy arguments
can be array slices that may be of non-unit stride. In short, an assumed size
Fortran array can reference non-contiguous chunks of memory.

Therefore, the compiler will reject an attempt to obtain a raw data pointer of
a Fortran array to pass to a C function like in the following module:

{% gist kynan/1e1b2ee82b2999b75044 test1.F90 %}

`gfortran` 4.7 emits the following error message:

```
Error: Assumed-shape array 'v' at (1) cannot be an argument to the procedure 'c_loc' because it is not C interoperable
```

What if you know that the assumed-shaped array passed to your function is
actually contiguous (or you choose to leave it the caller's responsibility)?
Surely the compiler should be able to give you a pointer to the first array
element like so:

{% gist kynan/1e1b2ee82b2999b75044 test2.F90 %}

Turns out `gfortran` doesn't and generates the same error as before. The
`gfortran` developers have recognized this as a [bug][1], which will be fixed in
the 4.9 release.

In the meantime you can work around it by defining a pure function that simply
returns the first argument of an assumed shape array. `gfortran` is happy with
that since now the type of the argument passed to `c_loc` is a scalar and
therefore interoperable:

{% gist kynan/1e1b2ee82b2999b75044 test3.F90 %}

However, this workaround confuses Intel's and PGI's Fortran compilers, so
simply escape it using the a preprocessor `#define`:

{% gist kynan/1e1b2ee82b2999b75044 test4.F90 %}

[1]: http://gcc.gnu.org/bugzilla/show_bug.cgi?id=50269
