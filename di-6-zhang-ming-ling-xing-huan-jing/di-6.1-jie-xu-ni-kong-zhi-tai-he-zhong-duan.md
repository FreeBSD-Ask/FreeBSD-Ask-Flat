# 6.1 Virtual Consoles and Terminals

A Virtual Console is a multi-terminal mechanism provided by FreeBSD that allows users to use multiple independent login sessions simultaneously on the same physical display and keyboard. Each virtual console has its own login prompt and Shell, and users can switch between them using the Alt+F1 through Alt+F8 key combinations.

The concept of a Terminal originates from physical devices (teletypewriters, TTY) used to communicate with the host computer in early computing systems, and has evolved into software-emulated terminal devices in modern systems. FreeBSD's virtual console device names range from ttyv0 to ttyvf (16 total, using hexadecimal suffixes), with ttyv0 through ttyv7 enabled by default. Among them, ttyv0 is the System Console, where system messages are output by default. The number of virtual consoles is configured in the **/etc/ttys** file.

## Logging into FreeBSD

After the FreeBSD system is installed and boots normally, the user will see the following system prompt on the screen:

```sh
FreeBSD/amd64 (ykla) (ttyv0)

login:
```

This screen interface is known in the history of computer technology as a TTY (teletypewriter), also referred to as a physical terminal. TTY was an early interface form for user interaction with the operating system kernel, and was the primary means of human-computer interaction (HCI) before the widespread adoption of graphical user interfaces (GUI).

Explanation:

| Item | Description |
| ---- | ----------- |
| `FreeBSD` | Operating system name |
| `amd64` | Architecture; Intel and AMD processors typically use amd64 (i.e., x86-64) |
| `ykla` | Hostname, set by the user during system installation |
| `ttyv0` | The first TTY; numbering sequences for many things in computers start from 0 |
| `login:` | Prompts the user to log in |

Enter the username and password to log into the system:

```sh
FreeBSD/amd64 (ykla) (ttyv0)

login: root # Enter the username here, then press Enter ①
Password: # Enter the password here, then press Enter
Last login: Tue Mar 18 17:24:48 2025 from 3413e8b6b43f
FreeBSD 15.0-CURRENT (GENERIC) main-n275981-b0375f78e32a

Welcome to FreeBSD!

Release Notes, Errata: https://www.FreeBSD.org/releases/
Security Advisories:   https://www.FreeBSD.org/security/
FreeBSD Handbook:      https://www.FreeBSD.org/handbook/
FreeBSD FAQ:           https://www.FreeBSD.org/faq/
Questions List:        https://www.FreeBSD.org/lists/questions/
FreeBSD Forums:        https://forums.FreeBSD.org/

Documents installed with the system are in the /usr/local/share/doc/freebsd/
directory, or can be installed later with:  pkg install en-freebsd-doc
For other languages, replace "en" with a language code like de or fr.

Show the version of FreeBSD installed:  freebsd-version ; uname -a
Please include that output and any error messages when posting questions.
Introduction to manual pages:  man man
FreeBSD directory layout:      man hier

To change this login announcement, see motd(5).
```

At this point, you have successfully logged into the FreeBSD operating system.

> **Note**
>
> The password will not be echoed to the screen. When FreeBSD involves password input, the screen will not display any echo characters (UNIX systems typically do not display asterisks or other placeholders). Even when a password has been entered, the screen remains blank. Simply type the password and press Enter.

- ①: root is the superuser account in UNIX systems, possessing the highest privileges. Common operations such as Android rooting, Apple jailbreaking, and Kindle jailbreaking all aim to obtain this root privilege.

### Troubleshooting and Unresolved Issues

- If the username is correct but the password is incorrect:

```sh
login: root
Password:
Login incorrect # Indicates incorrect login information
login:
```

- If both the username and password are incorrect:

```sh
login: test # This user does not exist on the current system
Password:
Login incorrect
login:
```

If you cannot confirm the username, it is recommended to recover the root password and then check the list of user accounts on the system, or reinstall the system directly.

### References

- ItsFOSS. What is TTY in Linux?[EB/OL]. [2026-03-25]. <https://itsfoss.com/what-is-tty-in-linux/>. Detailed introduction to TTY concepts and history.

## Virtual Consoles

