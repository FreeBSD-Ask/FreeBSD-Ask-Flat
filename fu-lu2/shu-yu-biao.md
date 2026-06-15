# Glossary

This glossary collects the technical terms covered in this book, arranged in the order of their first appearance in the main text.

| Term | Chinese | Description |
| ---- | ------- | ----------- |
| 操作系统 | / | System software that manages computer hardware and software resources, provides common services for computer programs, and serves as the interface between users and computer hardware |
| 分时系统 | / | An operating system that allows multiple users to simultaneously use a single computer by dividing CPU time into time slices and serving each user in turn |
| 内核 | / | The core part of an operating system that resides in memory, responsible for managing the system's processes, memory, device drivers, file systems, and networking |
| 微内核 | / | An operating system kernel architecture that runs only the most essential services in kernel mode, with other services implemented as user-space processes |
| 宏内核 | / | An operating system kernel architecture that integrates all core services such as process management, memory management, file systems, and device drivers within the kernel space. Also known as a monolithic kernel |
| UNIX 哲学 | / | A software engineering methodology system derived from the development practices of the UNIX operating system, emphasizing small and refined design, each program focusing on a single task, prototyping first, and prioritizing portability over efficiency as core design principles |
| 可移植性 | / | The ease with which software can be transferred from one hardware platform or operating system environment to another and still function properly |
| 自由软件 | / | Software that grants users the freedom to run, copy, distribute, study, modify, and improve it |
| copyleft | 著佐权 | A licensing approach, similar to GPL terms, that imposes copyright constraints on derivative works, such as requiring source code to be made public |
| 开源软件 | / | Software whose source code is publicly available and allows users to freely use, modify, and distribute it |
| 专有软件 | / | Proprietary Software, a type of non-free software, also known as private software. Software that users are not free to use, modify, or distribute; most commercial software falls into this category |
| 许可证 | / | License, a legal agreement that authorizes others to use, modify, or distribute software |
| GNU | / | GNU's Not Unix, an operating system project initiated by the Free Software Foundation |
| GPL | / | GNU General Public License, a strong copyleft license |
| BSD 许可证 | / | A permissive open source license that allows commercial use and closed-source derivatives |
| BSD | 伯克利软件发行版 | Berkeley Software Distribution, an operating system distribution based on AT&T UNIX, improved and extended by the University of California, Berkeley, and subsequently developed by the Computer Systems Research Group (CSRG), representing an important branch in the technical evolution of UNIX |
| 类 UNIX | / | Unix-like, operating systems that behave similarly to UNIX but are not necessarily certified under the SUS |
| 发行版 | / | Distribution, a complete operating system packaged and released based on a kernel and its accompanying software collection |
| 内核模块 | / | A code segment that can be dynamically loaded into or unloaded from the kernel at runtime, extending kernel functionality without requiring kernel recompilation |
| Base System | 基本系统 | The combination of the kernel and user space (known as world in FreeBSD), i.e., all components from the src source tree |
| 用户空间 | / | Userland / Userspace, the runtime environment of the operating system outside the kernel, where applications execute |
| ABI | 应用程序二进制接口 | Application Binary Interface, the binary interface standard between applications and the operating system |
| RELEASE | 稳定版 | An official release version suitable for production environments |
| STABLE | / | FreeBSD's stable development branch, providing ABI stability guarantees while still under development |
| CURRENT | / | FreeBSD's main development branch, corresponding to the head or main branch in typical projects, containing the latest code changes but potentially unstable |
| MFC | 合并自 CURRENT | Merge From CURRENT, the process in FreeBSD development of merging changes from the CURRENT branch into stable branches |
| Jail | / | An operating system-level isolation technology developed on the basis of Chroot, achieving lightweight virtualization through isolation mechanisms at the file system, process, network, and user dimensions; one of the important early practices of modern container technology |
| ZFS | / | Originally an abbreviation for Zettabyte File System, later no longer officially treated as an acronym. An advanced storage system integrating a file system and logical volume manager, employing a copy-on-write transaction model with powerful data integrity protection mechanisms, efficient data compression, and a scalable storage architecture |
| OpenZFS | / | The open source community version of ZFS, unifying the open source development of ZFS |
| bhyve | / | BSD Hypervisor, FreeBSD's built-in hypervisor |
| DTrace | / | Dynamic Tracing, a dynamic tracing framework originally developed by Sun Microsystems and first released with Solaris 10, usable for real-time debugging of kernel and application issues in production systems |
| Capsicum | / | A lightweight operating system capability and sandbox framework for application sandboxing |
| PF | 包过滤器 | Packet Filter, firewall software originating from OpenBSD, available as an optional firewall in FreeBSD, supporting features such as ALTQ traffic shaping |
| Netgraph | / | FreeBSD's network graph framework, providing graph-based abstraction and programmable interconnection of kernel-level network nodes |
| GEOM | / | FreeBSD's disk I/O request transformation framework, providing features such as disk partitioning, encryption, and mirroring |
| UFS | Unix 文件系统 | Unix File System, FreeBSD's traditional file system |
| Port | / | A source code package for a single software in the FreeBSD system, containing the configuration files and scripts needed to compile and install that software |
| Ports | / | FreeBSD's software package management system, containing the collection of all Ports, used to compile and install software from source code |
| Ports Collection | / | The complete collection of the Ports system, including software category directories, build tools, and dependency management mechanisms |
| pkg | / | FreeBSD's binary package manager, used to install, update, and manage pre-compiled software packages; formerly known as pkgng |
| PkgBase | / | FreeBSD's base system package management scheme, using the pkg package manager to implement updates for user space and the kernel, available as a Technology Preview starting from FreeBSD 15.0 |
| freebsd-update | / | FreeBSD base system update tool, used to fetch security updates and perform system version upgrades |
| Poudriere | / | A FreeBSD tool that tests Ports and builds FreeBSD package images through Jail environments |
| 安全启动 | / | Secure Boot, a security mechanism based on UEFI firmware that verifies the integrity of boot loaders and operating system kernels through digital signatures |
| Linuxism | / | The phenomenon where software overly depends on Linux-specific features, making it difficult to port to other Unix-like operating systems |
| POLA | 最小惊讶原则 | Principle of Least Astonishment, a design principle stating that designs must conform to users' habits, expectations, and cognitive abilities |
| UNIX | / | An operating system originally developed at AT&T Bell Laboratories, now a standard specification and legal trademark |
| Single UNIX Specification | 单一 UNIX 规范 | SUS, the standard specification for the UNIX operating system |
| CDDL | 通用开发与发行许可 | Common Development and Distribution License, an open source license created by Sun Microsystems for OpenSolaris, adopted by projects such as ZFS, allowing commercial use and modification |
| TCP/IP | 传输控制协议/网际协议 | Transmission Control Protocol/Internet Protocol, the foundational protocol suite of the Internet |
| 校验和 | / | Checksum, a fixed-length value obtained by performing a specific algorithm on a data sequence, used to detect whether errors have occurred during data transmission or storage |
| 哈希函数 | / | Hash Function, also known as a hash function, a function that maps data of arbitrary length to a fixed-length value, used for data integrity verification and digital signatures |
| 内核错误（Kernel Panic） | / | Kernel Panic, a self-protection mechanism invoked when the operating system kernel encounters an unrecoverable fatal error; the system stops running and displays an error message |
| 分发文件 | / | Distribution Set, a collection of compressed packages in the FreeBSD installation media divided by function, such as base, kernel, src, lib32, etc. |
| 无线管制域 | / | Regulatory Domain, a legally defined region specifying the frequency, power, and other parameters for wireless device usage in each country or region |
| UEFI | 统一可扩展固件接口 | Unified Extensible Firmware Interface, the firmware interface standard for modern computers |
| BIOS | 基本输入输出系统 | Basic Input/Output System, the traditional firmware interface standard for computers |
| GPT | 全局唯一标识分区表 | GUID Partition Table, a disk partition table standard |
| MBR | 主引导记录 | Master Boot Record, the traditional disk partition table standard |
| 引导加载程序 | / | Bootloader, a program executed when a computer starts, responsible for initializing hardware devices and loading the operating system kernel into memory for execution |
| 分区 | / | Partition, the operation of dividing a physical disk into several logical storage areas |
| 交换 | / | Swap, the mechanism by which the operating system writes temporarily unused memory pages to a disk swap area to free physical memory |
| 挂载 | / | Mount, the operation of associating a file system with a directory in the directory tree, making its contents accessible |
| DHCP | 动态主机配置协议 | Dynamic Host Configuration Protocol, a protocol that automatically assigns IP addresses and other network configurations to hosts |
| SSH | 安全外壳协议 | Secure Shell, a protocol for secure remote login |
| DNS | 域名系统 | Domain Name System, a distributed naming system that maps domain names to IP addresses |
| 虚拟化 | / | Virtualization, technology that abstracts computer physical resources into logical resources, enabling multiple operating systems or applications to run independently on the same physical hardware |
| 虚拟机 | / | Virtual Machine, a complete computer system simulated through virtualization technology |
| VirtIO | / | A virtualization I/O framework providing standardized high-performance I/O interfaces for virtual machines |
| 串口控制台 | / | Serial Console, an interface for system management and debugging through a serial port |
| Chroot | / | Change Root, an operation that changes the root directory of a process and its child processes to another location in the file system |
| TTY | / | Teletypewriter, a teletypewriter, extended to refer to text terminal devices |
| 虚拟控制台 | / | Virtual Console, multiple independent logical terminals simulated on a physical terminal |
| Shell | / | A program in the operating system that provides a user interface, serving as the interface between the user and the kernel, responsible for interpreting and executing user-entered commands |
| 命令解释器 | / | Command Interpreter, a program that receives user-entered commands and translates them into instructions executable by the operating system |
| 环境变量 | / | Environment Variable, named value pairs in the operating system used to store system runtime environment and user configuration information, readable and usable by processes |
| POSIX | / | Portable Operating System Interface, an operating system standard developed by IEEE and The Open Group |
| 进程 | / | Process, a single execution instance of a program on a computer, the basic unit of resource allocation and scheduling in the operating system |
| 守护进程 | / | Daemon, a system process running in the background, not associated with any controlling terminal, typically responsible for listening for network requests or performing system maintenance tasks |
| 信号 | / | Signal, a software interrupt mechanism in the operating system used to notify a process that an event has occurred |
| 文件系统 | / | File System, the software component in the operating system responsible for managing and accessing file information |
| 符号链接 | / | Symbolic Link, a special type of file whose content is a path name pointing to another file or directory. Also known as a soft link |
| 硬链接 | / | Hard Link, multiple directory entries pointing to the same inode in a file system, sharing the same data blocks as the original file |
| 权限提升 | / | Privilege Escalation, the act of obtaining higher privileges beyond the authorized scope |
| SUID | 设置用户 ID | Set User ID, a file permission bit that temporarily grants the user executing the file the permissions of the file owner |
| SGID | 设置组 ID | Set Group ID, a file permission bit that temporarily grants the user executing the file the permissions of the file's group owner |
| 设备文件 | / | Device File, a special file in the operating system that represents a hardware device or virtual device |
| procfs | / | Process File System, a file system that presents kernel process information in a file system format (FreeBSD does not mount it by default; manual mount_procfs(8) is required) |
| devfs | / | Device File System, a file system that automatically manages device nodes in the **/dev** directory |
| fdescfs | / | File Descriptor File System, a file system that provides file system access to process file descriptors |
| 伪终端 | / | Pseudo-Terminal, a terminal device simulated in software, used for remote login and terminal emulation in window systems |
| PAM | 可插拔认证模块 | Pluggable Authentication Modules, a flexible system authentication framework originally developed by Sun Microsystems in 1995 |
| Kerberos | / | A network authentication protocol that uses a Key Distribution Center (KDC) and ticket mechanism to achieve secure identity verification |
| NIS | 网络信息服务 | Network Information Service, a system developed by Sun Microsystems for centrally managing user and host information on a network, formerly known as Yellow Pages (YP) |
| NTP | 网络时间协议 | Network Time Protocol, a protocol used to synchronize clocks across nodes in a computer network |
| syslog | / | A system logging protocol and tool used for recording system messages and events |
| OpenBSM | / | Open Basic Security Module, FreeBSD's security auditing system |
| 审计 | / | Audit, the process of recording, examining, and analyzing system security-related events |
| inetd | 互联网超级服务器 | Internet Super Server, a daemon that uniformly manages multiple network services |
| IPFW | / | ipfirewall, FreeBSD's built-in firewall system, employing first-match rules |
| IPF | / | IPFilter, a firewall software available as an optional firewall component in FreeBSD |
| ALTQ | 交替队列 | Alternate Queuing, a traffic shaping framework developed by Kenjiro Cho, first released in March 1997, later incorporated into the KAME project, integrated with the PF firewall to provide queue management and traffic shaping functionality |
| 二进制包 | / | Binary Package, a pre-compiled software package that can be installed directly without building from source code |
| 镜像站 | / | Mirror Site, a server that synchronizes and replicates official software repositories or distribution files, providing faster local download speeds |
| 源代码 | / | Source Code, the original text of a computer program written by programmers, which must be compiled or interpreted before execution |
| 依赖 | / | Dependency, a reference relationship between software where the normal operation of one software requires the presence of another |
| 启动环境 | / | Boot Environment, a bootable file system snapshot on ZFS that supports system version rollback |
| 数据集 | / | Dataset, a management unit in ZFS, similar to an independent partition in a traditional file system |
| 存储池 | / | Storage Pool / zpool, a storage space in ZFS composed of one or more virtual devices |
| 快照 | / | Snapshot, a read-only copy of a file system or storage system at a specific point in time |
| 写时复制 | / | Copy-on-Write (COW), an optimization strategy that copies data before writing modifications, ensuring data consistency |
| etcupdate | / | A FreeBSD tool used to merge configuration file changes after system upgrades |
| buildworld | / | One of the FreeBSD source code build processes, compiling the complete user space |
| buildkernel | / | One of the FreeBSD source code build processes, compiling the kernel |
| installworld | / | One of the FreeBSD source code build processes, installing the compiled user space |
| releng | / | Release Engineering, FreeBSD's release engineering branch |
| Linux 兼容层 | / | A FreeBSD system feature that enables running Linux binary programs on FreeBSD, providing application compatibility |
| Linuxulator | / | The Linux system call translation layer in the FreeBSD kernel, implementing Linux binary compatibility |
| RISC-V | / | An open source instruction set architecture based on RISC principles, initiated by the University of California, Berkeley in 2010; FreeBSD supports hardware platforms with the RISC-V architecture |
| kqueue | / | FreeBSD's event notification interface, providing a more efficient and scalable event notification mechanism than select/poll |
| TrustedBSD | / | FreeBSD's security extension project, based on the POSIX.1e draft |
| W^X | 写或执行互斥 | Write XOR Execute, a security policy where memory pages cannot simultaneously have write and execute permissions |
| PIE | 位置无关可执行程序 | Position Independent Executable, a security mitigation technique where executables can be loaded at arbitrary addresses |
| ASLR | 地址空间布局随机化 | Address Space Layout Randomization, a security mitigation technique that increases attack difficulty by randomizing the memory layout of processes |
| DAC | 自主访问控制 | Discretionary Access Control, the standard Unix security model where resource owners can autonomously determine access permissions |
| ACL | 访问控制列表 | Access Control List, an object-centric access control mechanism that provides more granular control than traditional Unix permissions |
| CHERI | / | Capability Hardware Enhanced RISC Instructions, a CPU architecture extension developed by the University of Cambridge and SRI International, a project led by Robert N.M. Watson alongside Capsicum; CHERI was conceptually inspired by Capsicum, extending the capability model from software to hardware, but the two are independent projects, not derivative of each other |
| Container | 容器 | A lightweight operating system-level virtualization technology that achieves process isolation and resource control through isolation mechanisms and resource limits |
| NAT | 网络地址转换 | Network Address Translation, a technology that translates addresses in IP packet headers to another address |
| 代理服务器 | / | Proxy Server, a server that acts as an intermediary between clients and servers, forwarding requests and responses |
| 防火墙 | / | Firewall, a system that enforces access control policies between networks, protecting network security by allowing or denying packet transmission |
| ICMP | 互联网控制报文协议 | Internet Control Message Protocol, a protocol used to send control messages and error reports in IP networks |
| UDP | 用户数据报协议 | User Datagram Protocol, a connectionless transport layer protocol |
| TCP | 传输控制协议 | Transmission Control Protocol, a connection-oriented, reliable transport layer protocol |
| HTTP | 超文本传输协议 | HyperText Transfer Protocol, an application layer protocol that is the foundation of data communication on the World Wide Web |
| HTTPS | 超文本传输安全协议 | HyperText Transfer Protocol Secure, a protocol encrypted through TLS on top of HTTP |
| 拥塞控制 | / | Congestion Control, a mechanism in networks that prevents excessive data injection from causing network congestion |
| Wayland | / | A display server protocol designed to replace X11 |
| 显示服务器 | / | Display Server, the core service program in a graphics system responsible for managing screen output and input devices |
| 显示管理器 | / | Display Manager, a program in a graphics system that provides a user login interface and manages session startup, such as GDM, SDDM, LightDM, etc. |
| 合成器 | / | Compositor, a component in a graphics system responsible for compositing the rendering results of multiple windows into the final screen image |
| X11 | X 窗口系统 | X Window System, a window system for graphical user interfaces |
| CDE | 通用桌面环境 | Common Desktop Environment, a classic UNIX desktop environment |
| 驱动程序 | / | Driver, interface software between the operating system and hardware devices, responsible for converting operating system requests into hardware-executable operations |
| 人工智能 | / | Artificial Intelligence, the discipline and technology field studying how to make computers simulate human intelligent behavior |
| OpenBSD | / | A security-focused BSD operating system forked from NetBSD by Theo de Raadt in 1995 |
| NetBSD | / | A portability-focused BSD operating system founded in 1993 |
| DragonFly BSD | / | A BSD operating system forked from FreeBSD 4.8 by Matthew Dillon in 2003 |
| LLVM | / | LLVM is not an acronym (it once stood for Low Level Virtual Machine, but is no longer officially treated as one), referring to a collection of modular and reusable compiler and toolchain technology projects (see LLVM Project. LLVM FAQ[EB/OL]. [2026-04-16]. <https://llvm.org/>. This page is the LLVM official overview page) |
| 编译程序 | / | Compiler, also known as a compiler, a program that translates high-level programming language source code into machine code or intermediate code |
| 解释程序 | / | Interpreter, also known as an interpreter, a program that reads and executes high-level programming language source code line by line |
| Clang | / | The C/C++ language frontend and tool infrastructure of the LLVM project |
| NFS | 网络文件系统 | Network File System, a distributed file system protocol |
| SMB | 服务器消息块 | Server Message Block, a protocol used for file sharing |
| 交叉编译 | / | Cross-compilation, the compilation process of producing executable code for another platform on one platform |
| 设备树 | / | Device Tree, a data structure describing hardware device information, used for hardware discovery in embedded systems |
| 单板计算机 | / | Single-Board Computer, a complete computer integrated on a single circuit board |
| 固件 | / | Firmware, software embedded in hardware devices that controls the basic operations of the device |
| 运行级别 | / | Runlevel, levels defining system running states in SysVinit systems |
| 伪静态 | / | URL Rewriting, a technology that maps dynamic URLs to static URLs through server-side rules |
| 反向代理 | / | Reverse Proxy, a proxy server that receives client requests, forwards them to backend servers, and returns the responses to the clients |
| 时序数据库 | / | Time Series Database, a database specifically designed for storing and querying time series data |
| ORDBMS | 对象关系型数据库管理系统 | Object-Relational Database Management System, a relational database that supports features such as objects, classes, and inheritance |
| NoSQL | / | Not Only SQL, a database management system that does not strictly follow the relational model |
| 文档型数据库 | / | Document-oriented Database, a database that stores and retrieves data in units of documents |
| SCRAM-SHA-256 | / | Salted Challenge Response Authentication Mechanism, an authentication mechanism based on SHA-256 |
| ACID | / | Atomicity, Consistency, Isolation, Durability, the four fundamental properties of database transactions: atomicity, consistency, isolation, and durability |
| SSL/TLS | 安全套接层/传输层安全 | Secure Sockets Layer / Transport Layer Security, protocols that provide security and data integrity for network communications |
| FastCGI | / | Fast Common Gateway Interface, an improved CGI protocol that handles requests through persistent processes to improve performance |
| JIT 编译程序 | 即时编译程序 | Just-In-Time Compiler, a compiler that compiles bytecode into machine code while the program is running |
| Java Servlet | / | A server-side component on the Java platform for handling HTTP requests |
| SPICE | / | Simple Protocol for Independent Computing Environments, a remote display protocol |
| VNC | 虚拟网络计算 | Virtual Network Computing, a remote desktop protocol |
| RDP | 远程桌面协议 | Remote Desktop Protocol, a remote desktop protocol developed by Microsoft |
| FUSE | 用户空间文件系统 | Filesystem in Userspace, a framework that allows non-privileged users to create file systems in user space |
| autofs | / | Automatic File System, an automatic mount file system framework |
| fsck | / | File System Consistency Check, a file system consistency check and repair tool |
| GELI | / | FreeBSD's disk encryption framework |
| sysctl | / | System Control, the interface for reading and setting FreeBSD kernel runtime parameters |
| securelevel | / | Security level, FreeBSD's kernel security level mechanism; the higher the level, the stricter the restrictions |
| rctl | / | Resource Control, FreeBSD's resource limiting tool |
| mac_bsdextended | / | FreeBSD's Mandatory Access Control (MAC) policy module, providing firewall-like rule-based access control for file systems |
| devd | / | Device Daemon, FreeBSD's device status daemon that responds to hardware events |
| powerd | / | Power Daemon, FreeBSD's power management daemon |
| cron | / | The Unix system's scheduled task execution daemon, used to automatically execute tasks at predetermined times |
| DMA | / | ① Direct Memory Access, a technology that allows devices to directly read and write system memory; ② DragonFly Mail Agent, incorporated into the FreeBSD base system in 2014 and replacing Sendmail as the default local mail transfer agent (not a full-featured MTA) starting from FreeBSD 14.0; the default MTA for FreeBSD 13 and earlier was Sendmail |
| mtree | / | FreeBSD's directory tree specification and verification tool |
| authpf | / | Authenticating Gateway User Shell, the authentication gateway shell for the PF firewall |
| Unbound | / | A validating recursive DNS server |
| Kyua | / | The automated testing framework adopted by the FreeBSD base system for running regression tests. Originally developed for the NetBSD project, later incorporated into the FreeBSD base system |
| 内存盘 | / | RAM Disk, a technology that simulates memory as a disk device, with data stored in memory and lost on power-off. FreeBSD provides RAM disk functionality through mdconfig and the md(4) driver. mfsBSD is a minimal FreeBSD distribution that runs from a RAM disk |
| GRUB | / | Grand Unified Bootloader, a universal multi-operating system boot loader |
| init | / | The initialization process with PID 1 in UNIX systems, responsible for post-boot initialization work |
| 调试器 | / | Debugger, a tool used to detect and locate program errors |
| 版本控制 | / | Version Control, the system and method for managing the change history of files or code |
| 补丁 | / | Patch, fix code provided for issues or functional defects discovered in software |
| 网络协议 | / | Network Protocol, a collection of rules, standards, or conventions established for data exchange in computer networks |
| 端口 | / | Port, a logical identifier used in network communication to distinguish different applications or services, with a value range of 0–65535 |
| 套接字 | / | Socket, an endpoint of network communication, composed of an IP address and port number, serving as the interface between applications and the network |
| 管道 | / | Pipe, an interprocess communication mechanism that directly connects the output of one process to the input of another |
| 中断 | / | Interrupt, the process by which a CPU responds to an event occurring in the system, suspending the current program to execute an interrupt handler |
| 线程 | / | Thread, an execution unit within a process that shares the process's resources but has an independent execution flow |
| 虚拟存储器 | / | Virtual Memory, also known as virtual memory, a memory management technology provided by the operating system that combines physical memory with disk space to provide a contiguous address space for processes |
| 高速缓存 | / | Cache, high-speed memory located between the CPU and main memory, used to reduce memory access latency |
| 固态硬盘 | / | Solid State Drive, a storage device using flash memory chips to store data, with no moving mechanical parts |
| 图形处理器 | / | Graphics Processing Unit, a processor specialized for graphics rendering and parallel computing |
| 路由器 | / | Router, a device that forwards data packets between networks, selecting the best transmission path based on routing tables |
| 虚拟专用网络 | / | Virtual Private Network, technology that establishes encrypted tunnels over public networks, enabling secure remote network access |
| 带宽 | / | Bandwidth, the maximum amount of data that a communication channel can transmit per unit of time |
| 吞吐量 | / | Throughput, the amount of data actually processed or transmitted by a system per unit of time |
| 数字签名 | / | Digital Signature, a signature generated by encrypting a message digest with a private key, used to verify the integrity of the message and the identity of the sender |
| 数字证书 | / | Digital Certificate, an electronic document issued by a certificate authority, used to prove the binding between a public key and an entity |
| 对称加密 | / | Symmetric Encryption, an encryption method that uses the same key for both encryption and decryption |
| 非对称加密 | / | Asymmetric Encryption, an encryption method that uses a public and private key pair for encryption and decryption |
