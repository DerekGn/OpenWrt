# OpenWrt SSH tunneling

A simple how to guide to setting up OpenSSH tunneling on an OpenWrt router that has luci interface installed. This method will use key based authentication over password based authentication as its more secure.

## Versions

This guide is based on the following version of software

* OpenWrt 21.02.0 r16279-5cc0535800
* LuCI openwrt-21.02 branch git-22.083.69138-0a0ce2a

## Install SSHTunnel Package

Install the sshtunnel package via the System->Software menu item.

## Generate SSH Key

There are a number of methods to generate a ssh key file. Using windows 10/11 the following methods can be [used](https://www.howtogeek.com/762863/how-to-generate-ssh-keys-in-windows-10-and-windows-11
)

Note if using putty key gen the key must be converted to openssh format. For example:

``` txt
ssh-rsa AAAAB3...................Key.........................== TheUser
```

## Install the SSH Key

The generated key must be installed via the luci/admin/system/admin/sshkeys luci interface.

![InstallKey](.\images\InstallKey.PNG)

## Configure SSHTunnel

The configuration of the SSH Tunnel requires configuration via the OpenWrt shell. The configuration for the sshtunnel is located in the /etc/config/sshtunnel file. See [here](https://openwrt.org/docs/guide-user/services/ssh/sshtunnel) for more information.

### Add Server

The following is an example of a simple sshtunnel server

```txt
config server home
    option user root
    option hostname localhost
```

Note that it is more secure to configure the server to run as a non root user but that requires additional configuration that is beyond the scope of this gist.

### Add Local Forwarding

Local forwarding allows a local port on a client machine to be forwarded to a port on a remote ssh server. This allows access to a server behind the router without exposing the server on the internet.

```txt
config tunnell remoteserver
    option server home
    option localaddress localhost
    option localport 8443
    option remoteaddress 192.0.0.76
    option remoteport 443
```

## Add Dynamic Proxy

The following adds a dynamic tunnel that will use SOCKS4 or SOCKS5 protocol to forward to the remote host.

```txt
config tunneld propxy
    option server home
    option localaddress *
    option localport 8080
```

## Reload SSHTunnel

To apply t he sshtunnel settings execute the following command from the OpenWrt shell.

```ssh
/etc/init.d/sshtunnel reload
```

## Open Firewall Port


## Test With Putty

### Configure

## Disable SSH Password Login
