# V2Ray

V2Ray is a proxy software that supports multiple proxy protocols (VMess, Shadowsocks, etc.) and traffic routing capabilities. The routing module can distribute traffic to different outbound proxies based on conditions such as destination address and port, enabling flexible traffic routing. On FreeBSD systems, V2Ray can be installed via pkg(8) or Ports.

Xray-core is a fork of V2Ray that optimizes performance and extends functionality while maintaining core feature compatibility. The configurations of the two are largely compatible; Xray can follow V2Ray's configuration methods. Compared to Xray, V2Ray has slower support for some new protocols. Note: The VLESS protocol has been deprecated in V2Ray (the official documentation states "VLESS is deprecated and may be removed"), and its active development (including XTLS, REALITY, and other enhancements) takes place in Xray-core.

## Installing V2Ray

### Installing V2Ray

- Install using pkg:

```sh
# pkg install v2ray
```

- Or install using Ports:

```sh
# cd /usr/ports/net/v2ray/
# make install clean
```

### Installing Xray-core

Install Xray-core using pkg:

```sh
# pkg install xray-core
```

Or install Xray-core using Ports:

```sh
# cd /usr/ports/security/xray-core/
# make install clean
```

## Starting the Proxy Software

After installation, you need to start the proxy software. If you have already configured proxy nodes using a client such as V2RayN on another platform (e.g., Windows), you can export the configuration as a config.json file and copy it to the FreeBSD system.

Start V2Ray with the specified configuration file:

```sh
# v2ray run -c /path/to/config.json
```

Start Xray with the specified configuration file:

```sh
# xray run -c /path/to/config.json
```

At this point, the proxy software should have started successfully; you can verify this through logs or process status.

## Configuring Proxy Parameters

Xray configuration files are largely compatible with V2Ray, but some advanced features have different configuration methods.

After the proxy software starts, you need to configure the proxy parameters for related software. V2Ray/Xray uses an inbound (inbounds) and outbound (outbounds) architecture: inbounds define how the proxy software receives traffic, and outbounds define how the proxy software forwards traffic.

Edit the config.json file and find the corresponding inbounds attribute. inbounds is an array where each element represents an inbound interface configuration, including the listen address, port number, and proxy protocol type. In the software that needs to use the proxy, set the proxy server address and port number to the corresponding values here.

### Configuring a Browser to Use a Proxy (Using Firefox as an Example)

#### Finding the Port

Open the config.json configuration file and confirm the port number under the inbounds field. Typically, in configurations exported from Windows, the SOCKS5 port is 10808 and the HTTP port is 10809.

For example, one inbound interface has `protocol` as `http`, `listen` as **127.0.0.1**, and `port` as 10809. This means the inbound interface listens for HTTP proxy requests on port 10809 of the local loopback address.

#### Firefox Settings

- Open Settings → Network Settings → Proxy Server.
- Select "Manual proxy configuration."
- For the HTTP proxy: set the address to **127.0.0.1** and the port to 10809.
- For the SOCKS proxy: set the SOCKS host to **127.0.0.1** and the port to 10808.

> **Important**
>
> Check "Proxy DNS when using SOCKS v5" at the bottom to forward domain name resolution requests through the proxy server as well, bypassing DNS pollution.

### Configuring Terminal Command-Line Programs

Most terminal commands read the environment variables `HTTP_PROXY`, `HTTPS_PROXY`, and `ALL_PROXY`, and automatically use the corresponding proxy based on their values.

The following commands apply to sh, Bash, and Zsh (temporary settings, effective only for the current session):

```sh
$ export HTTP_PROXY="http://127.0.0.1:10809" # Set HTTP proxy
$ export HTTPS_PROXY="http://127.0.0.1:10809" # Set HTTPS proxy
$ export ALL_PROXY="socks5://127.0.0.1:10808" # Set SOCKS5 proxy
```

For persistent configuration, use the login class method.

After completing the setup, visit a web page in the Firefox browser and observe the V2Ray logs to confirm that browser traffic is being forwarded through the proxy. Command-line programs in the terminal also access the network through the proxy, but some commands support environment variables differently; you need to check their proxy configuration methods based on the specific software.

## Proxy Traffic Routing

Some websites do not need to be accessed through a proxy server, such as websites within China or local network resources, and require routing treatment.

