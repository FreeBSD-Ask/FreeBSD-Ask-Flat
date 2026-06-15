# 8.2 FreeBSD Software Repositories

FreeBSD's software repositories are divided into four categories: pkg binary package repositories, kernel module repositories, PkgBase system repositories, and Ports source repositories, each configured separately and defaulting to official servers. Domestic users typically need to switch to mirror sites to improve download speeds.

## 15.0-RELEASE Quick Switch of pkg Software Repository to USTC Open Source Software Mirror

This configuration requires the reader to use the PkgBase method during installation, and allows setting the pkg binary package repository (built from Ports), PkgBase repository, and kernel module repository.

Use the ee editor to open the **/usr/local/etc/pkg/repos/FreeBSD.conf** file.

> **Tip**
>
> If the file does not exist or the content after opening is not `FreeBSD-base: { enabled: yes }`, this section does not apply. Please configure manually as described below.

Clear the original content `FreeBSD-base: { enabled: yes }` in the **FreeBSD.conf** file.

Write the following content:

```sh
USTC-ports: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}

FreeBSD-ports: { enabled: no }

USTC-ports-kmods: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/kmods_quarterly_${VERSION_MINOR}",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}

FreeBSD-ports-kmods: { enabled: no }

USTC-base: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/base_release_${VERSION_MINOR}",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkgbase-${VERSION_MAJOR}",
  enabled: yes
}
```

Then run the command `pkg update -f` to refresh the software repository.

> **Tip**
>
> Change both instances of `quarterly` in the above configuration to `latest` to use the rolling-update software repository.

## Switching the pkg Binary Package (Binary Packages Built from Ports) Repository

In FreeBSD, the pkg repository is divided into system-level and user-level configuration files. **Since this file changes with base system updates, it is not recommended** to directly modify the **/etc/pkg/FreeBSD.conf** file.

