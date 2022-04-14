# kasm-hackbox

> Attention: Work in progress! Not ready for production...

Hack-the-Box ready Docker Image for KASM Workspaces, based on Ubuntu Core maintained by KASM Tech

## Test this image locally

To run this image in a local environment start the docker container with the following command, and connect to your machine on `https://localhost:6901` and log in with username `kasm_user` and password `password`.

```
docker run --rm -it --shm-size=512m -p 6901:6901 -e VNC_PW=password xcad2k/hackbox
```

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

