# 7.2 Basic Network Management

This section covers the basics of network management and troubleshooting techniques.

## Configuring IPv4

### Configuring a Dynamic IPv4 Address

If there is a DHCP server on the network, DHCP can be used to dynamically obtain an IP address. FreeBSD uses dhclient(8) as the DHCP client, which can automatically obtain an IP address, subnet mask, and default router.

> **Note**
>
> dhclient(8) **does not support DHCPv6** (RFC 3315/RFC 8415). For dynamic IPv6 addresses, use rtsold(8) (SLAAC) or a third-party DHCPv6 client (such as `dhcp6c`).

To configure an interface to use DHCP, execute the following command:

```sh
# sysrc ifconfig_em0="DHCP"
```

dhclient(8) can also be run manually:

```sh
# dhclient em0
DHCPREQUEST on em0 to 255.255.255.255 port 67
DHCPACK from 192.168.1.1
bound to 192.168.1.19 -- renewal in 43200 seconds.
```

dhclient(8) can also be started in the background. Running in the background may affect programs that depend on the network, but in most cases it can speed up the boot process.

To run dhclient(8) in the background, execute the following command:

```sh
# sysrc background_dhclient="YES"
```

Then restart the network interface:

```sh
# service netif restart
```

### Configuring a Static IPv4 Address

Network interfaces can also be configured on the command line using ifconfig(8), but if these configurations are not also written to the **/etc/rc.conf** file, they will be lost after a reboot.

The IP address can be set using the following command:

```sh
# ifconfig em0 inet 192.168.1.150/24
```

To make the changes persistent across reboots, execute the following command:

```sh
# sysrc ifconfig_em0="inet 192.168.1.150 netmask 255.255.255.0"
```

Set the default route once:

```sh
# route add default 192.168.1.1
```

Permanently add a default router:

```sh
# sysrc defaultrouter="192.168.1.1"
```

Add DNS server addresses to the **/etc/resolv.conf** file:

```ini
nameserver 223.5.5.5   # Specify the primary DNS server as Alibaba Cloud Public DNS
nameserver 223.6.6.6   # Specify the secondary DNS server as Alibaba Cloud Public DNS
```

Then restart the network interface and routing:

```sh
# service netif restart
# service routing restart
```

ping(8) can be used to test connectivity:

```sh
$ ping -4c2 FreeBSD.org
PING FreeBSD.org (96.47.72.84): 56 data bytes
64 bytes from 96.47.72.84: icmp_seq=0 ttl=51 time=239.916 ms
64 bytes from 96.47.72.84: icmp_seq=1 ttl=51 time=236.443 ms

--- FreeBSD.org ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 236.443/238.179/239.916/1.737 ms
```

If ICMP response packets are received normally, the network is connected.

## Configuring IPv6

IPv6 is the successor protocol to IPv4. IPv6 offers several advantages and new features over IPv4:

- Its 128-bit address space can accommodate approximately 3.4×10^38 addresses, thereby solving the IPv4 address shortage and exhaustion problem.
- Routers only store network aggregation addresses in their routing tables. Address aggregation reduces the number of routing table entries, which significantly reduces router memory and CPU requirements compared to IPv4.
- Address autoconfiguration (RFC 4862).
- Replaces broadcast addresses with multicast.
- Supports IPsec (IP Security), but it is not mandatory (RFC 6434 has adjusted the requirement for IPv6 nodes to implement IPsec from MUST to SHOULD, making it recommended rather than required).
- Simplified header structure.
- Support for Mobile IP.
- IPv6-to-IPv4 transition mechanisms.