Although the system console can be used for interaction, system messages are output to the system console by default, which can overwrite commands or files the user is currently working on and interfere with operations. Therefore, users accustomed to the command line typically choose virtual consoles over the system console.

By default, FreeBSD configures multiple virtual consoles for entering commands. Each virtual console has its own login prompt and shell, and switching between virtual consoles is quite convenient. This essentially provides the command-line equivalent of opening multiple windows simultaneously in a graphical environment.

FreeBSD reserves the Alt+F1 through Alt+F8 key combinations for switching between virtual consoles. Use Alt+F1 to switch to the system console (ttyv0), Alt+F2 to access the first virtual console (ttyv1), Alt+F3 to access the second virtual console (ttyv2), and so on. When using Xorg as the graphical console, the key combination changes to Ctrl+Alt+F1 to return to a text-based virtual console.

When switching from one console to another, FreeBSD manages screen output, thereby achieving the effect of parallel sessions across multiple virtual screens and keyboards, allowing users to enter commands for FreeBSD to execute. Programs started in a virtual console will not stop running when the user switches to another virtual console.

In FreeBSD, the number of available virtual consoles is configured in the following section of the **/etc/ttys** file:

```ini
……other entries omitted……

# name	getty				type	status		comments
#
# If console is marked "insecure", init will ask for the root password
# when going to single-user mode.
console	none				unknown	off insecure
#
ttyv0	"/usr/libexec/getty Pc"		xterm	onifexists secure
# Virtual terminals
ttyv1	"/usr/libexec/getty Pc"		xterm	onifexists secure
ttyv2	"/usr/libexec/getty Pc"		xterm	onifexists secure
ttyv3	"/usr/libexec/getty Pc"		xterm	onifexists secure
ttyv4	"/usr/libexec/getty Pc"		xterm	onifexists secure
ttyv5	"/usr/libexec/getty Pc"		xterm	onifexists secure
ttyv6	"/usr/libexec/getty Pc"		xterm	onifexists secure
ttyv7	"/usr/libexec/getty Pc"		xterm	onifexists secure
ttyv8	"/usr/local/bin/xdm -nodaemon"	xterm	off secure

……other entries omitted……
```

To disable a virtual console, add the comment symbol `#` at the beginning of the line corresponding to that virtual console. For example, to reduce the number of available virtual consoles from eight to four, add `#` at the beginning of the last four lines representing virtual consoles ttyv5 through ttyv8:

```ini
……other entries omitted……

#ttyv5	"/usr/libexec/getty Pc"		xterm	onifexists secure
#ttyv6	"/usr/libexec/getty Pc"		xterm	onifexists secure
#ttyv7	"/usr/libexec/getty Pc"		xterm	onifexists secure
#ttyv8	"/usr/local/bin/xdm -nodaemon"	xterm	off secure

……other entries omitted……
```

> **Tip**
>
> If a mistake is made but the SSHD service has been configured, you can still connect to the FreeBSD system remotely via SSH, which will generate a pts(4) pseudo-terminal **/dev/pts/n**.
>
> ```sh
> $ w
> 10:22PM  up 27 mins, 3 users, load averages: 0.03, 0.04, 0.00
> USER       TTY      FROM            LOGIN@  IDLE WHAT
> ykla       pts/0    192.168.179.1   9:55PM     4 su (sh)
> ykla       pts/1    192.168.179.1  10:22PM     - w
> ```

Note that the last virtual console (ttyv8) is used to access the graphical environment (if Xorg has been installed and configured).

For a detailed description of each column in this file and the available options for virtual consoles, refer to ttys(5).

## Single-User Mode

The FreeBSD boot menu provides an option labeled "Boot Single User." If this option is selected, the system will boot into a special mode called "single-user mode." This mode is typically used to repair a system that cannot boot, or to reset the root password when it is unknown.

In single-user mode, networking and other virtual consoles are unavailable. However, the user can obtain full root access, and by default, no root password is required. For these reasons, physical access to the keyboard is required to boot into this mode, so the ownership of physical access to the keyboard is an important security consideration when securing a FreeBSD system.

The settings controlling single-user mode are located in the following section of the **/etc/ttys** file:

```ini
……above omitted……

# name	getty				type	status		comments
#
# If console is marked "insecure", init will ask for the root password
# when going to single-user mode.
console	none				unknown	off secure	# Note this line
#

……below omitted……
```

