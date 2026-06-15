# Introduction

This introduction provides an overall overview of the book, including its positioning, requirements for readers, organizational structure, and the meaning of commands and symbols used in the book.

## Book Positioning

This book covers the installation and daily use of FreeBSD 15.x-RELEASE and 14.x-RELEASE, with as much backward compatibility with earlier versions as possible, and also includes some content from 16.0-CURRENT.

This book primarily targets the amd64 (x86-64) and arm64 (AArch64) architectures, with support for other hardware architectures where possible.

The testing environment for this book is Windows 11, with the system kept up to date.

> **Note**
>
> The software version numbers, feature statuses, and compatibility information mentioned in this book are accurate as of the time of writing; subsequent versions may have changed. Readers should refer to the current official documentation when performing actual operations.

## Requirements for Readers

This book uses the competency level of a qualified or above graduate from a computer science and technology program at a higher education institution as its difficulty baseline. Readers should possess the following foundational knowledge:

- Basic operating system concepts (processes, memory management, file systems, etc.)
- Command-line operation experience
- Basic computer networking knowledge
- English reading ability (for reading official documentation and manual pages)

## Organizational Structure of This Book

**Chapter 1 First Encounter with FreeBSD**: Introduces what UNIX is, the GNU operating system and the free software movement, Linux and UNIX-like systems, what FreeBSD is, and George Berkeley and the BSD cultural tradition.

**Chapter 2 Introduction to FreeBSD**: An in-depth exploration of FreeBSD's ideals, reality, and the middle path; about the FreeBSD project, the FreeBSD development model, and a brief history of FreeBSD.

**Chapter 3 Migration Guide**: Provides migration guides for Windows, Linux, and macOS users, introduces an overview of other BSD distributions and BSD licenses, helping users of different operating systems transition smoothly to FreeBSD.

**Chapter 4 FreeBSD Installation Basics**: Introduces pre-installation preparations, FreeBSD 15 installation guide (AMD64), installation troubleshooting, and the method for restoring a USB boot drive to a regular storage device in Windows.

**Chapter 5 Installing FreeBSD on Virtualization Platforms**: Introduces methods for installing FreeBSD using VMware Workstation Pro, VirtualBox, and Hyper-V, as well as installation solutions based on Apple M1 with Parallels Desktop and VMware Fusion Pro.

**Chapter 6 Command-Line Environment**: Covers virtual consoles and terminals, shell basics, switching shells, command-line fundamentals, text editors, and compression and decompression basics.

**Chapter 7 Network Management**: Introduces computer networking fundamentals, basic network management, wireless network management, Bluetooth, USB network tethering, and system proxy configuration methods.

**Chapter 8 Software Management**: Introduces an overview of the FreeBSD package manager, FreeBSD software repositories, managing binary packages with pkg, installing software from source using Ports, Ports build optimization, installing software from DVD, and the current state of FreeBSD mirror sites.

**Chapter 9 X Window System**: Introduces an overview of graphics drivers, installation and configuration of Intel, AMD, and NVIDIA graphics drivers, system fonts, remote desktop, and root user desktop login.

**Chapter 10 Desktop Environments**: Introduces various desktop environments and window managers running on FreeBSD, including KDE 6 (X11 and Wayland sessions), MATE, Xfce, Cinnamon, LXQt, GNOME, IceWM, and CDE (pending removal).

**Chapter 11 Multimedia and External Devices**: Introduces the use of sound cards, printers, cameras, human input devices, audio players, video players, multimedia processing, document viewers, and web browsers.

**Chapter 12 Localization and Input Methods**: Introduces localization environment variable configuration, locale configuration for specific languages, and the installation and setup of Fcitx and IBus input method frameworks and Wubi input methods.

**Chapter 13 FreeBSD System Updates**: Introduces methods for updating FreeBSD using freebsd-update and source code, as well as updating the base system with PkgBase and achieving multi-version coexistence through ZFS boot environments.

**Chapter 14 User Accounts and Permissions**: Introduces user and basic account management, permissions, user classification, and privilege escalation tools.

