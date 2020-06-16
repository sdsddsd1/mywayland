# Native experimental Wayland session for KISS 🌿

Tests done with Intel graphics. Eudev dependency for Wlroots!  
Sway/hikari/Cagebreak are build with suid bit. This seems to be commonly used  
next to elogind but is not considered 100% ideal.   
The capability method got abandoned because of questionable security.  

NOTE: Due to problems with the latest release of `wf-config` and `wayfire`, they   
both are packaged as a git version. Therefore wlroots has also to be build as  
git.


## NoXland

NoXland is an approach to provide a wayland only session with as many  
dependencies of X ditched as possible.  
While `libxkbcommon` and `xkeyboard-config` are the minimum requiered for a  
graphical base system, they are also sufficent for qt based browsers in commu-   
nity and the ones mentioned here, without deficiencies in performance.  
Webkit2gtk based browser also get along with the aforementioned, but the  
performance is very poor. Therefore, mesa can be build with `libglvnd` to provide  
the missing opengl functionality. This pulls in libX11 and some others. The  
package count is still less than a conventional build.   
Furthermore gtk+3 can be build without the X11 backend(gdkx.h). Wyeb and surfer  
work without it. Vimb not.  
Firefox needs the X11 gtk+3 backend and opengl(`libglvnd`). X dependencies can  
be ditched, but the requierements are high.  
  
Note: Some games requiere libX11 and opengl.  
  
  
Steps to reproduce my efforts:  
First, while you might have to rebuild stuff more than once, install ccache.  
Following the motto "It is easier to add stuff than to remove", I recommend  
a system reset and start from scratch. This way you have no unwelcomed packages  
to build against and error messages will point you to the right places to look  
at. Also check after each build if there is stuff which can be removed.  
I also suggest to compare the above packages, to the official ones. Mesa is pro-  
vided with `libglvnd` and can be forked towards your own needs.  

Note:The following expects  that `libxkbcommon` and `xkeyboard-config` are  
     installed.  

Qt5 is the easiest to build. Just remove everything with a "x" in the depends-  
file and there should be no problem towards qt5-webengine. Falkon requieres the  
configure flag `-DNO_X11=ON`. Qt5-x11extras is not needed.  

Gtk+3 without X11 backend is also easy to build. See the package above. However,  
to enable it, `--enable-x11-backend` has to be explicitly set.  
There is more X requiered at build time that will be orphaned afterwards.

libX11` `libXau` `libXext` `libXi` `libxcb`  

For Firefox, I was able to remove `libXinerama` `libXxf86vm` `libxshmfence` at  
buildtime.


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
- [Crusta](https://github.com/Tarptaeya/Crusta)
- [Dooble](https://github.com/textbrowser/dooble/tree/master/2.x)

