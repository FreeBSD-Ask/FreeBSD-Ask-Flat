# 6.3 Switching Shells

The easiest way to permanently change the default shell is to use chsh. Running this command opens the editor configured by the EDITOR environment variable, which defaults to vi(1).

## Zsh

Zsh is the default shell for Kali Linux and macOS. Zsh is not only a shell designed for interactive use, but also a powerful scripting language. zsh integrates many practical features from most mainstream shells such as bash, ksh, and tcsh, while also adding numerous original features.

### Installing Zsh

- Install using pkg:

```sh
# pkg install zsh zsh-completions zsh-autosuggestions zsh-syntax-highlighting
```

Package descriptions:

| Package | Description |
| ------- | ----------- |
| `zsh` | Zsh shell |
| `zsh-completions` | Automatic completion |
| `zsh-autosuggestions` | Fish shell-like Zsh auto-suggestions |
| `zsh-syntax-highlighting` | Fish shell-like Zsh syntax highlighting |

- Install using Ports:

```sh
# cd /usr/ports/shells/zsh/ && make install clean
# cd /usr/ports/shells/zsh-completions && make install clean
# cd /usr/ports/shells/zsh-autosuggestions/ && make install clean
# cd /usr/ports/shells/zsh-syntax-highlighting/ && make install clean
```

- View Zsh installation information

```sh
# pkg info -D zsh
```

### Configuring Zsh

Change the current user's default login shell to Zsh:

```sh
# chsh -s /usr/local/bin/zsh # Switch shell to zsh
chsh: user information updated
```

Enter your password at the prompt and press **Enter** to change the shell. After logging out and logging back in, you can start using the new shell.

> **Note**
>
> `chsh`, `chfn`, and `chpass` are the same program, invoked through different names. Non-superusers can only change their shell to a standard shell listed in **/etc/shells**; changing from or to a non-standard shell will be rejected. The editor is determined by the `EDITOR` environment variable, defaulting to vi(1). After information verification, chpass automatically calls pwd_mkdb(8) to update the user database.

Edit the **~/.zshrc** file and add the following lines:

```sh
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh   # Load Zsh auto-suggestions plugin
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh   # Load Zsh syntax highlighting plugin
fpath+=/usr/local/share/zsh/site-functions/   # Add custom function directory to Zsh function search path
```

Directory structure:

```sh
~/
└── .zshrc # Zsh configuration file
/usr/local/
└── share/
    ├── zsh-autosuggestions/
    │   └── zsh-autosuggestions.zsh # Zsh auto-suggestions plugin
    ├── zsh-syntax-highlighting/
    │   └── zsh-syntax-highlighting.zsh # Zsh syntax highlighting plugin
    └── zsh/
        └── site-functions/ # Zsh custom functions directory
```

Use immediately:

```sh
# zsh                        # Switch the current shell to Zsh
# source ~/.zshrc            # Reload Zsh configuration file, refresh environment variables
# rm -f ~/.zcompdump         # Delete Zsh completion cache file
# autoload -Uz compinit       # Load compinit function
# compinit                   # Initialize completion system and force cache rebuild
```

### Theming and Beautification

In addition to basic configuration, you can also beautify the shell interface through themes. Powerlevel10k is a widely used Zsh theme. The installation and configuration method is as follows:

```sh
$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k   # Clone the Powerlevel10k theme repository to the home directory
$ echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc                    # Append the Powerlevel10k theme loading command to ~/.zshrc
```

Directory structure:

```sh
~/
├── .zshrc # Zsh configuration file
└── powerlevel10k/ # Powerlevel10k theme directory
    └── powerlevel10k.zsh-theme # Powerlevel10k theme file
```

Reload the Zsh configuration file to activate the Powerlevel10k theme:

```sh
# source ~/.zshrc
```

Follow the prompts to answer several questions to complete the configuration. It will take effect after restarting.

### References

- romkatv. Powerlevel10k[EB/OL]. [2026-03-26]. <https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#installation>. Theme project official website.
- FreeBSD Project. csh -- a shell with C-like syntax[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=csh&sektion=1>. C-style syntax shell manual page.

## Bash

Bash (Bourne Again shell) is a shell program developed by the GNU Project as an enhanced replacement for the Bourne shell (sh). Bash is compatible with sh syntax and integrates useful features from csh and ksh, including command-line editing, command history, programmable completion, and job control. Bash is the default shell for most Linux distributions, but it is not a base system component in FreeBSD.

### Installing Bash

- Install using pkg:

```sh
# pkg install bash bash-completion-freebsd bash-completion-zfs
```

- Install using Ports:

```sh
# cd /usr/ports/shells/bash/ && make install clean
# cd /usr/ports/shells/bash-completion-freebsd/ && make install clean
# cd /usr/ports/shells/bash-completion-zfs/ && make install clean
```

Package descriptions:

| Package | Description |
| ------- | ----------- |
| `bash` | Bash shell main program |
| `bash-completion-freebsd` | Bash completion library extension for FreeBSD, installed automatically as a dependency |
| `bash-completion-zfs` | Bash completion library extension for OpenZFS |

- View post-installation configuration

```sh
# pkg info -D bash-completion # View configuration notes for bash-completion installed as a dependency
```

### Configuring Bash

After installing Bash and the related completion libraries, configuration is required for proper operation.

```sh
chsh -s /usr/local/bin/bash   # Switch the current user's default login shell to Bash
touch ~/.bash_profile         # Create ~/.bash_profile file for configuring Bash environment variables
```

Related file structure:

```sh
~/
└── .bash_profile # Bash configuration file
/usr/local/
└── share/
    └── bash-completion/
        ├── bash_completion.sh # Bash completion script
        └── README.md # Bash completion documentation
```

To load Bash completion functionality, edit the **~/.bash_profile** file and add the following line:

```bash
[[ $PS1 && -f /usr/local/share/bash-completion/bash_completion.sh ]] && source /usr/local/share/bash-completion/bash_completion.sh
```

Use immediately:

```bash
# bash                     # Switch the current shell to Bash
# source ~/.bash_profile    # Reload Bash configuration file, refresh environment variables
```
