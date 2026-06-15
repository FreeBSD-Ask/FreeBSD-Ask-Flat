# 7.4 Bluetooth

Bluetooth is a wireless technology used to create personal networks within the 2.4 GHz unlicensed band, typically with a coverage range of 10 meters. Such networks are typically formed on demand by portable devices (such as mobile phones, handheld devices, and laptops). Unlike Wi-Fi, Bluetooth provides higher-level service protocols, such as FTP-like file servers, file push, voice transmission, serial line emulation, and more.

This section covers the use of USB Bluetooth adapters on FreeBSD systems, as well as various Bluetooth protocols and utilities.

## Loading Bluetooth Support

The FreeBSD Bluetooth protocol stack is implemented based on the Netgraph framework. USB Bluetooth adapters require loading the `ng_ubt` kernel module. To automatically load it at boot, add the following configuration to **/boot/loader.conf**:

```ini
ng_ubt_load="YES"
```

The implementation of Bluetooth functionality relies on the coordination of multiple system services. Add the following configuration to **/etc/rc.conf** (or use the `service` command to enable):

```ini
hcsecd_enable="YES"
sdpd_enable="YES"
bthidd_enable="YES"
```

You can also enable and start the related services through the following commands:

```sh
# service hcsecd enable  # Enable hcsecd service
# service sdpd enable    # Enable sdpd service
# service bthidd enable  # Enable bthidd service
# service hcsecd start   # Start hcsecd service immediately
# service sdpd start     # Start sdpd service immediately
# service bthidd start   # Start bthidd service immediately
```

Related component descriptions:

| Component | Description |
| --------- | ----------- |
| `ng_ubt` | Netgraph USB Bluetooth driver, providing transport layer support for USB Bluetooth adapters, the foundational driver of the Bluetooth protocol stack |
| `hcsecd` | Manages link keys and PIN codes for Bluetooth devices, responsible for Bluetooth device security authentication |
| `sdpd` | Bluetooth Service Discovery Protocol daemon, responsible for discovering and advertising Bluetooth services |
| `bthidd` | Supports Bluetooth HID (Human Interface Device) devices, such as Bluetooth mice, keyboards, etc. |
| `hccontrol` | Through hccontrol(8), you can control the Bluetooth HCI low-level interface, including querying device status, scanning nearby devices, managing connections, and other operations; it is the core tool for Bluetooth debugging |

## Bluetooth Mouse Pairing

Taking the Logitech M337 mouse as an example, you need to press and hold the pairing button on the bottom of the mouse until the indicator light flashes rapidly, putting it into discoverable pairing mode.

Run the `bluetooth-config scan` command with root privileges to scan for available Bluetooth devices nearby, and follow the prompts to complete device pairing and addition:

```sh
# bluetooth-config scan
Scanning for new Bluetooth devices (Attempt 1 of 5) ... done.
Found 1 new bluetooth device (now scanning for names):
[ 1] 34:88:5d:12:34:56  "Bluetooth Mouse M336/M337/M535" (Logitech-M337)
Select device to pair with [1, or 0 to rescan]: 1

This device provides human interface device services.
Set it up? [yes]:
```

## Appendix: Intel Bluetooth

Intel wireless network cards driven by iwm, iwx, or iwlwifi typically integrate Bluetooth functionality, which can be enabled by installing the **comms/iwmbt-firmware** firmware package to load the microcode required by the Bluetooth hardware.

- Install using pkg (binary package manager):

```sh
# pkg install iwmbt-firmware
```

- Install using Ports:

```sh
# cd /usr/ports/comms/iwmbt-firmware/
# make install clean
```

Bluetooth devices are connected via the USB bus, and you can use the `usbconfig` tool to view all USB devices (including Bluetooth devices). If the firmware is not automatically loaded at system startup, it can be loaded manually. For example, if the Bluetooth device is identified as `ugen1.5`, you can execute:

```sh
# iwmbtfw -d ugen1.5 -f /usr/local/share/iwmbt-firmware/
```

> **Note**
>
> The FreeBSD Bluetooth protocol stack initialization script (**/etc/rc.d/bluetooth**) may cause device locking during the system service startup phase (when the Bluetooth HID daemon attempts to open the device). Therefore, you must run `iwmbtfw` to load the firmware before starting the Bluetooth protocol stack; otherwise, a full power-off reboot or suspend-resume cycle is required to unlock the device.

## Troubleshooting and Outstanding Issues

### Automatic Disconnection After Pairing

This issue may be caused by inconsistency between the device information cached by the bthidd service and the actual device state. Solution: delete the `bd_addr` line corresponding to the mouse in the **/var/db/bthidd.hids** file (this line contains the device's Bluetooth address in the format `xx:xx:xx:xx:xx:xx`, such as `34:88:5d:12:34:56`), clearing the old pairing information.

After completing the above operation, restart the Bluetooth HID daemon service for the changes to take effect:

```sh
# service bthidd restart
```
