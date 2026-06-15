# 2.4 A Brief History of FreeBSD

- 1961 Timesharing OS

In the early 1960s, timesharing operating systems were born. In November 1961, Fernando Corbató at MIT first demonstrated the Compatible Time-Sharing System (CTSS) on an IBM 709, one of the earliest timesharing systems. Around the same time, the Atlas monitor was implemented on the Atlas computer designed at the University of Manchester; this system officially went into operation on December 7, 1962, marking the first practical use of virtual memory — the concept of virtual memory was first proposed by German physicist Fritz-Rudolf Güntsch in his doctoral dissertation in 1957 (see: Denning P J. Virtual Memory[J]. ACM Computing Surveys, 1970, 2(3): 153-189). In that era, a timesharing system meant two people sharing the same computer, typically requiring an hourly schedule to plan computer usage time.

- 1964 MULTICS (**Multiplexed** Information and Computing Service)

The initial planning and development of Multics began in 1964 in Cambridge, Massachusetts. Originally, Multics was a project led by MIT (Fernando Corbató's MAC project); in August 1964, General Electric signed on as the hardware supplier; in November of the same year, Bell Labs joined, forming a three-party collaboration. Multics was developed on the General Electric 645 computer designed specifically for operating systems; the GE 645 prototype was delivered to MIT in January 1967, and Multics first booted on this hardware in December of that year.

- 1969 UNIX (UNIX Operating System)

Before Bell Labs exited the Multics project, Dennis Ritchie and Ken Thompson had already recognized the potential of Multics. In 1969, Ken Thompson began developing a new program called Unics (Uniplexed Information and Computing Service, **single-plexed** information and computing service) on an idle PDP-7 computer. Subsequently, in early 1970, they submitted a purchase request under the pretext of developing text processing, and with the support of research department managers Lee McMahon and Doug McIlroy, obtained funding. The order was placed in May of that year, and the PDP-11/20 computer was received in late summer, and Unics was ported to this more powerful machine.

- 1973 UNIX Code Migrated to C

Dennis Ritchie decided to develop a high-level language for UNIX whose statements could compile into a few machine instructions. This led him to develop the C programming language. In the summer of 1973, the Fourth Edition Research Unix (Research Unix V4) rewrote the kernel in C (Thompson had attempted this in 1972 but abandoned it). This gave UNIX portability, thereby rewriting the history of operating systems.

- 1974 University of California, Berkeley Introduces UNIX

In 1974, Professor Bob Fabry at the University of California, Berkeley obtained a UNIX source code license from AT&T. Bob Fabry had seen Unix Version 4 at the 1973 Association for Computing Machinery (ACM) Symposium on Operating Systems Principles and intended to bring it to UC Berkeley. In 1979, Bob Fabry established the Computer Systems Research Group (CSRG) at Berkeley. In April 1980, after the Defense Advanced Research Projects Agency (DARPA) signed a contract with Berkeley, CSRG began systematically modifying and improving AT&T Research Unix, calling the modified version "BSD Unix" or "BSD".

- March 9, 1978 1BSD Released

The Berkeley Software Distribution (1BSD), created based on UNIX, was an add-on to Sixth Edition UNIX, not a standalone complete operating system. Approximately 30 copies of this version were distributed.

- May 10, 1979 2BSD Released

The second Berkeley Software Distribution (2BSD) was released in May 1979. It included software updates from 1BSD, as well as two programs newly developed by Bill Joy that are still used on UNIX systems today: the vi text editor (visual version of ex) and Csh. 2BSD was the last BSD version Bill Joy worked on related to PDP-11, with approximately 75 copies distributed.

- April 1980 DARPA Sponsorship

In early 1980, the Defense Advanced Research Projects Agency (DARPA) was looking for an operating system that could aid military projects. Based on the success of 3BSD, Bob Fabry submitted a proposal to DARPA in the fall of 1979, suggesting that Berkeley develop an enhanced version of 3BSD for the DARPA community. In April 1980, DARPA signed an 18-month contract with Berkeley, beginning sponsorship of Berkeley's related work.

- August 1983 4.2BSD Released

After Bill Joy left Berkeley to co-found Sun Microsystems in 1982, 4.2BSD was officially released in August 1983, the first version after Bill Joy's departure. In 1984, the cover of the Unix System Manager's Manual published by USENIX for 4.2BSD first featured the BSD mascot image drawn by John Lasseter, which later became BSD's most widely recognized symbol. Over 1,000 copies of this version were distributed, indicating that a large number of computers were in use.

- June 1988 4.3BSD-Tahoe

As developers gradually phased out the aging VAX platform, 4.3BSD-Tahoe released a version for the Power 6/32 platform (TAHOE). This platform was developed by Computer Consoles Incorporated, codenamed Tahoe; although commercially unsuccessful, it achieved the first BSD port to a non-VAX architecture, separating machine-dependent code from machine-independent code, laying the foundation for subsequent system portability.

- 1991 386BSD and Net/2

Keith Bostic initiated a project to reimplement most standard UNIX software without using AT&T code. This ultimately produced Networking Release 2 (Net/2) — a nearly completely freely distributable operating system. Based on Net/2, BSD was ported to the Intel 80386 architecture in two versions: the free 386BSD developed by William Jolitz and Lynne Jolitz, and the proprietary BSD/386 (later renamed BSD/OS) developed by Berkeley Software Design (BSDi). 386BSD itself was short-lived but became the code base for the NetBSD and FreeBSD projects that started shortly thereafter.

- 1992 USL Lawsuit

BSDi soon became entangled in a legal dispute with AT&T's UNIX System Laboratories (USL) subsidiary, which at the time was the owner of the System V copyright and UNIX trademark. USL's lawsuit against BSDi was filed in 1992 and resulted in an injunction against the distribution of Net/2. The lawsuit was settled in January 1994. One of the settlement conditions was that UC Berkeley acknowledged that three files in Net/2 were "encumbered code" because these files were owned by Novell (which had previously acquired these rights from AT&T) and had to be removed. In exchange, Novell "approved" the code that would be declared unencumbered when 4.4BSD-Lite was released, and strongly recommended that all existing Net/2 users migrate to 4.4BSD-Lite. FreeBSD was among these, and the project was required to stop releasing Net/2-based products by the end of July 1994. Under the agreement terms, the project was allowed to make one final release before the deadline, namely FreeBSD 1.1.5.1. Out of approximately 18,000 files in BSD, only three files needed to be deleted, and another 70 files needed to be modified to add USL copyright notices. This settlement paved the way for the first FreeBSD RELEASE based on 4.4BSD-Lite.

- June 1993 FreeBSD Project Founded

The FreeBSD project originated in early 1993, partly from the ideas of the last three coordinators of the unofficial 386BSD patch kit: Nate Williams, Rod Grimes, and Jordan Hubbard. Their initial goal was to produce an interim snapshot of 386BSD to address issues that the patch kit mechanism could not resolve. The project's early working name was 386BSD 0.5 or 386BSD Interim, reflecting this fact. 386BSD was an operating system developed by Bill Jolitz. The system had been released for nearly a year but had been severely neglected. As the patch kit grew increasingly bloated, they decided to help Bill out of his predicament by providing a 386BSD transition project. However, without clearly providing an alternative, Bill Jolitz suddenly decided to withdraw from the 386BSD transition project, and their plans were forced to stall. The three believed that the project was worthwhile even without Bill's support, so they adopted the name "FreeBSD" coined by David Greenman. To improve FreeBSD's distribution channels, Jordan Hubbard subsequently contacted Walnut Creek CDROM. Walnut Creek CDROM not only supported distributing FreeBSD on CD but also provided a working machine and high-speed internet connection for the project. Without Walnut Creek CDROM's nearly unprecedented trust in this completely unknown project at the time, FreeBSD would likely not have grown to its current scale so quickly. On June 19, 1993, the project was officially named "FreeBSD." The first FreeBSD RELEASE (FreeBSD 1.0) was released in November 1993, based on the 4.3BSD Net/2 (Networking Release 2) tape, and included many components from 386BSD and the Free Software Foundation.

- August 1994 FreeBSD Ports

FreeBSD's Ports and packages provide users and administrators with a simple way to install applications. Ports currently offers up to 37,000 ports. They first appeared in 1994, when Jordan Hubbard committed "port make macros" to FreeBSD's CVS repository, intended to patch his package installation suite **Makefile**.

- November 22, 1994 IPFW

ipfirewall (IPFW) was introduced with FreeBSD 2.0-RELEASE; this firewall using "First Match" rules has since become an important component of the system. ipfw once served as the built-in packet filtering firewall for Mac OS X (versions 10.0 through 10.6; starting from Mac OS X 10.7 "Lion," Apple replaced the default firewall with PF).

- May 1998 Soft Updates

In May 1998, FreeBSD adopted the Soft Updates dependency tracking system. Soft Updates are designed to maintain filesystem metadata integrity by tracking and enforcing dependencies between metadata updates, preventing corruption from crashes or power outages.

- October 17, 1999 First BSD Conference

The first FreeBSD Conference (FreeBSDCon'99) was held in Berkeley, California. Over 350 developers and users from around the world attended the event, marking an important milestone in FreeBSD's popularity and influence.

- March 14, 2000 FreeBSD Jail

FreeBSD Jail was introduced with FreeBSD 4.0, released in early 2000. Jail is an operating system-level virtualization mechanism that allows administrators to partition a FreeBSD system into multiple independent subsystems, each isolated from the others.

- March 28, 2000 FreeBSD Foundation Established

The FreeBSD Foundation is a US-based non-profit organization, registered as a 501(c)(3) institution, dedicated to supporting the FreeBSD project, its development, and its community. Funding comes from individual and corporate donations, used to sponsor developers for specific activities, purchase hardware and network infrastructure, and provide travel stipends for developer summits. The Foundation was founded by Justin Gibbs and others on March 28, 2000.

- July 27, 2000 kqueue(2)

kqueue(2) is an innovative solution designed to replace select/poll, introduced with FreeBSD 4.1-RELEASE on July 27, 2000. This scalable event notification interface shares similar goals and design philosophy with the epoll mechanism later introduced in the Linux kernel.

- September 2000 First Core Team Election

Although a self-appointed Core Team had existed previously, the first Core Team formed through election took place in September 2000. A team of 9 members was appointed at that time, and elections have been held every two years since.

- November 2001 EuroBSDCon

EuroBSDCon 2001 was held from November 9 to 11, 2001, in Brighton, UK (EuroBSDCon. Short History of EuroBSDCon[EB/OL]. [2026-04-18]. <https://2024.eurobsdcon.org/history.html>.). As the global community continued to expand, EuroBSDCon aimed to bring together users and developers working on the BSD operating system family and related projects.

- January 12, 2004 5.2-RELEASE

After version 5.1 experimentally supported the amd64 architecture, 5.2-RELEASE officially provided support for amd64. amd64 became the first 64-bit platform classified as Tier 1.

- March 2004 First AsiaBSDCon; May First BSDCan

Following the success of EuroBSDCon, the first AsiaBSDCon was held from March 13 to 15, 2004, at Academia Sinica in Taiwan, followed by BSDCan, held from May 13 to 16 in Ottawa, Canada. As the FreeBSD community continued to grow, the global demand for BSD-themed conferences also increased.

- November 6, 2004 5.3-RELEASE Ported PF

PF (Packet Filter) was originally designed for OpenBSD, ported to FreeBSD in March 2003, integrated into the base system by Max Laier on February 26, 2004, and released with 5.3-RELEASE.

- November 6, 2004 Libarchive

Libarchive was originally developed for FreeBSD 5.3 and released with that version. It is a library written in C that provides streaming access to multiple different archive formats.

- 2005 Google Summer of Code

The FreeBSD Foundation participated in the inaugural Google Summer of Code. Google Summer of Code began in 2005, providing new developers with an opportunity to participate in open source programming projects. After the program ended, many students who participated became FreeBSD contributors.

- August 2005 First Executive Director

Deb Goodkin joined the Foundation in August 2005 as its first employee, later serving as Executive Director. She previously had over 20 years of work experience in marketing, sales, and development of data storage devices.

- November 1, 2005 New FreeBSD Logo

The project held a Logo design contest, won by the design from Anton K. Gural (still in use today).

- 2005 jemalloc

Jason Evans began conceptualizing jemalloc in early 2004; it was integrated into FreeBSD's libc in September 2005 and became the default memory allocator with FreeBSD 7.0-RELEASE (February 27, 2008), replacing the original phkmalloc. This tool improved FreeBSD's scalability and fragmentation behavior.

- February 2008 ZFS

ZFS was developed by Sun Microsystems starting in 2001 as a system integrating a filesystem and logical volume manager. The system has good scalability and provides strong data integrity protection and efficient data compression. The OpenSolaris version of ZFS was imported into the FreeBSD source tree by Pawel Jakub Dawidek on April 6, 2007, and first entered the FreeBSD system with FreeBSD 7.0-RELEASE released on February 27, 2008.

- January 4, 2009 DTrace

Sun Microsystems developed DTrace, which can be used for real-time debugging of kernel and application issues in production systems. Although the tool was originally developed for Solaris, it became a standard part of FreeBSD with FreeBSD 7.1-RELEASE (released January 4, 2009), with FreeBSD providing comprehensive support for DTrace.

- August 2010 Capsicum

Capsicum is a lightweight operating system capability and sandbox framework that can be used for application sandboxing, decomposing large software architectures into isolated components, and limiting the impact scope of software vulnerabilities. Capsicum was originally developed at the University of Cambridge, with its first paper published at the USENIX Security symposium in August 2010, subsequently released as an optional feature in FreeBSD 9.0, and later became a default feature in FreeBSD 10.0.

- 2012 CHERI

The CHERI (Capability Hardware Enhanced RISC Instructions) project originated from DARPA's CRASH/CTSRD program, which launched in September 2010, conducted by the University of Cambridge in collaboration with SRI International, with principal investigators including Robert N. M. Watson et al. (the same researcher as Capsicum). In March 2012, the first CHERI paper "CHERI: a research platform deconflating hardware virtualization and protection" was presented at the RESoLVE'12 workshop in London. CHERI introduces a hybrid capability model into the CPU architecture domain, achieving fine-grained isolation within a process address space and supporting current software design.

- July 2012 Ports began migrating from CVS to Subversion, completed in February 2013

The Ports collection entered a CVS and SVN dual-track operation period starting in July 2012, and the project officially closed CVS access on February 28, 2013, completing the transition to Subversion.

- August 2012 Poudriere

Poudriere is a tool for testing ports through Jail and building FreeBSD images, developed by Baptiste Daroussin, first released in August 2012.

- November 2012 Clang/LLVM

The LLVM project is a collection of modular and reusable compiler and toolchain technologies. The Clang project provides a C language frontend infrastructure for the LLVM project. FreeBSD set Clang as the default compiler for i386 and amd64 architectures in November 2012, and it was officially delivered with FreeBSD 10.0-RELEASE.

- September–November 2012 Hacker Intrusion (possibly compromised since September 19, discovered on November 11)

The FreeBSD project cluster detected a hacker intrusion; the attack was carried out by stealing developer SSH keys, affecting the third-party package build system but not the base system source code. The project spent several months on auditing and restoration.

- September 17, 2013 OpenZFS Project Launched

The OpenZFS project derived from OpenSolaris. On September 17, 2013, the ZFS open source project announced OpenZFS as the community-led successor to ZFS and created a formal community to sustain development and support. However, at this time FreeBSD was still using the original OpenSolaris ZFS.

- January 20, 2014 pkg Became the Default Package Manager

pkg first appeared in 9.1-RELEASE. In 10.0-RELEASE, pkg became the default package manager, replacing the series of `pkg_*` commands.

- January–February 2014 FreeBSD Journal Inaugural Issue

As the voice of the FreeBSD community and an important channel for understanding the latest FreeBSD releases and developments, the inaugural issue of the FreeBSD Journal was the January/February 2014 issue, focusing on FreeBSD 10. It was initially published under a paid subscription model until January 2019, when the FreeBSD Journal was converted to a free publication, subsequently published on the Foundation's website (providing both HTML and PDF formats).

- June 19, 2017 "FreeBSD Day" Established

International FreeBSD Day is an annual celebration to recognize FreeBSD's pioneering and continued impact on technology and to commemorate the values it carries. On June 15, 2017, the National Day Calendar registrar announced June 19 as National FreeBSD Day.

- 2018 FreeBSD Chinese Community (CFC) Established

Multiple Chinese communities had existed in the millennium era but gradually declined. Some core members of these early communities are still active in the FreeBSD project but no longer focus on Chinese community affairs, having turned to personal careers and family. The FreeBSD Chinese Community (CFC) originally developed from the Baidu Tieba FreeBSD forum.

- April 6, 2021 Migration from Subversion to Git

The FreeBSD project's migration from Subversion to Git began with the DevSummit in May 2019, when a Git working group was established. The src repository was migrated in December 2020, and ports completed migration on April 6, 2021.

- April 13, 2021 Switched from OpenSolaris ZFS to OpenZFS

In 13.0-RELEASE, FreeBSD switched its ZFS implementation from the illumos-based codebase to the unified OpenZFS 2.0 codebase, enabling FreeBSD to receive ZFS upstream improvements more quickly. This migration plan was first proposed in 2018.

- August 2024 German Sovereign Tech Fund Sponsors FreeBSD Project for Infrastructure Modernization

The main objectives of this project were to improve security tools for the base system, Ports, and packages; update project infrastructure to accelerate development, enhance build security, and lower the barrier for new developers to participate. The project started in August 2024 and was completed in December 2025.

- November–December 2024 Laptop and Desktop Working Group (LDWG) Established

The Laptop and Desktop Working Group (LDWG), as its name suggests, is dedicated to making FreeBSD achieve an "out-of-the-box" experience on personal devices through a series of feature improvements and additions. This work plan is scheduled for 1 to 2 years.

- 2024–2025 Alpha-Omega Audit

The Alpha-Omega project audited FreeBSD's bhyve hypervisor and Capsicum sandbox framework to improve the security of the FreeBSD project.

- December 2, 2025 PkgBase Introduced

After ten years of refinement, FreeBSD finally added the PkgBase installation method in 15.0-RELEASE, enabling management of the base system through packages. This approach originally stemmed from the TrueOS project.

## References

- OpenZFS. OpenZFS[EB/OL]. [2026-06-07]. <https://en.wikipedia.org/wiki/OpenZFS>.
- FreeBSD Foundation. Infrastructure Modernization[EB/OL]. (2025-12-19)[2026-06-07]. <https://freebsdfoundation.org/blog/infrastructure-modernization-commissioned-by-the-sovereign-tech-agency/>.
- FreeBSD Foundation. Contributing to FreeBSD Ports with Git[EB/OL]. [2026-06-07]. <https://freebsdfoundation.org/wp-content/uploads/2022/03/mingrone.pdf>.
- Watson R N M, et al. CHERI: A Research Platform Deconflating Hardware Virtualization and Protection[C]//RESoLVE'12 Workshop, London, UK, 2012-03.
- Gunkies. Power 6/32[EB/OL]. [2026-06-07]. <https://gunkies.org/wiki/Tahoe>.
- Laier M. Packet Filter (pf) An Extended Introduction[EB/OL]. (2004-10-21)[2026-06-07]. <https://people.freebsd.org/~mlaier/sucon.pdf>. PF porting report.
- Evans J. A Scalable Concurrent malloc(3) Implementation for FreeBSD[C]//BSDCan, 2006. Implementation of a new memory allocator.
- Dawidek P J. Porting the Solaris ZFS file system to the FreeBSD operating system[C]//AsiaBSDCon, 2007. ZFS porting report.
- National Day Calendar. NEW DAY PROCLAMATION | NATIONAL FREEBSD DAY - June 19[EB/OL]. (2017-06-15)[2026-06-07]. <https://www.nationaldaycalendar.com/proclamations/new-day-proclamation-national-freebsd-day-june-19>. US National Calendar.
- FreeBSD Project. FreeBSD.org intrusion announced November 17th 2012[EB/OL]. (2012-11-17)[2026-06-06]. <https://www.freebsd.org/news/2012-compromise/>. FreeBSD 2012 intrusion incident.
- FreshPorts. ports-mgmt/poudriere-devel[EB/OL]. [2026-06-06]. <https://www.freshports.org/ports-mgmt/poudriere-devel/>. Port **poudriere** added on 2012-08-16.
- FreeBSD Foundation. Development Project Update: Toolchain Modernization[EB/OL]. (202007)[2026-06-06]. <https://freebsdfoundation.org/wp-content/uploads/2020/07/Development-Project-Update-Toolchain-Modernization.pdf>.
- Ritchie D M. The Evolution of the Unix Time-sharing System[EB/OL]. (1984)[2026-06-07]. <https://www.read.seas.harvard.edu/~kohler/class/aosref/ritchie84evolution.pdf>. Records "In early 1970 we proposed acquisition of a PDP-11".
- Multicians. GE-635s at Project MAC and BTL[EB/OL]. [2026-06-07]. <https://dps8m.gitlab.io/w3/multicians.org/ge635.html>.
- FreeBSD Foundation. Timeline[EB/OL]. [2026-03-25]. <https://freebsdfoundation.org/freebsd/timeline/>. FreeBSD development history timeline.
- McKusick M K, Neville-Neil G V, Watson R N M. The Design and Implementation of the FreeBSD Operating System[M]. Chen Xiangqun, Guo Lifeng, Ye Shunping, trans. 2nd ed. Beijing: China Machine Press, 2021: VI. Records "In 1995, the OpenBSD group split from the NetBSD group".
- FreeBSD Foundation. Staff[EB/OL]. [2026-04-16]. <https://freebsdfoundation.org/about-us/our-team/>. Records Deb Goodkin joined the Foundation in August 2005 as the first employee.
- SeaGL. 25+ Years of FreeBSD and Why You Should Get Involved![EB/OL]. (2019-11-15)[2026-04-16]. <https://osem.seagl.org/conferences/seagl2019/program/proposals/611>. Records Deb Goodkin "joining as the first employee back in August 2005".
- FreeBSD Wiki. Jails[EB/OL]. [2026-04-16]. <https://wiki.freebsd.org/Jails>. Records "Jails were introduced by Poul-Henning Kamp in March 2000 with FreeBSD 4.0-RELEASE".
- Watson R N M, et al. CHERI: A Hybrid Capability-System Architecture for Scalable Software Compartmentalization[C]//ISCA. 2015. Original CHERI paper, describing the design and implementation of hardware capability architecture extensions.
- ACM. Fernando J ("Corby") Corbato[EB/OL]. [2026-04-17]. <https://amturing.acm.org/award_winners/corbato_1009471.cfm>. Records CTSS first demonstrated on IBM 709 in November 1961.
- Tom Van Vleck. The Multicians web site[EB/OL]. (2026-04-08)[2026-04-17]. <https://multicians.org/history.html>. Records Multics project history; General Electric signed on in August 1964; Bell Labs joined in November 1964.
- FreeBSD Project. Core Bylaws[EB/OL]. [2026-04-17]. <https://www.freebsd.org/internal/bylaws/>. Records the first Core Team election held in September 2000.
- FreeBSD Foundation. Resolutions Document[EB/OL]. [2026-04-17]. <https://freebsdfoundation.org/wp-content/uploads/2015/12/ResolutionsDocument-1.pdf>. Records the FreeBSD Foundation founding document signed on March 28, 2000.
- FreeBSD Project. FreeBSD News Flash October 1999[EB/OL]. [2026-04-17]. <https://ftpmirror.your.org/pub/FreeBSD-CVS/www/data/news/1999/index.html>. Records FreeBSDCon'99 attendance exceeding 350 people.
- USENIX. Announcing the USENIX AsiaBSDCon and Request for Papers[EB/OL]. (2003-10)[2026-04-17]. <https://web.archive.org/web/20041217153402/https://www.bsdnewsletter.com/2003/10/News107.html>. Records the first AsiaBSDCon held from March 13 to 15, 2004, at Academia Sinica in Taiwan.
- IEEE Computer Society. Linus Torvalds[EB/OL]. [2026-04-17]. <https://www.computer.org/profiles/linus-torvalds/>. Records Linus Torvalds enrolled at the University of Helsinki in 1988, later earning a master's degree in computer science.
- Evans J. A Scalable Concurrent malloc(3) Implementation for FreeBSD[C]//BSDCan, 2006. Records the design and implementation of jemalloc, integrated into FreeBSD libc in 2005.
- Evans J. jemalloc Background[EB/OL]. [2026-04-17]. <https://github.com/jemalloc/jemalloc/wiki/Background>. Records jemalloc initially conceptualized in early 2004, integrated into FreeBSD in September 2005.
- OpenZFS. History[EB/OL]. [2026-04-17]. <https://www.openzfs.org/wiki/History>. Records ZFS development began in 2001, ported and released with FreeBSD 7.0 in 2008.
- FreeBSD Project. FreeBSD 7.0-RELEASE Announcement[EB/OL]. (2008-02-27)[2026-04-17]. <https://www.freebsd.org/releases/7.0R/announce/>. FreeBSD 7.0-RELEASE release date was February 27, 2008.
- FreeBSD Project. FreeBSD 7.1-RELEASE Announcement[EB/OL]. (2009-01-04)[2026-04-17]. <https://www.freebsd.org/releases/7.1R/announce/>. FreeBSD 7.1-RELEASE release date was January 4, 2009; DTrace was introduced with this version.
- Watson R N M, Neumann P G, Woodruff J, et al. CHERI: A Research Platform Deconflating Hardware Virtualization and Protection[C]//RESoLVE'12 Workshop, London, UK, 2012-03. First CHERI paper.
- SRI International. CTSRD Final Technical Report[EB/OL]. [2026-04-17]. <https://web.archive.org/web/20251122133531/https://www.csl.sri.com/~neumann/20191213-ctsrd-ftr-final.pdf>. Records CTSRD/CHERI project began on September 24, 2010.
- MCKUSICK M K. Twenty Years of Berkeley Unix: From AT&T-Owned to Freely Redistributable[M]//SALUS P H, ed. Open Sources: Voices from the Open Source Revolution. Sebastopol: O'Reilly, 1999. Records DARPA contract began in April 1980 for 18 months.
- FreeBSD Core Team. Change to FreeBSD release scheduling and support period[EB/OL]. (2024-07-10)[2026-04-18]. <https://lists.freebsd.org/archives/freebsd-announce/2024-July/000143.html>. Starting from FreeBSD 15, the stable branch support period was shortened from 5 years to 4 years.
- USL v. BSDi Settlement Agreement[EB/OL]. (1994-02-04)[2026-04-17]. <https://web.archive.org/web/20240613194116/https://www.bell-labs.com/usr/dmr/www/bsdi/USLsettlement.pdf>. Records that AT&T code still remained in Net/2; only after the settlement was 4.4BSD-Lite able to completely remove it.
- FreeBSD Foundation. Navigating FreeBSD's New Quarterly and Biennial Release Schedule[EB/OL]. (2024-07-16)[2026-04-17]. <https://freebsdfoundation.org/blog/navigating-freebsds-new-quarterly-and-biennial-release-schedule/>. Records that starting from 15.x, the maintenance period was shortened from 5 years to 4 years, with major version releases every 2 years.
- FreeBSD Project. FreeBSD 5.2-RELEASE Announcement[EB/OL]. (2004-01-12)[2026-04-17]. <https://www.freebsd.org/releases/5.2R/announce/>. FreeBSD 5.2-RELEASE release date was January 12, 2004; amd64 became a Tier 1 architecture.
- FreeBSD Project. FreeBSD 5.3-RELEASE Announcement[EB/OL]. (2004-11-06)[2026-04-17]. <https://www.freebsd.org/releases/5.3R/announce/>. FreeBSD 5.3-RELEASE release date was November 6, 2004; PF and Libarchive were both released with this version.
- Libarchive Project. LibarchiveUsers[EB/OL]. [2026-04-17]. <https://github.com/libarchive/libarchive/wiki/LibarchiveUsers>. Records "libarchive was originally developed for FreeBSD; it was first released with FreeBSD 5.3 in November 2004".
- Max Laier. Packet Filter (pf) An Extended Introduction[EB/OL]. (2004-10-21)[2026-04-17]. <https://web.archive.org/web/20250503030531/https://people.freebsd.org/~mlaier/sucon.pdf>. Presentation by PF porter Max Laier; PF was ported to FreeBSD in March 2003 and integrated into the FreeBSD base system on February 26, 2004.

## Exercises

1. Run FreeBSD 1.0 in QEMU, record the significant differences from contemporary FreeBSD during the boot process, and analyze what these differences reflect about the evolution of system design.
