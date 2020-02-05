# What is this?  

What'is nimskia? *nimskia* is a project to make the Skia C++ library available from **nim**.
The library is composed of two main pieces:

1. Skia Nim bindings generated from Skia([mono/skia](https://github.com/mono/skia/tree/xamarin-mobile-bindings/include/c)) C interface
2. a more Nim-friendly api based on the above wrapper

note: the api is heavily inspired by the SkiaSharp project.

## 🔥 News / Notes 🔥

- 
  > ⚠️ enabled compilation on **Windows**.
  > Most of the examples probably will not work because
  > the default configuration was tuned to work on Linux.(i expect SkSurface to be nil in most cases)
  >. additional fixes may be needed. More to come.