By default, the status is set to `secure`. This setting assumes that the ownership of physical access to the keyboard is not a concern, or is already managed by physical security policies.

Because anyone with access to the keyboard can boot into single-user mode, changing this setting to `insecure` indicates that the physical environment itself is considered insecure. When `secure` in this line is changed to `insecure`, that is:

```ini
console	none				unknown	off insecure
```

FreeBSD will prompt for the root password when the user chooses to boot into single-user mode:

```sh
Enter root password, or ^D to go multi-user
Password:
```

Be cautious when changing this setting to `insecure`. If the root password is forgotten, it is still possible to boot into single-user mode using installation media, but users unfamiliar with the FreeBSD boot process may face difficulties.

## Adjusting the Boot Interface and TTY Resolution

### Modifying "gop" (General Method)

When the FreeBSD menu appears, press **ESC** to exit the boot, and the `OK` prompt will appear. Enter `gop list` to view the list of all supported resolutions:

```sh
OK gop list
mode 0: 1920x1080x32, stride=1920   # Display mode 0, resolution 1920x1080, color depth 32-bit, stride 1920
mode 1: 640x480x32, stride=640       # Display mode 1, resolution 640x480, color depth 32-bit, stride 640
mode 2: 800x600x32, stride=800       # Display mode 2, resolution 800x600, color depth 32-bit, stride 800
mode 3: 1024x768x32, stride=1024     # Display mode 3, resolution 1024x768, color depth 32-bit, stride 1024
mode 4: 1280x720x32, stride=1280     # Display mode 4, resolution 1280x720, color depth 32-bit, stride 1280
mode 5: 1280x1024x32, stride=1280    # Display mode 5, resolution 1280x1024, color depth 32-bit, stride 1280
```

Here, select `mode 0` for testing:

```sh
OK gop set 0
```

The effect will be displayed immediately.

After confirming the effect is suitable, continue booting:

```sh
OK menu
```

This command confirms the operation or enters the menu interface.

Write this configuration to the **/boot/loader.conf** file, setting the GOP mode to 0:

```ini
exec="gop set 0"
```

### `efi_max_resolution` (UEFI) or `vbe_max_resolution` (BIOS)

You can also set the resolution under UEFI or BIOS through configuration files. According to the documentation LOADER.CONF(5), these two variables accept the following values:

| Value | Resolution |
| ----- | ---------- |
| `480p` | 640x480 |
| `720p` | 1280x720 |
| `1080p` | 1920x1080 |
| `1440p` | 2560x1440 |
| `2160p` | 3840x2160 |
| `4k` | 3840x2160 |
| `5k` | 5120x2880 |
| `width x height` | width x height |

This section tests using the `efi_max_resolution` variable: write `efi_max_resolution="1080p"` into the **/boot/loader.conf** file. After rebooting, the effect is the same as the gop method.

### References

- FreeBSD Project. loader.conf(5)[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=loader.conf&sektion=5>.
- FreeBSD Forums. gop set < mode > being ignored in **/boot/loader.conf**[EB/OL]. [2026-03-26]. <https://forums.freebsd.org/threads/gop-set-mode-being-ignored-in-boot-loader-conf.77779/>. Discussion on reasons and solutions for GOP mode settings not taking effect in loader.conf.
- FreeBSD Forums. How to find the valid values of efi_max_resolution[EB/OL]. [2026-03-26]. <https://forums.freebsd.org/threads/how-to-find-the-valid-values-of-efi_max_resolution.84840/>. Exploring methods for querying valid values of efi_max_resolution.

## Exercises

1. Switch between multiple virtual consoles (ttyv0—ttyv3) in FreeBSD, log in with different users respectively, use the `w` command to record session information for each terminal, and analyze the differences in session management between virtual consoles and pseudo-terminals.
2. Review the core source code of the TTY subsystem in the FreeBSD kernel (**sys/kern/tty\*.c** and **sys/sys/tty\*.h**), and analyze its input/output buffer management and line discipline implementation mechanisms.
3. Modify the contents of `/etc/motd.template` (`/etc/motd` is a symbolic link to `/var/run/motd`, generated from `/etc/motd.template` at startup; see motd(5)) and its display behavior (e.g., controlling via the `hushlogin` feature in `/etc/login.conf`), and record the differences in information output when users log in before and after the modification.