**Chapter 15 System Booting**: Introduces the boot loader, boot messages (dmesg), boot managers and UEFI firmware, processes and daemons, and managing services in FreeBSD.

**Chapter 16 Advanced FreeBSD Installation**: Introduces installing dual systems (installing FreeBSD first vs. installing FreeBSD second), installing FreeBSD on platforms such as Tencent Cloud Lightweight Cloud and KVM/QEMU (legacy boot and MBR partition table), installing FreeBSD on Alibaba Cloud Lightweight Application Server (UEFI and GPT partition table), and installing RISC-V FreeBSD with QEMU (based on x86 Windows host).

**Chapter 17 System Administration**: Covers system directory structure, bsdconfig configuration tool, OpenSSH, device resource hints, Cron and Periodic, system log management, sysctl tool, NTP time synchronization and time zones, and live images and system recovery.

**Chapter 18 Linux Compatibility Layer**: Introduces the architecture of FreeBSD's Linux compatibility layer, covering the setup of compatibility environments for various Linux distributions including Rocky Linux, Ubuntu/Debian/Kali Linux, Arch Linux, Slackware, and Gentoo, as well as running methods and troubleshooting for Linux applications such as WeChat, QQ, and WPS Office.

**Chapter 19 Games, Scientific Computing, and Professional Tools**: Introduces the Godot open-source game engine, Minecraft server and client, Steam client, R language, Wine configuration, and the use of research and professional computing tools.

**Chapter 20 Artificial Intelligence**: Introduces AI terminology and concepts, Transformer mathematical foundations and program demonstrations, selected readings in AI philosophy, and local deployment of large models and AI programming tools (llama.cpp, Ollama, Claude Code, GitHub Copilot CLI).

**Chapter 21 Development Environments**: Introduces the setup of development environments for languages including C/C++, Java, Qt, Python, Rust, Go, and Node.js.

**Chapter 22 Development Tools**: Introduces code-server and clangd development environments, Vim development environment, debugging FreeBSD with IDA Pro, and the use of the DTrace dynamic tracing tool.

**Chapter 23 Embedded Platforms and Development Environments**: Introduces the installation and use of FreeBSD on Raspberry Pi, Linux compatibility layer configuration, Radxa X4 x86 development board, and the setup of embedded development environments for STM32, ESP-IDF, and Arduino.

**Chapter 24 Advanced Networking**: Introduces the TCP/IP protocol stack, bridging, link aggregation and failover, and VLAN configuration methods.

**Chapter 25 Storage Management**: Introduces USB storage devices, virtual memory disks, automatic filesystem mounting, adding swap partitions, and encrypted swap partitions.

**Chapter 26 Other Filesystems**: Introduces the use of Windows, Linux, and macOS filesystems.

**Chapter 27 UFS Filesystem**: Introduces UFS filesystem overview, adding UFS disks, UFS disk expansion, UFS disk snapshots, UFS disk quotas, and UFS disk encryption.

**Chapter 28 ZFS Filesystem**: Covers ZFS history and current state, features and terminology, storage pool management, updating ZFS storage pools, ZFS management, ZFS tuning, ZFS delegated administration, updating OpenZFS, and boot environments.

**Chapter 29 Security**: Introduces information security overview, account authentication security, resource limits, security levels, and OpenSSL.

**Chapter 30 Security Auditing**: Introduces security event auditing, intrusion detection systems (IDS), third-party vulnerability and security advisories, and the Mandatory Access Control framework (MAC framework).

**Chapter 31 Firewalls**: Introduces firewall overview and FreeBSD's built-in firewall systems, including configuration of ipfirewall (IPFW), IPFilter (IPF), and Packet Filter (PF), as well as the use of Fail2Ban (based on IPFW, PF, and IPF) and the blocklistd tool.

**Chapter 32 Jail Container Management**: Introduces the basic configuration of FreeBSD's native lightweight virtualization technology Jail, thick jails, and the Qjail management tool.

