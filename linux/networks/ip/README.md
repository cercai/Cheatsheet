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
In this example, my local IP is 192.168.1.45, the interface is dev enp4s0, my router's IP is 192.168.1.1 and my User ID is 1000 which is important for user-specific routing rules.<br>
You (192.168.1.45) ──> Router (192.168.1.1) ──> Internet ──> 8.8.8.8


Show ARP table
```
$ ip neigh
192.168.1.1 dev enp4s0 lladdr 7c:c1:xx:xx:xx:xx REACHABLE
fe80::xxxx:xxxx:xxxx:xxxx dev enp4s0 lladdr 7c:c1:xx:xx:xx:xx router REACHABLE
```
These systems are those your machine has seen or communicated with recently at layer 2 (Ethernet)
It is possible to find the manufacturer of the device with the MAC address.
`curl -s https://api.macvendors.com/xx:xx:xx:xx:xx:xx`

