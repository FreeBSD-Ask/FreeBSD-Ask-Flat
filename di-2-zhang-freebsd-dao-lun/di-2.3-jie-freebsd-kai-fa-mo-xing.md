# 2.3 FreeBSD Development Model

FreeBSD's version releases follow this pattern: after branching STABLE from CURRENT (main branch), the STABLE branch goes through the iterative cycle of ALPHA → BETA → RC → RELEASE, while the STABLE branch's ABI remains fixed, each with clearly defined use cases.

## FreeBSD Version Overview

FreeBSD's version management system includes two development branches (CURRENT and STABLE) and four version stages (ALPHA → BETA → RC → RELEASE), where ALPHA is a pre-release snapshot created after the STABLE branch is established.

The specific process: CURRENT → branch off STABLE branch → ALPHA → BETA → RC → RELEASE.

RELEASE versions go through a complete BETA→RC testing cycle, and after release only accept security and stability fixes, making them suitable for production environments.

![FreeBSD Version Evolution](../.gitbook/assets/bsd-release-versions.png)

> **Note**
>
> The version evolution relationship shown in this diagram is somewhat simplified. In the actual process, X.0-RELEASE comes from the X-STABLE branch, and X-STABLE is branched from the main branch (i.e., X.CURRENT).

> **Note**
>
> The `freebsd-update` command only applies to ALPHA, BETA, RC, and RELEASE versions; other versions (such as CURRENT and non-release snapshots of STABLE) require source code compilation or binary PkgBase updates.

## CURRENT Branch

FreeBSD-CURRENT is the most cutting-edge source code of FreeBSD, containing work in progress, experimental changes, and transitional mechanisms that may appear in the next official release. Although many FreeBSD developers compile FreeBSD-CURRENT source code daily, there are times when the source code cannot compile. These issues are resolved as quickly as possible, but whether FreeBSD-CURRENT brings disaster or new features depends on the version of the synchronized source code.

FreeBSD-CURRENT is primarily aimed at three interest groups:

- FreeBSD community members who actively participate in a portion of the source tree.

- Active testers who are willing to spend time solving problems, making suggestions about changes and the overall direction of FreeBSD, and submitting patches.

- Users who want to follow the state of FreeBSD, use the current source code as a reference, or occasionally make comments or contribute code.

Pre-release features have not been thoroughly tested and are likely to contain defects; FreeBSD-CURRENT should not be considered a shortcut to early access to new features. Other commits are just as likely to introduce new defects as to fix existing ones, so it is also not a shortcut for obtaining defect fixes. FreeBSD-CURRENT is not "officially supported."

Users of -CURRENT should track FreeBSD-CURRENT:

1. Join the [FreeBSD-CURRENT mailing list](https://lists.freebsd.org/subscription/freebsd-current) and the [source repository main branch commit messages list](https://lists.freebsd.org/subscription/dev-commits-src-main). This is **required** in order to understand people's comments about the current state of the system and to receive important announcements about the current state of FreeBSD-CURRENT. The [source repository main branch commit messages list](https://lists.freebsd.org/subscription/dev-commits-src-main) records the commit log entries for each change, along with related information about possible side effects.

   To join these lists, visit the [FreeBSD list server](https://lists.freebsd.org/), click on the list you want to subscribe to, and follow the instructions. If you want to track changes to the entire source tree, not just FreeBSD-CURRENT changes, subscribe to the [source repository commit messages list for all branches](https://lists.freebsd.org/subscription/dev-commits-src-all).
2. Synchronize with the FreeBSD-CURRENT source code. Typically, git is used to check out the -CURRENT code from the `main` branch of the FreeBSD Git repository.
3. Due to the large repository size, some users choose to synchronize only the parts of the source code they are interested in or are contributing patches to. However, users planning to compile the operating system from source must download **all** of FreeBSD-CURRENT, not just selected portions. Please read the [FreeBSD-CURRENT mailing list](https://lists.freebsd.org/subscription/freebsd-current) and **/usr/src/UPDATING** to stay updated and learn about boot procedures that may sometimes become necessary.
4. Participate actively! FreeBSD-CURRENT users are encouraged to submit enhancements or defect fixes. Proposals accompanied by code are always welcome.

## STABLE Branch

FreeBSD-STABLE is the development branch used for releasing major versions.

Unlike the concept of "stable release" in typical Linux distributions, the "stable" in its name refers to the stability of the branch's ABI (Application Binary Interface), not the overall stability of the system; it can also be understood as "fixed." No production server should be updated to FreeBSD-STABLE without adequate testing in a development or test environment. The latest official release of FreeBSD, i.e., RELEASE, should be used.

Code in the CURRENT branch is pushed to the STABLE branch after sufficient testing (typically requiring a minimum MFC period of three days; MFC stands for `Merge From CURRENT`, similar to `backporting`), but this does not guarantee that both branches are free of major defects. Although the FreeBSD-STABLE branch should always compile and run, there is no guarantee.

STABLE is still a development branch; at any time, FreeBSD-STABLE source code may not be suitable for general use. It is simply another engineering development track, not intended for end users.

More users run FreeBSD-STABLE than FreeBSD-CURRENT, and some defects and edge cases not found in FreeBSD-CURRENT are exposed in FreeBSD-STABLE, so one should not blindly track FreeBSD-STABLE.

Developers who wish to track or participate in the FreeBSD development process, especially those related to the next FreeBSD release, should consider tracking FreeBSD-STABLE.

To track FreeBSD-STABLE:

1. Join the [FreeBSD-STABLE mailing list](https://lists.freebsd.org/subscription/freebsd-stable) to stay informed about build dependencies or other issues that may arise in FreeBSD-STABLE that require special attention. Developers also announce controversial fixes or updates they are considering on this mailing list, and users can respond and provide feedback during this period.

   Join the relevant git list to track the selected branch. For example, users tracking the 15-STABLE branch should join the [stable branch commit messages list](https://lists.freebsd.org/subscription/dev-commits-src-branches). This list records the commit log entries for each change, along with any related information about possible side effects.

   To join these lists, visit the [FreeBSD list server](https://lists.freebsd.org/), click on the list you want to subscribe to and follow the instructions. If you want to track changes to the entire source tree, subscribe to the [source repository commit messages list for all branches](https://lists.freebsd.org/subscription/dev-commits-src-all).
2. To install a new FreeBSD-STABLE system, you can obtain weekly snapshots based on FreeBSD-STABLE from FreeBSD mirror sites.
   If you want to compile or upgrade an existing FreeBSD system to FreeBSD-STABLE, use git to check out the source code of the desired branch. Branch names (such as **stable/15**) are listed at <https://www.freebsd.org/releng>.
3. Before compiling or upgrading to FreeBSD-STABLE, carefully read **/usr/src/Makefile** and the [FreeBSD-STABLE mailing list](https://lists.freebsd.org/subscription/freebsd-stable) and **/usr/src/UPDATING** to stay updated and learn about additional boot procedures that are sometimes required during the transition to the next release.

## Exercises

1. Analyze the version change history of FreeBSD.