> **Warning**
>
> Do not enable multiple pkg mirror sites simultaneously. Whether mixing official mirror sites (e.g., `pkg.freebsd.org` with USTC) or domestic unofficial mirror sites, mixing is not recommended. The consequences are similar to mixing FreeBSD quarterly branch Ports with latest branch pkg, which may break software dependency relationships. Example: [Mixing caused KDE desktop to be removed](https://blog.mxdyeah.com/post/freebsd-exp-kde6).

> **Warning**
>
> Do not mix `quarterly` and `latest` simultaneously; they should be consistent across all configuration files.

To get rolling-update packages, change `quarterly` to `latest`. For the differences between the two, see above.

> **Note**
>
> For `CURRENT`, only `latest` is provided.

Example: Use a command to change the system-level `pkg` repository to latest:

```sh
# sed -i '' 's/quarterly/latest/g' /etc/pkg/FreeBSD.conf
```

### 14.X-RELEASE

#### Creating User-Level Repository Directory and File

Create the pkg repository configuration directory:

```sh
# mkdir -p /usr/local/etc/pkg/repos
```

Use the `ee` editor to open the configuration file **/usr/local/etc/pkg/repos/USTC.conf** (this will automatically create the text file **USTC.conf**):

```sh
# ee /usr/local/etc/pkg/repos/USTC.conf
```

> **Note**
>
> In this section, **/usr/local/etc/pkg/repos/USTC.conf** is the shared configuration file for the pkg binary repository, module repository, and PkgBase repository. Subsequent configurations will not repeat this step.

#### USTC Open Source Software Mirror

Edit the **/usr/local/etc/pkg/repos/USTC.conf** file and write **one** of the following configurations:

- quarterly:

```sh
USTC: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD: { enabled: no }
```

- latest:

```sh
USTC: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/latest",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD: { enabled: no }
```

### 15.0-RELEASE

Starting from `FreeBSD 15.0-RELEASE`, the name of the `FreeBSD` repository has been changed from `FreeBSD` to `FreeBSD-ports`.

#### Official Repository

For more information, see the source code **usr.sbin/pkg/FreeBSD.conf.quarterly-release**[EB/OL]. [2026-03-26]. <https://github.com/freebsd/freebsd-src/blob/releng/15.0/usr.sbin/pkg/FreeBSD.conf.quarterly-release>. Same below.

This is the default software repository after 15.0-RELEASE system installation is complete.

#### USTC Open Source Software Mirror

Edit the **/usr/local/etc/pkg/repos/USTC.conf** file and write the following configuration (quarterly branch, i.e., 2024Q3, 2025Q1, etc.):

```sh
USTC-ports: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-ports: { enabled: no }
```

If using the latest branch (rolling update, built from the Ports main branch), change to the following configuration:

```sh
USTC-ports: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/latest",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-ports: { enabled: no }
```

## Kernel Module Repository (Kernel modules, kmods)

### 14.X-RELEASE

#### USTC Open Source Software Mirror

Edit the **/usr/local/etc/pkg/repos/USTC.conf** file and write the following configuration (quarterly branch, i.e., 2024Q3, 2025Q1, etc.):

If using the quarterly branch, the configuration is as follows:

```sh
USTC-kmods: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/kmods_quarterly_${VERSION_MINOR}",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-kmods: { enabled: no }
```

If using the latest branch, the configuration is as follows:

```sh
USTC-kmods: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/kmods_latest_${VERSION_MINOR}",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-kmods: { enabled: no }
```

### 15.0-RELEASE

Starting from `FreeBSD 15.0-RELEASE`, the name of the `kmods` repository has been changed from `FreeBSD-kmods` to `FreeBSD-ports-kmods`.

#### USTC Open Source Software Mirror

Edit the **/usr/local/etc/pkg/repos/USTC.conf** file and write the following configuration (quarterly branch, i.e., 2024Q3, 2025Q1, etc.):

If using the quarterly branch, the configuration is as follows:

```sh
USTC-ports-kmods: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/kmods_quarterly_${VERSION_MINOR}",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-ports-kmods: { enabled: no }
```

If using the latest branch, the configuration is as follows:

```sh
USTC-ports-kmods: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/kmods_latest_${VERSION_MINOR}",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-ports-kmods: { enabled: no }
```

> **Note**
>
> It is recommended to keep the Ports repository and kmods repository (and the PkgBase repository if PkgBase is enabled) on the same mirror site to avoid potential dependency issues.

## PkgBase Repository for the Base System (FreeBSD 15.0 Technology Preview, 14.x Can Be Manually Enabled)

`PkgBase` was introduced as a technology preview in `FreeBSD 15.0-RELEASE`. VM and cloud images use PkgBase by default, while physical machines still use the traditional method by default. The FreeBSD Project will continue to support the traditional method until the end of 15.X (planned for removal in FreeBSD 16). When using `PkgBase` to upgrade the system in a production environment, be sure to back up your data.

> **Tip**
>
> 14.x users can choose to directly convert from the traditional installation method to PkgBase; see other relevant chapters in this book.

### Official Repository

Default path: **/etc/pkg/FreeBSD.conf** (do not modify, shown for reference only)

```sh
FreeBSD-base: {
  url: "pkg+https://pkg.FreeBSD.org/${ABI}/base_release_${VERSION_MINOR}",
  mirror_type: "srv",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkgbase-${VERSION_MAJOR}",
  enabled: no
}
```

> **Note**
>
> According to the FreeBSD source code [usr.sbin/bsdinstall/scripts/pkgbase.in](https://github.com/freebsd/freebsd-src/blob/releng/15.0/usr.sbin/bsdinstall/scripts/pkgbase.in) last few segments of source code, although the FreeBSD-base repository in **/etc/pkg/FreeBSD.conf** is `enabled: no`, users who chose PkgBase during installation will have `FreeBSD-base: { enabled: yes }` written to the **/usr/local/etc/pkg/repos/FreeBSD.conf** file to explicitly override the default configuration. Therefore, the FreeBSD-base repository is effectively enabled by default for PkgBase users.

#### USTC Open Source Software Mirror

Disable the default official FreeBSD-base repository:

```sh
# mv /usr/local/etc/pkg/repos/FreeBSD.conf /usr/local/etc/pkg/repos/FreeBSD.conf.back
```

> **Tip**
>
> In some environments, the file **/usr/local/etc/pkg/repos/FreeBSD.conf** may also contain definitions for non-FreeBSD-base repositories. Users are advised to check the file contents before renaming.

Edit the **/usr/local/etc/pkg/repos/USTC.conf** file and write the following configuration:

```sh
USTC-base: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/base_release_${VERSION_MINOR}",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkgbase-${VERSION_MAJOR}",
  enabled: yes
}
```

> **Warning**
>
> For **RELEASE** versions of the system, PkgBase is almost entirely fixed throughout its lifecycle!
>
> The `base_latest` and `base_weekly` repositories are only for STABLE or CURRENT!
>
> Do not change the string `base_release_${VERSION_MINOR}`!

> **Tip**
>
> When upgrading from 14.X PkgBase system to 15.0, signing key issues are commonly encountered. Please ensure that **/usr/share/keys/pkgbase-15** exists (if missing, you can manually fetch it from the official repository or refer to the upgrade instructions in the Release Notes). Otherwise, a "no trusted public keys found" error will occur. For details, see [15.0 Release Notes - Upgrading](https://www.freebsd.org/releases/15.0R/relnotes/#upgrade) and related forum discussions.

## STABLE/CURRENT Quick Switch of pkg Software Repository to USTC Open Source Software Mirror

> **Warning**
>
> STABLE/CURRENT are not production versions and are not suitable for production environments. Users of these versions are presumed to have a certain level of knowledge, so this section does not list specific steps or excessive explanations.

### Kernel Module Repository

- For `FreeBSD 14.x-STABLE`

```sh
USTC-kmods: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/kmods_latest",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-kmods: { enabled: no }
```

- For `FreeBSD 15.0-STABLE / FreeBSD 16.0-CURRENT`:

```sh
USTC-ports-kmods: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/kmods_latest",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
FreeBSD-ports-kmods: { enabled: no }
```

### PkgBase Repository

```sh
USTC-base: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/base_latest",
  mirror_type: "none",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkgbase-${VERSION_MAJOR}",
  enabled: yes
}
```

## Ports Source (Distfiles)

For methods to obtain Ports themselves (via Git or archive files), see other chapters. This repository is used to download the source code of software (called Ports) in the Ports framework.

> **Warning**
>
> The Ports source may not be complete, which is a limitation of the Ports framework structure. For details, see <https://github.com/ustclug/discussions/issues/408>.

Create or modify the file **/etc/make.conf**. Write **one** of the following configurations:

- NJU Open Source Mirror

```ini
MASTER_SITE_OVERRIDE?=https://mirrors.nju.edu.cn/freebsd-ports/distfiles/${DIST_SUBDIR}/
```

- USTC Open Source Software Mirror

```ini
MASTER_SITE_OVERRIDE?=https://mirrors.ustc.edu.cn/freebsd-ports/distfiles/${DIST_SUBDIR}/
```

## Troubleshooting and Unfinished Business

### Balancing Security and Convenience

Using unofficial mirror sites improves download speed but introduces the risk of man-in-the-middle attacks — mirror site administrators could theoretically inject malicious code into software packages. The FreeBSD official cluster internally uses `zfs send/receive` instead of rsync for data synchronization, and the official rsync service is not open to the public, partly to reduce such risks. For production environments with higher security requirements, it is recommended to use official repositories or build your own Poudriere build server to fully control software packages.

### Why Write Complete Options in the pkg Configuration File (mirror_type / signature_type / fingerprints)

Although pkg works fine with only `url` and `enabled: yes` (pkg defaults to `mirror_type: "none"` and `signature_type: "none"`), doing so will **disable signature verification**. The system will not check whether the packages downloaded by pkg have been tampered with, which may pose security risks (especially for ports, kmods, and PkgBase system packages).

The advantages are:

- Enabling `signature_type: "fingerprints"` and `fingerprints`: Uses FreeBSD's built-in official keys to verify package signatures
- `mirror_type: "none"`: Suitable for domestic HTTPS direct-link mirrors (because `pkg+https://` supports DNS SRV, the official site uses `"srv"`, but mirror sites do not need it)

It is recommended to always enable signature verification in production environments. Removing this configuration disables signature verification, which is not recommended unless you fully trust the network environment.

> **Warning**
>
> There are currently no FreeBSD official mirror sites in mainland China.
>
> For users with higher security requirements, the default official mirror `pkg.freebsd.org` should be used! It is built, distributed, and maintained by the FreeBSD Project officially.
