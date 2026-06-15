# 7.3 Wireless Network Management

FreeBSD supports a variety of wireless network cards and authentication methods.

> **Tip**
>
> Wi-Fi is not an abbreviation for any word. The term is merely a registered trademark held by the [Wi-Fi Alliance](https://www.wi-fi.org/), and has no derived meaning such as "Wireless Fidelity." (Doctorow C. WiFi isn't short for "Wireless Fidelity"[EB/OL]. (2005-11-08)[2026-04-21]. <https://boingboing.net/2005/11/08/wifi-isnt-short-for.html>.)

## Quick Connection (Based on COMFAST CF-912AC 1200M 802.11AC)

### Wireless Network Configuration

A basic wireless network consists of multiple stations, each interconnected through radio communication in the 2.4 GHz and 5 GHz bands (6 GHz band support on FreeBSD is still under development). Configuring a wireless network involves three steps:

1. Scan and select an access point
2. Authenticate the station
3. Configure an IP address or use DHCP

### Example Network Card

This section uses the COMFAST CF-912AC 1200M 802.11AC wireless network card as an example to introduce the general driver configuration method for wireless network cards. This network card uses a Realtek chipset. Other network cards using Realtek chipsets have similar configuration methods.

### Identifying the Wireless Network Card

Before configuring the wireless network, you need to confirm whether the system has recognized the wireless network card. You can check the kernel wireless device list to confirm hardware recognition status:

```sh
# sysctl net.wlan.devices
net.wlan.devices: rtwn0
```

The `rtwn0` in the above output is the device name of the example network card (COMFAST CF-912AC); actual output will vary depending on the hardware. If the colon `:` in the output is followed by nothing, it means the wireless network card has not been recognized, and you need to check the hardware connection or replace it with a more compatible wireless network card.

### Virtual Wireless Interface Mechanism

In FreeBSD, the wireless network uses a layered architecture. A virtual wireless interface `wlan0` must be created and bound to the physical wireless network card before it can be used.

Create a new wireless interface `wlan0` and bind it to the physical device `rtwn0`:

```sh
# ifconfig wlan0 create wlandev rtwn0
```

In the above command, `rtwn0` is the physical network card name from the `sysctl net.wlan.devices` output, and should be replaced according to the actual hardware (~~unless you are also using a COMFAST CF-912AC 1200M 802.11AC~~).

> **Tip**
>
> The `wlan0`, `rtwn0`, **192.168.1.100**, `freebsdap`, and `freebsdcn` in the above example are placeholders and must be replaced with actual values.

After creation, you can use the `ifconfig` command to view the interface status (the following output omits the Ethernet card and `lo0` interface):

```sh
# ifconfig

...part of the output omitted...

wlan0: flags=8802<BROADCAST,SIMPLEX,MULTICAST> metric 0 mtu 1500
	options=200001<RXCSUM,RXCSUM_IPV6>
	ether 20:0d:b0:c4:ab:59
	groups: wlan
	ssid "" channel 1 (2412 MHz 11b)
	regdomain FCC country US authmode OPEN privacy OFF txpower 30 bmiss 7
	scanvalid 60 wme bintval 0
	parent interface: rtwn0
	media: IEEE 802.11 Wireless Ethernet autoselect (autoselect)
	status: no carrier
	nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>
```

Under normal circumstances, the output should include the `wlan0` interface.

### Scanning Wireless Networks

Before connecting to a wireless network, you need to scan for available wireless networks nearby. You can use ifconfig(8) to scan for available wireless networks:

```sh
# ifconfig wlan0 up scan
SSID/MESH ID                      BSSID              CHAN RATE    S:N     INT CAPS

...part of the output omitted...

test_5G                           50:d6:c5:93:d7:64   36   54M  -78:-95   100 EP   APCHANREP WPA RSN WPS BSSLOAD HTCAP VHTCAP VHTOPMODE WME
```

Parameter descriptions:

| Parameter | Meaning |
| --------- | ------- |
| **SSID/MESH ID** | Identifies the network name |
| **BSSID** | Identifies the MAC address of the access point |
| **CHAN** | Channel |
| **RATE** | Rate |
| **S:N** | Signal strength and quality |
| **INT** | Beacon interval |
| **CAPS** | Identifies the type of each network and the capabilities supported by the station |

The scan results will display a list of available wireless networks.

### Definition and Role of SSID

The Service Set Identifier (SSID) is the name of a wireless network, used to distinguish different wireless networks.

Connect the `wlan0` interface to the wireless network with SSID `test_5G` (applicable to open networks without a password):

```sh
# ifconfig wlan0 ssid test_5G
```

In the above command, `test_5G` is an example Wi-Fi name (SSID) and should be replaced with the actual network name.

If you cannot scan for Wi-Fi networks, you may need to modify the wireless region settings or router channel. You can reconfigure by following these steps:

```sh
# ifconfig wlan0 destroy
# ifconfig wlan0 create wlandev rtwn0
# ifconfig wlan0 country HR regdomain ETSI
```

The first command destroys the existing `wlan0` interface and releases its occupied resources, avoiding the `ifconfig: SIOCS80211: Device busy` error; the second command recreates the wireless interface and binds it to the physical device; the third command sets the wireless country code to HR and uses the ETSI wireless frequency band specification. This setting needs to be executed when the target network uses channels greater than 48 (DFS channels). If the channel is less than 48, this step can be omitted.

After completing the above configuration, restart the network service to connect to Wi-Fi:

```sh
# service netif restart
# dhclient wlan0
```

The first command restarts the network interface service, and the second command obtains a dynamic IP address for the `wlan0` interface.

### Using WPA2 Authentication

Connecting to an encrypted wireless network requires using a Wi-Fi Protected Access (WPA) configuration file. WPA2 is the wireless network security protocol currently working properly on FreeBSD, providing data encryption and authentication functions. For WPA3/SAE, the FreeBSD base system's `wpa_supplicant` can support SAE negotiation configuration through the `CONFIG_SAE=y` compile option, but the kernel net80211 protocol stack has not yet completed SAE authentication support. Actual client connections will fail due to available key management types being empty (`available key_mgmt 0x0`) (as of late 2025, multiple users' actual tests on FreeBSD 14.3 and 15.0 have confirmed this situation). The FreeBSD Foundation has listed net80211 protocol stack updates as work in progress.

