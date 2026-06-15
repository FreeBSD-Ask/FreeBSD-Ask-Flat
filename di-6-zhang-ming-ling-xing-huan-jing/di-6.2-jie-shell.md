# 6.2 Shell Basics

This section introduces what a Shell is and how to modify the default login environment.

## Overview

![What is shell](../.gitbook/assets/what-is-shell.png)

A shell is a command interpreter that enables user interaction with the operating system kernel, accepting commands entered by the user and passing them to the kernel for execution. User commands run within the shell, and users interact with the system through the shell. The shell provides a command-line interface, receiving commands from an input channel and executing them. Many shells provide built-in features to assist with daily tasks, such as file management, filename globbing, command-line editing, command macros, and environment variables. The FreeBSD base system includes multiple shells, including the extended POSIX shell (sh(1)) and the extended C shell (tcsh(1)). Other shells are available through the FreeBSD Ports, such as Zsh and Bash.

The default shell for the FreeBSD root user is sh (since FreeBSD 14), but the base system also provides csh/tcsh as an alternative.

> **Tip**
>
> Relationship between csh and tcsh
>
> In FreeBSD, csh and tcsh are the same binary program, but their behavior differs when invoked under different names. This can be verified by examining the source code at [https://github.com/freebsd/freebsd-src/blame/main/bin/csh/Makefile](https://github.com/freebsd/freebsd-src/blame/main/bin/csh/Makefile) and running man csh: both redirect to tcsh.

> **Note**
>
> Although csh and tcsh are essentially the same program, if invoked with csh's parameters, some tcsh extensions will be disabled, resulting in behavioral differences during use.

## POSIX Shell Specification

POSIX (Portable Operating System Interface) is an operating system interface standard developed by IEEE and The Open Group, aimed at ensuring application portability across different UNIX systems. The shell and utilities specification (Shell Command Language) in the POSIX.1 standard defines the minimum set of functionality that a conforming shell must implement, including command syntax, variable expansion, quoting rules, conditional expressions, loop structures, and built-in commands. The current version is POSIX.1-2024 (IEEE Std 1003.1-2024).

Core requirements of the POSIX shell specification include:

- **Command Execution**: Support for executing simple commands, pipelines, lists, and compound commands.
- **Variable and Parameter Expansion**: Support for positional parameters, special parameters, and various forms of variable expansion.
- **Quoting Mechanisms**: Support for single quotes (preserving literal values), double quotes (allowing variable expansion and command substitution), and backslash escaping.
- **Pattern Matching**: Support for filename globbing, including `*`, `?`, and bracket expressions.
- **Conditionals and Loops**: Support for control structures such as `if`, `while`, `for`, `case`, etc.
- **Built-in Commands**: Must implement built-in commands including `cd`, `echo`, `exit`, `export`, `read`, `return`, `set`, `shift`, `trap`, `unset`, etc.

The default shell used by FreeBSD is sh. FreeBSD's **/bin/sh** is not the original Bourne shell written by Stephen R. Bourne at Bell Labs for UNIX V7, but is based on the Almquist shell (ash) released by Kenneth Almquist in 1989, which was designed as a more compact and efficient replacement for the Bourne shell. During the 4.3BSD-Reno/Net/2 period (1990—1991), ash was introduced into the BSD world, and NetBSD subsequently adopted it as the default /bin/sh. FreeBSD then imported the ash implementation from NetBSD, which is largely compliant with the shell specification requirements in the POSIX.1-2024 standard.

The commonly used shell in Linux is bash (Bourne Again shell, a pun on "Born Again," meaning "the reborn Bourne shell"). The default shell in macOS is typically zsh (Z shell).

Linux also provides sh, but it is usually a symbolic link to another shell (e.g., linked to dash in Debian/Ubuntu, or to bash in some distributions), and they are not the actual sh.

For example, the default shell in Ubuntu 24.04 LTS is dash:

```bash
lrwxrwxrwx 1 root root 4 Feb 25 23:19 /bin/sh -> dash
$ ls -l /bin/sh # View detailed information about the /bin/sh file in long format
```

dash is a direct descendant of the NetBSD version of ash (Almquist SHell).

## Keyboard Shortcuts

> **Note**
>
> The execution of the following keyboard shortcuts is not affected by the keyboard's caps lock state (whether Caps Lock is on or off).

### Using the Scroll Lock Key to Scroll Up/Down in the TTY Interface

Use the **Scroll Lock** key: after pressing **Scroll Lock**, you can use the Up ↑/Down ↓ arrow keys and the **Page Up**/**Page Down** keys to scroll the screen.

Differences:

- Up ↑/Down ↓ arrow keys: scroll the TTY interface up or down by one line
- **Page Up**/**Page Down** keys: scroll the TTY interface up or down by one page

Press **Scroll Lock** again to exit this mode.

> **Tip**
>
> The SL key is located above the **HOME** key, to the right of the PS screenshot key **Print Screen**, and to the left of the PB key **Pause/Break**.

From a historical perspective, the **Scroll Lock** key was designed precisely for this purpose—it enables scrolling in a text interface without affecting the cursor position.

### Using Shift Key Combinations to Scroll Up/Down in the TTY Interface

Use **Shift** shortcuts:

- **Shift** + Up ↑/Down ↓ arrow keys — scroll the TTY interface up or down by one line
- **Shift** + **Page Up**/**Page Down** keys — scroll the TTY interface up or down by one page

### Completing Commands or Directories

You can use the **Tab** key to complete commands or directories; the Up arrow **↑** is used to view the previous command, and the Down arrow **↓** is used to view the next command.

- Completing commands

```sh
# lo # Press the Tab key at this point to view a list of commands starting with lo; continue typing more letters and press Tab again to further filter
local                    localedef                login
local-unbound            locate                   logins
local-unbound-anchor     lock                     logname
local-unbound-checkconf  lockf                    look
local-unbound-control    lockstat                 lorder
local-unbound-setup      locktest                 lowntfs-3g
locale
```

- Completing file directories or filenames

```sh
$ cp /home/ykla/ # Press the Tab key here, then press Tab again to observe the effect
$ cp /home/ykla/test/1.txt
.cache/                 .login                  bin/                    test2
.config/                .profile                HW_PROBE/               test3
.cshrc                  .sh_history             mine
.gitconfig              .sh_history.Y8RqIDNDIv  mydir/
.k5login                .shrc
```

### Terminating Commands

To terminate a command, you can use **Ctrl**+**C**:

```sh
# ping 163.com  # Test network connectivity with 163.com
PING 163.com (59.111.160.244): 56 data bytes
64 bytes from 59.111.160.244: icmp_seq=0 ttl=52 time=27.672 ms
64 bytes from 59.111.160.244: icmp_seq=1 ttl=52 time=27.580 ms
^C # Note here, ^C indicates that the Ctrl+C key combination was pressed, and the command was subsequently terminated
--- 163.com ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 27.580/27.626/27.672/0.046 ms
```

FreeBSD's `ping` incorporates the functionality of the former `ping6`, distinguishing protocol versions via the `-4`/`-6` options (Google Summer of Code 2019 project). Linux supports `-O` to report no reply received; FreeBSD's `-O` has a different meaning (used only for IPv6 ICMPv6 Node Information supported query types) and does not support reporting no reply received. `ping -y` (ICMPv6 Node Information DNS Name query) and `ping -k` (Node Information Node Addresses query) originate from the KAME project's ICMPv6 Node Information implementation.

`ping` uses ICMP protocol ECHO_REQUEST datagrams to trigger an ECHO_RESPONSE from the host. IPv4 targets use ICMP, and IPv6 targets use ICMPv6 (RFC 4443). The default data size is 56 bytes, plus an 8-byte ICMP header for a total of 64 bytes. If the data space is at least 8 bytes, the first 8 bytes are used for a timestamp to calculate the round-trip time.

### Others

- **Ctrl**+**L** (the letter L): clear the screen
- **Ctrl**+**A**: move the cursor to the beginning of the command line
- **Ctrl**+**E**: move the cursor to the end of the command line

## Configuring csh/tcsh

For the C shell (csh/tcsh), the login shell reads **/etc/csh.cshrc**, **/etc/csh.login**, **~/.tcshrc** (or **~/.cshrc** if it does not exist), and **~/.login** in sequence.

- Add the following line to the **~/.cshrc** file to set colored output for the `ls` command.

```sh
alias ls ls -G
```

And log in again.

- How to make FreeBSD's csh list candidate files that cannot be completed when pressing Tab, like Bash? Add the following to the **~/.cshrc** file:

```sh
set filec              # Enable command-line filename completion
set autolist           # Automatically display completion list
```

Reload the C shell configuration file to refresh aliases and environment settings:

```sh
# source ~/.cshrc
```

- How to give csh command error correction functionality similar to zsh?

For example, when using emacs to write a C program, typing `emacs ma` and pressing `Tab` followed by Enter will match all files starting with `ma`. This configuration can ignore certain matched files, meaning that pressing `Tab` will not list ignored files, which is convenient for programming and will not match binary `.o` files, etc.

```sh
set correct = cmd        # Enable automatic command spelling correction, prompting for the correct command
# Example: lz/usr/bin tcsh>ls /usr/bin (y|n|e|a)?  # Prompt example when a command spelling error is detected

set fignore = (.o ~)   # Set filename ignore patterns, used to exclude specified files or patterns during completion
```

## References

- The Open Group. Shell Command Language[EB/OL]. [2026-04-23]. <https://pubs.opengroup.org/onlinepubs/9799919799/utilities/V3_chap02.html>.
- Herbert. Dash[EB/OL]. [2026-05-08]. <http://gondor.apana.org.au/~herbert/dash/>. Dash official website
- Almquist K. ash (Almquist shell)[EB/OL]. (1989-05-30)[2026-04-18]. <https://github.com/dsipher/ash>. FreeBSD's **/bin/sh** is based on ash, not Stephen R. Bourne's original Bourne shell.
- Fox B, Ramey C. Bash Reference Manual[M]. Boston: Free Software Foundation, 2022. "Bourne Again shell" is a pun on "Born Again."

## Exercises

1. Write an sh script in FreeBSD that implements a minimal example of filename completion, and document the functional differences between sh's built-in completion mechanism and Bash completion.
2. Review the FreeBSD sh source code (**bin/sh/**), analyze its line editing and keyboard shortcut handling implementation, and compare the gap in interactive functionality with Bash.
3. Modify the shell's default prompt configuration (e.g., via `PS1` or `set prompt`), and document the syntax differences for prompt customization across different shells (sh, csh, zsh).
