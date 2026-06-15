# 7.1 Computer Network Basics

FreeBSD is an ideal operating system for internet or intranet servers, capable of providing robust network services under extremely high loads, efficiently using memory, and maintaining good response times with thousands of concurrent user processes.

FreeBSD network configuration involves several core commands and configuration files:

| Command/File | Purpose |
| ------------ | ------- |
| `ifconfig` | Configure network interface parameters |
| `route` | Manually manipulate network routing tables |
| **/etc/rc.conf** | The core file for system startup configuration; persistent configurations for network interfaces are stored in this file |
| **/etc/resolv.conf** | Configure DNS resolver information |
| **/etc/hosts** | Provide static mapping of local hostnames to IP addresses |

## Network Model Basics

A computer network is a system that connects multiple computers and their peripheral devices, which are located in different geographical positions and have independent functions, through communication lines. Under the management and coordination of network operating systems, network management software, and network communication protocols, this network enables resource sharing and information transmission.

The FreeBSD network subsystem is implemented based on the TCP/IP protocol suite, following the layered architecture of the Internet protocol suite.

The TCP/IP protocol suite uses a four-layer model to organize network functions:

| Layer | Name | Description |
| ----- | ---- | ----------- |
| Layer 1 | Link Layer | Responsible for sending and receiving data frames on physical network media, handling hardware address (MAC address) resolution. This layer corresponds to network interface drivers in FreeBSD, whose configuration is managed by the `ifconfig` command. Ethernet frame transmission and reception, and address resolution (ARP, Address Resolution Protocol) are all completed at this layer |
| Layer 2 | Internet Layer | Responsible for routing and forwarding of data packets. The core protocol is IP (Internet Protocol), which also handles logical addressing (IP addresses), fragmentation and reassembly, and route selection. Routing table management and `route` command operations all belong to this layer |
| Layer 3 | Transport Layer | Provides end-to-end communication services. The main protocols are TCP (Transmission Control Protocol) and UDP (User Datagram Protocol). TCP provides reliable connection-oriented transmission, while UDP provides connectionless unreliable transmission. FreeBSD implements a multi-TCP stack coexistence architecture, allowing the system to load multiple TCP protocol stack implementations simultaneously |
| Layer 4 | Application Layer | Contains various user-oriented network application protocols, such as HTTP, SSH, DNS, SMTP, etc. FreeBSD provides a large number of network service software through Ports and pkg |

### References

- Kurose J F, Ross K W. Computer Networking: A Top-Down Approach (8th Edition)[M]. Translated by Chen Ming. Beijing: China Machine Press, 2022. ISBN: 978-7-111-71236-7.

## Identifying Network Adapters

FreeBSD supports a variety of wired and wireless network adapters. Check the hardware compatibility list for the FreeBSD version in use to confirm whether the network adapter is supported.

### Identifying Network Adapters via the pciconf Command

To obtain the network adapters used by the system, execute the following command:

```sh
% pciconf -lv | grep -A1 -B3 network
```

Example output is as follows:

```sh
em0@pci0:2:1:0:	class=0x020000 rev=0x01 hdr=0x00 vendor=0x8086 device=0x100f subvendor=0x15ad subdevice=0x0750
    vendor     = 'Intel Corporation'
    device     = '82545EM Gigabit Ethernet Controller (Copper)'
    class      = network
    subclass   = ethernet

iwm0@pci0:3:0:0: class=0x028000 rev=0x00 hdr=0x00 vendor=0x8086 device=0x4237 subvendor=0x8086 subdevice=0x1211
    vendor     = 'Intel Corporation'
    device     = 'PRO/Wireless 5100 AGN [Shiloh] Network Connection'
    class      = network
```

The text before the `@` symbol is the name of the driver that controls the device. In this example, they are em(4) and iwm(4), respectively.

### Identifying Network Adapters via the ifconfig Command

Use the `ifconfig` command to view the list of network interfaces and their status on the system. The output should be similar to the following:

