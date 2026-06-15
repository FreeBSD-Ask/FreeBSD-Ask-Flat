# 9.5 System Fonts

FreeBSD's default fonts do not render Chinese text well. This section describes how to introduce Windows TrueType fonts into the graphical interface.

> **Warning**
>
> The Windows font End User License Agreement (EULA) only authorizes the use of these fonts within genuine Windows systems. Copying them to install on other operating systems violates the license agreement. Additionally, the copyright of some fonts (such as Microsoft YaHei) is retained by third-party font vendors (such as Founder Corporation), and unauthorized commercial use may constitute infringement. It is recommended to prioritize open-source, commercially-usable alternative fonts, such as Source Han Sans and LXGW WenKai.

## GUI Graphical Interface Fonts

First, extract all `.ttf` and `.ttc` font files from the Windows **C:\Windows\Fonts** directory. Although macOS font files also use the `.ttf` format, they still require special handling.

To manage new fonts, create a directory to store Windows fonts:

```sh
# mkdir -p /usr/local/share/fonts/WindowsFonts
```

Copy the font files to the **WindowsFonts** directory.

Font directory structure:

```sh
/usr/local/share/
└── fonts/
    └── WindowsFonts/ # Windows font storage directory
```

Set the permissions of the Windows font directory and its contents to 755:

```sh
# chmod -R 755 /usr/local/share/fonts/WindowsFonts
```

You also need to refresh the font cache:

```sh
# fc-cache
```

## Appendix: Installing Windows 11 Fonts (Custom Package)

This package can also run under the FreeBSD compatibility layer of Debian and older versions of Ubuntu. Installation method:

```sh
# apt install git                          # Install Git
# git clone https://github.com/ykla/ttf-mswin11-zh-deb   # Clone the font package repository
# cd ttf-mswin11-zh-deb                    # Enter the font package directory
# dpkg -i ttf-ms-win11-*.deb               # Install the Windows 11 Chinese font package
```

## Exercises

1. Extract font files from a Windows system and configure them in FreeBSD, testing the font display in multiple GTK and Qt applications.
2. Download font files in bdf or hex format, use the vtfontcvt tool to convert them to fnt format, and test the display in the console.
3. Try using third-party tools (such as vt-fnt) to generate fnt files for Chinese fonts, and verify their display in the FreeBSD console.
