# compiler-rt
compiler-rt is used to build two libraries: `oecompiler-rt` and, if `CODE_COVERAGE` option is enabled, `libgcov`.

The source is based on the profile library as part of [LLVM compiler-rt libraries](http://compiler-rt.llvm.org/).
The current version is 10.0.1

## oecompiler-rt
Currently no modification is made when building this library.

## libgcov

### Modifications
Below change will not affect `oecompiler-rt` as patched file is not used by it.

The following changes are made to the source to accommodate TEEs (see patches/ for more detail):
- Initialize the `hostfs` during the GCOV initialization (`llvm_gcov_init()`).
  - Ensure that the following filesystem operations work correctly.
- Replace `mmap` and `ummap` (in `map_file()` and `unmap_file()`)
  - `mmap` is not supported by OE. Use `fread()` instead.
  - Alternatively, OE can support `mmap` only for code coverage testing (if there is a performance gap). Note that `mmap` implies using the host memory.