**Chapter 33 Linux Jails**: Introduces methods for running Linux distributions in FreeBSD Jails, covering Linux Jail basics, as well as Jail creation for Debian, Ubuntu, antiX Linux, and Alpine Linux, and GUI configuration in Linux Jails.

**Chapter 34 Virtualization and Container Management**: Introduces installing Windows 11 using bhyve and vm-bhyve tools, managing bhyve virtual machines through BVCP's web interface, Podman container management, and installing VirtualBox on FreeBSD.

**Chapter 35 Database Management**: Introduces database overview, as well as the installation and configuration of PostgreSQL, pgAdmin4, MySQL, and MongoDB on FreeBSD.

**Chapter 36 File Transfer Protocol (FTP)**: Introduces File Transfer Protocol (FTP) overview, as well as configuration of FTP servers including Pure-FTPd (MySQL-based), ProFTPD (MySQL-based), and vsftpd.

**Chapter 37 Servers**: Introduces the setup of services including Rsync data synchronization, Samba file sharing, Network File System (NFS), Zero-configuration networking (mDNS/DNS-SD), and the Webmin management platform.

**Chapter 38 Web Servers**: Introduces the deployment of Apache, Nginx, and Caddy web servers, as well as the configuration of PHP, Tomcat, and WildFly application servers, while also covering Nextcloud cloud services (PostgreSQL-based), OnlyOffice deployment (PostgreSQL-based), GitLab Enterprise Edition deployment, and OpenList deployment.

**Chapter 39 Monitoring Systems**: Introduces the Zabbix monitoring system (PostgreSQL-based), Prometheus monitoring deployment, and the Telegraf, InfluxDB, and Grafana monitoring platform architecture.

**Chapter 40 FreeBSD Kernel Architecture**: Introduces FreeBSD source code directory structure, kernel file structure, annotations for machine-dependent and machine-independent kernel options, GENERIC kernel option annotations (AMD64), methods for building custom kernels, and cross-building FreeBSD on Linux systems.

**Appendix I Tools and Resources**: Includes bug reporting procedures, FreeBSD mailing list subscription, FreeBSD development participation guide, microSD card parameter overview, and configuration methods for V2Ray and Mihomo.

**Appendix II References and Glossary**: Lists the core references and glossary for the entire book.

**Afterword**: Collects literary content related to FreeBSD, including personal stories and reflections.

## Commands and Symbols in This Book

### Permission Indicators

`#` indicates operations performed with `root` privileges, typically obtained through `su`, `sudo`, or `doas`.

`$` and `%` indicate regular user privileges. This book typically uses `$` as the regular user prompt; in FreeBSD official documentation (such as the Handbook), `%` is also commonly used as the regular user prompt.

### Tip Blocks

#### Note Tip Block

> **Note**
>
> Content that readers should not overlook.

#### Tip Block

> **Tip**
>
> Methods that help improve efficiency or shift one's thinking.

#### Warning Tip Block

> **Warning**
>
> If you do not understand or do not follow the instructions, you will be unable to complete the operation or may cause significant harm.

### Thought Questions

