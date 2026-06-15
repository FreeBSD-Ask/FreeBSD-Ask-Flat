# 6.4 Command-Line Basics

The Command Line Interface (CLI) is the primary interaction method for UNIX-like systems, providing a direct way to operate the system.

## Who Am I?

- View the username of the currently logged-in user:

```sh
$ whoami
ykla
```

> **Tip**
>
> `whoami` has been superseded by id(1), and is equivalent to `id -un`.

- View information about the groups the currently logged-in user belongs to.

```sh
$ id
uid=1001(ykla) gid=1001(ykla) groups=1001(ykla),0(wheel)
```

- View the terminal the current user is logged into and the current login time.

```sh
$ who
root             pts/0        Mar 19 15:00 (3413e8b6b43f)
```

BSD who differs significantly from GNU/Linux's `who`; FreeBSD's `who` does not support Linux's `-d` (dead processes), `-p` (init active processes), `--lookup`, `--ips`, and other options.

## Where Do I Come From?

Display which users are currently logged in and what they are doing:

```sh
$ w
 3:02PM  up 21:52, 1 user, load averages: 0.01, 0.01, 0.00
USER       TTY      FROM           LOGIN@  IDLE WHAT
root       pts/0    3413e8b6b43f   3:00PM     - w
```

BSD `w` differs significantly from GNU/Linux's `w`; FreeBSD's `w` does not support Linux `w`'s `-f`, `-s`, `-u`, and other options.

## Where Am I?

- View the current path.

`pwd` stands for `print working directory`, used to print the current working directory.

```sh
$ pwd
/usr/ports/editors/vscode
```

BSD `pwd` is compatible with the POSIX standard and is largely compatible with GNU/Linux's `pwd`.

## Who Exactly Am I?

Account switching and logging out:

```sh
root@ykla:/ # su ykla ①
ykla@ykla:/ $ ②
ykla@ykla:/ $ su ③
Password: ④
root@ykla:/ #
root@ykla:/ # exit ⑤
ykla@ykla:/ $ exit ⑥
root@ykla:/ # exit ⑦

FreeBSD/amd64 (ykla) (ttyv0)

login:
```

- ① Using `su` followed by a space and a username switches to the user ykla. If switching from root to ykla, no password for ykla is required:
  - `root@ykla:/`:
  - `root`: the current user is root
  - `@`: indicates "who" is on the "xx" host
  - `ykla`: this is the hostname, unrelated to the user ykla. The hostname can be set by the user
  - `:/`: represents being in the `/` path
- ② Note the change in the prompt symbol: root uses `#`, and regular users use `$` (csh uses `%`)
- ③ If you only enter `su` and press Enter, the command means switching from the current user to the root account (if already root, nothing changes). Non-members of the wheel group cannot directly `su` to root; the system will return a `sorry` error message, but they can `su` to other users.
- ④ Switching from a regular user to root requires the root account's login password.
- ⑤ Entering `exit` logs out the current user. If this is the only logged-in user, it will log out to the TTY.

> **Thought Question**
>
> What users did ⑥ and ⑦ switch to, or what operations were performed?

The `su` command switches to the login shell defined for the target user in **/etc/passwd**. Note: when using `su -m`, if the target user's shell is not in **/etc/shells** and the caller is not root, `su` will fail. The `chsh` command can only change the shell to a standard shell listed in **/etc/shells**. `su -` or `su -l` not only switches users but also changes the working directory to the target user's home directory and resets environment variables.

Comparison of BSD and GNU `su` behavior:

| Item | FreeBSD `su` Behavior | Linux `su` Behavior |
| ---- | --------------------- | ------------------- |
| `-c` | Specifies login class | Executes the specified command |
| `-s` | Sets MAC label (Mandatory Access Control label) | Specifies the login shell |
| Command execution method | Passes the command as an argument to the target user's shell for execution | Uses `-c` to directly execute the command |

## Where Am I Going?

The `cd` command, which stands for "change the working directory," switches the current working directory.

