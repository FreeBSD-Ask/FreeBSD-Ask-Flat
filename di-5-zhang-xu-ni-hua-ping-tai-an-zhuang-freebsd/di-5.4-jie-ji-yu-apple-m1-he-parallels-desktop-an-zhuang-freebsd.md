# 5.4 Installing FreeBSD on Apple M1 with Parallels Desktop

Under macOS 15.7.3 and Parallels Desktop 26.3.3 (57507), the graphical interface, keyboard, and mouse of FreeBSD 16.0 all work properly.

> **Warning**
>
> This section is based on Apple M1, so you should select the aarch64 architecture version of FreeBSD.

## Configuring the Virtual Machine

After the environment is ready, install FreeBSD. The figure below shows the new virtual machine page. Select "Install Windows, Linux, or macOS from an image file", confirm and continue.

![New](../.gitbook/assets/parallels-1.png)

Click "Continue without source" at the bottom left, confirm and continue.

![Continue Without Specifying Source](../.gitbook/assets/parallels-2.png)

In the "Select your operating system" interface, select "Other".

![Select Your Operating System](../.gitbook/assets/parallels-3.png)

Preview the operating system type, confirm and continue.

![Preview Operating System Type](../.gitbook/assets/parallels-3-2.png)

On the "Name and Location" page, you can adjust various settings. Check "Customize settings before installation", confirm and continue.

![Name and Location](../.gitbook/assets/parallels-3-3.png)

After entering the configuration page, you can adjust various settings.

![Name and Location](../.gitbook/assets/parallels-3-4.png)

Select the "Hardware" tab at the top, select "CD/DVD" in the left sidebar, and choose the downloaded FreeBSD image in the "Source" on the right.

![CD/DVD](../.gitbook/assets/parallels-4.png)

Preview the virtual machine configuration and click Continue.

![CD/DVD](../.gitbook/assets/parallels-4-1.png)

> **Tip**
>
> Parallels Desktop's default settings are usually sufficient. It uses UEFI boot by default, so no hardware configuration adjustments are needed.

## Installing FreeBSD

Begin installing the FreeBSD system.

![Install FreeBSD System](../.gitbook/assets/parallels-8.png)

Preview after installing the desktop environment:

![Desktop Interface](../.gitbook/assets/parallels-9.png)

After installing the desktop environment, the system runs normally, but the resolution cannot be adjusted.

## CPU Usage Verification

After manually installing Port **sysutils/htop**, observation shows that CPU usage is normal and no additional adjustments are needed.

![htop](../.gitbook/assets/parallels-10.png)
