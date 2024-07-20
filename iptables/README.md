# FIREWALL

List the rules with `-L` or `--List`.
The input is the trafic that comes from the internet into you computer.
The output is the trafic that you send to the internet.
Forwarding is for routing the trafic, it is for routers.

```sh
iptables --list

    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination
```

To change the policy, use `-P`, add the Chain and finally the policy.
```sh
iptables -P FORWARD DROP
```

```sh
iptables -A INPUT -s 192.168.0.23 -j DROP     # Bloquer la connexion, la personne ne pourra plus nous joindre
iptables -A INPUT -s 192.168.0.0/24 -p tcp --destination-port 25 -j DROP   # Arrêter les emails depuis le subnet local
iptables -A INPUT -s 192.168.0.48 -j ACCEPT   # Accepter les emails de l'IP en question

iptables -D INPUT 3

Les règles du parfeu sont lues de haut en bas (en haut sont les règles prioritaies)
iptables -A INPUT   # Ajoute une règle à la fin
iptables -I INPUT   # Ajoute une règle au début (elle sera don prioritaire)
```

