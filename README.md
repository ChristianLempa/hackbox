# kasm-hackbox

> [!attention]
> Work in progress! Not ready for production...

Hack-the-Box ready Docker Image for KASM Workspaces, based on Ubuntu Core maintained by KASM Tech

## Use this Image in Kasmweb

Please insert the following argument in the Kasm web `Docker Run Config Override (JSON)` to allow the container to establish the VPN connection.

```
{"cap_add":["NET_ADMIN"],"devices":["dev/net/tun","/dev/net/tun"],"sysctls":{"net.ipv6.conf.all.disable_ipv6":"0"}}

```

## OpenVPN to Hack The Box

This Docker image includes an OpenVPN client, just upload the .ovpn config into the container, and start the openvpn client.

```
openvpn --config <your-config-file>
```

