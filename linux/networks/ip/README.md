# ip

The basic structure of the ip command:
```cmd
$ ip [OPTIONS] [OBJECT] [ACTION]
# example
$ ip addr show
```

If the action is not mentioned, then by default it is `show`, like `ip addr` or `ip link`.

The most common objects are:
  - ip addr
  - ip link
  - ip route
  - ip rule
  - ip neigh

Add an IP to an Interface
```cmd
$ sudo ip addr add 192.168.1.100/24 dev eth0
```

Delete an IP from an interface
```cmd
$ sudo ip addr del 192.168.1.100/24 dev eth0
```
Add a default route
```cmd
$ sudo ip route add default via 192.168.1.1
```
Ping route lookup
```cmd
$ ip route get 8.8.8.8
8.8.8.8 via 192.168.1.1 dev enp4s0 src 192.168.1.45 uid 1000
```
In this example, my local IP is 192.168.1.45 and my router is 192.168.1.1.<br>
You (192.168.1.45) ──> Router (192.168.1.1) ──> Internet ──> 8.8.8.8

