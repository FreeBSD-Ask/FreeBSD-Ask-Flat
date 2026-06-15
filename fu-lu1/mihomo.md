# Mihomo

Mihomo is a derivative of the Clash series and has been included in the FreeBSD Ports tree (net/mihomo). This section provides installation and basic configuration steps.

## Installing Mihomo

Install using pkg:

```sh
# pkg install mihomo
```

Or install using Ports:

```sh
# cd /usr/ports/net/mihomo/
# make install clean
```

## Mihomo File Structure

The Mihomo file structure is as follows:

```sh
/usr/
├── ports/
│   └── net/
│       └── mihomo/ # Mihomo Ports directory
├── local/
│   ├── bin/
│   │   └── mihomo # Mihomo executable
│   └── etc/
│       └── rc.d/
│           └── mihomo # Mihomo RC service script
└── etc/
    └── rc.conf # System service configuration file
```

Mihomo can also run the Linux version through FreeBSD's Linux binary compatibility layer, but it is recommended to prioritize the native FreeBSD version (net/mihomo); the compatibility layer should only be used as a fallback.

## RC Script

A merge request has been submitted to the Ports maintainer ([Bug 291295 - net/mihomo: Add rc.conf and some Post-installation](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=291295)) to add system service management support for Mihomo. As of writing, no response has been received. Until official integration is complete, you can use the custom RC script provided below for service management.

### Custom RC Script

To facilitate Mihomo service management, you can use the following script. Save the script below as mihomo and place it in **/usr/local/etc/rc.d/**, then grant executable permissions using the root account: `chmod +x /usr/local/etc/rc.d/mihomo`.

```sh
#!/bin/sh

. /etc/rc.subr   # Include the rc.d script framework, the standard prerequisite for FreeBSD service scripts

name="mihomo"    # Define the service name, used to identify and manage this service
desc="mihomo server"    # Service description
rcvar="mihomo_enable"    # Service toggle variable, controls whether the service starts automatically at boot

: ${mihomo_datadir:="/var/run/mihomo"}
: ${mihomo_user:="root"}    # Default user; if using another user, ensure the mihomo_datadir directory and $pidfile and log files are writable
: ${mihomo_extra_flags:=""}	# Extra parameters for mihomo, used to pass custom startup options

procname="/usr/local/bin/mihomo"    # Used with pidfile to detect the service process
pidfile="${mihomo_datadir}/mihomo.pid"    # Stores the Process ID (PID) of the main process, used with procname to detect the service process
logfile="${mihomo_datadir}/mihomo.log"
start_cmd="mihomo_start"    # Set the start command to call the mihomo_start function; stop and other commands are implemented by default by the rc.d framework
extra_commands="init reconfig regeoip"    # Register additional custom commands, extending the rc.d framework's standard command set
reconfig_cmd="mihomo_reconfig"    # Specify the reconfig command to call the mihomo_reconfig function, used to download the config.yaml file
regeoip_cmd="mihomo_regeoip"    # Specify the regeoip command to call the mihomo_regeoip function, used to download the geoip.dat file; can specify using this file via mihomo_extra_flags="-m"
init_cmd="mihomo_init"    # Specify the init command to call the mihomo_init function. Creates the data file directory, sets ownership, to avoid read/write permission issues when running as a non-root user

mihomo_start()
{    # Use daemon to start mihomo; the -p parameter makes daemon automatically manage the pidfile, cleaning it up when the process exits
	daemon -u ${mihomo_user} -p "$pidfile" -o "${logfile}" $procname -d "${mihomo_datadir}" -f "${mihomo_datadir}/config.yaml" ${mihomo_extra_flags}
}
mihomo_reconfig()
{
	startmsg "begin to refresh config.yaml"
	startmsg "config.yaml : ${mihomo_config}"
	if ( fetch -o ${mihomo_datadir}/config.yaml.new "${mihomo_config}" );then
		mv ${mihomo_datadir}/config.yaml.new ${mihomo_datadir}/config.yaml    # On successful download, overwrite the original config; on failure, keep the original config
        startmsg "rename config.yaml.new to config.yaml"
	else
		err "fetch config.yaml failed! check ${mihomo_config}!"
	fi
}
mihomo_regeoip()
{
	startmsg "begin to refresh geoip.dat"
	startmsg "geoip.dat : ${mihomo_geoip}"
	if ( fetch -o ${mihomo_datadir}/geoip.new "${mihomo_geoip}" );then
		mv ${mihomo_datadir}/geoip.new ${mihomo_datadir}/geoip.dat
        startmsg "rename geoip.new to geoip.dat"
	else
		err "fetch geoip.dat failed! check ${mihomo_geoip}"
	fi
}
mihomo_init()
{
	startmsg "begin init"
	install -d -m 0700 -o ${mihomo_user} ${mihomo_datadir}
    startmsg "all data is in ${mihomo_datadir}"
    startmsg "remember reconfig/regeoip before start"
}

load_rc_config $name
run_rc_command "$1"
```

### Available Parameters and Options

The RC script provides multiple command-line parameters and configuration options. The commonly used parameters are listed below. These commands write the configuration directly to the **/etc/rc.conf** file; if the configuration is incorrect, you can directly modify the corresponding line.

