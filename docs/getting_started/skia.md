## Building Skia

## Building on Linux

```bash
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
cd skia
git checkout xamarin-mobile-bindings -b v1.60.1
sh tools/install_dependencies.sh
python ./tools/git-sync-deps
./bin/gn gen 'out/linux/x64' --args='
    is_official_build=true skia_enable_tools=false
    target_os="linux" target_cpu="x64"
    skia_use_icu=false skia_use_sfntly=false skia_use_piex=true
    skia_use_system_expat=false skia_use_system_freetype2=false skia_use_system_libjpeg_turbo=false 
    skia_use_system_libpng=false skia_use_system_libwebp=false skia_use_system_zlib=false
    skia_enable_gpu=true
    extra_cflags=[ "-DSKIA_C_DLL" ]
    linux_soname_version="60.1.0"'
../depot_tools/ninja 'SkiaSharp' -C 'out/linux/x64'
mkdir out/Shared && cp out/linux/x64/libSkiaSharp.so.60.1.0 out/Shared/libskia.so
```

references:

- [https://github.com/mono/SkiaSharp/wiki/Building-SkiaSharp]()
- [https://skia.org/user/build]()

> ℹ️ Starting from version **1.68.x** linux binaries should also be included
> in the SkiaSharp nuget package

## Getting Skia on Windows

Extract a pre-build dll from SkiaSharp nuget package.
> e.g https://www.nuget.org/api/v2/package/SkiaSharp/1.68.1.1 <br>
> skiasharp.1.68.1.1\runtimes\win-x64\native\libSkiaSharp.dll > libskia.dll

see:
- [https://github.com/mono/SkiaSharp/wiki/SkiaSharp-with-Python]()
- [https://www.nuget.org/packages/SkiaSharp/]()