```sh
em0: flags=1008843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST,LOWER_UP> metric 0 mtu 1500
	options=4e504bb<RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,JUMBO_MTU,VLAN_HWCSUM,LRO,VLAN_HWFILTER,VLAN_HWTSO,RXCSUM_IPV6,TXCSUM_IPV6,HWSTATS,MEXTPG>
	ether 00:0c:29:84:0f:86
	inet 192.168.5.22 netmask 0xffffff00 broadcast 192.168.5.255
	inet6 fe80::20c:29ff:fe84:f86%em0 prefixlen 64 scopeid 0x1
	inet6 240e:341:207:a600:2d60:b653:3a68:8605 prefixlen 64 autoconf pltime 101808 vltime 188208
	media: Ethernet autoselect (1000baseT <full-duplex>)
	status: active
	nd6 options=823<PERFORMNUD,ACCEPT_RTADV,AUTO_LINKLOCAL,STABLEADDR>
lo0: flags=1008049<UP,LOOPBACK,RUNNING,MULTICAST,LOWER_UP> metric 0 mtu 16384
	options=680003<RXCSUM,TXCSUM,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>
	inet 127.0.0.1 netmask 0xff000000
	inet6 ::1 prefixlen 128
	inet6 fe80::1%lo0 prefixlen 64 scopeid 0x2
	groups: lo
	nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
wlan0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
	options=200001<RXCSUM,RXCSUM_IPV6>
	ether 20:0d:b0:c4:ab:59
	inet 192.168.5.23 netmask 0xffffff00 broadcast 192.168.5.255
	groups: wlan
	ssid test_5G channel 153 (5765 MHz 11a vht/80-) bssid e4:60:4d:97:00:e8
	regdomain FCC country US authmode WPA2/802.11i privacy ON
	deftxkey UNDEF AES-CCM 2:128-bit AES-CCM ucast:128-bit txpower 17
	bmiss 7 mcastrate 6 mgmtrate 6 scanvalid 60 ampdulimit 64k
	ampdudensity 2 shortgi -stbc -uapsd vht vht40 vht80 -vht160 -vht80p80
	wme roaming MANUAL
	parent interface: rtwn0
	media: IEEE 802.11 Wireless Ethernet VHT mode 11ac
	status: associated
	nd6 options=829<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL,STABLEADDR>
```

FreeBSD names network interfaces using the driver name followed by a unit number. The unit number indicates the order in which the adapter was detected at boot time, or the order in which it was subsequently detected. For example, `em0` is the first network card in the system using the em(4) driver, and `wlan0` is the first wireless interface created using the rtwn(4) driver.

This example shows the following devices:

- `em0`: Ethernet interface.
- `lo0`: Local loopback interface, used for internal communication within the local machine, not a physical network card. It can be used for performance analysis, software testing, and local communication.
- `wlan0`: Generic WiFi 802.11 link layer interface, used for connecting to wireless networks.

  > **Tip**
  >
  > If the `ifconfig` output only shows the `lo0` interface, it means the system has not recognized any physical network card. In this case, check the network card hardware connection and driver loading status. You can use the `dmesg | grep ether` command to view the network card driver loading log.

This example shows that `em0` and `wlan0` are up and running normally.

Key indicator items:

| Indicator | Description |
| --------- | ----------- |
| **UP** | Indicates that the interface has been enabled by the administrator (in the up state). Currently, both `em0` and `wlan0` are enabled |
| **inet** (IPv4 address) | The address of the wired interface `em0` is updated to **192.168.5.22**; the address of the wireless interface `wlan0` is **192.168.5.23** |
| **inet6** (IPv6 address) | `em0` currently has two IPv6 addresses: the link-local address `fe80::20c:29ff:fe84:f86%em0` and the global dynamic address `240e:341:207:a600:2d60:b653:3a68:8605` |
| **netmask** (subnet mask) | The masks for both `em0` and `wlan0` are `0xffffff00`, which is equivalent to the standard **255.255.255.0** |
| **broadcast** (broadcast address) | The valid broadcast addresses for the network segments where both interfaces are currently located are **192.168.5.255** |
| **ether** (MAC address) | The physical address of the wired interface `em0` is `00:0c:29:84:0f:86`; the physical address of the wireless interface `wlan0` is `20:0d:b0:c4:ab:59` |
| **media** (physical media) | `em0` shows Ethernet autoselect mode (`Ethernet autoselect (1000baseT <full-duplex>)`); `wlan0` shows wireless high-speed mode (`IEEE 802.11 Wireless Ethernet VHT mode 11ac`) |
| **status** (link status) | The status of `em0` is `active`, indicating that the network cable is connected and the carrier signal is normal; the status of `wlan0` is `associated`, indicating that it has successfully associated with the wireless access point (SSID externally displayed as `test_5G`) |

