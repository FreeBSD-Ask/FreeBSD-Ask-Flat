# 1.2 The GNU Operating System and the Free Software Movement

Richard Stallman launched the GNU Project in 1983, aiming to create a completely free and UNIX-compatible operating system.

## The Origins of the GNU Operating System and the Free Software Movement

The central role of the GNU General Public License (GPL) in the free software ecosystem can be glimpsed from the Linux kernel documentation. According to the relevant description in the Linux Kernel Documentation:

> Code is contributed to the Linux kernel under a number of licenses, but all code must be compatible with version 2 of the GNU General Public License (GPLv2), which is the license covering the kernel distribution as a whole. In practice, that means that all code contributions are covered either by GPLv2 (with, optionally, language allowing distribution under later versions of the GPL) or the three-clause BSD license. Any contributions which are not covered by a compatible license will not be accepted into the kernel.

In its later development, UNIX gradually became closed — initially an open research project (as required by U.S. law at the time), it later became a commercial product. Ordinary users could not freely access or modify its source code; license prices were high, and usage rights were strictly restricted by commercial companies. Richard Matthew Stallman (RMS) wished to create a free and UNIX-compatible operating system, and this vision directly drove the rise of the free software movement.

- In 1983, Richard Matthew Stallman published the initial announcement of the GNU Project; GNU is a recursive acronym for "GNU's Not Unix." GNU is an operating system designed to be a complete replacement for UNIX.
- In 1984, Richard Matthew Stallman formally created the [GNU Project](https://www.gnu.org/).
- In 1985, Richard Matthew Stallman wrote the GNU Manifesto; that same year, he founded the Free Software Foundation (FSF).
- In 1989, the Free Software Foundation released GNU General Public License V1.0 (GPLv1).
- In 1991, the Free Software Foundation released GNU General Public License V2.0 (GPLv2).
- In 2007, the Free Software Foundation released GNU General Public License V3.0 (GPLv3).

In the early days of the GNU Project, Richard Matthew Stallman developed a large number of utilities (user-space components) for UNIX, yet GNU never produced a stable kernel. [**GNU Hurd**](https://hurd.gnu.org/) is the kernel of the GNU Project, designed with a microkernel architecture, with development starting in 1990. Although the Debian GNU/Hurd port team released **Debian GNU/Hurd 2025** on **August 10, 2025** (this version is a snapshot of Debian sid at the time of the Debian 13 Trixie release, with most source code identical to Trixie; this is an official Debian GNU/Hurd port release, but not an official Debian distribution), its hardware support and software ecosystem remain immature. The birth of the Linux kernel broke this technical impasse.

Linux is a combination of the Linux kernel and GNU software, continuously incorporating GNU principles during its development, ultimately forming the GNU/Linux operating system. Despite GNU's enormous contributions, its role is often underestimated. This is partly because the development of the Linux kernel was led by Linus Torvalds, who did not fully align with the principles of the Free Software Foundation and Richard Matthew Stallman. There are certain divergences between the Linux kernel project and the free software movement, or even strict open-source principles (for example, the Linux kernel includes binary firmware modules that do not conform to the definitions of free software or strict open source — see **Linux-libre**). Furthermore, the Linux kernel uses the GPLv2 license rather than the GPLv3 recommended by GNU.

Before the birth of Linux, the representative movement in the field of software freedom was the "Free Software Movement" (represented by Richard Matthew Stallman). The popularity of Linux facilitated the rise of another philosophy, namely the "Open Source Movement," represented by Eric S. Raymond, co-founder of the Open Source Initiative (OSI) and author of *The Art of UNIX Programming* and *The Cathedral and the Bazaar*, as well as Bruce Perens, another co-founder of OSI and former leader of the Debian Project. The open source standards defined by OSI originated from the Debian Free Software Guidelines (DFSG).

The philosophical differences between the free software movement and the open source movement remain one of the core divisions in the field of software licensing to this day.

### References

- Linux Kernel Documentation. 1. Introduction — The Linux Kernel documentation[EB/OL]. [2026-03-26]. <https://www.kernel.org/doc/html/latest/process/1.Intro.html>.
- Linux Kernel Documentation. Linux kernel licensing rules — The Linux Kernel documentation[EB/OL]. [2026-03-26]. <https://www.kernel.org/doc/html/latest/process/license-rules.html>.
- Stallman R. The GNU Manifesto[EB/OL]. [2026-03-26]. <https://www.gnu.org/gnu/manifesto.html>.
- Linux-libre Project. Linux-libre[EB/OL]. [2026-03-26]. <https://www.fsfla.org/ikiwiki/selibre/linux-libre/>. A variant of the Linux kernel maintained by the Free Software Foundation Latin America (FSFLA) with all binary firmware removed

## The Tension Between the Free Software Movement and the Open Source Movement

There are certain philosophical differences between the free software movement and the open source movement. In private correspondence, Richard Matthew Stallman emphasized that GNU and the free software movement do not emphasize "open source" (and even oppose the use of this term), but rather emphasize the freedom represented by "Free." Some views equate "open source" with OSI's definition, which is a one-sided perspective.

The following is quoted from Richard Matthew Stallman's private correspondence and does not involve private information.

> To all NSA and FBI agents reading my email: Please consider whether defending the U.S. Constitution against all enemies, foreign and domestic, requires you to follow the example of Edward Snowden.
>
>> But I'm a bit confused: does the GNU Project mandate that development must be open source? Because the GNU General Public License (GPL) mandates open source.
>
> The GNU Project does not advocate "open source." We never use that term, except to express our disagreement with it. We stand for Free Software — free as in free speech. We are committed to fighting for users' freedom in computing.
>
> Please see: free-software-even-more-important[EB/OL]. [2026-03-26]. <https://gnu.org/philosophy/free-software-even-more-important.html>.
>
> The term "open source" was coined by people who opposed the free software movement — they disagreed with us. They wanted to talk about the same software while obscuring the idea of freedom.
>
> For the difference between free software and open source, please see: open-source-misses-the-point[EB/OL]. [2026-03-26]. <https://gnu.org/philosophy/open-source-misses-the-point.html>.
>
> Also read Evgeny Morozov's article: <https://thebaffler.com/salvos/the-meme-hustler>, where he also discusses this.
>
> So please don't ask us about "open sourcing" or "opening" something. We don't think that way. What you should really ask us is: how do we do things the free software way.
>
>> It belongs to the GNU Project, so does the GNU Project also force others to...
>
> I'm not quite sure what you mean by "force" — there may be a misunderstanding here. Usually we tell people what we think is right and what is wrong, but we cannot command them to do anything.
>
> The only exception is when they use GPL-licensed software code. In that case, the GNU GPL itself is a legal constraint that governs how they use the code. It requires them to respect other users' freedom when redistributing the code.
>
> This is precisely the meaning of Copyleft.
>
> If you have more questions, please write to <licensing@gnu.org>.

> **Tip**
>
> Unlike the "Copyleft" mechanism of the GPL license, the FreeBSD Project uses the more permissive BSD license. The FreeBSD Project's source code includes some software licensed under the GNU General Public License (GPL) and the GNU Lesser General Public License (LGPL), whose Copyleft provisions impose stricter restrictions on source code redistribution. However, since GPL software may introduce additional complexity in commercial use, the FreeBSD Project prefers to accept BSD-licensed software submissions where reasonably practicable. This difference in license choice is a core distinction between FreeBSD and Linux in software ecosystem governance.

### References

- Debian Project. Debian GNU/Hurd[EB/OL]. [2026-04-17]. <https://www.debian.org/ports/hurd/>. The official Debian GNU/Hurd page; Debian GNU/Hurd 2025 was released on August 10, 2025 (based on a snapshot of Debian sid at the time of the Trixie release, with most source code identical to Trixie; this is an official Debian GNU/Hurd port release, but not an official Debian distribution).
- GNU Project. What is the Hurd?[EB/OL]. [2026-04-17]. <https://www.gnu.org/software/hurd/hurd/what_is_the_gnu_hurd.html>. Introduction to the GNU Hurd project, elaborating on its microkernel architecture design philosophy.
- Open Source Initiative. The Open Source Definition[EB/OL]. (2024-02-16)[2026-03-26]. <https://opensource.org/osd>.

## Appendix: Clarifying Common Misconceptions About Free Software and Open Source Software

The concepts of free software and open source software are often confused. This appendix starts from the distinction between the terms "营利" (seeking profit) and "盈利" (making profit), outlines the four basic freedoms of free software and the ten criteria of OSI open source licenses, clarifies the relationship between free software and open source software, and delineates the boundaries of non-open-source licenses such as CC-BY-NC/ND.

### Clarifying "营利" and "盈利"

These two terms have different meanings and usages.

- "盈利" (yíng lì): Noun, the profit obtained after deducting costs; also written as 赢利. Cited from Institute of Linguistics, Chinese Academy of Social Sciences, ed. Contemporary Chinese Dictionary[M]. 7th ed. Beijing: The Commercial Press, 2016: 1572. ISBN: 978-7-100-12450-8.
- "营利" (yíng lì): Verb, to seek profit. Cited from Institute of Linguistics, Chinese Academy of Social Sciences, ed. Contemporary Chinese Dictionary[M]. 7th ed. Beijing: The Commercial Press, 2016: 1572. ISBN: 978-7-100-12450-8.

### The Free Software Definition: Four Essential Freedoms

> For a program to be free software, it must offer its users the following four essential freedoms:
>
> Freedom 0: The freedom to run the program as you wish, for any purpose.
>
> Freedom 1: The freedom to study how the program works, and change it so it does your computing as you wish. Access to the source code is a precondition for this.
>
> Freedom 2: The freedom to redistribute copies so you can help others.
>
> Freedom 3: The freedom to distribute copies of your modified versions to others. By doing this you can give the whole community a chance to benefit from your changes. Access to the source code is a precondition for this.

That is: "Users are free to run, copy, distribute, study, modify, and improve the software."

Corollary 1: If commercial users modify (Freedom 1) and redistribute (Freedom 2) the software for the purpose of making a profit (Freedom 0), as long as the modifications remain open source (Freedom 3), the commercial users' behavior is fully compliant. This demonstrates that commercial users have every right to freely use, modify, distribute, and make a profit from the software. None of the freedoms restrict the act of **making a profit**. Evidence 1: [Free software can be commercial software](https://www.gnu.org/philosophy/free-sw.zh-cn.html#four-freedoms).

### Open Source License Definition and Open Source Software Definition

The Open Source Initiative (OSI) aims to uphold the Open Source Definition (OSD) and prevent abuse of the open source movement's principles. Its definition of open source licenses is as follows — the distribution terms of open source software must comply with the following core criteria:

| No. | Criterion | Description |
| --- | --------- | ----------- |
| 1 | **Free Redistribution** | The license shall not restrict any party from selling or giving away the software as a component of an aggregate distribution containing programs from several different sources. The license shall not require a royalty or other fee for such sale. |
| 2 | **Source Code** | The program must include source code, and must allow distribution in source code as well as compiled form. |
| 3 | **Derived Works** | The license must allow modifications and derived works, and must allow them to be distributed under the same terms as the license of the original software. |
| 4 | **Integrity of The Author's Source Code** | The license may restrict source-code distribution in modified form only if the license allows the distribution of "patch files" with the source code. |
| 5 | **No Discrimination Against Persons or Groups** | The license must not discriminate against any person or group of persons. |
| 6 | **No Discrimination Against Fields of Endeavor** | The license must not restrict anyone from making use of the program in a specific field of endeavor. |
| 7 | **Distribution of License** | The rights attached to the program must apply to all to whom the program is redistributed without the need for execution of an additional license by those parties. |
| 8 | **License Must Not Be Specific to a Product** | The rights attached to the program must not depend on the program's being part of a particular software distribution. |
| 9 | **License Must Not Restrict Other Software** | The license must not place restrictions on other software that is distributed along with the licensed software. |
| 10 | **License Must Be Technology-Neutral** | No provision of the license may be predicated on any individual technology or style of interface. |

Open source licenses certified by OSI based on the Open Source Definition (OSD) have become the globally recognized standard for determining open software, and their authority has been established through adoption by international organizations, industry, and government policies.

For OSI-certified open source licenses, see the [OSI Official License List](https://opensource.org/licenses). Strictly speaking, only the licenses on this list qualify as open source licenses.

### Definitions of Various Types of Software

Based on the Free Software Foundation. Categories of Free and Non-Free Software[EB/OL]. (2025-12-28)[2026-03-26]. <https://www.gnu.org/philosophy/categories.zh-cn.html>. The definitions are as follows:

- Free software: Software that meets the four essential freedoms defined above is called free software. Corollary: Commercial software can be free software; commercial does not mean non-free. Evidence 1: Free Software Foundation. What is free software?[EB/OL]. [2026-03-26]. <https://www.gnu.org/philosophy/free-sw.zh-cn.html>. Evidence 2: Free Software Foundation. Words to Avoid (or Use with Care) Because They Are Loaded or Confusing[EB/OL]. [2026-03-26]. <https://www.gnu.org/philosophy/words-to-avoid.html>. "Commercial" section.
- Open source software: Software licensed under the open source licenses described above. All free software is open source software, but not all open source software is free software. The two differ in both definition and philosophy. Free software is a sufficient but not necessary condition for open source software.
- Proprietary Software: This is "non-free software" in the truest sense; the original goal of the GNU Manifesto was to eliminate such non-free software. Most commercial software falls into this category.
- Freeware: The criteria for definition vary. All of the aforementioned types of software may be distributed for free, but freeness alone does not constitute a basis for determining their nature.
- Commercial software: Commercial software is developed by a business as part of its operations.

#### Corollaries

- Corollary 1: Commercial software can be open source software or free software. Conversely, open source software or free software can also be commercial software.
- Corollary 2: Open source software or free software in the form of commercial software may be used for profit (regardless of whether the project authors participate in this commercial software), and this is compliant. For example, a typical scenario is merchants on e-commerce platforms using open source-licensed software for profit. Provided the license terms are complied with, such profit-making is ethical. In reality, criticism of such merchants mostly stems from the public's lack of understanding of the open source licenses adopted.
- Corollary 3: Commercial software is not necessarily proprietary software. Conversely, proprietary software is not necessarily commercial software (e.g., distributing a personal project as closed source to friends for non-commercial purposes).

### Free/Open Source Software and Freeware

Based on "Freedom 2: The freedom to redistribute copies so you can help others" and the relevant provisions of open source licenses such as GPLv2/v3, the aforementioned licenses do not restrict the profit-making activities of open source software. Red Hat is a typical example.

Open source software/free software does not equal freeware.

### Open Source Does Not Mean No Copyright

Some members of the public confuse open source with no copyright, but the view that "open source means no copyright" actually has a logical problem: without holding copyright, one cannot require others to comply with the open source license. Some people transfer copyright to the Free Software Foundation, but copyright still exists in fact.

In judicial practice, many open source software project authors are still held legally accountable. If there were no copyright, it would be impossible to determine the responsible party.

Under the copyright laws of various countries, all rights of copyright generally cannot be fully transferred; only some rights can be assigned. Taking the Copyright Law of the People's Republic of China as an example, rights such as the right of publication, the right of authorship, the right of alteration, and the right of integrity cannot be assigned.

In practice, authors are generally not restricted by the open source license itself (unless they incorporate others' projects); the license is intended to bind third parties.

### CC-BY-NC and CC-BY-ND Are Neither Free Nor Open Source

The Creative Commons Attribution-NonCommercial (CC-BY-NC) and Creative Commons Attribution-NoDerivs (CC-BY-ND) licenses are neither open source software licenses nor free software licenses. Based on an analysis of various licenses and their commentaries, the aforementioned licenses restrict commercial users' use and deprive commercial users of their "freedom"; the aforementioned CC licenses also do not conform to the spirit of free software, as their requirement for lengthy attribution and notices increases compliance costs when reusing projects.

### Access to Source Code Does Not Mean Open Source or Free

Under Microsoft's Enterprise Source Licensing Program (ESLP) or Microsoft Reference Source License (Ms-RSL), commercial users can obtain the corresponding source code after meeting certain conditions.

Although the source code is accessible and modification is allowed in some cases (such as Cmcc China Government Edition Windows 10), such agreements are not open source licenses. In essence, CC-BY-NC and CC-BY-ND are not fundamentally different from such license agreements.

### References

- Free Software Foundation. Categories of Free and Non-Free Software[EB/OL]. [2026-03-26]. <https://www.gnu.org/philosophy/categories.zh-cn.html>.
- Open Source Initiative. Licenses - Open Source Initiative[EB/OL]. [2026-03-26]. <https://opensource.org/licenses>.
- Open Source Initiative. International Authority & Recognition - Open Source Initiative[EB/OL]. (2025-06-03)[2026-03-26]. <https://opensource.org/about/authority>.
- Debian Project. Debian Social Contract[EB/OL]. (2022-10-01)[2026-03-26]. <https://www.debian.org/social_contract#guidelines>. DFSG.
- Free Software Foundation. What is free software?[EB/OL]. [2026-03-26]. <https://www.gnu.org/philosophy/free-sw.zh-cn.html>. Citing Free Software Foundation content to define the freedoms of free software.

## Exercises

1. Consult theoretical literature and analyze the technical, legal, and community relationships among Linux distributions (Debian, Ubuntu, Fedora, Arch Linux, etc.), the GNU Project, and the GNU GPL license. Draw a diagram illustrating the interactions among the three.
2. Select a Linux distribution and study the dependency relationships between its core system components and the GNU Project. Which key components in that distribution come from the GNU Project? How do its kernel (the Linux kernel) and the GNU toolchain work together? Consult relevant technical documentation and architecture materials, and write a technical analysis report.
3. Read the original text of the GNU GPLv2 license, summarize the core provisions of the copyleft mechanism, and compare them item by item with the BSD 2-Clause License in terms of redistribution restrictions.
