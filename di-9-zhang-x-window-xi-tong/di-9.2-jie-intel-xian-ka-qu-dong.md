# 9.2 Intel Graphics Driver

This section covers the driver installation and configuration for Intel integrated graphics (i915 DRM module). Please read the Graphics Driver Overview first.

## Installing Intel Integrated Graphics

### FreeBSD 14.x

> **Tip**
>
> To install using pkg, refer to the kernel modules (kmods) source configured in other chapters of this book.

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

> **Note**
>
> Early Intel integrated graphics such as HD 4000 from the 3rd generation processors, when no DRM driver is installed, can only use VESA framebuffer (without GPU acceleration) as a fallback display in legacy BIOS mode, and may exhibit screen corruption in UEFI mode. Only after installing the corresponding version of the DRM graphics driver can normal GPU hardware acceleration be obtained.

## Configuring Intel Integrated Graphics

Add the `i915kms` kernel module to `kld_list` in the **/etc/rc.conf** file so that it loads at system startup:

```sh
# sysrc -f /etc/rc.conf kld_list+=i915kms
```

## Video Hardware Decoding

> **Warning**
>
> If you skip this section, software that depends on GPU acceleration such as Blender may not run properly or may produce "segmentation faults".

### Installing the Intel VA-API Media Driver

- Install using pkg:

```sh
# pkg install libva-intel-media-driver
```

- Or install using Ports:

```sh
# cd /usr/ports/multimedia/libva-intel-media-driver/
# make install clean
```

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

## Appendix: Intel Graphics Power Management

Some graphics cards may consume too much power. FreeBSD can reduce power consumption through specific configurations.

If using Intel graphics with the **graphics/drm-kmod** driver, you can add the following options to the **/boot/loader.conf** file:

```ini
compat.linuxkpi.i915_fastboot=1  	# ①
compat.linuxkpi.i915_enable_dc=2 	# ②
compat.linuxkpi.i915_enable_fbc=1	# ③
```

Feature descriptions:

| Configuration Item | Value | Description |
| ------------------ | ----- | ----------- |
| `compat.linuxkpi.i915_fastboot` | `1` | Attempt to skip unnecessary mode setting at boot |
| `compat.linuxkpi.i915_enable_dc` | `2` | Enable power-saving display C-states |
| `compat.linuxkpi.i915_enable_fbc` | `1` | Enable frame buffer compression to save power |

## References

- FreeBSD Project. Graphics[EB/OL]. [2026-03-25]. <https://wiki.freebsd.org/Graphics>. The official FreeBSD wiki, providing detailed lists of graphics hardware compatibility and configuration guides.

## Exercises

1. Try to automate the DRM porting process on FreeBSD.
2. Try to re-port the DRM implementation from OpenBSD.