In addition, the key indicator items for `wlan0` are as follows:

| Indicator | Description |
| --------- | ----------- |
| **ssid** (wireless network name) | The name of the currently connected Wi-Fi is `test_5G` |
| **channel** (operating channel and frequency) | Currently operating on channel 153, with a frequency of 5765 MHz (in the 5GHz band), and 80MHz bandwidth enabled (vht/80-) |
| **bssid** (wireless router MAC address) | The physical address of the currently connected Wi-Fi hotspot is `e4:60:4d:97:00:e8` |
| **country / regdomain** (country region code and wireless regulatory domain) | Currently complying with US standards (country US) and the radio regulations of the US Federal Communications Commission (regdomain FCC) |
| **authmode / privacy** (authentication and encryption mode) | The authentication method is WPA2/802.11i (currently the mainstream secure wireless authentication standard), and privacy encryption is enabled (privacy ON). Unicast and transmission keys use AES-CCM (128-bit) |
| **txpower** (transmit power) | The current wireless transmit power is 17 dBm |

If the ifconfig(8) output is similar to the following, it indicates that the network interface is pending configuration:

```sh
em0: flags=1008843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST,LOWER_UP> metric 0 mtu 1500
        options=4e504bb<RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,JUMBO_MTU,VLAN_HWCSUM,L
RO,VLAN_HWFILTER,VLAN_HWTSO,RXCSUM_IPV6,TXCSUM_IPV6,HWSTATS,MEXTPG>
        ether 00:0c:29:84:0f:86
        inet6 fe80::20c:29ff:fe84:f86%em0 prefixlen 64 scopeid 0x1
        inet6 240e:341:207:a600:2d60:b653:3a68:8605 prefixlen 64 autoconf pltime 1018
08 vltime 188208
        media: Ethernet autoselect (1000baseT <full-duplex>)
        status: active
        nd6 options=823<PERFORMNUD,ACCEPT_RTADV,AUTO_LINKLOCAL,STABLEADDR>
lo0: flags=1008049<UP,LOOPBACK,RUNNING,MULTICAST,LOWER_UP> metric 0 mtu 16384
        options=680003<RXCSUM,TXCSUM,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>
        inet 127.0.0.1 netmask 0xff000000
        inet6 ::1 prefixlen 128
        inet6 fe80::1%lo0 prefixlen 64 scopeid 0x2
        groups: lo
        nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
```

## Gateways and Routing

**Routing** is the mechanism by which a system finds network paths to other systems. Each route is defined by a pair of addresses, representing the "destination" and the "gateway," respectively. A route indicates that when connecting to a specified destination, data packets should be sent through the specified gateway. Destinations are divided into three types: a single host, a subnet, and "default." The "default route" takes effect when no other route is applicable. Gateways are also divided into three types: a single host, an interface (also called a link), and an Ethernet hardware (MAC) address. Known routes are stored in the routing table.

### Routing Basics

Use netstat(1) to view the routing table of a FreeBSD system. Adding the `-n` option avoids reverse DNS resolution delays, which is particularly important when troubleshooting network issues:

```sh
$ netstat -rn
Routing tables

Internet:	# IPv4
# Path				Gateway		  Flags			Interface  Expire
Destination        Gateway            Flags         Netif Expire
default            192.168.179.2      UGS             em0   # Default route, via em0 interface
localhost          link#3             UH              lo0   # Loopback address, using lo0 interface
192.168.5.0/24     link#2             U               em1   # 192.168.5.0/24
192.168.5.16       link#3             UHS             lo0   # 192.168.5.16
192.168.179.0/24   link#1             U               em0   # 192.168.179.0/24 subnet
192.168.179.128    link#3             UHS             lo0   # Local host address 192.168.179.128

Internet6:	# IPv6
Destination        Gateway            Flags         Netif Expire
::/96              link#3             URS             lo0   # IPv4-compatible IPv6 address block (deprecated, RFC 4291)
default            fe80::5%em1        UG              em1   # Default IPv6 route, next hop is link-local address fe80::5, flag UG indicates reachable and is a gateway
localhost          link#3             UHS             lo0   # IPv6 loopback address ::1
::ffff:0.0.0.0/96  link#3             URS             lo0   # IPv4-mapped IPv6 addresses
240e:341:22b:ae00: link#2             U               em1   # Global unicast addresses
240e:341:22b:ae00: link#3             UHS             lo0   # Local host route
fe80::%lo0/10      link#3             URS             lo0   # Link-local IPv6 network
fe80::%em0/64      link#1             U               em0   # em0 interface link-local IPv6 address
fe80::20c:29ff:fe8 link#3             UHS             lo0   # Static host route
fe80::%em1/64      link#2             U               em1   # em1 interface link-local IPv6 address
fe80::20c:29ff:fe8 link#3             UHS             lo0   # Static host route (duplicate of the previous fe80::20c:29ff:fe8 entry, a normal phenomenon in routing table caching)
fe80::%lo0/64      link#3             U               lo0   # lo0 interface link-local subnet
fe80::1%lo0        link#3             UHS             lo0   # lo0 local host link-local address, static host route
ff02::/16          link#3             URS             lo0   # IPv6 multicast addresses
```

