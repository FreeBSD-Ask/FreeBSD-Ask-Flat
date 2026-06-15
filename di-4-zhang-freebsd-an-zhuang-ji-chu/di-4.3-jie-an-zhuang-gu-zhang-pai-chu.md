# 4.3 Installation Troubleshooting

The most common blocking issues during installation include being unable to enter the installation interface, partition table errors, and boot loss. This section provides troubleshooting steps and recovery methods for each issue.

## Unable to Enter the Installation Interface

If you cannot enter the installation interface, first distinguish between a virtual machine environment and a physical machine environment, then troubleshoot accordingly.

If it is a virtual machine, check the virtual machine configuration.

If it is a physical machine, check the following items in order:

- Is the computer a standard personal computer?
- Is the processor brand Intel or AMD?
- Has Secure Boot been disabled in the BIOS?
- Was the image downloaded from <https://www.freebsd.org>?
- Was the latest RELEASE image downloaded?
- Does the downloaded image file have the extension `img` (USB device) or `iso` (optical disc image)?
- Did the image checksum (SHA-256) pass?
- Does the downloaded image contain `amd64` (standard personal computers)?
  - Please confirm it is `amd64` (for standard x86 personal computers) **not** `arm64` (for ARM architecture devices, such as Raspberry Pi and other embedded platforms).
- Is the USB flash drive an expanded-capacity drive (a counterfeit capacity product)?
- Was the Ventoy tool used?
  - If using Ventoy - Multi-boot USB boot drive creation tool <https://www.ventoy.net/en/index.html> fails to boot, try using Rufus - Create bootable USB drives the easy way <https://rufus.ie/> to burn the image instead.

If problems persist, please first ask a question in English on the FreeBSD official forum <https://forums.freebsd.org/>; if no answer is received, follow the guidelines in other chapters to submit a bug.

## Re-entering the Installation Interface After Reboot

When rebooting the system after installation is complete, you may re-enter the installation interface. In this case, you need to check the boot device.

If it is a virtual machine, manually eject or disconnect the virtual DVD drive's automatic connection, then reboot. If it is a physical machine, unplug the USB flash drive or eject the installation disc, then reboot.

## ACPI Error Messages Output at Boot

> **Warning**
>
> Some articles recommend disabling ACPI. This practice lacks technical justification on modern hardware; disabling ACPI may cause the system to fail to boot properly or have limited functionality. ACPI is closely related to power state management, device power saving, and multi-processor support; the option to disable ACPI should be considered a legacy feature.

If ACPI error messages appear, they usually do not affect normal operation in most cases. This can typically be resolved by updating the motherboard BIOS or firmware. In rare cases, it may be necessary to patch the SSDT (Secondary System Description Table) and DSDT (Differentiated System Description Table).

Some manufacturers recommend upgrading the motherboard BIOS/firmware only when necessary. The upgrade process **may** encounter issues (such as unexpected power loss), resulting in an incomplete BIOS firmware that prevents the computer from functioning properly.

## System Stalling at a Service During Boot

When installing older versions, the system boot may stay for a long time at services such as sendmail; when a static IP address needs to be configured, the system may continuously attempt DHCP.

In this case, try pressing **Ctrl** + **C** to interrupt the service, allowing the system to continue booting.

### References

- FreeBSD Project. FreeBSD 14.0-RELEASE Release Notes[EB/OL]. [2026-04-17]. <https://www.freebsd.org/releases/14.0R/relnotes/>. Starting from FreeBSD 14.0, the default Mail Transfer Agent (MTA) was replaced from sendmail to dma (Dragonfly Mail Agent), and sendmail no longer starts by default, resolving the boot stalling issue in older versions.
