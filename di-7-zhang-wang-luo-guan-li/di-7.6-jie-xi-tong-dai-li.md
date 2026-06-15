# 7.6 System Proxy

Proxy technology is a fundamental concept in computer networks. Its core principle is to introduce an intermediate node between the client and the target server, with the intermediate node forwarding requests and responses on behalf of the client.

## How Proxies Work

The workflow of a proxy can be summarized as: the client initiates a request → the request is redirected to the proxy server → the proxy server forwards the request to the target server → the target server responds to the proxy server → the proxy server returns the response to the client. During this process, the target server sees the proxy server's address, not the client's real address.

Proxies can be divided into three categories based on deployment mode:

| Type | Description | Typical Use |
| ---- | ----------- | ----------- |
| **Forward Proxy** | The client explicitly configures the proxy address, and the proxy initiates requests to external servers on behalf of the client | Bypassing network access restrictions and cache acceleration |
| **Reverse Proxy** | The proxy receives client requests on behalf of the server; the client does not know the real server's address | Load balancing, SSL termination, and static content caching |
| **Transparent Proxy** | The client does not need configuration; network devices (such as routers) redirect traffic to the proxy | Content filtering and traffic monitoring in enterprise networks |

Before configuring a system proxy, you need to understand the environment variable configuration methods provided by FreeBSD.

## Configuring HTTP_PROXY Proxy

By setting environment variables such as HTTP_PROXY, HTTPS_PROXY, and ALL_PROXY, most command-line tools can forward traffic through a proxy. The following are the configuration methods.

### Temporary Setting

Temporarily set proxy environment variables in the current shell session:

```sh
$ export HTTP_PROXY=http://192.168.X.X:7890
```

> **Warning**
>
> The IP address and port number **192.168.X.X:7890** in the example must be replaced with the actual proxy endpoint.

Unset the configured HTTP proxy environment variable:

```sh
$ unset HTTP_PROXY
```

### Persistent Configuration (User Classification Method)

Use the user classification method for persistent configuration, so that proxy environment variables automatically take effect at each login, regardless of the shell type. Edit the **~/.login_conf** file:

```ini
me:\
	:setenv=HTTP_PROXY=http://192.168.X.X:7890,HTTPS_PROXY=http://192.168.X.X:7890:
```

After editing, execute the following command to update the login capability database:

```sh
$ cap_mkdb ~/.login_conf
```

It will take effect after logging in again.

## Configuring Git Proxy

Git supports setting proxies through configuration options such as `http.proxy` and `core.gitProxy`.

## Configuring a Proxy for the Browser

### Chrome Command Options

[Chromium](https://www.chromium.org/) is the open-source version of the Google Chrome browser, supporting various command-line parameters.

The Chromium browser does not have an independent proxy configuration interface in directories such as **~/.config**, but supports setting proxies through `http_proxy`/`https_proxy` environment variables (in non-Gnome/KDE environments) and startup parameters.

You can specify the proxy server and port in the following format:

```sh
--proxy-server="<IP address>:<port>"
```

Launch Chrome using the specified local proxy server:

```sh
$ chrome --proxy-server="127.0.0.1:1234"
```

HTTP protocol is used by default. Specify a SOCKS proxy server and port:

```sh
--proxy-server="socks://<IP address>:<port>"
```

Specify a SOCKS4 proxy server and port:

```sh
--proxy-server="socks4://<IP address>:<port>"
```

To make Chromium start with a proxy by default in the graphical interface, you can modify the desktop launch file to achieve persistent configuration.

Find the desktop file created by the desktop environment for Chromium, typically located in the **~/.local/share/applications/** directory:

Open the Chromium desktop file `chromium-browser.desktop` with an editor, find the `Exec=chrome %U` line, and add the required parameters after it:

```ini
Comment[zh_CN]=Google web browser based on WebKit
Comment=Google web browser based on WebKit
Encoding=UTF-8
Exec=chrome %U
GenericName[zh_CN]=
......
```

Launch Chromium using the specified proxy server:

```sh
Exec=chrome %U --proxy-server="192.168.2.163:20172"
```

> **Tip**
>
> The **192.168.2.163** and `20172` in the above example are placeholders and must be replaced with actual values.

### Configuring a Proxy for Firefox Individually

The settings page of the Firefox browser provides a graphical proxy configuration module in the network settings tab.

![Firefox proxy settings](../.gitbook/assets/firefox-proxy.png)

### References

- Owynn, graudeejs, rjohn, olli@, Sevendogsbsd, kpedersen. chromium proxy settings page doesn't exist[EB/OL]. [2026-03-25]. <https://forums.freebsd.org/threads/chromium-proxy-settings-page-doesnt-exist.31927/>. Provides practical solutions for Chromium proxy configuration on FreeBSD.

## Exercises

1. Modify Chromium's desktop file to make it start with a SOCKS5 proxy by default, verify whether its DNS queries are forwarded through the proxy, and analyze the impact of the `--proxy-server` parameter on the DNS resolution path.
2. Write proxy toggle scripts for csh and sh respectively. After setting the proxy, use `tcpdump` to verify the actual network traffic paths of commands such as git and fetch, and analyze the differences in environment variable case conventions across different shells and their specification origins.
3. Write a shell script for Firefox that implements automatic proxy switching by modifying its `prefs.js` configuration file, and compare the differences in user controllability between Firefox's configuration file approach and Chromium's command-line parameter approach.