The entries in this example are as follows:

- **default**:
  The default route. When the local system needs to connect to a remote host, it checks the routing table to determine whether a known path exists. If the remote host matches an entry in the table, the system checks whether the connection can be completed through the interface specified by that entry.
  The default route is used when there is no more specific prefix match, and is not affected by the status of other paths. For a host in a local area network, the `Gateway` field of the default route should be set to a system that can directly connect to the internet. When reading this entry, confirm that the `Flags` column indicates the gateway is reachable (`UG`).

  For a host acting as an external gateway, its default route points to the gateway connected to the Internet Service Provider (ISP).

  In the IPv4 network, the default gateway is **192.168.179.2**, marked as UGS (Up, Gateway, Static). All IPv4 traffic destined for unknown external networks (such as the internet) is sent through the `em0` interface to this router for forwarding.

  In the IPv6 section, the default route specifies the next hop as the link-local address **fe80::5%em1**, which is typically a router provided by the ISP. The flag is UG, and all unmatched IPv6 traffic is sent out through the em1 interface. Link-local addresses are an IPv6 feature, used only for local link communication, and are not equivalent to publicly reachable addresses.

  This machine has dual-stack network capability, with IPv4 and IPv6 egress depending on different network cards (em0 and em1), respectively.

- **localhost**: The second route. The interface specified for `localhost` in the `Netif` column is **lo0**, also known as the loopback device. All traffic sent to this destination remains within the local machine and is not sent over the network.

- **Subnets**

  The IPv4 routing table shows that this device is connected to two local area networks simultaneously: **192.168.5.0/24** and **192.168.179.0/24**. The corresponding gateways are `link#2` and `link#1`, respectively, with flags containing only U (Up), indicating that both are "directly connected networks." When this machine communicates with other devices within these two network segments (such as computers in the `192.168.5.x` range), data packets do not need to pass through a router; instead, after resolving the target MAC address via ARP, they are sent directly as link-layer unicast frames through the em1 and em0 interfaces.

- **Global Unicast Addresses**

  In the IPv6 routing table (Internet6), entries starting with **240e:341:22b:ae00:** are bound to the em1 interface. This address belongs to a Global Unicast Address, typically assigned by an ISP (such as China Telecom, starting with 240e), and is theoretically routable on the internet, but actual accessibility depends on firewall policies and service configurations.

- **Flags**

  The `Flags` column displays the attributes of each route. The following table summarizes common routing table flags and their meanings.

  **Common Routing Table Flags**

  | Flag | Purpose |
  | ---- | ------- |
  | U | Route is up |
  | H | Route target is a single host |
  | G | Forward any traffic for this destination to this gateway, and the gateway decides how to further forward it |
  | S | This route is statically configured |
  | M | Route has been modified by a redirect |
  | B | Blackhole route: silently discard matching packets |
  | D | Dynamically created by a redirect |
  | L | Link-layer route, involving Ethernet hardware address reference |
  | R | Reject route: destination host or network is unreachable |
  | b | Route represents a broadcast address |

### Tracing Route Information

After address space is allocated to a network, the service provider configures its own routing table to ensure that all traffic is sent to the link for that site. How do external sites determine that data packets should be sent to the ISP for that network?

