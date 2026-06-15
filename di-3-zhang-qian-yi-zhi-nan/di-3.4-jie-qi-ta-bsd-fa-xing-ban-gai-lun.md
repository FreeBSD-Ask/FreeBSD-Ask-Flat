# 3.4 Overview of Other BSD Distributions

## NetBSD Project Overview

This section introduces another member of the BSD family: NetBSD. NetBSD is an open source, portable UNIX-like operating system. As an important member of the BSD family, it originated from 4.3BSD Net/2 and 386BSD, with its first version 0.8 released on April 19, 1993 (subsequently incorporating changes from 4.4BSD-Lite).

NetBSD's academic lineage can be traced back to the Berkeley Software Distribution (BSD) project at the University of California, Berkeley, and it holds a place in the field of operating system portability research.

This section introduces NetBSD's technical characteristics, ecosystem, and practical applications.

### Technical Characteristics and Ecosystem

NetBSD's [slogan](https://www.netbsd.org/about/portability.html) is "Of course it runs NetBSD," reflecting its technical pursuit of cross-platform compatibility.

NetBSD supports [multiple architectures](https://wiki.netbsd.org/ports/). Among these, Tier I architectures are platforms with official strategic priority support, and Tier II architectures are platforms maintained through community-driven evolution. Currently, NetBSD supports 8 Tier I architectures and 49 Tier II architectures; its breadth of architecture support ranks among the top in similar operating systems.

NetBSD primarily targets technology enthusiasts and developers, presenting a certain learning curve for ordinary users; developers are the core users of this system.

The [pkgsrc](https://www.pkgsrc.org/) package management framework developed by NetBSD also supports multiple operating systems including macOS and Linux. pkgsrc achieves cross-platform software management through portable build scripts, embodying NetBSD's portability design philosophy.

NetBSD provides a Linux compatibility layer that can run some Linux binary programs, primarily supporting command-line tools and basic graphical applications.

In terms of hardware drivers, NetBSD includes i915 graphics drivers and AMD-related drivers, and supports UEFI boot and NVMe storage devices.

NetBSD roughly supports NVIDIA graphics cards up to the Pascal architecture (GeForce GTX 10 series). Related information can be found at [nouveau / NetBSD](https://nouveau.freedesktop.org/NetBSD.html) and [nouveau(4) - NetBSD Manual Pages](https://man.netbsd.org/nouveau.4) (specific hardware support lists).

### Project Support Channels

The simplest way to support the NetBSD project is by donating through [GitHub Sponsors](https://github.com/sponsors/netbsd). ~~You can also get a GitHub badge~~ [~~Public Sponsor~~](https://github.com/orgs/community/discussions/19916)~~.~~

> **Tip**
>
> After payment, the payment method will be bound. To unbind, you can contact GitHub customer support by [submitting a ticket](https://support.github.com/), which is typically processed within one business day.

You can also donate via [Donate using Stripe](https://www.netbsd.org/stripe.html), which supports China UnionPay, Google Pay, and other payment methods.

### ZFS on NetBSD

ZFS (Zettabyte File System), as a feature-complete enterprise-grade file system, also has an implementation on NetBSD. The following are related resources:

* Manual page [zfs(8) - NetBSD Manual Pages](https://man.netbsd.org/zfs.8), providing official command reference for the ZFS file system
* [Finish ZFS](https://wiki.netbsd.org/projects/project/zfs/), ZFS porting project progress report
* [Google Summer of Code 2007](https://developers.google.com/open-source/gsoc/2007?hl=zh-cn), documenting the early stages of the ZFS introduction plan
* [Google Summer of Code zfs-port project](https://blog.netbsd.org/tnf/entry/google_summer_of_code_zfs), official technical report on the ZFS porting project
* [Root On ZFS](https://wiki.netbsd.org/root_on_zfs/), providing a ZFS root partition installation guide
* [NetBSD zfs Wiki](https://wiki.netbsd.org/zfs/), NetBSD ZFS wiki, documenting the process of introducing ZFS code into NetBSD — initially ported from OpenSolaris, imported into the NetBSD base system in 2009, and in 2018 the ZFS codebase was re-based on FreeBSD's OpenZFS implementation for a more modern ZFS experience

In the NetBSD source code, the earliest visible ZFS commit is [Import Opensolaris source code used with zfs port. Zfs code si from date](https://github.com/NetBSD/src/commit/c1cb2cd89c023350f357f813e12b526f6f71002f) (2009). NetBSD ZFS code was initially ported from OpenSolaris, imported into the base system in 2009, and re-based on FreeBSD's OpenZFS implementation in 2018. As of 2026, ZFS code continues to be maintained (e.g., regular commits are visible under the src/external/cddl/osnet path), but there are several known issues (such as insufficient kernel stack space on some architectures, excessive ARC memory usage, etc.).

### References

* The NetBSD Foundation. Information about NetBSD 0.8[EB/OL]. [2026-04-14]. <https://www.netbsd.org/releases/formal-0.8/>. Official NetBSD project release page. This page records that NetBSD 0.8 was released on April 19, 1993.
* The NetBSD Foundation. The History of the NetBSD Project[EB/OL]. [2026-04-18]. <https://www.netbsd.org/about/history.html>. Official NetBSD project history, recording that NetBSD originated from 4.3BSD via Net/2 and 386BSD.
* The NetBSD Foundation. NetBSD Portability[EB/OL]. [2026-04-14]. <https://www.netbsd.org/about/portability.html>. Official NetBSD portability description, elaborating on cross-platform support strategy.
* The NetBSD Foundation. Platforms Supported by NetBSD[EB/OL]. [2026-04-18]. <https://wiki.netbsd.org/ports/>. Official NetBSD architecture support page, currently with 8 Tier I architectures and 49 Tier II architectures.
* The NetBSD Foundation. pkgsrc -- The NetBSD Packages Collection[EB/OL]. [2026-04-14]. <https://www.pkgsrc.org/>. Official pkgsrc package manager framework site, providing cross-platform software management solutions.
* The NetBSD Foundation. zfs(8) -- NetBSD Manual Pages[EB/OL]. [2026-04-14]. <https://man.netbsd.org/zfs.8>. ZFS file system command reference manual page on NetBSD.
* The NetBSD Foundation. NetBSD ZFS Wiki[EB/OL]. [2026-04-14]. <https://wiki.netbsd.org/zfs/>. NetBSD ZFS wiki, documenting ZFS porting progress and usage guides.
* Nouveau Wiki. Feature Matrix[EB/OL]. [2026-04-16]. <https://nouveau.freedesktop.org/FeatureMatrix.html>. Nouveau driver feature support list, listing feature comparisons across GPU architectures.
* Nouveau Project. CodeNames[EB/OL]. [2026-04-17]. <https://nouveau.freedesktop.org/CodeNames.html>. Nouveau GPU architecture and codename mapping table.

## OpenBSD Project Overview

OpenBSD is an important member of the BSD family. Since its inception in 1995, it has consistently adhered to a "security first" design philosophy, and its development is closely linked to the open source movement, network security, and software engineering methodology.

### Selected Quotes

#### Theo de Raadt (OpenBSD Founder)

> The Linux people do this because they hate Microsoft. We do this because we love Unix.

> This thing is garbage (referring to Linux), and everyone is still using it, completely unaware of how big the problems are. Those Linux folks don't think about stopping and fixing things properly; they just keep using it and piling on more features. Nobody is willing to say: "This thing is really garbage; we need to reinvent the wheel."

> I occasionally browse DragonFly BSD's code to see if there's anything OpenBSD can use, but so far I haven't found anything.

> Our biggest problem is that people will slap GPL on our code and exclude us in the same way (here referring to redistributing BSD code under the GPL, which means the code can no longer be merged back into the original BSD project). ...We have many companies that consistently contribute code back. But once code is GPL'd, we can no longer access it.

#### Linus Torvalds (Linux Founder)

> I think the OpenBSD crowd is a bunch of masturbating monkeys, making a huge deal out of their "we focus on security" thing.

### Official OpenBSD Introduction

OpenBSD is a fully functional multi-platform UNIX-like operating system based on Berkeley Networking Release 2 (Net/2) and 4.4BSD-Lite. Although there are multiple operating systems in this family, OpenBSD stands out for its priority on "security" and "correctness." The OpenBSD team strives to achieve a "secure by default" state, meaning OpenBSD users can be confident that a newly installed system will not be easily compromised. OpenBSD achieves this through proactive security strategies.

Security vulnerabilities are essentially errors in design or implementation. The OpenBSD team focuses not only on writing new code but also on discovering and fixing existing design flaws and implementation vulnerabilities, making the OpenBSD system not only more secure but also more stable. The source code of all critical system components has been audited to prevent issues such as remote access, local access, denial of service, data corruption, and information gathering.

In addition to fixing vulnerabilities, OpenBSD integrates strong cryptographic functionality into the base system. OpenBSD provides a fully featured IPsec implementation and supports common protocols such as SSL and SSH. At the same time, OpenBSD includes built-in network filtering and monitoring tools, such as packet filtering, NAT, and bridging, as well as various routing services such as BGP and OSPF. To meet high-performance requirements, support for hardware cryptography has also been added. Although security is often considered to involve trade-offs with usability, OpenBSD provides as many security options as possible so that users can enjoy secure computing without feeling burdened.

Since OpenBSD originates from Canada, its cryptographic components (such as OpenSSH and IPsec) can be exported worldwide without restriction.

(Note: If OpenBSD enters the United States, it cannot be re-exported from the US. Therefore, if you are located outside of Canada and the United States, do not obtain releases from mirror servers located in the US.)

### OpenBSD Overview

OpenBSD is also a UNIX-like computer operating system, born in 1995, forked from NetBSD by South African-born Canadian programmer Theo de Raadt (who was also one of the founders of NetBSD). According to extant [NetBSD mailing list](https://mail-index.netbsd.org/netbsd-users/1994/12/23/0000.html) records, he was removed from the core team due to inappropriate language and personal attacks against others.

> On December 20th (1994), the remaining core members of the NetBSD project asked Theo de Raadt to step down. This was an extremely difficult decision, the reasons for which were Theo's long-standing discourteous and abusive behavior toward NetBSD users and developers. We believe that such behavior should not be exhibited by representatives of the NetBSD project, and that this behavior has been detrimental to NetBSD as a whole.
>
> This decision was difficult because Theo had long made positive contributions to NetBSD. He was the primary maintainer of NetBSD SPARC support and wrote countless amounts of code. We are certainly willing to accept (and would very much like to see) Theo continue to contribute to NetBSD in the future, but we believe he is no longer suitable to continue as an "official" representative of NetBSD.

OpenBSD releases a new version every 6 months.

OpenBSD is widely regarded as the most secure operating system in the world. OpenBSD's slogan is "Only two remote holes in the default install, in a heck of a long time!"

OpenBSD uses the LLVM/Clang project build system, with ksh as the default shell (based on pdksh, not David Korn's original ksh88/ksh93). OpenBSD's mascot is a pufferfish: Puffy.

Compared to other BSD systems, OpenBSD's design orientation leans more toward security (for example, removing the Linux compatibility layer, replacing sudo with doas, and [limiting hyperthreading](https://marc.info/?l=openbsd-tech&m=153504937925732&w=2) by default). User experiences with this vary (the overall system performance is relatively conservative, such as the package manager running slowly); after all, one consequence is that there is relatively less software — not only far behind FreeBSD, but even somewhat less than NetBSD. However, with very limited human and material resources, OpenBSD maintains support for many architectures including amd64/i386, arm64/armv7, and riscv64, making it a genuinely general-purpose operating system. To expand its desktop user base, it also packages desktop environments such as GNOME, MATE, and Xfce, along with a large number of software applications including Blender, Firefox, Krita, and LibreOffice. In the future, OpenBSD is expected to optimize performance while maintaining its security features, expanding its use cases.

On the Chinese internet, OpenBSD is known to many because of a deeply moving news story — in 2014, [the OpenBSD project faced the risk of shutting down due to unpaid electricity bills](https://marc.info/?l=openbsd-misc&m=138972987203440&w=2), yet the software developed by OpenBSD is used by almost everyone who uses the internet at every moment — OpenSSH is a subproject of OpenBSD. This is also common; the entire open source ecosystem is supported by a small number of projects, which may have fewer than a handful of maintainers, yet they often do not receive the recognition they deserve while fulfilling their open source mission. The crisis was ultimately resolved by a $20,000 donation from Romanian Bitcoin entrepreneur Mircea Popescu. Subsequently, multiple companies and individuals, including [Smartisan Technology](https://undeadly.org/cgi?action=article&sid=20161123193708) (led by Luo Yonghao) in China, also made donations to the OpenBSD Foundation.

Many people have deep misunderstandings about OpenBSD, believing that insufficient funding or manpower has affected its development progress, or that it only focuses on security while neglecting other aspects. But this is not the case; OpenBSD has modernized its drivers (DRM synced to Linux 6.12.50, supporting Intel AX200/AX210 and other Wi-Fi 6/6E adapters via the [iwx driver](https://man.openbsd.org/iwx.4) — though note that this driver currently only operates in 802.11ac mode and has not yet implemented 802.11ax protocol functionality), and also supports [UFS](https://man.openbsd.org/ufshci.4) (Universal Flash Storage). This shows that while funding and manpower are important, they are not the decisive factors. Whether this world is composed of vast makeshift operations or of academic elites is always a question worth pondering.

OpenBSD's package management policy is relatively strict; outdated packages are removed. For example, users can hardly find a usable graphical display manager (except GDM).

For NVIDIA graphics card support on OpenBSD, the `nv` driver roughly stops at the GT200 series from around 2009, while the `nouveau` driver supports up to the Pascal architecture (GeForce GTX 10 series).

#### File System

**FFS (Berkeley Fast File System)** is a high-performance UNIX file system first introduced in 4.2BSD in 1983, improving file I/O performance by optimizing data block layout and inode design.

The FFS file system used by default in OpenBSD belongs to the same family as FreeBSD's UFS in terms of implementation. They maintain consistency in mount parameters.

#### References

- nv - NVIDIA video driver[EB/OL]. [2026-03-26]. <https://man.openbsd.org/nv.4>. `nv` driver specific hardware support list.
- nouveau - NVIDIA video driver[EB/OL]. [2026-03-26]. <https://man.openbsd.org/nouveau.4>. `nouveau` driver specific hardware support list.

- FreeBSD Project. ffs -- Berkeley fast file system[EB/OL]. [2026-04-02]. <https://man.freebsd.org/cgi/man.cgi?query=ffs&sektion=7>. Detailed introduction to the technical specifications and implementation principles of the FFS file system, providing reference for system file system design.
- Leffler S J. Changes to the Kernel in 4.2BSD[R]. University of California, Berkeley, 1983-07-25. Documents kernel changes in 4.2BSD; FFS was first introduced in this version. The official release date of 4.2BSD was August 1983; see MCKUSICK M K. Twenty Years of Berkeley Unix[M]//SALUS P H, ed. Open Sources: Voices from the Open Source Revolution. Sebastopol: O'Reilly, 1999.
- The Register. Romanian Bitcoin baron stumps up $20k to keep OpenBSD's lights on[EB/OL]. (2014-01-20)[2026-04-17]. <https://www.theregister.com/2014/01/20/openbsd_bailed_out/>. Documents the 2014 electricity bill crisis resolved by Mircea Popescu's $20,000 donation.
- OpenBSD Project. iwx(4) Device Drivers Manual[EB/OL]. [2026-04-16]. <https://man.openbsd.org/iwx>. OpenBSD iwx driver manual page, noting that the driver does not support 802.11ax functionality.
- OpenBSD Project. OpenBSD 7.8 Release[EB/OL]. (2025-10-22)[2026-04-16]. <https://www.openbsd.org/78.html>. OpenBSD 7.8 release notes, DRM synced to Linux 6.12.50.
- OpenBSD Project. OpenBSD FAQ - Introduction to OpenBSD[EB/OL]. [2026-04-17]. <https://www.openbsd.org/faq/faq1.html>. OpenBSD official FAQ, recording that the default shell is ksh (based on pdksh).

### Opportunities and Challenges

#### Community and Collaboration

After communicating with several OpenBSD maintainers and the community, it was found that some maintainers hold relatively firm positions during technical discussions. For example, a user reported that KDE 5 could not start properly and displayed a black screen; after months of troubleshooting, the issue was found to actually occur in a virtual machine environment, particularly when UEFI was enabled.

Another issue involves the handling of the EFI partition. The default partition layout created by the OpenBSD installer may not be optimal in some scenarios, especially when only 100 GB of disk space is available and careful planning is needed. The default partitioning divides space quite finely; in practice, for example, running `pkg_add gnome` requires reserving sufficient space. In earlier versions, the system did not allow users to customize the EFI partition during installation: even if users created an EFI partition themselves, the system could not recognize it. Users had to first let the installer create the default EFI partition, then manually delete other partitions, keeping only the EFI partition, before proceeding with custom partition setup. This aspect was not improved until version 7.6.

The OpenBSD installer also has room for improvement in network handling and error recovery. For example, drivers need to be downloaded online during installation, but the error handling mechanism for download failures could be more robust; if a user tries to interrupt with Ctrl+C, the system may crash directly, requiring a restart of the installation; in an offline state, the installer may also stall at a certain step and be unable to continue.

The above issues have been reported to the relevant developers. Technical discussions were also held regarding the KDE 5 port.

#### Development Methods and Collaboration Processes

OpenBSD still uses traditional development methods, primarily collaborating through mailing lists. The project does not accept Pull Requests (PRs) on GitHub. The entire project still uses CVS (Concurrent Versions System) for version control, and CVS has limited support for HTTP proxies. For many modern developers, SVN (Subversion) may already be unfamiliar, let alone the even older CVS workflow. This development approach has caused some difficulties for new developers.

OpenBSD's source code largely lacks detailed comments and documentation, making it difficult for beginners to directly understand and learn from it.

OpenBSD's bug reporting lacks a unified tracking and reporting platform; feedback is primarily submitted through mailing lists, where information can easily be lost.

OpenBSD's release notes structure and readability need improvement; the content presentation is relatively brief and difficult to read directly. There is no wiki or similar platform to track the development progress of specific sub-projects.

#### Unstable ABI

**ABI (Application Binary Interface)** defines the interaction specifications between compiled binary code and the operating system kernel, including system call conventions, data structure layouts, and function calling mechanisms. ABI stability is a key factor in ensuring cross-version binary compatibility.

According to feedback from Rust maintainers ([OpenBSD support](https://github.com/rust-lang/rustup/issues/2168)), OpenBSD's ABI is highly unstable, in stark contrast to the Linux kernel's strict maintenance of user-space ABI backward compatibility. As a result, the Rust team cannot provide pre-compiled Rust toolchains to OpenBSD users via `rustup`.

Currently, OpenBSD users can only obtain the Rust toolchain through Ports or pull the source code to compile it themselves.

> OpenBSD does not attempt to maintain backward compatibility. At any given time, OpenBSD only supports two stable releases, along with a frequently updated development version called `-current`. Typically, binaries from an older stable release cannot run on a newer stable release. There is also no guarantee that a `-current` binary built last week will run correctly on this week's `-current`.

Some may consider cross-version incompatibility to be normal, but it is important to note OpenBSD's frequent release cycle.

#### Security Considerations

In OpenBSD, some security models or frameworks common in modern operating systems are not yet supported:

* No support for Secure Boot and TPM
* The build process for the system and packages is not sufficiently clear and transparent, and the technology stack is relatively outdated (e.g., CVS maintenance is clearly insufficient)
* Almost no compliance certifications have been performed, not even CIS benchmarks
* No support for Access Control Lists (ACL), nor Mandatory Access Control (MAC) frameworks
* No runtime integrity protection provided
* No support for Multi-Level Security (MLS) or Biba integrity models
* Lack of Role-Based Access Control (RBAC) mechanisms
* Lack of cutting-edge or authoritative academic papers verifying its security
* No independent external security audits conducted
* No security certifications of any kind obtained

A study from several years ago showed that the number of code vulnerabilities in OpenBSD has been decreasing year by year, and discovered vulnerabilities are typically difficult to exploit for attacks. (J. Shi, Zou D, Xu S, Deng X, Jin H. Does OpenBSD and Firefox's Security Improve With Time?[J]. IEEE Transactions on Dependable and Secure Computing, 2023, 20(4): 2781-2793. doi: 10.1109/TDSC.2022.3153325. This study verified the continuous improvement of open source operating system security protection capabilities through time series analysis.)

OpenBSD's security policy is based on the judgment that hyperthreading mechanisms may introduce potential security risks. Therefore, rather than patching microcode, it directly disables hyperthreading functionality. Whether its insights into security models and security philosophy are profound requires more research to verify.

> **Discussion Question**
>
>> Some argue: "If any operating system adopted security policies as aggressive and strict as OpenBSD, its security would surpass OpenBSD. But is this really the case? Android's security policies are considered the most comprehensive among all operating systems, combining both hardware and software. Yet, what is the reality?"
>>
>> What do you think?

#### Donating to OpenBSD

[Donating to the Foundation](https://www.openbsdfoundation.org/donations.html) is somewhat difficult for users in mainland China because the foundation only accepts international PayPal, with the message "Donations to this recipient are not supported in this country or region." This issue has been reported via email, and a registered letter was once sent to the OpenBSD Foundation in Canada, but no response was received. Even using GitHub Sponsors is possible, but after multiple communications, support was still not provided. This is deeply inconvenient.

Currently, mainland China users can donate to the OpenBSD Foundation by initiating a transfer via the domestic version of PayPal or a regular UnionPay savings card to the PayPal account `obsd-paypal@openbsdfoundation.org` (this account is for receiving payments, not an email address). For specific operation methods, please search for "PayPal personal transfer" on your own.

### Appendix: The OpenBSD IPsec Stack FBI Backdoor Incident

In 2010, Theo de Raadt received and [made public an email](https://marc.info/?l=openbsd-tech&m=129236621626462&w=2):

> I received an email about the initial development of the OpenBSD IPSEC stack. Someone alleged that some former developers (and the companies they worked for) accepted funding from the US government to plant backdoors in our network stack, particularly the IPSEC stack — approximately during the 2000–2001 period.
>
> Since this was the first freely usable IPSEC stack we had, this code now appears in many other projects and products. Over the past decade or more, the IPSEC code has undergone many changes and fixes, so it is currently unclear how much actual impact these allegations have.
>
> This email was sent privately by someone I had not been in contact with for over a decade. I refuse to participate in this conspiracy, and I will not discuss this matter with Gregory Perry. Therefore, I have decided to make this letter public so that:
>
> (a) People using this code can audit it for such issues;
>
> (b) People who are angered by these claims can take other actions;
>
> (c) If these claims are untrue, the accused individuals can defend themselves.
>
> I also do not appreciate having my private emails forwarded. But comparatively speaking, the "minor transgression" of leaking private emails falls far short of the "major transgression" of government-funded companies buying off open source developers (members of our community) to plant privacy-violating backdoors in software.
>
>> From: Gregory Perry <Gregory.Perry@GoVirtual.tv>
>>
>> To: "<deraadt@openbsd.org>" <deraadt@openbsd.org>
>>
>> Subject: OpenBSD Crypto Framework
>>
>> Date: Sat, 11 Dec 2010 23:55:25 +0000
>>
>> Hello Theo,
>>
>> Long time no see. You may recall that I was the CTO of NETSEC in the earlier days and secured some funding and donations for the OpenBSD crypto framework. At the same time, I also provided consulting services to the FBI, specifically at their GSA Technical Support Center, working on a cryptographic reverse engineering project aimed at implanting backdoors and key escrow mechanisms into smart cards and other hardware-based computing technologies.
>>
>> My NDA with the FBI has recently expired, and I want you to know that the FBI planted a series of backdoors and side-channel key leakage mechanisms in the OpenBSD crypto framework, with the explicit purpose of monitoring the site-to-site VPN encryption systems deployed by the US Executive Office for US Attorneys (which falls under the US Department of Justice, the FBI's parent agency). Jason Wright and several other developers were responsible for implementing these backdoors. You should thoroughly review all code committed by Wright, as well as code commits from other NETSEC-origin developers who worked with him.
>>
>> This is likely also the reason you lost DARPA funding back then — they probably detected the existence of these backdoors and did not want to develop any derivative products based on this code.
>>
>> This is also why some people within the FBI have recently been strongly advocating the use of OpenBSD for VPN and firewall deployments in virtualized environments. For example, Scott Lowe is a well-respected author in the virtualization field who happens to be an FBI employee. He recently published several tutorials on using OpenBSD virtual machines in enterprise VMware vSphere deployments.
>>
>> Merry Christmas...
>>
>> Gregory Perry
>>
>> Chief Executive Officer
>>
>> GoVirtual Education
>>
>> "VMware Training Products and Services"
>>
>> Phone: 540-645-6955 ext. 111 (local)
>>
>> Toll Free: 866-354-7369 ext. 111
>>
>> Mobile: 540-931-9099
>>
>> Fax: 877-648-0555

Does the operating system that claims to be the most secure in the world contain an FBI (Federal Bureau of Investigation, responsible for intelligence and security affairs) backdoor? The OpenBSD project quickly audited the existing code and found no backdoors.

A week later, Theo de Raadt published another letter: [Update on OpenBSD IPsec backdoor allegations](https://lwn.net/Articles/420858/).

> Update on OpenBSD IPsec backdoor allegations
>
> From: Theo de Raadt <deraadt-AT-cvs.openbsd.org>
>
> To: Kurt Knochner <cdowlker-AT-googlemail.com>
>
> Subject: Re: Allegations regarding OpenBSD IPsec
>
> Date: Tue, 21 Dec 2010 12:34:54 -0700
>
> Cc: tech-AT-openbsd.org
>
> No "leads" (true or false),
>
> Indeed, these allegations provided no facts pointing to specific code.
>
> My current view is roughly as follows:
>
> (a) NETSEC was a company engaged in a specialized business near Washington D.C., accepting contracts from certain government departments for security and counter-security work.
>
> (b) Background: 1999–2001 was a special period when many US government departments were relaxing restrictions on encryption technology, as encryption technology was transferred from the Department of Defense (DOD) to the Department of Commerce so it could be exported "under certain restrictions"; the result was that private-sector use of encryption was about to explode, so the government was not only inventing technical means to continue eavesdropping but also inventing "justifications" (they had always been addicted to surveillance).
>
> (c) Gregory Perry worked at NETSEC and was responsible for interviewing and hiring Jason (who had just graduated); by the time Jason started working, Perry had already been "expelled" from the company for unknown reasons.
>
> (d) Jason did not specifically work on cryptography; he primarily wrote device drivers, but since the ipsec layer also involved IPCOMP, he also modified ipsec layer code. That is, he worked on the data flow part, not the algorithm part.
>
> (e) After Jason left, Angelos accepted a NETSEC contract (he had been working on the ipsec stack for 4 years and was the architect and primary developer) and wrote the crypto layer while traveling around the world, enabling the ipsec stack to pass requests to the drivers Jason had previously written. This crypto layer included a poorly conceived and insecure "half-IV" idea that was promoted by the US government at the time. After the contract ended, this code was removed. Shortly thereafter, the CBC oracle issue was also published, and the crypto/ipsec code began transitioning to random IVs (before that, this may not have been feasible because we lacked high-quality, fast pseudo-random number generators like arc4random). I do not believe these two issues, or other undiscovered issues, were malicious. The issues we have uncovered so far are all products of a specific historical period.
>
> (f) Jason and Angelos wrote a lot of code in many parts of the system we rely on daily, not just the ipsec stack. I forwarded the allegations naming them, but I still find it difficult to personally accuse them. Please see items (a)-(c) in my original email.
>
> (g) I believe NETSEC was likely required by contract to write backdoors, as alleged.
>
> (h) If these backdoors were indeed written, I do not believe they entered our source tree. They may have been deployed as NETSEC's own products.
>
> (i) If these NETSEC projects existed, I do not know whether Jason, Angelos, or others were aware of or involved in them.
>
> (j) If Jason and Angelos knew NETSEC was doing such things, I would have expected them to tell me. The project and I might have made some adjustments based on this situation; I'm not sure exactly what adjustments. From this perspective, I feel Jason's email was not entirely candid.
>
> (k) I am glad people are taking this opportunity to audit this important part of the source tree — for too long, many people assumed it was "secure."
>
> Where do you start auditing the code? There is so much code.
>
> Actually, this is only a small part of the source tree. If each of us does our part, things will improve. It still won't be perfect; it's still too large. But we have proven that as long as we start looking for small bugs or unclear areas in the source tree and improve them bit by bit, there will eventually be results. So I can't point to a specific place to start.
>
> Since I've already started, I will continue, at least starting with the crypto code and pseudo-random number generators.
>
> Since you sent your email, at least 10 people have gone to study this code. I myself found a small bug in another part of the random number subsystem and am cleaning up a function with a relatively complex structure.
>
> This is already the best we could hope for.
>
> From a "glass half full" perspective, seeing people try is actually quite encouraging! :-)
>
> By the way: iTWire mentioned that two bugs were found in the crypto code. Can you tell me the details of these two bugs?
>
> <http://www.itwire.com/opinion-and-analysis/open-sauce/439>...
>
> These are the first two bugs discovered. The first is related to the aforementioned CBC oracle issue (Angelos fixed it in the software crypto stack, but all drivers maintained by Jason ignored this issue. At the time, neither Jason nor Angelos was working at NETSEC, so I believe this was simply an oversight, albeit a rather serious one).
>
> ```text
> CVSROOT:        /cvs
> Module name:    src
> Changes by:     [mikeb@cvs.openbsd.org](mailto:mikeb@cvs.openbsd.org)   2010/12/15 16:34:23
>
> Modified files:
>    sys/arch/amd64/amd64: aesni.c via.c
>    sys/arch/i386/i386: via.c
>    sys/arch/i386/pci: glxsb.c
>    sys/dev/pci: hifn7751.c hifn7751var.h safe.c safevar.h
>             ubsec.c ubsecvar.h
>
> Log message:
> Bring in CBC oracle attack countermeasures from cryptosoft.c rev 1.32 into hardware-accelerated crypto drivers. This fixes aes-ni, via xcrypt, glxsb(4), hifn(4), safe(4), and ubsec(4) drivers.
> ```
>
> Original commit message (from angelos):
>
> ```text
> In CBC mode, do not keep the last blocksize bytes of ciphertext for the next message's IV. Each message should use arc4random() to obtain a new IV.
>
> OK and approved by: deraadt, markus, djm
>
> CVSROOT:        /cvs
> Module name:    src
> Changes by:     [jsg@cvs.openbsd.org](mailto:jsg@cvs.openbsd.org)     2010/12/16 09:56:08
>
> Modified files:
>    sys/crypto: cryptodev.h
>    lib/libssl/src/crypto/engine: hw_cryptodev.c
>
> Log message:
> Remove CRYPTO_VIAC3_MAX from cryptodev.h and place it in the only file that uses it.
>
> Requested and OK'd by mikeb.
> ```
>
> There are also some more recent commits resulting from this audit. Please see the Changelog.

Jason L. Wright, mentioned in the letter, also publicly stated that he did not plant backdoors in the software:

> I hereby explicitly state that I did not add backdoors to the OpenBSD operating system or the OpenBSD Cryptographic Framework (OCF). The code I was involved with in this work was primarily related to device drivers supporting the framework. I believe I never touched isakmpd or photurisd (userland key management programs), and I rarely touched the ipsec internals (though I did work on cryptodev and cryptosoft). However, I welcome an audit of everything I have committed to the OpenBSD codebase.

Gregory Perry later [replied](https://cryptome.org/2012/01/0032.htm):

> FBI, OpenBSD Backdoors and RSA Encryption Vulnerabilities
>
> From: Gregory Perry <Gregory.Perry [at] govirtual.tv>
>
> Subject: Follow-up on the FBI / OpenBSD / OpenBSD Crypto Framework backdoor discussion thread
>
> Date: Thu, 12 Jan 2012 01:57:39 +0000
>
> As promised, here is the follow-up on the FBI / OpenBSD / OpenBSD Crypto Framework crypto backdoor discussion thread. Our family experienced a three-alarm fire during the Christmas holiday, and I have only just come back online.
>
> 1) Around 1997, the FBI approached Lew Jenkins, who was the Chairman and CEO of Premenos Technology Corp., which was developing a software suite called "Templar" for inter-enterprise EDI (Electronic Data Interchange) transactions.
>
> 2) At that time, encryption technology (especially public key encryption algorithms) was still considered munitions by the US government, so the FBI may have been interested in Premenos's research on key escrow and RSA encrypted session recovery.
>
> 3) Part of Mr. Jenkins's research was conducted in collaboration with an Ecuadorian who provided Premenos with at least one mathematical vulnerability related to the RSA encryption algorithm, which involved the conversion of encrypted RSA moduli across different number bases. Jenkins and Premenos also maintained extensive connections with the British royal family, including some British nobility involved in internet communication technology.
>
> 4) A Premenos investor, Ross Pirasteh, was said to have been the Shah of Iran's finance minister, or possibly the Shah himself. Legend has it that before or during the 1979 Islamic Revolution led by Khomeini, Ross and his family were secretly smuggled out of Iran wrapped in Persian carpets. Upon arriving in the US, the FBI provided his family with new identities, for obvious reasons. I met Ross in the early 1990s during a GPS-based vehicle tracking project (non-military GPS), and in 1995 I introduced him to information about Templar; shortly thereafter, he became a Premenos investor.
>
> 5) In 1999, I co-founded a computer security engineering company with Ken Ammon and Jerry Harold: Network Security Technologies Inc. (NETSEC). Ken was CEO, Jerry was COO, and I was CTO. Ken and Jerry were former senior staff in the NSA's Information Assurance division, with extensive connections in the Department of Defense and federal government. We provided managed network security, penetration testing, vulnerability analysis, and reverse engineering services to the federal government and private sector.
>
> 6) NETSEC's first investor was Ross Pirasteh, who provided a bridge loan to start the company. The Series A early funding came from an angel investment group in Boca Raton, which I believe Ross introduced to Ken and his wife, but I don't have detailed knowledge of this.
>
> 7) The first product we planned to launch was a high-speed embedded network security device based on ATM, deployed in customer networks for remote protocol analysis, monitoring, intrusion detection, and prevention. Each device connected to NETSEC's NOC (Network Operations Center) via encrypted accelerated VPN tunnels for monitoring. The NOC prototype and network management system were designed by me. During the development of this embedded hardware and the search for a contract manufacturer, I hired two engineers, Doug Bostrom and Wayne Mitzen, who had previously worked at a telemetry-related company of Ross's in Boston (see US Patent 6,208,266).
>
> 8) In that project, I contacted Theo de Raadt of the OpenBSD project, hoping to fund and implement POSIX-compliant preemptive real-time threading capabilities in OpenBSD as an alternative to the costly VxWorks RTOS (OpenBSD and its licensing were free and unencumbered by patents). NETSEC provided hardware and funding to the OpenBSD project to support the initial development of its cryptographic framework (OCF), based on the HiFN series of cryptographic acceleration chips, which was eventually merged into the OpenBSD kernel. We had wanted to use Broadcom, but since Ken had a relationship with HiFN, we initially used HiFN's chips. Late 1990s x86 hardware could not handle the high-speed DES and 3DES encryption tasks required by FIPS 140-1 and 140-2 certification, so dedicated cryptographic processors were needed to achieve ATM-level network throughput.
>
> 9) Shortly thereafter, NETSEC launched a project with the US General Services Administration (GSA), the GSA Technical Support Center. This center was a joint FBI and Department of Defense project aimed at providing reverse engineering and cryptanalysis services to federal and military agencies. The project lead was the FBI's Ron Bitner (at least that's what he claimed), and the GSA funding representative was Dave Jarrell. When I first became involved in this project, I expressed concerns about the blurred (or non-existent) division of responsibilities between the FBI and the DoD, which was a clear violation of the Posse Comitatus Act (PCA). Ken's explanation was that a multi-level security system (MLS) like Trusted Solaris would be used to allow information at different classification levels to be shared between the FBI and the DoD, thereby nominally maintaining the traditional separation between military and civilian governance. But I could see things were going wrong, so I stopped participating in the project.
>
> 10) Later that same year, I announced my resignation from NETSEC at a company meeting and founded an embedded wireless bandwidth management company. Due to a two-year non-compete agreement with NETSEC, I could not work in the security field during that period.
>
> Clearly, this story cannot be fully told on a single page. But I think it is worth noting the seemingly opposing yet intricately connected relationship between the US and "hostile nations" like Iran. In 1995, the FBI firmly opposed relaxing encryption technology export regulations, yet by 1999 their attitude had abruptly changed (for example, this report: <https://www.nytimes.com/1999/10/11/business/technology-easing-on-software-exports-has-limits.html>
>
> I personally believe that the FBI, or certain officials in the government at the time, actively pushed for relaxing encryption export restrictions after discovering a critical vulnerability in the RSA encryption algorithm. Compared to the then-mainstream Diffie-Hellman public key encryption method, RSA did indeed have undisclosed weaknesses. It is also worth noting that RSA Security Inc. did not seek to extend the RSA patent after it expired (US Patent 4,405,829), which is surprising. They could have easily obtained an extension based on national security grounds but voluntarily abandoned the patent, making the RSA algorithm a public standard, even though the company had earned considerable revenue from licensing the RSA algorithm in the US.
>
> If the above speculation is true, then it is reasonable to assert that the FBI deliberately promoted the adoption of a fundamentally weak encryption algorithm for the purpose of monitoring domestic communications, thereby severely weakening the critical infrastructure and military capabilities of the United States. This has had profound implications for the vast number of technologies based on the RSA encryption algorithm, such as: military GPS used for weapons targeting, Common Access Card smart cards, and commercial smart card technology used for RFID and contactless payments. For embedded systems, these standards are essentially set in stone, and most OpenBSD / OpenBSD Crypto Framework installations are embedded, with almost no upgrade path due to their small footprint and permissive BSD license. There may be millions or even hundreds of millions of embedded devices running OpenBSD worldwide, such as routers, firewalls, and VPN devices. Additionally, many operating systems directly integrate OpenBSD's crypto framework and PF firewall stack without auditing the source code, relying on the OpenBSD project's security reputation and trustworthiness.
>
> If you have other questions, feel free to contact me at any time. Wishing you and Cryptome a pleasant 2012.
>
> Gregory Perry

At this point, the main course of events has been presented. From the results, the incident contains some content that may be controversial.

## DragonFly BSD Overview

DragonFly BSD is a UNIX-like system derived from FreeBSD 4.8. The project was initiated by Matthew Dillon (an Amiga developer from the late 1980s to early 1990s, and a FreeBSD developer from 1994 to 2003) in June 2003, and was officially announced on the [FreeBSD mailing list](https://lists.freebsd.org/pipermail/freebsd-current/2003-July/006889.html) in July 2003.

Dillon started the DragonFly BSD project because he had a different judgment about the SMP (Symmetric Multiprocessing) parallel computing architecture adopted by FreeBSD 5. SMP refers to an architectural design where multiple processors share the same memory space; he believed this design might introduce unnecessary performance overhead. This technical divergence led to discussions with the FreeBSD core development team, ultimately resulting in an independent project. Despite the differing technical paths, DragonFly BSD and the FreeBSD project still maintain collaborative relationships in areas such as bug fixes and driver updates.

DragonFly BSD inherited FreeBSD 4's technical lineage while making innovative designs at multiple key system levels, including lightweight kernel thread implementation mechanisms and the HAMMER/HAMMER2 file systems as core components. Some of DragonFly BSD's design concepts were inspired by the AmigaOS architecture.

As of June 2026, the latest version of DragonFly BSD is 6.4.2 (released in May 2025). The 6.4 series added NVMM support for Type-2 Hypervisors, the amdgpu graphics driver, and experimental functionality for remotely mounting HAMMER2 volumes.

In terms of current hardware support, DragonFly BSD includes i915 graphics drivers, supports only the x86-64 architecture, and does not provide a Linux compatibility layer. Its DPorts package system is largely compatible with FreeBSD Ports, currently based on the FreeBSD Ports 2024Q3 branch and progressing toward 2025Q2. Note that DragonFly BSD's driver support is relatively lagging, particularly in the update pace of graphics drivers.

Donating to DragonFly BSD: [Sponsoring projects](https://www.dragonflybsd.org/donations/), currently only supports international PayPal.

> DragonFly BSD's documentation is relatively outdated, but this does not reflect its actual development progress. DragonFly BSD's development remains active, and one should not abandon using it due to outdated official documentation.

### Appendix: Other Security-Focused BSD Systems

#### HardenedBSD

OpenBSD is not the only security-focused BSD operating system; you can also try [HardenedBSD](https://hardenedbsd.org/), which was forked from FreeBSD in 2014.

Its website states: "Our primary goal is to perform clean-room development of the publicly documented portions of the Grsecurity patch set (a kernel patch set that enhances Linux kernel security)."

> The Grsecurity patch set for the Linux kernel is garbage.
>
> — Linus Torvalds (Linux Founder)

#### CheriBSD

[CheriBSD Official Website](https://www.cheribsd.org/)

CheriBSD implements Capability (capability-based pointers) on top of FreeBSD, providing memory protection and software isolation. CheriBSD primarily targets ARM and RISC-V architectures and is jointly developed by SRI International and the University of Cambridge.

#### FuguIta

[FuguIta Official Website](https://fuguita.org/)

FuguIta is a Live system based on OpenBSD that also supports some models of Raspberry Pi (Raspberry Pi 3/4/5, arm64 architecture).

## Exercises

1. Analyze the evolution of the term "Distribution."
2. What is the fundamental difference between different BSD distributions and different Linux distributions?