> **Thought Question**
>
>> The Digital Identity Guidelines published by the National Institute of Standards and Technology (NIST. NIST SP 800-63B-4: Digital Identity Guidelines: Authentication and Authenticator Management[EB/OL]. (2025-07)[2026-04-04]. <https://pages.nist.gov/800-63-4/sp800-63b.html>) state:
>>
>> Verifiers SHALL require that passwords used as a single-factor authentication mechanism be at least 15 characters in length. Verifiers MAY allow passwords used solely as part of a multi-factor authentication process to be shorter, but SHALL require them to be at least 8 characters in length.
>>
>> Verifiers SHALL allow a maximum password length of at least 64 characters.
>>
>> Verifiers SHALL accept all printable ASCII (RFC 20) characters and the space character as password input.
>>
>> Verifiers SHALL accept Unicode [ISO/IEC 10646] characters as password input. When evaluating password length, each Unicode code point SHALL be counted as a single character.
>>
>> Verifiers SHALL NOT impose other composition rules (e.g., requiring mixtures of different character types) for passwords.
>
>> Verifiers SHALL NOT require subscribers to change passwords periodically. However, verifiers SHALL force a change if there is evidence of compromise of the authenticator.
>>
>> Verifiers SHALL NOT permit the storage of hint information (e.g., reminders about how the password was created) that could be accessed by an unauthenticated claimant.
>>
>> Verifiers SHALL NOT prompt subscribers to use knowledge-based authentication (KBA) (e.g., "What was the name of your first pet?") or security questions when selecting a password.
>>
>> Verifiers SHALL require the entire password to be entered (not partially), and SHALL verify the entire submitted password (e.g., SHALL NOT truncate the password).
>
>> "Although many experts have repeatedly criticized the drawbacks of current password rules in recent years, banks, internet platforms, and government agencies mostly still cling to these outdated, ineffective, and even harmful rules." (GoUpSec. Is "periodic password change" the stupidest password rule?[EB/OL]. (2024-09-27)[2026-04-04]. <https://www.goupsec.com/news/17579.html>.)
>
> Question: Does the "knowledge" we take for granted truly have a solid and reliable foundation? Are some things we cling to a tradition, or merely outdated conventions? How do we draw the boundary between the two?

Thought questions are intended for readers who are willing to explore and motivated to inquire further. This format is similar to common after-class exercises, with the difference that thought questions appear at random positions. It should be noted that although certain positions and understandings may be presupposed, the answers are typically open-ended and depend on the reader's own perspective. Additionally, if readers feel fatigued, they may choose to skip them.

### Special Sections

```text
Troubleshooting and Unfinished Matters
```

Intended to document existing problems, directions for improvement, or unresolved mysteries, in the hope of leveraging the wisdom of those who come after.

```text
After-Class Exercises
```

Purely theoretical chapters typically conclude with "After-Class Exercises" to help readers explore the chapter content in greater depth.

### Strikethrough

~~Why must people live? The more one tries to explore the meaning of life, the more one falls into confusion.—*Caligula* (Wada Junichi, dir. Caligula[V]. Japan: Caligula Production Committee, 2018. <https://www.bilibili.com/bangumi/media/md77552>.) Episode 3 title.~~

~~Right-clicking in KDE 6 on Wayland causes a black screen~~ (Note: This issue was fixed via a qt6-wayland patch in July 2025)

Strikethrough indicates outdated content or text with a humorous undertone; it is not part of the main text. If you cannot understand the relevant references, you may skip it directly without affecting your reading.

## pkg and ports

FreeBSD provides two methods for installing software: pkg (binary package management) and Ports (source code compilation). Each method has its advantages; you can choose based on your actual needs, or use them in combination.

pkg is FreeBSD's binary package manager, providing a fast and convenient installation method through pre-compiled software packages, suitable for most users' daily use. Ports provides a method for installing from source code, allowing users to customize configuration options as needed, suitable for scenarios requiring customization or optimization.

> **Please Note**
>
> Ports typically corresponds to the HEAD branch. It is recommended to keep pkg and Ports on the same main branch, i.e., both using `latest`. You can also use the Ports quarterly branch corresponding to your pkg, such as `2025Q1`.

If you want to install the software `yyy`, and `yyy` is `xxx/yyy` in Ports, then the path is **/usr/ports/xxx/yyy**.

First, you can install the binary package via pkg, consistent with the usage on most Linux distributions:

```sh
# pkg install yyy
```

You can also use the following format:

```sh
# pkg install xxx/yyy
```

Or use the abbreviated form:

```sh
# pkg ins yyy
```

You can also compile and install via Ports:

```sh
# cd /usr/ports/xxx/yyy
# make install clean
```

The system will sequentially pop up configuration windows for user selection. If using default options, execute the following command:

```sh
# cd /usr/ports/xxx/yyy
# make BATCH=yes install clean
```

If you need to complete all dependency configurations at once:

```sh
# cd /usr/ports/xxx/yyy
# make config-recursive # Will recursively prompt until all dependency configurations are complete
# make install clean
```
