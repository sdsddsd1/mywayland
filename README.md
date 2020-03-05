# KISSlinux compatible wayland repository with wlroots and sway. 🌿



## Native waylandsession with:
```
Alacritty, Bemenu, Mpv, SDL(Doom) and one of the following browsers.
```
```
Vimb (webkit2gtk and gtk+3)works very well.
Surf doesnt work due the lack of xprop.
Falkon (QT5) works quite well(performance) but crashes now and then + buggy right-click but ok to use.
Firefox 72.0.1 works somehow but I cant force the same hardware acceleration I do in X. Flickery on resizing.
FF is compiled with 'ac_add_options --enable-default-toolkit=cairo-gtk3-wayland' and has to be started with
'MOZ_ENABLE_WAYLAND=1 firefox --no-remote'.
```

## Variables for wayland.
```
BEMENU_BACKEND=wayland
SDL_VIDEODRIVER=wayland
MOZ_ENABLE_WAYLAND=1
QT_QPA_PLATFORM=wayland-egl
QT_WAYLAND_FORCE_DPI=physical
```

## Wayland communication socket.
```
if test -z "${XDG_RUNTIME_DIR}"; then
    export XDG_RUNTIME_DIR=/tmp/${UID}-runtime-dir
    if ! test -d "${XDG_RUNTIME_DIR}"; then
        mkdir "${XDG_RUNTIME_DIR}"
        chmod 0700 "${XDG_RUNTIME_DIR}"
    fi
fi
```
