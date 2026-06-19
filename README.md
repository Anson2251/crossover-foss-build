# CrossOver FOSS Build for macOS

CrossOver FOSS 26.2.0 (Wine 11.0) base with DXMT support.

Built from [CodeWeavers CrossOver FOSS sources](https://www.codeweavers.com/crossover/source).

`cxbuilder.sh` is modified from [101arrowz/cxbuilder](https://github.com/101arrowz/cxbuilder).

## Requirements

- **DXMT** — a required Metal-based D3D11/10 implementation.  
  Get it from [3Shain/dxmt](https://github.com/3Shain/dxmt)
- macOS 14+ (Apple Silicon or Intel)
- [Homebrew](https://brew.sh)

## Build

```sh
./cxbuilder.sh -w ./<source dir>/wine --no-gptk --no-dxvk --out ./build <source dir>
```

The script automatically fetches x86_64 dependencies via Homebrew bottles and builds Wine. After the build completes, the output is in `build/`.

## Install DXMT

If DXMT is built with `-Dwine_builtin_dll=true` (including GitHub Actions artifact),
copy files to the Wine build and wineprefix:

| File | Destination |
|------|-------------|
| `winemetal.so` | `<wine>/lib/wine/x86_64-unix/winemetal.so` |
| `winemetal.dll` | `<wine>/lib/wine/x86_64-windows/winemetal.dll` _and_ `<wineprefix>/drive_c/windows/system32/winemetal.dll` |
| `d3d11.dll` | `<wine>/lib/wine/x86_64-windows/d3d11.dll` |
| `dxgi.dll` | `<wine>/lib/wine/x86_64-windows/dxgi.dll` |
| `d3d10core.dll` | _(optional)_ `<wine>/lib/wine/x86_64-windows/d3d10core.dll` |

## Configure Flags

```
--disable-tests
--enable-archs=i386,x86_64
--with-coreaudio
--with-cups
--with-freetype
--with-gettext
--with-gnutls
--with-gstreamer
--with-inotify
--with-mingw
--with-pcap
--with-pcsclite
--with-pthread
--with-unwind
--with-vulkan
--without-alsa
--without-capi
--without-dbus
--without-fontconfig
--without-gettextpo
--without-gphoto 
--without-gssapi
--without-krb5
--without-netapi
--without-opencl
--without-opengl
--without-oss
--without-pulse
--without-sane
--without-sdl
--without-udev
--without-usb
--without-v4l2
--without-wayland
--without-x
```

### Graphics Stack

```
D3D11/12  → DXMT      → Metal
D3D9/8    → wined3d   → Vulkan → MoltenVK → Metal
GDI       → winemac.drv → Metal (via CAMetalLayer)
```

- No OpenGL dependency (macOS OpenGL is deprecated 4.1)
- DXVK intentionally excluded (DXMT replaces it)
- GPTK intentionally excluded (conflicts with DXMT)

## Notes

- **Do not open issues here.** Report bugs to [CodeWeavers](https://www.codeweavers.com/support/) or [DXMT](https://github.com/3Shain/dxmt)
- This build includes CodeWeavers patches (d3dmetal compatibility layer, msync)
- `wine --version` reports `wine-11.0`
- Wordpad and other GDI apps work **ONLY** when DXMT is installed
- Releases in this repo do **not** include DXMT; install it separately
