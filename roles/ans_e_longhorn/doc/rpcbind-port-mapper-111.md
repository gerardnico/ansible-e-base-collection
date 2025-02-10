# Port 111 (rpcbind known as port mapper)

## What?
After installing NFS with `nfs-common` package, the `rpcbind` service (also known as `portmapper`) is started
And we got an [abuse email](rpcbind-alert.eml)

## What usage?

`rpcbind` is needed for nfs v3 connections.
You should also be able to firewall that service to only work on the internal network

https://github.com/longhorn/longhorn/issues/2603

## Command
### Discover
On the server:
```bash
sudo netstat -tulpn | grep :111
```
```
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/init
tcp6       0      0 :::111                  :::*                    LISTEN      1/init
udp        0      0 0.0.0.0:111             0.0.0.0:*                           1/init
udp6       0      0 :::111                  :::*                                1/init
```
It's one of the systemd services.
```bash
grep 111\/ /etc/services
```
```bash
sunrpc          111/tcp         portmapper      # RPC 4.0 portmapper
sunrpc          111/udp         portmapper
```

### Disable manually

```bash
systemctl disable --now rpcbind.service rpcbind.socket
sudo netstat -tulpn | grep :111
```