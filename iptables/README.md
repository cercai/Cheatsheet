# FIREWALL

List the rules with `-L` or `--List`.

```sh
iptables --list

    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination
```

INPUT: For packets destined to the local system.
FORWARD: For packets routed through the system.
OUTPUT: For packets generated locally and destined to the network.
PREROUTING: For altering packets as they arrive.
POSTROUTING: For altering packets before they leave.

To change the policy, use `-P`, add the Chain and finally the policy.
```sh
iptables -P FORWARD DROP
```
Append a rule to a chain with `-A` then define the source and the rule DROP/ACCEPT.
```sh
iptables -A INPUT -s 192.168.0.23 -j DROP   # Drop the traffic from this IP
```
Drop the trafic using the port 25 from all the network.
```sh
iptables -A INPUT -s 192.168.0.0/24 -p tcp --destination-port 25 -j DROP
```

The priority of rules in iptables is determined by their order within a chain
To accept the trafic comming from a source, the rule that ACCEPT the trafic must be before the rule that DROP it.
`-A` appends the new rule at the end of the list and `-I` adds it at the beginning.
```sh
iptables -I INPUT -s 192.168.0.48 -j ACCEPT
```

To delete a rule use `-D`. Here the third rule in chain INPUT is removed.
```sh
iptables -D INPUT 3
```