FreeBSD integrates the [KAME](https://www.kame.net/) project's IPv6 reference implementation and includes all components required for IPv6. The KAME project ended in 2006, and its code continues to be maintained and evolved by the FreeBSD project.

IPv6 addresses have three different types:

| Type | Description |
| ---- | ----------- |
| **Unicast** | Packets sent to a unicast address arrive at the interface belonging to that address |
| **Anycast** | Addresses are syntactically indistinguishable from unicast addresses, but point to a group of interfaces. Packets sent to an anycast address will arrive at the nearest interface |
| **Multicast** | An address identifies a group of interfaces. Packets sent to a multicast address will arrive at all interfaces belonging to the multicast group. IPv4 broadcast addresses (typically xxx.xxx.xxx.255) are represented by multicast addresses in IPv6 |

When reading IPv6 addresses, the canonical form is represented as `x:x:x:x:x:x:x:x`, where each x represents a 16-bit hexadecimal value. For example, **FEBC:A574:382B:23C1:AA49:4592:4EFE:9982**.

Consecutive all-zero fields often appear in IPv6 addresses. `::` (double colon) can be used to replace a contiguous segment of all-zero fields in an address. Additionally, up to three leading zeros in each hexadecimal value can be omitted. For example, **fe80::1** corresponds to the canonical form **fe80:0000:0000:0000:0000:0000:0000:0001**.

Some IPv6 addresses are reserved:

| IPv6 Address | Description | Notes |
| ------------ | ----------- | ----- |
| **::/128** | Unspecified address | Equivalent to **0.0.0.0** in IPv4 |
| **::1/128** | Loopback address | Equivalent to **127.0.0.1** in IPv4 |
| **::ffff:0.0.0.0/96** | IPv4-mapped IPv6 address | The lower 32 bits are the IPv4 address |
| **fe80::/10** | Link-local unicast | Equivalent to **169.254.0.0/16** in IPv4 |
| **fc00::/7** | Unique local | Routable only within a cooperating set of sites |
| **ff00::/8** | Multicast | — |
| **2000::/3** | Global unicast | All global unicast addresses are allocated from this pool |
| **2001:db8::/32** | Documentation use | IPv6 address prefix used in documentation |

### Configuring a Dynamic IPv6 Address

To dynamically configure an interface's IPv6 address using SLAAC:

```sh
# sysrc ifconfig_em0_ipv6="inet6 accept_rtadv"
# sysrc rtsold_enable="YES"
```

Note that when IPv6 packet forwarding is enabled (i.e., `ipv6_gateway_enable=YES`), the system will not configure SLAAC addresses unless the `net.inet6.ip6.rfc6204w3` sysctl(8) variable is set to 1.

### Configuring a Static IPv6 Address

To configure a FreeBSD system as an IPv6 client with a static IPv6 address, set the IPv6 address:

```sh
# sysrc ifconfig_em0_ipv6="inet6 2001:db8:4672:6565:2026:5043:2d42:5344 prefixlen 64"
```

To assign a default router:

```sh
# sysrc ipv6_defaultrouter="2001:db8:4672:6565::1"
```

ping(8) can be used to test connectivity:

```sh
$ ping -6c2 FreeBSD.org
PING(56=40+8+8 bytes) 240e:341:228:f400:cfbe:d56:7f75:26e0 --> 2610:1c1:1:606c::50:15
16 bytes from 2610:1c1:1:606c::50:15, icmp_seq=0 hlim=47 time=223.377 ms
16 bytes from 2610:1c1:1:606c::50:15, icmp_seq=1 hlim=47 time=223.348 ms

--- FreeBSD.org ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 223.348/223.362/223.377/0.014 ms
```

If ICMP response packets are received normally, the network is connected.

### References

- Li Q, Jinmei T, Shima K. IPv6 Core Protocols Implementation (Volume 1)[M]. Translated by Chen Juan, Zhao Zhenping. Beijing: People's Posts and Telecommunications Press, 2009: 846. ISBN: 978-7-115-18950-9 (English reprint edition ISBN: 978-7-115-19551-7). Detailed explanation of IPv6 core protocol implementation, based on FreeBSD KAME project code analysis.
- Li Q, Jinmei T, Shima K. IPv6 Advanced Protocols Implementation (Volume 2)[M]. Translated by Wang Jiazhen, et al. Beijing: People's Posts and Telecommunications Press, 2009: 869. ISBN: 978-7-115-20891-0 (English reprint edition ISBN: 978-7-115-19519-7). Detailed explanation of IPv6 advanced protocols and extension mechanisms, including key technologies such as Mobile IPv6.

## Network Interface Aliases

One common use of FreeBSD is virtual site hosting, where a single server appears as multiple servers on the network. This is accomplished by assigning multiple network addresses to a single interface.

A network interface can be configured with one primary IP address (Primary Address) and any number of additional IP addresses (Secondary Addresses), which have equal effect in underlying network communication.

> **Tip**
>
> The term "alias" address is merely a traditional naming convention adopted by early operating systems for management convenience; there is no distinction for physical network cards. In fact, IPv6 itself supports multiple addresses.

These aliases are typically implemented by adding alias entries in **/etc/rc.conf**, as shown below:

```sh
# sysrc ifconfig_em0_alias0="inet xxx.xxx.xxx.xxx netmask xxx.xxx.xxx.xxx"
```

Alias entries must start with `alias0` and use incrementing numbers, such as `alias0`, `alias1`, and so on. The configuration process will stop at the first missing number.

The calculation of alias subnet masks is very important. For a given interface, there must be one address that correctly represents the subnet mask of the network. Any other address belonging to that network must use a subnet mask of all `1`s, represented as **255.255.255.255** or **0xffffffff**.

For example, consider the following scenario: the `em0` interface is connected to two networks:

- **10.1.1.0**, with a subnet mask of **255.255.255.0**;
- **202.0.75.16**, with a subnet mask of **255.255.255.240**.

We need to configure the system with IP address ranges for two network segments: **10.1.1.1** to **10.1.1.5**, and **202.0.75.17** to **202.0.75.20**.

Only the first address in a given network range should have the actual subnet mask. The remaining addresses (**10.1.1.2** to **10.1.1.5** and **202.0.75.18** to **202.0.75.20**) must be configured with a subnet mask of **255.255.255.255**.

The following **/etc/rc.conf** entries configure the interface for this scenario:

```sh
# sysrc ifconfig_em0="inet 10.1.1.1 netmask 255.255.255.0"
# sysrc ifconfig_em0_alias0="inet 10.1.1.2 netmask 255.255.255.255"
# sysrc ifconfig_em0_alias1="inet 10.1.1.3 netmask 255.255.255.255"
# sysrc ifconfig_em0_alias2="inet 10.1.1.4 netmask 255.255.255.255"
# sysrc ifconfig_em0_alias3="inet 10.1.1.5 netmask 255.255.255.255"
# sysrc ifconfig_em0_alias4="inet 202.0.75.17 netmask 255.255.255.240"
# sysrc ifconfig_em0_alias5="inet 202.0.75.18 netmask 255.255.255.255"
# sysrc ifconfig_em0_alias6="inet 202.0.75.19 netmask 255.255.255.255"
# sysrc ifconfig_em0_alias7="inet 202.0.75.20 netmask 255.255.255.255"
```

A more concise way to express this is to use space-separated IP address ranges. The first address will use the specified subnet mask, while additional addresses will use the subnet mask **255.255.255.255**.

```sh
# sysrc ifconfig_em0_aliases="inet 10.1.1.1-5/24 inet 202.0.75.17-20/28"
```

## Hostname

The hostname represents the Fully Qualified Domain Name (FQDN) of the host on the network.

Check the current hostname:

```sh
$ hostname
ykla
```

Temporarily change the hostname:

```sh
root@ykla:/home/ykla # hostname f	# Temporarily change the hostname from ykla to f
root@f:/home/ykla #
```

Change the hostname and make it persistent across reboots:

```sh
# sysrc hostname="f"
```

## DNS

DNS can be thought of as a phone book, where IP addresses correspond to hostnames. Unless otherwise specified in the **/etc/nsswitch.conf** file, FreeBSD will first look up addresses in the **/etc/hosts** file, then look up DNS information in the **/etc/resolv.conf** file.

Related file structure:

```sh
/etc/
├── rc.conf             # System startup configuration file
├── resolv.conf          # DNS resolver server configuration file
└── resolvconf.conf      # resolvconf service configuration file
```

### Local Address hosts File

The **/etc/hosts** file is a simple text database that provides hostname-to-IP address mapping. Entries for local computers connected via LAN can be added to this file for simple hostname resolution without setting up a DNS server. Additionally, the **/etc/hosts** file can provide local records for internet domain names, reducing the need to query external DNS servers.

For example, if there is a local instance of www/gitlab-ce in the local environment, the following line can be added to the **/etc/hosts** file:

```ini
192.168.1.150 git.example.com git
```

### Configuring DNS Name Servers

The way FreeBSD systems access the Internet Domain Name System (DNS) is controlled by resolv.conf(5). The most common entries in the **/etc/resolv.conf** file are:

| Entry | Description |
| ----- | ----------- |
| `nameserver` | The IP address of the name server that the resolver should query. Servers are queried in the order listed, up to a maximum of three |
| `search` | The search list for hostname lookup. Usually determined by the domain of the local hostname |
| `domain` | The local domain name |

A typical **/etc/resolv.conf** file looks like this:

```ini
search example.com
nameserver 223.5.5.5
nameserver 223.6.6.6
```

The search and domain options are mutually exclusive; if both are specified, only the last one takes effect. When using DHCP, dhclient(8) typically rewrites the **/etc/resolv.conf** file with information received from the DHCP server.

Since the Dynamic Host Configuration Protocol (DHCP) client rewrites /etc/resolv.conf through the resolvconf service when obtaining network configuration, manually edited configurations may be overwritten upon system reboot.

If you need to use manually configured DNS servers and do not want the system to automatically update and overwrite them, you can disable the resolvconf service. Edit the **/etc/resolvconf.conf** file (create it if it does not exist) and add the line `resolvconf=NO`, which will disable the system's automatic updating of DNS configuration files.

### References

- FreeBSD Project. resolvconf[EB/OL]. [2026-03-26]. <https://man.freebsd.org/cgi/man.cgi?query=resolvconf&sektion=8>. Man page, providing complete technical documentation for the resolvconf tool, serving as an important reference for DNS configuration management.
- FreeBSD Forums. **8.8.8.8** or **1.1.1.1** if set in etc resolv conf doesn't stay as an entry in the file after a network restart[EB/OL]. [2026-03-26]. <https://forums.freebsd.org/threads/8-8-8-8-or-1-1-1-1-if-set-in-etc-resolv-conf-doesnt-stay-as-an-entry-in-the-file-after-a-network-restart.85951/>. Practical case analysis, providing solutions to prevent DNS configuration from being overwritten.

## Network Troubleshooting

First, check the following basic elements:

- Is the power connection normal?
- Is the router working properly?
- Have the broadband or network fees been paid?
- Is there a regional large-scale network outage?
- Is the computer's time correct?
- Is the network cable connected properly?
- Is the network service correctly configured?
- Is the firewall correctly configured?
- Is the network card supported by FreeBSD?

If the network card is working but performance is poor, refer to tuning(7). Incorrect network settings can cause slow connections; network configuration should also be checked.

The "No route to host" message indicates that the system cannot route packets to the destination host. This is usually because no default route has been specified or the network cable is not connected. Use the `route get <destination_address>` command to view the system's routing decision for a specific destination, then check the output of `netstat -rn` to ensure there is a valid route to the host.

The error message `ping: sendto: Permission denied` is usually caused by a firewall configuration error. If the IPFW firewall module is loaded directly without any rules configured, IPFW's default rule 65535 denies all traffic, even ping(8).

When enabling the firewall via rc.conf:

- The default value of `firewall_type` is `"UNKNOWN"`, in which case no allow rules are added, and only the default rule 65535 (deny all) takes effect
- If `firewall_type` is explicitly set to `"open"`, IPFW will add an allow rule at rule 65000, overriding the default deny behavior.

PF and IPFilter default to allowing traffic when no rules are present.

## Viewing Network Card Speed

To monitor network interface traffic statistics in real time, use the `systat` tool's network interface view. This command displays receive and transmit traffic data for each network interface at a specified refresh interval:

```sh
# systat -ifstat 2
```

The `-ifstat` parameter specifies displaying network interface information, and the number 2 indicates a refresh interval of 2 seconds.

## Viewing FreeBSD Download Traffic (bwm-ng)

To view more detailed network traffic statistics, install the `bwm-ng` tool, which provides multiple traffic display formats and interactive features:

```sh
# pkg install bwm-ng  # Install bwm-ng
# bwm-ng
  bwm-ng v0.6.3 (probing every 0.500s), press 'h' for help
  input: getifaddrs type: rate
  /         iface                   Rx                   Tx                Total
  ==============================================================================
              em0:           2.04 MB/s            6.03 KB/s            2.05 MB/s
              lo0:           0.00  B/s            0.00  B/s            0.00  B/s
        vm-public:           2.04 MB/s            2.05 MB/s            4.09 MB/s
             tap0:           5.49 KB/s            2.04 MB/s            2.04 MB/s
  ------------------------------------------------------------------------------
            total:           4.09 MB/s            4.09 MB/s            8.18 MB/s
```

Press the letter d to switch traffic display formats, and press h for more usage information.

## Appendix: /etc/rc.conf Network Configuration Examples

> **Note**:
>
> After modifying the **/etc/rc.conf** file, you need to restart the system or run the commands `service netif restart` and `service routing restart` in sequence to apply the network changes.

Hostname (must not be empty, otherwise Xorg cannot be used):

```ini
hostname="ykla"
```

### Dynamic DHCP Method

Dynamic DHCP method: configure network card igc0 to use DHCP:

```ini
ifconfig_igc0="DHCP"
```

### Static IP Method

Static IP method: set the IPv4 address of network card igc0 to **192.168.5.77**, with a subnet mask of **255.255.255.0**:

```ini
ifconfig_igc0="inet 192.168.5.77 netmask 255.255.255.0"
```

Default gateway/default route, typically the router's IP address:

```ini
defaultrouter="192.168.5.1"
```

Set an alias IPv4 address **192.168.5.12** for network card igc0, with a subnet mask of **255.255.255.255** (when on the same subnet as the primary address, an all-1s mask must be used) to obtain an additional IPv4 address:

```ini
ifconfig_igc0_alias0="inet 192.168.5.12 netmask 255.255.255.255"
```

> **Note**:
>
> If the alias address is on the same subnet as the primary address, the subnet mask must be set to 255.255.255.255 (0xffffffff), otherwise a duplicate route error will occur. Aliases on different subnets use the normal subnet mask for that subnet.

Static routes:

```ini
static_routes="static1 static2" # ①
```

① In the **/etc/rc.conf** file, if you need to write multiple configuration items at once, you must use the format `ABC_XYZ="xxx yyy ccc ddd"`. Writing it in the following form is **incorrect**:

```ini
ABC_XYZ="xxx" # First line
ABC_XYZ="yyy"
ABC_XYZ="ccc"
ABC_XYZ="ddd"
```

Because in this form, subsequent `ABC_XYZ` configuration lines will overwrite the previous line, so only the last line will take effect.

Example: To access the **192.168.50.0/24** address block (usable host IPs from **192.168.50.1** to **192.168.50.254**), send packets to **192.168.1.1** for forwarding:

```ini
route_static1="-net 192.168.50.0/24 192.168.1.1"
```

Example: To access the **10.88.200.0/24** address block (usable host IPs from **10.88.200.1** to **10.88.200.254**), send packets to 10.10.10.254 for forwarding:

```ini
route_static2="-net 10.88.200.0/24 10.10.10.254"
```

## Exercises

1. Configure dual static IP addresses on a FreeBSD system, set different DNS servers for each, use the `dig` command to verify the resolution behavior of each DNS server, and analyze the query order and fault tolerance mechanism of multiple DNS server entries in **/etc/resolv.conf**.
2. Modify the MTU (Maximum Transmission Unit) value of a network interface to 9000 (jumbo frames), use `ping` to test connectivity, record the impact of MTU changes on large packet transmission, and analyze the applicable scenarios of jumbo frames in local area networks and wide area networks.
3. Disable the `resolvconf` service, manually modify the **/etc/resolv.conf** file and restart the network service, verify whether the configuration is persistent, and analyze the mechanism of `resolvconf` for dynamic management of DNS configuration.