- Enable the Mihomo service and set it to start automatically at boot:

```sh
service mihomo enable
```

- Start the Mihomo service process:

```sh
service mihomo start
```

- Stop the Mihomo service process:

```sh
service mihomo stop
```

- Check the Mihomo service running status:

```sh
service mihomo status
```

- Specify the subscription link address (the example address is for demonstration purposes only and must be replaced with a valid link):

```sh
sysrc mihomo_config="https://xxxx.yyy"
```

- GeoIP data implements traffic routing or rule matching based on the geographic attribution of IP addresses:

```sh
sysrc mihomo_geoip="https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.dat" # Optional, but recommended
```

- Specify extra parameters for Mihomo:

```sh
sysrc mihomo_extra_flags="-m" # Optional, but recommended
```

- `-m`: Enables geodata mode, making Mihomo use geoip.dat and geosite.dat files (DAT format) for rule matching instead of the default MMDB format (Country.mmdb).

- Specify the user identity for running the Mihomo service:

```sh
sysrc mihomo_user="mihomo"  # Default user is root
```

- Specify the Mihomo data directory:

```sh
sysrc mihomo_datadir="/var/run/mihomo"
```

Related file structure:

```sh
/var/
└── run/
    └── mihomo/ # Mihomo data directory
        ├── mihomo.pid # Process ID (PID) file
        ├── mihomo.log # Log file
        ├── config.yaml # Configuration file
        └── geoip.dat # GeoIP data file
```

- Initialize the Mihomo data directory and prepare the service runtime environment:

```sh
service mihomo init
```

- Update the subscription configuration. Run this before `start` to ensure the configuration file is the latest version:

```sh
service mihomo reconfig
```

- Update the GeoIP geographic location database:

```sh
service mihomo regeoip # Recommended when first enabling; IP geographic location data changes slowly, no need to update frequently
```

### Minimal RC Example

The following is a minimal configuration example that can be modified as needed after understanding its meaning and written to the **/etc/rc.conf** file:

```ini
mihomo_config="https://xxx.yyy" # Actual subscription link address, must be replaced with a valid link
mihomo_geoip="https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.dat" # GeoIP data
mihomo_enable="YES" # Enable at boot / service item
```

### Unfinished Items

The following are issues to be investigated:

- How to implement traffic routing for "direct," "proxy," and "global" modes?

- How to implement TUN virtual network interface proxy? TUN mode can achieve lower-level network traffic interception.

- How to test node speeds using a subscription link?

- How to specify a particular node from a proxy group in the subscription link (e.g., using only a proxy node located in the US)? This involves fine-grained proxy node selection.

## Clash for FreeBSD

### Environment Preparation

FreeBSD's default login shell is sh, not bash. Before executing the following commands, you must first switch to `bash`:

```shell
$ bash
```

It is recommended to use a `root` privileged account.

```shell
# pkg update
# pkg install -y bash curl gtar gzip
```

Notes:

- The current script is executed via `bash`.
- The `freebsd-rc` backend depends on `service` and **/usr/local/etc/rc.d**.

### Installation and Initialization

```shell
$ git clone --branch master --depth 1 https://github.com/wenyinos/clash-freebsd.git
$ cd clash-freebsd
$ export KERNEL_TYPE=mihomo
# bash install.sh
```

First-time configuration:

```shell
$ clashctl add <subscription_link> <name>
$ clashctl use
$ clashctl select
$ clashon
$ clashctl status
```

### FreeBSD Service Management (rc.d)

The system installation uses the `freebsd-rc` backend by default, with the service name: `clash_freebsd`.

```shell
# service clash_freebsd status
# service clash_freebsd start
# service clash_freebsd stop
# service clash_freebsd restart
```

Manage kernel service auto-start at boot:

```shell
# clashctl autostart on
# clashctl autostart status
# clashctl autostart off
```

Notes:

- `clashctl autostart on` only controls whether the rc.d service starts with the system.
- `service` and `autostart` commands require root privileges (use root or `sudo`).
- FreeBSD auto-start configuration file: **/etc/rc.conf.d/clash_freebsd**.

### Tun and Route Diagnostics (FreeBSD)

Tun devices are typically **/dev/tun\***.

```shell
# clashctl tun on
# clashctl tun off
# clashctl tun doctor
$ route -n get default
$ netstat -rn -f inet
```

When Tun is not working, check first:

- `tun on/off` requires root privileges
- `ls -l /dev/tun*`
- Current user permissions (root recommended)
- Whether the default route has been taken over by the tun interface

### Common Troubleshooting Commands

```shell
$ clashctl doctor
$ clashctl logs
$ clashctl logs mihomo
$ clashctl config regen
```

### Uninstallation

```shell
# bash uninstall.sh
```

For a thorough cleanup of runtime data:

```shell
# bash uninstall.sh --purge-runtime
```

### References

- wenyinos. A more complete and elegant FreeBSD Clash / Mihomo runtime platform[EB/OL]. [2026-04-26]. <https://github.com/wenyinos/clash-freebsd>. Provides a complete deployment solution for Clash proxy on FreeBSD, supporting subscription link management.

### Unfinished Items

Decouple from Bash, support the default sh, and better adapt to the FreeBSD default environment.
