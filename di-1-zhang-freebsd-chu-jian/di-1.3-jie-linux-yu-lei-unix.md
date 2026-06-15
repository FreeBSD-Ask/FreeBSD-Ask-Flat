# 1.3 Linux and UNIX-like Systems

This section takes the definition from the official Linux kernel documentation as a starting point, elaborating on the technical architecture and licensing system of Linux as a UNIX clone operating system, and explaining the legal and functional basis for its "UNIX-like" attribute.

## What Is Linux?

Linux is a widely used open source operating system in the world today. The meaning of "Linux" varies depending on context: in a narrow sense, it refers to the Linux kernel; in a broad sense, it usually refers to the complete operating system, namely GNU/Linux. The official Linux kernel documentation answers this question as follows:

> What is Linux?
>
> Linux is a clone of the operating system Unix, written from scratch by Linus Torvalds with assistance from a loosely-knit team of hackers across the Net. It aims towards POSIX and Single UNIX Specification compliance.
>
> It has all the features you would expect in a modern fully-fledged Unix, including true multitasking, virtual memory, shared libraries, demand loading, shared copy-on-write executables, proper memory management, and multistack networking including IPv4 and IPv6.
>
> It is distributed under the GNU General Public License v2 - see the accompanying COPYING file for more details.
>
> See Linux Kernel Organization. What is Linux?[EB/OL]. [2026-04-04]. <https://www.kernel.org/doc/html/latest/admin-guide/README.html>.

The following framework diagram more clearly illustrates the layered structure of the Linux system:

```text
+-----------------------------------------+
|            Application Layer            |
|  (Browsers/Office/Dev Tools/Databases)  |
+-----------------------------------------+
|        Graphical Interface (Optional)   |
|       (GNOME █ KDE █ Xfce etc.)         |
+-----------------------------------------+
|        Core System Utilities Layer      |
|  █ GNU Base Utilities (bash/gcc/glibc)  |
|  █ Package Managers (apt/yum/pacman)    |
|  █ Init Systems (systemd/OpenRC)        |
+-------------------+---------------------+
|                   | Linux (narrow sense) |
|    Linux Kernel   +---------------------+
|      Layer        |(Directly controls HW)|
+-------------------+---------------------+
|               Hardware Layer            |
|  (CPU █ Memory █ Disk █ NIC █ Periph.)  |
+-----------------------------------------+
```

The **Core System Utilities Layer** includes GNU base utilities (such as bash, gcc, glibc, etc.), package managers, and init system components. **GNU Base Utilities** are command-line tools and system libraries developed by the GNU Project, providing basic functionality for the operating system.

The development of Linux was inspired by Minix, a microkernel operating system designed specifically for education, born in the context of restricted UNIX copyrights. Linus Torvalds was 21 years old at the time, studying in the Department of Computer Science at the University of Helsinki in Finland. In the Finnish university system at that time, students typically pursued a master's degree directly after enrollment; the bachelor's degree was merely an intermediate degree rather than an independent admission stage, making the academic structure relatively flexible. In 1991, Torvalds was still in the early stages of his studies.

The success of both GNU and Linux was inseparable from the community discussions sparked by Minix. In 1992, Andrew Stuart "Andy" Tanenbaum (the author of Minix) engaged in a fierce debate with Linus over kernel architecture, arguing that Linux's monolithic kernel design was already obsolete; Linus acknowledged that microkernels were theoretically superior but defended the monolithic kernel design from a practical standpoint. Linux adopted a monolithic kernel architecture from the very beginning, and this debate did not change its architectural choice, but it made the debate over the merits of monolithic versus microkernels a classic topic in operating system design. During the same period, Linus's original license prohibited any paid distribution (including charging for media costs), and driven by community requests for GNU copyleft compatibility, Linus changed the license to GPL with Linux version 0.12, and incorporated GNU components, gradually improving the Linux ecosystem.

