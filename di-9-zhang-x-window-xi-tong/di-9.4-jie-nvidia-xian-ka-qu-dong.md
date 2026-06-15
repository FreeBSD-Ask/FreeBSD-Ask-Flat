# 9.4 NVIDIA Graphics Drivers

## NVIDIA Graphics Driver Overview

For desktop computers, if the CPU is an Intel processor with a model number ending in F (such as [i5-9400F](https://www.intel.com/content/www/us/en/products/sku/190883/intel-core-i59400f-processor-9m-cache-up-to-4-10-ghz/specifications.html)) or KF (such as [i5-12600KF](https://www.intel.com/content/www/us/en/products/sku/134590/intel-core-i512600kf-processor-20m-cache-up-to-4-90-ghz/specifications.html)), then that model has no integrated graphics, and there is no need to handle integrated graphics-related configuration.

If you already have a discrete graphics card and the video output interface (DP or HDMI) is directly connected to the discrete graphics card, you typically do not need to configure the integrated graphics; just handle the discrete graphics driver.

Laptops without GPU passthrough capability must first install and configure the Intel integrated graphics driver (the relevant DRM module) as described in other chapters, and then follow the instructions below to complete the NVIDIA discrete graphics driver configuration.

## Joining the video Group

Add the specified user to the video group to access graphics card devices:

```sh
# pw groupmod video -m actual_username
```

## Installing the Graphics Driver

Install using pkg:

```sh
# pkg install nvidia-drm-kmod nvidia-settings
```

Or install using Ports:

```sh
# cd /usr/ports/graphics/nvidia-drm-kmod/ && make install clean
# cd /usr/ports/x11/nvidia-settings/ && make install clean
```

List installed NVIDIA-related software:

```sh
# pkg info -q | grep -i nvidia
```

## Configuring the NVIDIA Graphics Driver

### Loading NVIDIA-related Kernel Modules

Execute the following commands to configure the NVIDIA kernel modules:

```sh
# sysrc -f /boot/loader.conf hw.nvidiadrm.modeset=1  # Enable NVIDIA DRM mode setting
# sysrc -f /etc/rc.conf kld_list+=nvidia-modeset      # Add nvidia-modeset kernel module for loading at boot
```

> **Warning**
>
> X11 users should not attempt to load the `nvidia-drm.ko` kernel module, as this may cause the system to crash. Wayland users should load it as needed.

### Generating an X11 Configuration File

Note that if the system displays normally, there is no need to perform the steps in this section. Before executing `Xorg -configure`, you must ensure that no X server is currently running; otherwise, the command will fail.

```sh
# Xorg -configure                     # Automatically generate Xorg configuration file
# cp /root/xorg.conf.new /usr/local/etc/X11/xorg.conf.d/xorg.conf  # Copy the generated configuration file to the configuration directory
```

> **Warning**
>
> Do not attempt to install and use the Port **x11/nvidia-xconfig**. This tool is currently not applicable and may cause the system to become unresponsive.

## Hardware Acceleration and Decoders

Install the VDPAU driver and related libraries to support video hardware acceleration.

- Install using pkg:

```sh
# pkg install libva-vdpau-driver libvdpau libvdpau-va-gl
```

- Or install using Ports:

```sh
# cd /usr/ports/multimedia/libva-vdpau-driver/ && make install clean
# cd /usr/ports/multimedia/libvdpau/ && make install clean
# cd /usr/ports/multimedia/libvdpau-va-gl/ && make install clean
```

After restarting, the NVIDIA driver will be properly enabled.

## Viewing NVIDIA Driver Status

List all NVIDIA GPUs and their detailed information:

```sh
$ nvidia-smi
```

Example output of the `nvidia-smi` command:

```sh
# nvidia-smi
Mon Jan 19 19:06:59 2026
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.126.09             Driver Version: 580.126.09     CUDA Version: N/A      |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 3060 Ti     Off |   00000000:01:00.0  On |                  N/A |
|  0%   39C    P8             12W /  225W |     409MiB /   8192MiB |      0%      Default |
|                                         |                        |               N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
```

- View KDE system information:

![KDE System Information](../.gitbook/assets/nvi2.png)

- Open a movie using mpv, and you can observe the video memory usage increase significantly (from 3 MB to hundreds of megabytes); you can also use SMPlayer to watch.

![mpv Video Memory Usage](../.gitbook/assets/nvi1.jpg)

## Troubleshooting

### nvidia-smi Command Reports "mismatch" Error

![nvidia-smi error](../.gitbook/assets/no-version-vo.jpg)

When executing the nvidia-smi command, an error message "API mismatch" appears: this error indicates an API mismatch, usually caused by version compatibility issues. Possible causes include: version mismatch among NVIDIA driver components themselves, version mismatch between the NVIDIA driver and other NVIDIA software packages, or version mismatch between the NVIDIA driver and the current FreeBSD base system version.

It is recommended to first uninstall all NVIDIA software packages, then update the FreeBSD base system to the latest version, and then reinstall the driver.

### How to Uninstall Existing NVIDIA-related Packages

If a version mismatch is reported, you need to first uninstall all installed NVIDIA-related packages, then follow the configuration in this section:

First list the installed NVIDIA-related packages, then delete them one by one:

```sh
# pkg info -q | grep -i nvidia
# pkg delete nvidia-drm-kmod nvidia-driver nvidia-settings
```

### How to Prevent Driver Updates

Lock each relevant package from the output of `pkg info -q | grep -i nvidia` using the `pkg lock` command.

For example:

```sh
# pkg lock nvidia-drm-kmod
# pkg lock nvidia-settings
```

However, running `freebsd-update` or patching/updating the system via PkgBase may also affect driver compatibility, so you need to weigh security needs against usability based on actual circumstances.

## References

- Intel Corporation. A brief guide about our latest processors and naming updates[EB/OL]. [2026-03-25]. <https://www.intel.com/content/www/us/en/processors/processor-numbers.html>. Intel's official CPU naming rules and model identification guide.
- NVIDIA Corporation. NVIDIA Driver Documentation[EB/OL]. [2026-03-25]. <https://www.nvidia.com/Download/index.aspx>. NVIDIA's official driver download and technical documentation.

## Exercises

1. Test on a laptop with a directly connected GPU and submit a PR.
2. Test NVIDIA CUDA invocation under the Linux compatibility layer.
3. The official NVIDIA driver is distributed in binary closed-source form, while the FreeBSD kernel uses the BSD license. Analyze the compatibility controversy between closed-source kernel modules and the BSD license, and discuss whether the nouveau open-source driver can replace the closed-source solution on FreeBSD.
