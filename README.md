# Native experimental Wayland session with Sway and hikari for KISS 🌿
```
Tests done with Intel graphics.
Eudev dependency for Wlroots!
Sway/hikari/Cagebreak are build with suid bit. This seems to be commonly used
next to elogind but this is not considered 100% ideal. 
The capability method got abandoned because of questionable security.
```

## NoXland
```
NoXland is an approach to ditch as many X dependencies as possible. While 
`libxkbcommon` and `xkeyboard-config` are the minimum requiered for qt5 and
webkit2gtk based browsers, firefox needs a lot more.
Qt5 browsers like `viper-browser`, ´Crusta` and `jsml` are tested well and
just need their qt dependencies to behave performance wise like a conventional
X build.
On the webkit2gtk side of things the performance, espacially on heavy sites, is
very poor. This can be improved by coupling mesa with `libglvnd` to provide a so
called vendor neutral libgl.so which makes webkit2gtk play much nicer. When
there is a "libgl.so", `opengl` alongside with the wayland only `wpebackend-fdo`
for hardware accelerated rendering can be enabled.

NOTE: A libglvnd based build requieres:
      ```libX11 libXau libXext libXslt libxcb libxkbcommon xcb-proto
      xkeyboard-config xorgproto```

NOTE: Some browser like `surfer` and `wyeb` come along with gtk+3 and wayland
      backend only. Vimb e.g. requieres also the Xorg backend. My smallest X-
      and wayland backend enabled gtk+3 build needs the following(just disable
      everything with a "X"):
      'libX11' 'libXau' 'libXext' 'libXi' 'libxcb'
      
      NOTE: There may be more requiered at build time.

For Firefox `libglvnd` aswell as the X11 backend for gtk+3 is requiered. The
dependencies can be shrunk but while there is no option yet to disable the X11
backend, the efforts are low on reward. The follwing can be ditched:

`libXinerama` `libXxf86vm` `libxshmfence`
```
 
## Compatibility variables
```
BEMENU_BACKEND=wayland
SDL_VIDEODRIVER=wayland
MOZ_ENABLE_WAYLAND=1
QT_QPA_PLATFORM=wayland-egl
QT_WAYLAND_FORCE_DPI=physical
```

## Wayland communication socket
```
if test -z "${XDG_RUNTIME_DIR}"; then
    export XDG_RUNTIME_DIR=/tmp/${UID}-runtime-dir
    if ! test -d "${XDG_RUNTIME_DIR}"; then
        mkdir "${XDG_RUNTIME_DIR}"
        chmod 0700 "${XDG_RUNTIME_DIR}"
    fi
fi
```

## Migration
```
Starting off a fresh xorg-less KISS, installation *should* be as easy as
'kiss b sway && kiss i sway' without any further intervention.

However when you come over from Xorg KISS, some rebuilds may be rquired
in order to pickup Wayland:
gtk+3, intel-vaapi-driver, libav, mesa, mpv, sdl*, webkit2gtk. Maybe more.
Packages which dont pickup Wayland automtically are added with modified buildfiles.
Make sure to have KMS enabled.
```

## Links
- [mvi - view images in mpv](https://github.com/occivink/mpv-image-viewer)  
- [Surfer](https://github.com/nihilowy/surfer) 
- [wyeb](https://github.com/jun7/wyeb)  
- [jsml](https://github.com/troysung/jsml)  
- [Crusta](https://github.com/Tarptaeya/Crusta

