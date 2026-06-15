# Bug Reporting Process

> **Tip**
>
> Before submitting a bug report, it is recommended to consult the mailing list first to confirm the nature of the issue, avoid duplicate reports, and make more effective use of community resources.

The FreeBSD bug reporting system is located at [https://bugs.freebsd.org/bugzilla](https://bugs.freebsd.org/bugzilla), providing bug tracking and management functionality.

When encountering a problem, it is recommended to follow this process:

First, confirm that the issue is not caused by user error or misconfiguration. The following steps can help troubleshoot:

- Consult FAQs, documentation, or manual pages;
- Use a search engine to find relevant information;
- Search the mailing lists to see if anyone has reported a similar issue;
- Ask questions on the mailing list.

If you encounter issues on a recently upgraded system, check the instructions in **/usr/src/UPDATING**; if it is a recently upgraded third-party software, check **/usr/ports/UPDATING** and **/usr/ports/CHANGES**.

Related file structure:

```sh
/
├── usr/
│   ├── src/
│   │   └── UPDATING # System upgrade instructions
│   └── ports/
│       ├── UPDATING # Ports upgrade instructions
│       └── CHANGES  # Ports changelog
└── var/
    └── log/
        └── messages # System log file
```

Determine whether the issue is actually a bug. If the problem can be phrased as a question (e.g., "how to do...", "where can I find..."), it is recommended to consult the FreeBSD mailing list first.

If you confirm it is a bug, verify whether the system version is still supported; unsupported versions may not receive attention.

You need to confirm whether the bug has already been fixed:

- Check the system version to see if there are updates and patches available;
- Search the FreeBSD bug tracking system to see if anyone has reported a similar issue.

If no one has reported a similar issue, you can submit a report.

The following types of issues should not be submitted:

- An application has a new version available (automated notifications usually inform the maintainer);
- For unmaintained software, reporting a bug without a patch may go unaddressed; if you intend to maintain the software, submit a related request;
- If the issue cannot be reproduced, it is difficult for others to resolve;
- Requests for new features (these can be discussed on FreeBSD mailing lists, but it is recommended to implement them first before submitting).

Determine who the issue should be reported to:

| Issue Scope | Report To | Exceptions |
| ----------- | --------- | ---------- |
| Base system components, documentation, or website | FreeBSD developers | — |
| Software shipped with the system but maintained by others | FreeBSD developers | When you can confirm the issue is unrelated to the system, report to the software developer |
| Third-party software | Software developer | When you can confirm the issue is related to the system, report to FreeBSD developers |

Tips for writing the report:

- The Summary field will be used as the email subject; do not leave it blank;
- "Summary" should be representative and concise;
- If you have a patch, be sure to upload it;
- Specify the system version and computer architecture; if the software was self-compiled, also state the build settings;
- If the issue is reproducible, specify the conditions that trigger it;
- For kernel issues, prepare the following information: hardware configuration, kernel configuration, whether debug options are enabled, error messages or **/var/log/messages**, and state that you have checked **/usr/src/UPDATING** but the issue persists (as this is frequently asked), and whether you can run other kernels;
- For third-party software issues, prepare: installed software, all environment variables that override **bsd.port.mk** defaults, and state that you have checked **/usr/ports/UPDATING** but the issue persists (as this is frequently asked);
- Each report should contain only one issue, unless multiple issues are closely related;
- Be cautious with controversial issues; discussion is recommended first;
- Pay attention to formatting, especially when copying and pasting content;
- Back up your report in case of network issues causing submission failure;
- Be polite; most report recipients are volunteers, please be understanding.

The following explains how to fill in a bug report:

Please note that emails are public and you may receive harassing messages; take protective measures or use a public email address.

- Summary: a concise overview;
- Severity: affects only me / affects some people / affects many people; do not overestimate the severity;
- Category: the current Bugzilla uses a two-level Product/Component structure. The main products are as follows:
  - Base System: the base system
    - kern (kernel, libraries, built-in drivers, manual sections 2-4), except for the USB subsystem and threading library
    - bin (built-in programs)
    - conf (configuration files or startup scripts, man 5)
    - usb (USB subsystem)
    - threads (threading library)
    - standards (standards compliance)
    - specific architecture (architecture-specific)
    - misc (other, or truly unclassifiable; it is recommended to ask on the mailing list first), etc.;
  - Ports & Packages: third-party software; the main component is Individual Port(s);
  - Documentation: documentation or website;
- Description: the content should be complete and detailed, without personal speculation; it should also include runtime environment information (system version, program version, system configuration, etc.).

FreeBSD has limited developers, and bug reports may go unaddressed for a long time. Most report recipients are volunteers and should not be unduly pressured. If possible, try to resolve the issue yourself first, then submit it upstream.

> **Tip**
>
> The [FreeBSD GrimoireLab monitoring dashboard](https://grimoire.freebsd.org/) provides an overview of the current bug report status for the FreeBSD project, displaying data analysis of project development activity and community contributions for an intuitive understanding of the situation.

## References

- FreeBSD Project. Writing FreeBSD Problem Reports[EB/OL]. [2026-04-17]. <https://docs.freebsd.org/en/articles/problem-reports/>. The official FreeBSD problem report writing guide, detailing the bug report submission process and field meanings.

## Exercises

1. Find a submitted FreeBSD kernel bug report, analyze the completeness of its information, reproduce its reporting process, and attempt to resolve it.
2. Redesign a localized code of conduct.
3. The Code of Conduct (CoC) in free software communities has evolved from reflecting radical egalitarianism to a more moderate and inclusive organizational governance tool. Compare the differences between the FreeBSD community's current CoC and the Linux kernel community's CoC in dispute resolution mechanisms, and analyze whether a clear code of conduct has indeed improved contribution diversity in the community.
