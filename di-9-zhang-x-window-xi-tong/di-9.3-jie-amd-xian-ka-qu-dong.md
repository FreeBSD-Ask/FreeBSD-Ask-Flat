# 9.3 AMD Graphics Drivers

This section covers the installation and configuration of AMD graphics drivers. Please read the Graphics Driver Overview first.

## Installing AMD Graphics Driver

> **Note**
>
> When using GNOME, if the screen automatically locks/turns off, you may not be able to return to the desktop. For related technical issues, see: Bug 255049 - x11/gdm doesn't show the login screen[EB/OL]. [2026-03-26]. <https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=255049>.

> **Note**
>
> When installing via Ports, the drm driver requires a copy of the current system source code in **/usr/src**. For details, refer to the system update chapter. If you have already followed other chapters in this book to install it, the system typically already has a copy of the source code, and there is no need to obtain it again.

### FreeBSD 14.x

```sh
# cd /usr/ports/graphics/drm-61-kmod
# make BATCH=yes install clean
```

Or install using pkg (use this method if Ports installation has issues):

```sh
# pkg install drm-61-kmod
```

### FreeBSD 15.0

Install using Ports:

```sh
# cd /usr/ports/graphics/drm-66-kmod
# make BATCH=yes install clean
```

## AMD Graphics Configuration

- For AMD graphics cards based on the GCN architecture (HD 7750 and above) and newer, add the `amdgpu` kernel module to `kld_list` in the **/etc/rc.conf** file so that it loads at system startup (most users should use this driver; if it does not work, change it to `radeonkms`):

```sh
# sysrc -f /etc/rc.conf kld_list+=amdgpu
```

- For AMD graphics cards prior to the GCN architecture, add the `radeonkms` kernel module to `kld_list` in the **/etc/rc.conf** file:

```sh
# sysrc -f /etc/rc.conf kld_list+=radeonkms
```

## AMD Graphics Video Hardware Decoding

> **Warning**
>
> If you skip this section, software such as Blender will not run and will directly produce "segmentation faults".

### Installing Mesa's Gallium VA-API and VDPAU Support Packages

- Install using pkg:

```sh
# pkg install mesa-gallium-va mesa-gallium-vdpau
```

- Or install using Ports:

```sh
# cd /usr/ports/graphics/mesa-gallium-va/ && make install clean
# cd /usr/ports/graphics/mesa-gallium-vdpau/ && make install clean
```

### Appendix: X11 Settings

If the above configuration does not take effect, you may also need to configure X11.

Write the following content to the **/usr/local/etc/X11/xorg.conf.d/20-amdgpu-tearfree.conf** file (create this file yourself):

```ini
Section "Device"
  Identifier "AMDgpu"          # Set device identifier to AMDgpu
  Driver "amdgpu"              # Use amdgpu driver
  Option "TearFree" "on"       # Enable TearFree feature to prevent screen tearing
EndSection
```

After configuration, you can test using the command `mpv --hwdec=auto xxx.mp4`. You need to install mpv yourself.

## References

- FreeBSD Project. Graphics[EB/OL]. [2026-03-25]. <https://wiki.freebsd.org/Graphics>. The official FreeBSD wiki, providing detailed lists of graphics hardware compatibility and configuration guides.

## Exercises

1. Try to automate the DRM porting process on FreeBSD.
2. Try to re-port the DRM implementation from OpenBSD.