The internet tracks all allocated address space through a global routing system and defines its connection nodes to the internet backbone (the core lines that carry internet traffic). Each backbone router maintains a master routing table that directs traffic for specific networks to the corresponding backbone carrier, and then passes it through a series of service providers tier by tier to the destination network.

Service providers must announce their connection points to backbone sites so that traffic can reach that network. This process is called route propagation.

Route propagation occasionally experiences failures, causing some sites to become unreachable. In such cases, the commonly used command to find the routing break point is `traceroute` (use `traceroute6` for IPv6), which is particularly useful when `ping` fails.

Using `traceroute` requires providing the address of the remote host. The output will display the gateway hosts along the path, ultimately reaching the target host or terminating due to a connection interruption.

The following is an IPv6 route trace to the `freebsd.org` domain:

```sh
$ traceroute6 freebsd.org
traceroute6 to freebsd.org (2610:1c1:1:606c::50:15) from 240e:341:22b:ae00:f534:e5cb:2367:c033, 64 hops max, 28 byte packets
 1  * * *
 2  240e:c::201 (240e:c::201)  6.427 ms  5.508 ms  6.578 ms
 3  *
    240e:c:1:20e::2 (240e:c:1:20e::2)  6.229 ms  5.737 ms
 4  * * *
 5  240e::1:11:46:5c02 (240e::1:11:46:5c02)  10.729 ms * *
 6  240e::f:1:6601:503 (240e::f:1:6601:503)  13.093 ms *  11.332 ms
 7  240e:0:a::c9:360d (240e:0:a::c9:360d)  15.618 ms
    240e:0:a::c9:3649 (240e:0:a::c9:3649)  16.334 ms *
 8  * * *
 9  *
    zayo.ae10.mpr4.sjc7.us.zip.zayo.com (2001:438:ffff::407e:2f9)  180.002 ms  179.112 ms
10  ae34.mpr1.ewr4.us.zip.zayo.com (2001:438:ffff::407d:1455)  244.026 ms  243.263 ms *
11  2001:438:fffe::24ba (2001:438:fffe::24ba)  246.736 ms *  245.756 ms
12  cs89-cs80.nyinternet.net (2610:1c1::2502)  218.700 ms  218.396 ms  217.715 ms
13  2610:1c1::803 (2610:1c1::803)  228.099 ms  227.280 ms  226.003 ms
14  wfe0.nyi.freebsd.org (2610:1c1:1:606c::50:15)  214.724 ms  216.682 ms *
```

Detailed explanation:

| No. | Node | Description |
| --- | ---- | ----------- |
| 1 | `*` | No response from the first hop of the local network |
| 2 | **240e:c::201 (240e:c::201)** | ISP access gateway |
| 3, 4 | `*` | No response from backbone pre-level nodes, possibly dropping ICMPv6 |
| 5 | **240e::1:11:46:5c02 (240e::1:11:46:5c02)** | China Telecom core backbone router, domestic long-haul aggregation node |
| 6 | **240e::f:1:6601:503 (240e::f:1:6601:503)** | China Telecom backbone node before the international exit, some probe packets lost |
| 7 | **240e:0:a::c9:360d (240e:0:a::c9:360d), 240e:0:a::c9:3649 (240e:0:a::c9:3649)** | China Telecom intercontinental backbone routers for overseas exit |
| 8 | `*` | No response node, possibly dropping ICMPv6 |
| 9 | **zayo.ae10.mpr4.sjc7.us.zip.zayo.com (2001:438:ffff::407e:2f9)** | Zayo San Jose, California, USA backbone network (Zip Zayo) entry node |
| 10 | **ae34.mpr1.ewr4.us.zip.zayo.com (2001:438:ffff::407d:1455)** | Zayo Newark, USA backbone network node, responsible for East Coast traffic aggregation |
| 11 | **2001:438:fffe::24ba (2001:438:fffe::24ba)** | Zayo US backbone last hop, approaching NYInternet entry |
| 12 | **cs89-cs80.nyinternet.net (2610:1c1::2502)** | NYInternet US carrier core router, connecting to the freebsd.org network |
| 13 | **2610:1c1::803 (2610:1c1::803)** | NYInternet core backbone node, responsible for traffic distribution to freebsd.org servers |
| 14 | **wfe0.nyi.freebsd.org (2610:1c1:1:606c::50:15)** | freebsd.org server terminal node, target of traceroute6 |