The authentication process in wireless networks is managed by wpa_supplicant(8). The wpa_passphrase(8) tool can be used to convert an SSID and plaintext password into a secure PSK configuration entry, avoiding writing plaintext passwords directly in the configuration file. Additionally, wpa_cli(8) provides an interactive command-line management interface for wpa_supplicant, which can be used for runtime debugging and status queries. Create the **/etc/wpa_supplicant.conf** configuration file with the following content:

```ini
ctrl_interface=/var/run/wpa_supplicant   # Optional, control interface path, used for communication between wpa_supplicant and tools like wpa_cli
fast_reauth=1                             # Optional, enable fast reauthentication, speeding up reconnection to authenticated networks
network={
ssid="test_5G"
psk="freebsdcn"
}
```

Configuration descriptions:

| Parameter | Description |
| --------- | ----------- |
| `ssid` | Specifies the SSID (Wi-Fi name) of the wireless network to connect to; the example here is `test_5G` |
| `psk` | Specifies the password of the wireless network; the example here is `freebsdcn` |

If you cannot obtain the Service Set Identifier (SSID) and Pre-Shared Key (PSK) of the wireless network, you can contact the network administrator or reset the network equipment to obtain the credentials.

The next step is to configure the wireless connection in the **/etc/rc.conf** file. Using dynamic addressing:

```sh
# sysrc ifconfig_wlan0="WPA DHCP"
```

Then restart the network:

```sh
# service netif restart
```

If the network connection is normal, it can be permanently configured. Add or modify the relevant configuration in the **/etc/rc.conf** file:

```ini
wlans_rtwn0="wlan0"                      # Bind physical wireless device rtwn0 to wlan0 interface
ifconfig_wlan0="WPA SYNCDHCP"           # Configure wlan0 to use WPA and automatically obtain an IP address via DHCP (SYNCDHCP is synchronous mode, which pauses startup until DHCP completes; for asynchronous/background mode, use DHCP instead)
create_args_wlan0="country HR regdomain ETSI"  # Set wireless country code to HR and use ETSI frequency band specification when creating the wlan0 interface. This setting is required if the channel is greater than 48 (DFS).
```

### Wireless Network Configuration File Structure

FreeBSD wireless networking involves the following configuration files.

```sh
/etc/
├── rc.conf              # System startup configuration file
└── wpa_supplicant.conf  # WPA wireless network configuration file
```

After completing the above configuration, restart the system or network service to make all configurations take effect.

After rebooting, use `ifconfig` to check the connection status. Under normal circumstances, you should see a successful connection (in the example output, the IP is **192.168.31.178**):