Open the config.json file and find the routing attribute. The rules sub-attribute within it is used to configure traffic routing rules; each rule typically contains matching conditions such as ip or domain. When an IP or domain matches a rule, V2Ray forwards the traffic to the corresponding outbound configuration based on the outboundTag (e.g., proxy for proxy, direct for direct connection, block for blocking). Configure the domains or IPs that need routing into the corresponding rules; for details, refer to the [V2Ray official documentation](https://www.v2fly.org/). Configuration files exported from V2Ray clients typically include default routing rules.

V2Ray also includes two resource files: geosite.dat and geoip.dat. geosite.dat stores domain name information by category, and geoip.dat stores IP address information by category. The resource file path can be specified by setting the environment variable `V2RAY_LOCATION_ASSET`; V2Ray will automatically look for geosite.dat and geoip.dat files under that path. For Xray, use the `XRAY_LOCATION_ASSET` environment variable to specify the resource file path. Note: If using Xray, make sure to correctly set the `XRAY_LOCATION_ASSET` environment variable; otherwise, resource files may fail to load.

For example, in a direct connection rule, you can configure geosite's cn domains to use direct connection:

```json
      {
        "domain": [
          "geosite:cn"
        ],
        "outboundTag": "direct",
        "type": "field"
      },
```

The cn domain direct connection rules provided by the V2Ray community can be extended based on actual needs. You can also find community-maintained geosite and geoip files on GitHub (such as [Loyalsoldier/v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat)), which typically have detailed explanations for "whitelist mode" and "blacklist mode" configuration methods.

## Troubleshooting and Unfinished Items

### Xray Resource File Loading Failure (geosite.dat/geoip.dat)

Related directory file structure:

```sh
/usr/local/
├── bin/
│   ├── v2ray # V2Ray executable
│   └── xray # Xray executable
├── etc/
│   └── xray-core/
│       └── config.json # Xray configuration file
└── share/
    └── xray-core/
        ├── geosite.dat # Domain classification resource file
        └── geoip.dat # IP address classification resource file
```

When using Xray, you may encounter resource file loading failures. On FreeBSD, the pkg-installed xray-core resource files are located in **/usr/local/share/xray-core/**, but the program looks for them by default in the same directory as the executable, **/usr/local/bin/**. If you get an `open /usr/local/bin/geosite.dat: no such file or directory` error at startup, you can use one of the following two solutions:

#### Terminal Temporary Execution or User Classification Solution

**Temporary Execution**

Specify the resource path via an environment variable; this approach is suitable for single runs or testing scenarios:

```sh
# env XRAY_LOCATION_ASSET=/usr/local/share/xray-core/ xray run -c /path/to/config.json
```

> **Tip**
>
> If you need to run in the background, you can add the `&` symbol at the end of the command. However, this method does not detach the process from the current terminal session; if you need the process to continue running after closing the terminal, use `nohup` or `disown`.

**Persistent Configuration**

Persist the configuration through the login class method so that the environment variable takes effect automatically at each login, regardless of the shell type. Edit the **~/.login_conf** file:

```ini
me:\
	:setenv=XRAY_LOCATION_ASSET=/usr/local/share/xray-core/:
```

After editing, you need to run the following command to update the login capability database:

```sh
$ cap_mkdb ~/.login_conf
```

After completing the configuration, you should log in again for the changes to take effect. For the system service running method (such as rc.conf), since system services inject environment variables via sysrc, this configuration is not needed.

Create symbolic links so that Xray can find resource files regardless of where it is started from:

```sh
# ln -s /usr/local/share/xray-core/geosite.dat /usr/local/bin/geosite.dat
# ln -s /usr/local/share/xray-core/geoip.dat /usr/local/bin/geoip.dat
```

#### System Service Running Solution

**Place the configuration file:**

First, move the configuration to the system default directory and correct the permissions:

```sh
# cp /path/to/config.json /usr/local/etc/xray-core/config.json
# chown -R v2ray:v2ray /usr/local/etc/xray-core/      # Recursively change ownership of the /usr/local/etc/xray-core/ directory (and files such as config.json) to the v2ray user and group.
```

- FreeBSD's `security/xray-core` does not create a separate `xray` user/group, but instead uses the `v2ray:v2ray` from `net/v2ray` to maintain consistency in user privilege management.

If there are other `.json` sample files in the **/usr/local/etc/xray-core/** directory, they should be removed because Xray may scan all JSON files in the directory, which could cause configuration conflicts.

**Configure the rc.conf file:**

Run the following commands to enable the service and inject environment variables:

```sh
# sysrc xray_enable="YES"
# sysrc xray_config="/usr/local/etc/xray-core"      # Points to the xray-core proxy configuration directory, which is automatically generated when xray-core is installed
# sysrc xray_env="XRAY_LOCATION_ASSET=/usr/local/share/xray-core"      # Points to the xray-core resource file directory
```

**Service management:**

- Start: `service xray start`
- Stop: `service xray stop`
- Re-enable auto-start: use `service xray enable`
