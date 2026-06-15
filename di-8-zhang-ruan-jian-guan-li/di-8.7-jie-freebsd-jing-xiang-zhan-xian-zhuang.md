# 8.7 FreeBSD Mirror Site Status

The FreeBSD official rsync service is not open to the public, and the project does not accept applications for official secondary mirrors. This section outlines the policy constraints, historical evolution, and community practice solutions regarding mirror site deployment, providing background reference for readers interested in participating in mirror construction.

## Current Status and Basic Landscape of Mirror Sites

### Official rsync Service Not Open to Public

The current FreeBSD mirror site ecosystem faces two core constraints:

1. The official rsync service is not yet open to the public;
2. The project does not accept applications for official secondary mirror sites.

According to verifiable historical records, the FreeBSD Project stopped its public rsync service no later than May 2015. The relevant document [Add small section explaining we are not allowing public mirrors of packages and possible workarounds.](https://reviews.freebsd.org/R9:3418e47d2f6cd8dd04ac934f38d136ba9101a5a8) records the project's decision to prohibit public package mirroring and alternative solutions. The official reasons given are as follows:

> Due to very high requirements of bandwidth, storage and administration the FreeBSD Project has decided not to allow public mirrors of packages.
>
> Due to the extremely high requirements for bandwidth, storage, and administration, the FreeBSD Project has decided not to allow public mirroring of packages.

---

The latest official response obtained in February 2025 further clarifies the project's position:

> On Fri, 28 Feb 2025, at 17:45, ykla wrote:
>> How to mirror pkg and update from official mirror sites?
>
> As we replied on several occasions before: pkg and freebsd-update need machines under our control with internet connectivity to the rest of our cluster.
>
> At one point someone tried to offer a machine in Nanjing. That then turned into a virtual machine and the conversation went nowhere. We can't use a virtual machine. We need real hardware. With real storage. And real transit.

Translation:

> On Friday, February 28, 2025, at 17:45, ykla wrote:
>> How to mirror pkg and update from official mirror sites?
>
> As we have replied on several occasions, pkg and freebsd-update require physical servers under our control (note: this refers to root access), which need to maintain network connectivity with our cluster.
>
> Someone previously proposed providing a machine in Nanjing, but after the plan was changed to a virtual machine, the discussion stalled. We cannot use a virtual machine solution; we need real hardware (note: bare metal), physical storage media, and physical network transmission links.

### Analysis of Possible Reasons for Not Opening

#### Security Factors

Tracing historical records, the FreeBSD infrastructure cluster experienced a security intrusion incident in 2012. After fully transitioning to the pkg distribution mechanism, the project terminated external mirror authorization to safeguard software supply chain integrity and security.

References:

- delphij. FreeBSD.org intrusion event[EB/OL]. (2012-12-17)[2026-03-25]. <https://blog.delphij.net/posts/2012/12/freebsdorg-2/>. Chinese explanation.
- FreeBSD Project. FreeBSD.org intrusion announced November 17th 2012[EB/OL]. (2012-11-17)[2026-03-25]. <https://www.freebsd.org/news/2012-compromise.html>. FreeBSD Project official statement.

#### Transmission Mechanism Factors

The current cluster's data synchronization mechanism uses a direct transmission method based on the ZFS file system (according to community understanding, implemented via `zfs send` / `zfs receive` commands), rather than the traditional rsync mirror site synchronization model. This also limits the feasibility of external mirrors.

#### Resource Limitation Factors

According to [[NEW MIRROR] New full mirror in Belgium](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=288631), cluster administrator bofh responded as follows:

> There are couple of reasons:
>
> 1. Bandwidth from our central servers. Increasing number of community mirrors require more bandwidth in case everyone starts pulling altogether like just after a new quarterly branch
> 2. While many sites are excited to create mirrors as they think they have real bandwidth in reality they lose their moral bandwidth in couple of days and our mirrors are no longer in sync. We are often communicated regarding the problems of other community mirrors. We already operate in a tight schedule and often overseeing other mirrors is just another nail in the wall.
> 3. We still accept mirrors but on a different way. If people can sponsor bare metal we would be happy to deploy a new mirror. However our mirror requirements are pretty high. You can have a look at the following to understand our requirements:
>
> - <https://wiki.freebsd.org/Teams/clusteradm/generic-mirror-layout>
>
> - <https://wiki.freebsd.org/Teams/clusteradm/tiny-mirror>

Translation:

> There are several reasons:
>
> 1. Bandwidth from our central servers. When the number of community mirrors increases, if everyone starts pulling simultaneously, such as right after a new quarterly branch is released, more bandwidth is needed.
>
> 2. Many sites are very excited when creating mirrors, confident that they have real bandwidth, but in reality, their "moral bandwidth" often runs out after a few days, and they no longer sync our mirrors. We frequently receive communications about problems with other community mirrors. Our schedule is already tight, and overseeing other mirrors is like driving another nail into the wall.
>
> 3. We still accept mirrors, but in a different way. If someone can sponsor a bare metal server, we would be happy to deploy a new mirror. However, our requirements for mirrors are quite high. You can check the following to understand our requirements:
>
> - <https://wiki.freebsd.org/Teams/clusteradm/generic-mirror-layout>
>
> - <https://wiki.freebsd.org/Teams/clusteradm/tiny-mirror>

> **Discussion Question**
>
>> I believe that "not accepting community mirrors" and "refusing mirrors" are two completely different things. Ubuntu has a large number of community mirrors, but only a very few are officially recognized. This essentially reflects the user's freedom of choice. Users can choose the seemingly more secure official mirrors, or sacrifice some security and privacy for speed. We are not qualified to make this choice for users.
>>
>> For a person dying of thirst, the question is not whether they should drink Coca-Cola or sewage.
>
>> By closing rsync and not providing other synchronization channels, while most open source mirrors rely on rsync, they are actually hindering the development of the FreeBSD Project itself. The project claims insufficient bandwidth, but I doubt that OpenBSD or NetBSD have significantly more bandwidth. Yet they, through openness and freedom, let users make their own choices.
>>
>> Just like what happened today: I criticized a speed-testing tool that is supposedly for testing and switching different FreeBSD package mirrors, but installing it requires pulling in 45 additional dependencies. This is like imagining what clothes the emperor should wear and what kind of golden hoe to use for farming — or inventing a light bulb that can only light up during the day.
>>
>> I am not criticizing any individual; what I truly question is whether this system and its design are reasonable.
>>
>> FreeBSD has only technically switched to Git; its thinking remains in the SVN era. SVN is centralized, unified, and strongly permissioned; Git is distributed, de-permissioned, and allows free branching. The contemporary Internet emphasizes decentralization and sharing. There is no reason why; just as urbanization and counter-urbanization are both reasonable and unreasonable, it is just that a trend requires us to do things this way or that way, otherwise we lose the legitimacy of our own existence. What you say makes sense and is reality — they might sync for a few days and then abandon it. The problem is that the right to choose should belong to users, not the project itself. I believe this is a manifestation of paternalism. This also shows that the difficulty of transforming old projects is more about ideology than technology.
>
>> I have arrived at Longnan today, and will enter the nest tomorrow. The four routes of troops have all advanced as scheduled, and the enemy is bound to be defeated. I was previously at Hengshui and once wrote to Shide saying: "It is easy to defeat the bandits in the mountains, but difficult to defeat the bandits in the heart." Merely eliminating petty thieves is nothing remarkable. If the worthy gentlemen can sweep away the bandits in their hearts and achieve the merit of clearing and pacifying, this would truly be an extraordinary achievement of a great man. I trust that in the past few days you have already devised a sure-fire strategy, and victory reports are expected soon. What joy could be greater!
>>
>> Rifù has fine qualities and can truly study together; I expect he has already set sail by now. If not yet departed, please convey my regards on my behalf. The affairs of the office burden Shangqian, and I hope you will not find them tedious. My young son Zhengxian still hopes for your occasional guidance and discipline. (From *Complete Works of Wang Yangming, Volume 4, Literary Records Part 1, Letter to Yang Shide and Xue Shangqian*)
>
>> Combining the historical facts of Wang Yangming pacifying Nangan, reforming local administration, and purifying the court — breaking the "bandits in the hearts" of both officials and the people — please elaborate: forcing someone to be free — is this itself a form of freedom or fascism?

### There Are Currently No FreeBSD Official Mirror Sites in Mainland China

After multiple communication attempts (including contacting the mailing list approximately five times, of which three received responses and two received no response), no effective progress has been made. The main official response was "we deeply apologize, but there is already a mirror in the Taiwan region," without providing further explanation of the specific process for mirror applications. Additionally, a special mirror application was made to the University of Science and Technology of China Linux User Group, but FreeBSD officials also did not respond.

Currently closed (Closed as not planned) unofficial issue mirror applications:

USTC:

- <https://github.com/ustclug/mirrorrequest/issues/172>
- <https://github.com/ustclug/mirrorrequest/issues/171>

Currently closed unofficial issue mirror applications:

TUNA: <https://github.com/tuna/issues/issues/16>

## Calling on University Students to Participate in Mirroring FreeBSD

If you have the conditions to set up an unofficial mirror, you can use the synchronization scripts provided by USTCLUG:

1. USTCLUG. FreeBSD-pkg Script[EB/OL]. [2026-03-25]. <https://github.com/ustclug/ustcmirror-images/blob/master/freebsd-pkg/sync.sh>.
2. USTCLUG. FreeBSD-ports Script[EB/OL]. [2026-03-25]. <https://github.com/ustclug/ustcmirror-images/blob/master/freebsd-ports/sync-ports.sh>.

To set up an unofficial mirror site. The fragrance always stays in the hand that gives the rose.

It is recommended that university students prioritize using campus resources to set up mirrors, or directly synchronize from USTC's `rsync` service. It is recommended to consult USTCLUG before synchronizing to avoid unnecessary trouble. Contact: [lug@ustc.edu.cn](mailto:lug@ustc.edu.cn). Refer to [USTC Mirror Synchronization Methods and Precautions](https://mirrors.ustc.edu.cn/help/rsync-guide.html) for synchronization.

## Basic Requirements for Mirror Sites Given by the Official

- Root access to servers is equally scarce in China, and open source mirror sites typically do not grant it;
- IPv6 and BGP networking — also relatively lacking in China; CN2 networking is equally scarce in mainland China, but this is not a FreeBSD official requirement, rather an additional consideration of the domestic network environment;
- Sufficient storage space (approximately 16 TB for a tiny mirror, more for a full mirror) and 1 G bandwidth;
- A tiny mirror requires 1 server; a full mirror requires multiple;
- ICP filing issues — a specialized company/social organization is required to file for cn.FreeBSD.org;
- And the biggest issue: **lack of funding**.

For details, see:

- Single mirror: <https://wiki.freebsd.org/Teams/clusteradm/tiny-mirror>
- Full mirror: <https://wiki.freebsd.org/Teams/clusteradm/generic-mirror-layout>

## Unofficial Mirror Sites

FreeBSD does not have official mirror sites in mainland China; there are official mirror sites in the Taiwan region of China (as of the second quarter of 2026, the Taiwan mirror site has been in a prolonged outage state, and the project team is working to restore it).

The few FreeBSD mirror sites in mainland China that can synchronize normally all do not use `rsync` or similar methods for synchronization, but instead employ some special means.

- [FreeBSD-pkg script](https://github.com/ustclug/ustcmirror-images/blob/master/freebsd-pkg/sync.sh)
- [FreeBSD-ports script](https://github.com/ustclug/ustcmirror-images/blob/master/freebsd-ports/sync-ports.sh)

> **Tip**
>
> We call on those with the capacity to maintain and revise the above two scripts, to reduce the pressure on the USTC mirror site and provide better FreeBSD mirror services within the country.

FreeBSD currently has several unofficial mirror sites in mainland China:

| Mirror Site | Service Type | Link | Notes |
| ----------- | ------------ | ---- | ----- |
| University of Science and Technology of China Mirror (USTC) | pkg, ports, pub | <https://mirrors.ustc.edu.cn/> | Contact: [lug@ustc.edu.cn](mailto:lug@ustc.edu.cn) |
| University of Science and Technology of China Mirror (USTC) | FreeBSD Pub | <https://mirrors.ustc.edu.cn/freebsd/> | / |
| University of Science and Technology of China Mirror (USTC) | FreeBSD Packages | <https://mirrors.ustc.edu.cn/freebsd-pkg/> | / |
| University of Science and Technology of China Mirror (USTC) | FreeBSD Ports | <https://mirrors.ustc.edu.cn/freebsd-ports/> | [Usage documentation](https://mirrors.ustc.edu.cn/help/freebsd-ports.html) |
| NetEase 163 Mirror | pkg, ports (upstream is USTC) | <https://mirrors.163.com/> | / |
| NetEase 163 Mirror | FreeBSD Pub | <https://mirrors.163.com/freebsd/> | / |
| NetEase 163 Mirror | FreeBSD Packages | <https://mirrors.163.com/freebsd-pkg/> | / |
| NetEase 163 Mirror | FreeBSD Ports | <https://mirrors.163.com/freebsd-ports/> | / |
| Nanjing University Open Source Mirror | pkg, ports (upstream is USTC) | <https://mirrors.nju.edu.cn/> | Contact: [GitHub Issue](https://github.com/nju-lug/NJU-Mirror-Issue/issues) |
| Nanjing University Open Source Mirror | FreeBSD Pub | <https://mirrors.nju.edu.cn/freebsd/> | / |
| Nanjing University Open Source Mirror | FreeBSD Packages | <https://mirrors.nju.edu.cn/freebsd-pkg/> | / |
| Nanjing University Open Source Mirror | FreeBSD Ports | <https://mirrors.nju.edu.cn/freebsd-ports/> | / |

FreeBSD official contact information:

- [mirror-admin@freebsd.org](mailto:mirror-admin@freebsd.org)
- [freebsd-hubs@freebsd.org](mailto:freebsd-hubs@freebsd.org), this mailing list appears to no longer be active.

## Other Approaches or Solutions

Build and distribute using Poudriere yourself. See [RISC-V FreeBSD-pkg Software Repository Launched! 11619+ Pre-compiled Packages for Rapidly Building FreeBSD Environments](https://mp.weixin.qq.com/s/ngv3eZh1TEVgk3Pn3XfRBg) (project has ceased maintenance)

## Exercises

1. Find the FreeBSD-pkg synchronization script provided by USTCLUG, set up a minimal pkg mirror site locally, and test whether it can be used as a software source by other FreeBSD systems.

2. Compare the mirror site authorization policies of the FreeBSD, OpenBSD, and NetBSD projects, and analyze how these differences affect their respective user base sizes and ecosystem health.

3. Assuming a bare metal server can be sponsored to the FreeBSD Project, design a complete mirror site deployment plan.