```sh
...part of the output omitted...

wlan0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
	options=200001<RXCSUM,RXCSUM_IPV6>
	ether 11:7c:e8:c4:ab:58
	inet 192.168.31.178 netmask 0xffffff00 broadcast 192.168.31.255
	groups: wlan
	ssid test_5G channel 36 (5180 MHz 11a ht/20) bssid 50:d6:c5:93:d7:64
	regdomain NONE country CN authmode WPA2/802.11i privacy ON
	deftxkey UNDEF TKIP 2:128-bit txpower 17 bmiss 7 mcastrate 6
	mgmtrate 6 scanvalid 60 ht20 ampdulimit 64k ampdudensity 4 shortgi
	-stbc -uapsd wme roaming MANUAL
	parent interface: rtwn0
	media: IEEE 802.11 Wireless Ethernet MCS mode 11na
	status: associated
	nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>
```

## Intel Wireless Network Card Driver Overview

Intel network cards are among the most widely used wireless network cards. The iwlwifi driver supports a superset of the chips supported by iwm: iwlwifi currently only ports the mvm sub-driver portion of the Linux upstream driver, covering all chips supported by iwm as well as newer Wi-Fi 6E/7 chips. The Linux upstream iwlwifi driver also includes MLD/MLO extension functionality (for Wi-Fi 7 MLO multi-link operation), but MLO and other Wi-Fi 7 new features are still under development on FreeBSD.

Add the following configuration to the **/etc/rc.conf** file:

```ini
wlans_iwlwifi0="wlan0"        # Bind physical wireless device iwlwifi0 to wlan0 interface
ifconfig_wlan0="WPA SYNCDHCP"  # Configure wlan0 to use WPA and automatically obtain an IP address via DHCP
```

Create the **/etc/wpa_supplicant.conf** configuration file:

```sh
network={
ssid="WIFI name (SSID)"
psk="WIFI password"
}
```

After completing the configuration, execute the following commands to start Wi-Fi testing:

```sh
# ifconfig wlan0 create wlandev iwlwifi0
# service netif start wlan0
```

The first command creates the `wlan0` interface and binds it to the physical wireless device `iwlwifi0`, and the second command starts the `wlan0` interface.

