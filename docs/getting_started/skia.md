## Building Skia

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