```sh
ykla@ykla:~ $ pwd
/home/ykla
ykla@ykla:~ $ cd .
ykla@ykla:~ $ pwd
/home/ykla
ykla@ykla:~ $ cd ..
ykla@ykla:/home $ pwd
/home
ykla@ykla:/home $ cd ..
ykla@ykla:/ $ pwd
/
ykla@ykla:/ $ cd /home/ykla
ykla@ykla:~ $ cd ../..
ykla@ykla:/ $ pwd
/
```

Based on the output above, readers should consider: what do `.` and `..` represent respectively?

> **Tip**
>
> In FreeBSD's sh(1), the behavior of `cd` is specified by the POSIX standard.

## Command Line Format

Most command-line command names have clear meanings, for example, `ls` stands for `list`, `wget` means to `get` via the web (network); there are also a few commands whose functions are difficult to infer from their names, such as the `thefuck` command (used to automatically correct spelling errors).

The basic syntax structure of the command line follows the POSIX Shell Command Language specification ([IEEE Std 1003.1](https://pubs.opengroup.org/onlinepubs/9799919799/)). Its general format is as follows:

```sh
# Command  Option  Argument 1   Argument 2
# ls       -l     /home/ykla   /tmp
/home/ykla:
total 317
  ……some entries omitted……
drwxr-xr-x  2 ykla ykla        2 Mar  9 20:45 Downloads
drwxr-xr-x  2 ykla ykla        2 Mar  9 20:45 Desktop

/tmp:
total 6
drwxrwxrwt  2 root    wheel  3 Mar 18 17:23 .ICE-unix
-r--r--r--  1 root    wheel 11 Mar 18 17:10 .X0-lock
```

Here, `ls` (lowercase L) means to list files in the current directory or a specified directory; the option `-l` (lowercase L) means to print detailed information, outputting in long (*long*) format. Multiple short options can be combined, such as `-a -l` being equivalent to `-al`.

Options are used to modify the behavior of a command; arguments are the objects that the command operates on.

> **Tip**
>
> After a command executes, it returns an exit status: 0 indicates success, and non-zero indicates failure.

Note the difference between Chinese and English writing conventions: Chinese text does not use spaces for separation, while English words must be distinguished using spaces. For this reason, the components of a command line should be separated by spaces ` `. The number of spaces is generally not restricted, but there should be at least one, i.e., ` `.

> **Thought Question**
>
> If spaces or some other means (such as other symbols) are not used to separate the command line, how would the software interpret the entire sentence?
>
> Without spaces, from a natural language perspective, from a human point of view, what would the experience be like for this sentence: `Whatwelovedeclarespubliclywhoexactlyweare.`?
>
> Consider also: `ls-l/home/ykla/tmp`, `ls/`?
>
> ```sh
> # ls-l/home/ykla/tmp
> -sh: ls-l/home/ykla/tmp: not found
> # ls/
> -sh: ls/: not found
> ```
>
> As you can see, the shell parses and executes the entire string as a single executable command.

Also note that the command line itself does not have auto-correction functionality. Even if only one letter is misspelled or one character is omitted, the command will not execute correctly.

```sh
# LS # Test all-uppercase command name
-sh: LS: not found
# Ls # Test mixed case
-sh: Ls: not found
# ls /hom1 # Actual path should be /home
ls: /hom1: No such file or directory
# ls -z /home # Option -z does not exist
ls: invalid option -- z
usage: ls [-ABCFGHILPRSTUWZabcdfghiklmnopqrstuvwxy1,] [--color=when] [-D format] [--group-directories=] [file ...]
```

> **Tip**
>
> Windows is not only case-insensitive for filenames, but also case-insensitive for command names.
>
> ```powershell
> PS C:\Users\ykla> cd C:\ # Here cd is lowercase
> PS C:\> CD D:\ # Here CD is uppercase
> PS D:\> CD c:\ # Here the C drive is lowercase
> PS C:\> dir # Lowercase dir, lists directory, equivalent to ls
>
>    Directory: C:\
>
> ……some entries omitted……
>
> PS C:\Users\ykla> TREE # Uppercase tree, displays path relationships
> Folder PATH listing
> Volume serial number is 2A90-E989
> C:.
> ├─.android
> ├─.cache
> │ ├─selenium
> ……some entries omitted……
> ```

> **Tip**
>
> What does the `#` in front of a command mean? `#` in the shell typically serves as a comment (as specified by [POSIX.1-2024](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/V3_chap02.html)), equivalent to `//` in the C language. It means that the text following it serves only an explanatory purpose and has no actual effect.

## Command Execution and Interruption

Unlike Windows and graphical interface software, most command-line programs do not display progress indicators during execution. There are typically only two results:

- Successful execution:

```sh
# cp test /root/mydir/


```

- Execution interrupted:

```sh
# cp test9 /root/mydir/
cp: test9: No such file or directory
```

There are many possible scenarios for execution interruption; the above is just one example (the specified file or directory does not exist).

As you can see, the command line only provides a prompt when execution is interrupted; if execution completes successfully, no prompt is produced. This UNIX design philosophy aims to keep terminal output concise.

## Sources of Shell Commands

### Linux

In Linux, most commonly used commands come from GNU and other user-space packages; the Linux kernel itself does not provide user-level commands. The following is the verification process:

```bash
$ dpkg -S /bin/mv
coreutils: /bin/mv
$ dpkg -S /bin/cp
coreutils: /bin/cp
$ dpkg -S /bin/ls
coreutils: /bin/ls
$ dpkg -S /bin/pwd
coreutils: /bin/pwd
$ dpkg -S /bin/cat
coreutils: /bin/cat
$ dpkg -S /usr/sbin/chroot
coreutils: /usr/sbin/chroot
$ dpkg -S /bin/kill
procps: /bin/kill
$ dpkg -S /usr/bin/free
procps: /usr/bin/free
$ dpkg -S /bin/su
util-linux: /bin/su
```

It can be seen that in Linux, these common commands generally come from GNU software such as coreutils, util-linux, or procps. These packages are historically GNU Project re-implementations of UNIX software.

At the same time, the shell itself also has some built-in commands:

```bash
$ type cd
cd is a shell builtin
```

List all shell built-in commands:

```bash
$ compgen -b
.
:
[
alias
bg
bind
break
builtin
caller
cd
command
compgen
……some entries omitted……
ulimit
umask
unalias
unset
wait
```

### FreeBSD

```sh
$ type cd
cd is a shell builtin
```

In FreeBSD, in addition to the shell built-in commands mentioned above (see: sh(1)[EB/OL]. [2026-03-26]. <https://man.freebsd.org/cgi/man.cgi?query=sh&sektion=1>), commonly used commands are all included in the base system and do not belong to any package. For example, the `ls` command's source code is located at `freebsd-src/bin/ls/`[EB/OL]. [2026-03-26]. <https://github.com/freebsd/freebsd-src/tree/main/bin/ls>. This shows that the FreeBSD system is an organic whole, rather than a simple assembly of software packages maintained by different individuals or teams.

If PkgBase is configured, the output is similar to:

```sh
# pkg which /bin/ls # Query which package /bin/ls belongs to
/bin/ls was installed by package FreeBSD-runtime-15.snap20250313173555
```

If a command is missing, it can generally be obtained by installing the corresponding package. For example, the `lspci` command comes from the `sysutils/pciutils` package. However, many commands have Linux-specific issues and are incompatible with other operating systems. For example, the `ip` command comes from the iproute2 package.

## Common Commands

### The `cd` Command

`cd` (change working directory)

Switch to **/home**:

```sh
$ cd /home # Switch to /home
$ pwd # View current path
/home
```

### The `ls` Command

The basic usage of the `ls` (list) command has been introduced above. Below demonstrates how to make `ls` display file sizes in a human-readable format:

The option `-h`, which stands for `human`, needs to be used in combination with `-l` (`long` long output).

```sh
# ls -hl /home/ykla
total 326 KB
-rw-------  1 ykla ykla   50B Mar 18 17:23 .Xauthority
drwx------  6 ykla ykla    6B Mar 10 16:21 .cache
drwx------  9 ykla ykla   12B Mar 19 15:01 .config
-rw-r--r--  1 ykla ykla  1.0K Feb 24 12:18 .shrc
drwxr-xr-x  2 ykla ykla    2B Mar  9 23:48 .themes
-rw-r--r--  1 root ykla    0B Mar 19 15:13 abc.TXT
drwxr-xr-x  3 root ykla    7B Mar 19 15:17 vscode
-rw-------  1 ykla ykla   17M Mar 18 17:09 xrdp-chansrv.core
drwxr-xr-x  2 ykla ykla    2B Mar  9 20:45 Downloads
……some entries omitted……
```

In UNIX systems, files or directories starting with `.` (such as `.Xauthority` above) are hidden. The same applies to Android phones — you can verify this yourself using [MT File Manager](https://mt2.cn/).

The `-a` option can be used to display hidden directories and files:

```sh
$ ls -a
.		.cshrc		.login		.profile	Public		Videos
..		.dbus		.login_conf	.sh_history	Pictures	Music
.Xauthority	.face		.mail_aliases	.shrc		Documents
.cache		.icons		.mailrc		.themes		Desktop
.config		.local		.mozilla	Downloads	Templates
```

If the `-a` option is not used:

```sh
ykla@ykla:~ $ ls
Downloads	Public	Pictures	Documents	Desktop	Templates	Videos	Music
```

Hidden files will not be displayed.

### The `touch` Command for Creating Files

The literal meaning of `touch` is "to touch," indicating a minor change to a file's timestamp.

FreeBSD's `touch` is compatible with the POSIX standard and first appeared in Version 7 AT&T UNIX.

Create a file named `test`:

```sh
$ touch test
```

> **Tip**
>
> As you can see, the command above creates `test`, not `test.txt`, `test.word`, `test.pdf`, or similar. In fact, the `.txt` part is called a file extension, which primarily serves to indicate the file type to the user, not for system identification. Are many things we think we understand clearly really as we believe them to be?
>
> Even without the corresponding extension, the file type can still be identified in UNIX-like systems. This is determined based on the file's magic numbers:
>
> ```sh
> $ file book
> book: PDF document, version 1.7
> ```

The `file` command determines the file type through three sets of tests in sequence: filesystem tests (based on stat(2)), magic number tests (based on fixed-format identifiers in **/usr/share/misc/magic.mgc**), and language tests (based on text pattern matching). The concept of "magic numbers" originates from the UNIX executable file format, where fixed identifiers stored at specific offsets in the file header indicate the file type.

You can create multiple files at once using multiple arguments (this usage is nearly universal and will not be repeated):

```sh
$ touch test test1 test2 test3
```

### The `mkdir` Command for Creating Directories

`mkdir` stands for `make directories`, creating directories

Create a directory named ykla.

```sh
$ mkdir -v ykla # The -v option is used to display file change details, short for verbose, meaning output detailed information
ykla
```

If the file already exists:

```sh
$ mkdir ykla
mkdir: ykla: File exists # Indicates that the directory already exists.
```

---

If you want to create the directory `ykla/ykla1/ykla2/ykla3`, how should you proceed?

```sh
$ mkdir ykla/ykla1/ykla2/ykla3
mkdir: ykla/ykla1/ykla2: No such file or directory
```

The system returns the above error. In this case, the `-p` parameter is needed. The `p` stands for `parents`, meaning that if the parent directory does not exist, it will be created as well.

```sh
$ mkdir -vp  ykla/ykla1/ykla2/ykla3
ykla/ykla1
ykla/ykla1/ykla2
ykla/ykla1/ykla2/ykla3
```

FreeBSD's `mkdir` is largely compatible with GNU/Linux's `mkdir`.

### The `rm` Delete Command

> **Warning**
>
> The FreeBSD command line interface does not have a recycle bin; all commands, once executed, cannot be undone. Command-line operations with `rm` carry a high risk.

`rm` is an abbreviation of the English word `remove`, meaning to delete.

---

Delete the file `test`

```sh
$ rm test
```

If the file `test` does not exist:

```sh
$ rm test
rm: test: No such file or directory # Error indicating the specified file or directory does not exist
```

---

Delete the path **/home/ykla/test**

- If the directory is empty (contains no files, just an empty directory)

```sh
$ rm /home/ykla/test
$
```

You can also use the `rmdir` command (remove directory, which can only delete empty directories):

```sh
$ rmdir /home/ykla/test
$
```

- If the directory is not empty

```sh
$ rm /home/ykla/test/
rm: /home/ykla/test/: is a directory # Indicates that /home/ykla/test/ is a directory
```

Use the parameter `-r` (recursively) for recursion, and the parameter `-f` (force) for forced deletion:

> **Tip**
>
> What is recursion?
>
> "Once upon a time, there was a mountain, and on the mountain there was a temple, and in the temple there was an old monk telling a story to a young monk. The old monk said: 'Once upon a time, there was a mountain, and on the mountain there was a temple...'" This is an example of recursion.
>
> In this operation, it means first entering the deepest subdirectory under **/home/ykla/test/** (if one exists), deleting the files and subdirectories within it, and then repeating the process upward layer by layer until **/home/ykla/test/** is deleted. This uses the Depth-First Search (DFS) algorithm.

```sh
$ rm -rf /home/ykla/test/
```

> **Warning**
>
> Using `rm -rf` is a rather dangerous operation and cannot be undone. If a space is mistakenly entered in the command, such as mistyping **/home/ykla/test/** as **/home/ykla /test/**, it will result in deleting the wrong path:
>
> ```sh
> # rm -rf /home/ykla /test
> # ls /home/ykla
> ls: /home/ykla: No such file or directory # Indicates that the ykla directory no longer exists
> ```

> **Warning**
>
> There are often claims on the internet that using `sudo rm -rf /*` is a command that can perform a specific operation, misleading others into causing irreversible catastrophic damage to their systems. This command essentially uses root privileges (~~fortunately FreeBSD does not have sudo by default~~) to delete everything under **/** and its subdirectories. Now let's demonstrate the result:
>
> ```sh
> # rm -rf /*
> rm: /boot/efi: Device busy
> rm: /boot: Directory not empty
> rm: /dev/reroot: Operation not supported
> rm: /dev/input: Operation not supported
> rm: /dev/fd: Operation not supported
> ……some entries omitted……
> #
> ```
>
> ![Boot error](../.gitbook/assets/no-efi-partition.png)
>
> After restarting, you will find that the boot entry is missing.
>
>> **Thought Question**
>>
>> Do you now have a deeper understanding of the statement "root is the highest privilege in UNIX"? Does this illustrate the consistency of power and responsibility? If power is abused, it not only harms others but ultimately leads to the loss of one's own basis for existence.

### The `mv` Move/Rename Command

`mv` is an abbreviation of the English word `move`, meaning to move.

---

Move the file `test` to **/home/ykla**:

```sh
$ mv -v test /home/ykla # The -v option is used to display file change details, short for verbose, meaning output detailed information
test -> /home/ykla/test
```

Move a directory and its subdirectories to **/home/ykla**

---

- Rename

Rename `test5.pdf` to `test5.txt`

```sh
$ mv -v  test5.pdf test5.txt
test5.pdf -> test5.txt
```

Rename `test2` to `test2.pdf`

```sh
$ mv -v test2 test2.pdf
test2 -> test2.pdf
```

### The `cp` Copy Command

`cp` is an abbreviation of the English word `copy`, meaning to copy.

---

Copy the file `test` to **/home/ykla**

```sh
$ cp test /home/ykla/
```

If the trailing **/** is missing and the subdirectory ykla does not exist, `test` will be renamed to `ykla` (ykla was supposed to be a directory), so the trailing **/** cannot be omitted:

```sh
$ cp test /home/ykla/
cp: directory /home/ykla does not exist # If / is added, it will indicate the directory does not exist
```

If the trailing **/** is missing:

```sh
$ cp -v test /home/ykla # The -v option is used to display file change details, short for verbose, meaning output detailed information
test -> /home/ykla
```

> **Thought Question**
>
> Do other commands have similar issues? Please try them out.

---

Copy a file while changing its filename and extension:

```sh
$ cp -v test /home/ykla/abc.TXT
test -> /home/ykla/abc.TXT
```

This command is commonly used for backing up configuration files.

---

Copy a directory and its subdirectories:

```sh
$ cp -v /usr/ports/editors/vscode /home/ykla
cp: /usr/ports/editors/vscode is a directory (not copied).
```

Direct copying does not work; the system indicates that the target is a directory, not a file.

Therefore, the `-r` option is also needed. The `r` stands for `recursively`:

```sh
$ cp -vr /usr/ports/editors/vscode /home/ykla
/usr/ports/editors/vscode -> /home/ykla/vscode
/usr/ports/editors/vscode/distinfo -> /home/ykla/vscode/distinfo
……some entries omitted……
```

### The Wildcard `*`

Sometimes operations require selecting all items, and the wildcard `*` can be used.

- Delete all files whose filenames start with `test`:

```sh
$ rm test*
rm: test: is a directory
rm: test4: is a directory
```

As you can see: directories are not processed.

- Delete all files and **directories** whose filenames start with `test`:

```sh
$ ls test*  # Confirm the matched files
$ rm -rf test*
```

- Delete all files and **directories**:

```sh
$ ls *  # Confirm the matched files
$ rm -rf *
```

### The Logical Operator `&&`

`&&` (logical AND): the subsequent command will only be executed if the command before `&&` executes successfully; if the command before `&&` fails, the subsequent command will not be executed.

Its semantics are: only when the preceding command executes successfully (returns exit status 0) will the following command be executed; otherwise, the subsequent command is skipped.

Usage scenario: executing a series of dependent commands. For example, you must refresh the software sources before you can update the system, and only then can you reboot. Taking Ubuntu as an example: `sudo apt update -y && sudo apt upgrade -y && sudo reboot`. Only when the preceding command executes successfully will the following command be executed.

### The Logical Operator `||`

`||` (logical OR): the subsequent command will only be executed if the command before `||` fails; if the command before `||` executes successfully, the subsequent command will not be executed.

Its semantics are: only when the preceding command fails (returns a non-zero exit status) will the following command be executed; otherwise, the subsequent command is skipped.

Usage scenario: if a command keeps failing but needs to be executed repeatedly, you can use multiple `||` operators in succession to prevent having to manually re-execute the command after each failure, for example:

```sh
make BATCH=yes install || make BATCH=yes install || make BATCH=yes install || make BATCH=yes install
```

When one `make BATCH=yes install` fails, the next `make BATCH=yes install` will still be executed. That is, the previous command failed, so the following command is executed instead.

> **Tip**
>
> `&&` and `||` have the same precedence and are evaluated from left to right.

> **Thought Question**
>
> What does `touch a.txt && touch b.txt || touch c.txt || reboot` mean?
>
> If `touch a.txt` fails, which subsequent operation will be executed?

### dc (Desk Calculator)

Since version 13, FreeBSD has adopted the bc/dc implementation developed by Gavin D. Howard, which integrates `bc` and `dc` into the same binary file. `dc` is accessed via a symbolic link and no longer uses the traditional preprocessor architecture.

#### Postfix Notation (Reverse Polish Notation, RPN)

Understanding postfix notation is fundamental to using the dc calculator.

Since first encountering mathematics, the commonly used method of expressing calculations has been infix notation. For example, to calculate the sum of 3 and 4, you write `3 + 4`, with the operator placed between the operands. This is the mathematical expression format most familiar to humans.

In postfix notation, the operator is placed after the operands. For example, the above expression in postfix notation would be written as `3 4 +`. If there are multiple operators, the operator is placed after the second operand. The conventional infix expression `3 - 4 + 5` in postfix notation becomes `3 4 - 5 +`.

Postfix notation is easier for computers to process and can take advantage of stack structures to reduce memory access, making it widely adopted in compiler design and calculator implementations.

For example, the infix expression `5 + ((1 + 2) * 4) - 3` in postfix notation becomes `5 1 2 + 4 * + 3 -`.

The above example can be calculated using `dc`. Enter the following command in the terminal:

```sh
$ dc -e '5 1 2 + 4 * + 3 - p'
```

Use dc to calculate the expression `5 + ((1 + 2) * 4) - 3` and output the result.

The calculation result is `14`.

## Redirecting Input and Output

The UNIX shell also enables users to execute commands, redirect their output, redirect their input, and combine multiple commands to optimize the final output.

Shell redirection is the operation of sending a command's output or input to another command or file. For example, to capture the output of the ls(1) command into a file, you can redirect the output like this:

```sh
$ ls -l > test.txt
```

View the contents of the `test.txt` file:

```sh
total 1
drw-------  2 ykla ykla 2 Apr 28 00:24 test
-rw-r--r--  1 ykla ykla 0 Apr 28 00:43 test.txt

```

At this point, the original contents of `test.txt` will first be cleared (if the file exists), and then the `ls` command will be executed, saving the listed directory contents directly into the `test.txt` file.

> **Tip**
>
> Some commands can read input, such as sort(1). To sort this list, you can redirect the input like this:

```sh
$ sort < test.txt
-rw-r--r--  1 ykla ykla 0 Apr 28 00:43 test.txt
drw-------  2 ykla ykla 2 Apr 28 00:24 test
total 1
```

The sorted input will be displayed on the screen. To redirect this input to another file, you can redirect the output of sort(1), as follows:

```sh
$ sort < test.txt > sorted.txt
$ cat sorted.txt
-rw-r--r--  1 ykla ykla 0 Apr 28 00:43 test.txt
drw-------  2 ykla ykla 2 Apr 28 00:24 test
total 1
```

In all the above examples, commands are redirected through file descriptors. Every UNIX® system has file descriptors, including standard input (stdin), standard output (stdout), and standard error (stderr). Each type of descriptor has its purpose; for example, input can be a keyboard or mouse providing input; output can be a screen or paper in a printer; standard error is used for outputting diagnostic or error messages. All three are considered I/O-based file descriptors, sometimes also called streams.

By using these descriptors, the shell allows output and input to be passed between multiple commands, and redirected to or from files. Another method of redirection is the pipe operator.

The UNIX® pipe operator `|` allows the output of one command to be directly passed or redirected to another program. Simply put, a pipe allows the standard output of one command to be passed as standard input to another command, for example:

```sh
$ cat test.txt | sort | less
-rw-r--r--  1 ykla ykla 0 Apr 28 00:43 test.txt
drw-------  2 ykla ykla 2 Apr 28 00:24 test
total 1
```

In this example, the contents of `test.txt` will be sorted, and then the output is passed to less(1). This allows the user to browse the output at their own pace, preventing it from scrolling off the screen.

## Shutdown and Reboot

Shutdown:

The correct command to shut down and power off is `poweroff`, which is equivalent to the command `shutdown -p now`.

Reboot:

It is recommended to use `shutdown -r now` to reboot.

> **Note**
>
> In FreeBSD, `shutdown` can be executed by the root user and members of the operator group, but `reboot` and `halt` are restricted to the root user only. When executing shutdown, a warning message is broadcast to all logged-in users, and **/var/run/nologin** is created 5 minutes before the specified time to prevent new logins (if the shutdown time is less than 5 minutes away, it is created immediately).

> **Tip**
>
> For behavioral differences between `reboot`, `halt`, and `poweroff` in FreeBSD versus Linux, and the execution flow of the **/etc/rc.shutdown** script during shutdown, see other related chapters.

## References

- FreeBSD Project. make -- maintain program dependencies[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=make&sektion=1>. BSD make manual page, describing build tool syntax and usage.
- FreeBSD Project. grep -- file pattern searcher[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=grep&sektion=1>. Text search tool manual page.
- FreeBSD Project. sed -- stream editor[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=sed&sektion=1>. Stream editor manual page.

## Exercises

1. Compare the differences in commonly used options between BSD-style sed/awk/grep and GNU versions, create a compatibility reference table, and provide strategies for writing cross-platform scripts on FreeBSD.
2. Review the source code implementation of the `ls` command in FreeBSD (`bin/ls/`), and compare it with GNU coreutils' `ls` in terms of option parsing and output format implementation.
3. Write a shell script that uses pipes to combine multiple FreeBSD base commands (such as `ls`, `grep`, `sort`, `awk`) to implement a practical system information summarization function.