For troubleshooting and outstanding issues, please refer to: [wiki/WiFi/Iwlwifi](https://wiki.freebsd.org/WiFi/Iwlwifi)

### References

- FreeBSD Project. iwm(4) -- Intel IEEE 802.11ac wireless network driver[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=iwm&sektion=4>. Intel wireless network card driver technical documentation.
- FreeBSD Project. iwlwifi(4) -- Intel IEEE 802.11a/b/g/n/ac/ax/be wireless network driver[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=iwlwifi&sektion=4>. Intel wireless network card driver technical documentation.

## Broadcom Network Card Drivers

Broadcom is another common wireless network card manufacturer. FreeBSD's built-in Broadcom network card drivers mainly come in two types: `bwi` and `bwn`. `bwi` supports older models, while `bwn` supports newer models. The support ranges of the two partially overlap, but `bwn` has better hardware compatibility. These two drivers only support 802.11b/g; modern 802.11ac/ax Broadcom network cards do not have native driver support on FreeBSD.

For detailed information on driver selection, refer to Fuller L. Broadcom WiFi Improvements for FreeBSD[EB/OL]. (2018-01-22)[2026-04-05]. <https://web.archive.org/web/20240203102135/https://www.landonf.org/code/freebsd/Broadcom_WiFi_Improvements.20180122.html>.

### Example: BCM4301, BCM4303, BCM4306 rev 2

According to the references, the above network card models can only use the `bwi` driver.

First, add the following configuration to the **/boot/loader.conf** file to set the system to load the `bwi` driver at boot:

```sh
if_bwi_load="YES"
```

Then install the Broadcom wireless device firmware using Ports (this firmware does not provide a binary package):

```sh
# cd /usr/ports/net/bwi-firmware-kmod/
# make install clean
```

You can first obtain a network connection via USB or Ethernet sharing before installing, or download the required dependencies to a specified directory in advance.

Add the following configuration to the **/etc/rc.conf** file to bind the physical wireless device `bwi0` to the `wlan0` interface:

```ini
wlans_bwi0="wlan0"
```

After completing the above configuration, restart the system.

### Example: Configuring the bwn Driver

Install the Broadcom wireless device firmware:

```sh
# cd /usr/ports/net/bwn-firmware-kmod/
# make install clean
```

### Broadcom Network Card Driver Related File Structure

Broadcom network card drivers involve the following files:

```sh
/
├── boot/
│   └── loader.conf        # System boot loading configuration file
├── usr/
│   ├── ports/
│   │   └── net/
│   │       ├── bwi-firmware-kmod/   # Broadcom bwi firmware Port
│   │       └── bwn-firmware-kmod/   # Broadcom bwn firmware Port
│   └── src/
│       └── sys/
│           └── amd64/
│               └── conf/              # Kernel configuration file directory
```

Edit the **/boot/loader.conf** file and add the following configuration to set the system to load the `bwn` driver at boot:

```ini
if_bwn_load="YES"
```

Add the following configuration to the **/etc/rc.conf** file to bind the physical wireless device `bwn0` to the `wlan0` interface:

```ini
wlans_bwn0="wlan0"
```

## Wireless Network Troubleshooting

If no access points are listed during scanning, try switching the router's channel or downgrading the protocol to Wi-Fi 4.

If the device cannot associate with the access point, verify that the configuration matches the settings on the access point, including the authentication scheme and security protocol. It is recommended to simplify the configuration as much as possible. If using security protocols such as WPA2 or WPA, you can first configure the access point for open authentication with security disabled to verify whether traffic can pass through.

If the system can associate with the access point, use tools such as ping(8) to diagnose the network configuration.

## Appendix: Wireless Network Unavailable After System Version Update

### Firmware and System Update Compatibility Issues

In FreeBSD, firmware is the low-level software required for hardware devices to function properly, providing the communication interface between hardware devices and the operating system kernel. The firmware interface may change with kernel version updates.

### Firmware Re-acquisition Method

If the wireless network is unavailable after a system version update, you need to re-acquire firmware compatible with the current kernel version. FreeBSD provides the `fwget` tool for automatically acquiring and installing the required firmware:

```sh
# fwget
```

If the current system does not have a network connection, you can temporarily obtain a network connection through USB network sharing or other methods before executing the above command; alternatively, you can manually download the required firmware packages from the [USTC mirror site](https://mirrors.ustc.edu.cn/freebsd-pkg/FreeBSD%3A14%3Aamd64/kmods_latest_3/All/) (please replace `14` in the URL with the current system major version number), and then install using the following command:

```sh
# pkg add /path/to/firmware.pkg
```

## Appendix: Special Models Require Kernel Compilation

Special models of Broadcom wireless network cards may require recompiling the kernel for full support.

Some models listed in FULLER L. FreeBSD Broadcom Wi-Fi Improvements[EB/OL]. [2026-03-26]. <https://web.archive.org/web/20240203102135/https://www.landonf.org/code/freebsd/Broadcom_WiFi_Improvements.20180122.html> have a `$` annotation: `The optional bwn(4) PHY driver is derived from b43 GPL code, and must be explicitly enabled.`, indicating that this PHY submodule depends on code based on the GNU General Public License (GPL) (located in `sys/gnu/dev/bwn/phy_n/`). The main body of the bwn(4) driver uses the BSD 2-Clause license, but the kernel and base system built by FreeBSD by default do not compile this GPL PHY submodule. The `BWN_GPL_PHY` option must be explicitly enabled in the kernel configuration and the kernel must be recompiled.

```sh
# cd /usr/src/  # This is the FreeBSD kernel source code installation directory
# cd sys/amd64/conf/  # Switch to the FreeBSD kernel configuration file directory, note to select the corresponding architecture
# cp GENERIC MYKERNEL  # Copy the default kernel configuration file GENERIC to a custom kernel MYKERNEL
# echo "options BWN_GPL_PHY" >> MYKERNEL  # Add the BWN_GPL_PHY option to the MYKERNEL kernel configuration file
# cd /usr/src  # This is the FreeBSD kernel source code installation directory
# make -j4 buildkernel KERNCONF=MYKERNEL  # Compile the kernel using the MYKERNEL configuration with 4 parallel tasks enabled
# make -j4 installkernel KERNCONF=MYKERNEL  # Install the kernel compiled with the MYKERNEL configuration, with 4 parallel tasks enabled
```

In the above commands, **/usr/src/** is the FreeBSD kernel source code directory. Note that you should select the corresponding configuration file directory based on the actual architecture.

Then add the following configuration to the **/boot/loader.conf** file:

```ini
hw.bwn.usedma="1"               # Enable DMA mode (enabled by default; set to 0 to use PIO mode)
if_bwn_load="YES"               # Load the bwn driver at boot
bwn_v4_ucode_load="YES"         # Load BWN V4 wireless firmware
bwn_v4_lp_ucode_load="YES"      # Load BWN V4 low-power mode wireless firmware
```

After completion, restart the system, use `ifconfig` to check whether the `wlan0` interface exists, and then configure it according to the methods described earlier.

### References

- FreeBSD Foundation. Broadcom Wi-Fi Modernization[EB/OL]. [2026-03-26]. <https://freebsdfoundation.org/project/broadcom-wi-fi-modernization/>. Overview of the Broadcom Wi-Fi driver modernization project funded by the FreeBSD Foundation.
- FreeBSD Project. Revision 326841[EB/OL]. [2026-03-26]. <https://svnweb.freebsd.org/base?view=revision&revision=326841>. Code commit record for incorporating the Broadcom wireless driver into the FreeBSD base system.
- FreeBSD Forums. Installing Broadcom BCM43236 WiFi on 11.3 missing firmware error[EB/OL]. [2026-03-26]. <https://forums.freebsd.org/threads/installing-broadcom-bcm43236-wifi-on-11-3-missing-firmware-error.76470/>. Forum discussion about Broadcom wireless network card firmware missing causing unusability.
- helloSystem. ISO[EB/OL]. [2026-03-26]. <https://github.com/helloSystem/ISO/issues/78>. Feedback on Wi-Fi support issues in the helloSystem project.

## Troubleshooting Overview

### Unable to Connect or Unable to Search for Specific Channels

The wireless network region code setting affects the list of available channels. You can try adjusting the wireless region code setting.

Add the following configuration to the **/etc/rc.conf** file:

```ini
create_args_wlan0="country CN regdomain NONE"
```

This configuration sets the wireless country code to CN when creating the `wlan0` interface, without imposing specific wireless frequency band restrictions.

After completing the configuration, restart the system.

### Disconnecting Wi-Fi

Disable the `wlan0` interface:

```sh
# ifconfig wlan0 down
```

### WPA Authentication

Configure the `wlan0` interface to use WPA in the **/etc/rc.conf** file, and set a static IP address and subnet mask:

```ini
ifconfig_wlan0="WPA inet 192.168.1.100 netmask 255.255.255.0"
```

### Setting a Static IP

Configure a static IPv4 address and subnet mask for the `wlan0` interface:

```sh
# ifconfig wlan0 inet 192.168.0.100 netmask 255.255.255.0
```

### Enabling a Wireless Hotspot

Before configuring a wireless hotspot, confirm whether the network card supports hostap mode. You can list the wireless features and capabilities supported by the `wlan0` interface using the following command:

```sh
# ifconfig wlan0 list caps
drivercaps=591c541<STA,FF,IBSS,HOSTAP,SHSLOT,SHPREAMBLE,MONITOR,WPA1,WPA2,WME>
cryptocaps=b<WEP,TKIP,AES_CCM>
htcaps=207002d<LDPC,SHORTGI20>
```

If the output includes `HOSTAP`, it indicates that the network card supports hostap functionality.

After confirming the network card supports it, destroy the existing `wlan0` interface and release its occupied resources:

```sh
# ifconfig wlan0 destroy
```

Recreate the `wlan0` interface and configure it in hotspot mode:

```sh
# ifconfig wlan0 create wlandev rtwn0 wlanmode hostap
# ifconfig wlan0 inet 192.168.0.1 netmask 255.255.255.0 ssid freebsdap mode 11g channel 1
```

The first command creates the `wlan0` interface, binds it to `rtwn0`, and sets it to Host AP mode; the second command configures the IP address, SSID, wireless mode, and channel for `wlan0`.

![Creating a wireless hotspot](../.gitbook/assets/wifi-hotspot.png)

## Appendix: Graphical Network Configuration Tool

FreeBSD provides a graphical network management tool with functionality similar to Linux's NetworkManager:

Install using pkg:

```sh
# pkg install net-mgmt/networkmgr
```

Or install using Ports:

```sh
# cd /usr/ports/net-mgmt/networkmgr/
# make install clean
```

## Exercises

1. Using two wireless network cards from different manufacturers (such as Realtek and Intel), create virtual wireless interfaces on FreeBSD and connect to the same AP respectively. Use `systat -if` to monitor the throughput and latency differences between the two, and analyze the impact of driver implementation differences on performance.
2. Configure FreeBSD as a wireless hotspot (Host AP mode), modify the default channel and transmit power parameters, use another device to connect and test signal strength and throughput changes, and analyze the channel selection strategy in Host AP mode.
3. Review the creation logic of the `wlan` interface in the FreeBSD source code, and analyze the design considerations of the system's "physical network card + virtual interface" layered architecture versus directly managing physical devices.