Linus Torvalds's master's thesis was [《Linux: A Portable Operating System》](https://www.cs.helsinki.fi/u/kutvonen/index_files/linus.pdf), and he received his Master of Science degree in 1997 (at age 27). Since the University of Helsinki had no maximum study period at the time, he was able to maintain his enrollment for an extended period. According to the university's official website, course grades are valid for ten years, but course expiration does not affect the right to continue studying at the university. The official website explicitly states: "Course expiration does not affect the right to continue studying at the university." (Note: Finland's Universities Act specifies target and maximum times for degree completion effective from August 1, 2005.)

> We discuss the hardware portability issues that arise when porting the Linux operating system to multiple CPU and bus architectures. We also discuss software interface portability issues, particularly binary compatibility with other operating systems that can share the same hardware platform. The paper describes the approach taken by Linux and provides a more detailed introduction to several of these architectures.
>
> Abstract of the thesis.

> **Tip**
>
> The Management Engine (Intel Management Engine) on nearly every Intel processor runs a Minix-based microkernel. Since ME 11 (introduced with Skylake processors), the Intel Management Engine is based on the Intel Quark x86 microcontroller and runs the MINIX 3 operating system.
>
> ~~Perhaps the Minix-based microkernel is the most widely deployed operating system kernel in the world.~~

The UNIX standard SUS includes the POSIX standard; the former is a superset of the latter. The Base Specifications of the Single UNIX Specification are a superset of the existing POSIX.1 and POSIX.2 specifications. Linux implements the POSIX standard but has not obtained POSIX certification: IEEE, The Open Group.

### References

- Yang Tianping, Jin Ruyi. On the Bologna Process[J]. Journal of East China Normal University (Educational Sciences), 2009, 27(01): 9-22. DOI: 10.16382/j.cnki.1000-5560.2009.01.007. On the development of the Finnish educational system.
- Finland. Universities Act (No. 558/2009, amendments up to No. 644/2016 included) [Z]. 2009-07-24.
- The Open Group. The Base Specifications Issue 6, Preface[EB/OL]. [2026-04-23]. <https://pubs.opengroup.org/onlinepubs/009604299/frontmatter/preface.html>. Notes "These were selected since they were a superset of the existing POSIX.1 and POSIX.2 specifications and had some organizational aspects that would benefit the audience for the new revision."
- Portnoy E, Eckersley P. Intel's Management Engine is a security hazard[EB/OL]. (2017-05-08)[2026-04-23]. <https://www.eff.org/deeplinks/2017/05/intels-management-engine-security-hazard-and-users-need-way-disable-it>.
- HandWiki. Intel Management Engine[EB/OL]. [2026-04-23]. <https://handwiki.org/wiki/Intel_Management_Engine>.
- jmcph4. Intel Management Engine[EB/OL]. [2026-04-23]. <https://jmcph4.dev/wiki/ime.html>.
- University of Helsinki. Expiry of Studies[EB/OL]. (2026-02-16)[2026-04-04]. <https://studies.helsinki.fi/instructions/article/expiry-studies>. Official explanation from the University of Helsinki in Finland.
- POSIX Certification Policy[EB/OL]. (2012-12-05)[2026-04-04]. <http://get.posixcertified.ieee.org/docs/POSIX_Certification_Policy_v1.1.pdf>.
- Torvalds L. Linux: a Portable Operating System[D/OL]. Helsinki: University of Helsinki, 1997 [2026-04-04]. <https://www.cs.helsinki.fi/u/kutvonen/index_files/linus.pdf>. Linus's thesis.

## Linux in the Narrow Sense: The Operating System Kernel

The meaning of "Linux" varies depending on context. In the narrow sense, Linux refers to the Linux kernel. The [Linux kernel](https://www.kernel.org/) project began in 1991.

## Linux in the Broad Sense: The GNU/Linux Operating System

In the broad sense, Linux usually refers to the complete operating system. GNU/Linux = Linux kernel + GNU and other software + package manager.

**[Chimera Linux](https://chimera-linux.org/) is an exception.**

The full name of Linux is GNU/Linux.

From the recursive acronym GNU ("GNU's Not Unix"), it can be seen that Linux has no direct lineage relationship with UNIX.

Specifically:

- GNU/Linux distributions = Ubuntu, RHEL, Deepin, openSUSE...
  - Ubuntu = Linux kernel + apt/dpkg + GNOME (default desktop environment)
  - openSUSE = Linux kernel + libzypp/rpm (package manager backend, supports RPM format) + KDE (one of the default desktop environments)

> **Discussion Questions**
>
> 1. If the file system, Linux kernel, Shell, systemd (init), desktop environment, package manager, and all third-party software are removed, what content remains in a Linux distribution?
> 2. After removing all of the above components and recombining them, if the system is still called a "distribution," what essential differences exist between it and a traditional Linux distribution?
> 3. In this case, can the system still be regarded as the original distribution? Please explain your reasoning.
> 4. If it cannot be regarded as the original distribution, after removing which key category of components does it lose its "distribution" attribute?
> 5. If it can still be regarded as the original distribution, which parts can be considered truly inherited from the original distribution, and on what basis?

> **Tip**
>
> If in doubt, it is recommended to install [Gentoo](https://www.gentoo.org/downloads/) (stage3) or [Slackware](https://www.slackware.com/) yourself. If still uncertain, you can try [Gentoo (stage1)](https://wiki.gentoo.org/wiki/Stage_file) or [LFS](https://www.linuxfromscratch.org/lfs/) yourself.
>
> The above operations are relatively complex and require some experience and foundational knowledge. ~~Caught in the hermeneutic circle again.~~

## Defining the Concept of UNIX-like Systems

In addition to systems that have obtained formal UNIX certification, there are many operating systems that adopt design principles similar to UNIX.

UNIX-like refers to operating systems that basically conform to UNIX standards or basically comply with POSIX specifications but have not obtained the corresponding certification or trademark rights.

This term is used to describe systems that are highly similar to UNIX in design philosophy and technical implementation but lack formal certification.

Among current mainstream operating systems, many that follow POSIX specifications or adopt UNIX-like design principles can be called UNIX-like, with typical representatives including Linux and various BSD systems.

## Exercises

1. Configure a cross-compilation toolchain in a modern FreeBSD environment, build 4.4BSD-Lite2, and run it in QEMU. Document the toolchain compatibility issues encountered during the cross-compilation process.
2. Consult the formal documentation of SUS and POSIX standards, and list the similarities and differences between the two from three dimensions: system call interfaces, C standard library functions, and compilation program conventions.
3. Compare the differences between OpenRC and FreeBSD's native rc.d framework in terms of service dependency resolution mechanisms.
