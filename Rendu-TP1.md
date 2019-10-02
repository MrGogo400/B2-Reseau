# Rendu TP-1

## 1. Gather Informations

### Récupérer une **liste des cartes réseau** avec leur nom, leur IP et leur adresse MAC

 

       [root@localhost ~]# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:ca:39:a3 brd ff:ff:ff:ff:ff:ff
        inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
           valid_lft 84926sec preferred_lft 84926sec
        inet6 fe80::87e0:26d6:e64b:c3d8/64 scope link noprefixroute
           valid_lft forever preferred_lft forever
    3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:4e:8b:06 brd ff:ff:ff:ff:ff:ff
    4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:8d:ca:d8 brd ff:ff:ff:ff:ff:ff
    5: enp0s10: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:94:88:2e brd ff:ff:ff:ff:ff:ff
        inet 192.168.18.55/24 brd 192.168.18.255 scope global noprefixroute enp0s10
           valid_lft forever preferred_lft forever
        inet6 fe80::60be:3f99:58fd:11bc/64 scope link noprefixroute
           valid_lft forever preferred_lft forever


### Déterminer si les cartes réseaux ont récupéré une **IP en DHCP** 

La carte enp0s3 (La nat) est en DHCP

    DHCP4.OPTION[1]:                        domain_name_servers = 89.2.0.1 89.2.0.2
    DHCP4.OPTION[2]:                        expiry = 1570117787
    DHCP4.OPTION[3]:                        ip_address = 10.0.2.15

### Afficher la **table de routage** de la machine et sa **table ARP**

    default via 10.0.2.2 dev enp0s3
    default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
    10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
    192.168.18.0/24 dev enp0s10 proto kernel scope link src 192.168.18.55 metric 101


### Récupérer **la liste des ports en écoute** (_listening_) sur la machine (TCP et UDP)

    [root@localhost ~]# ss -lntu
    NetidState  Recv-Q Send-Q        Local Address:Port   Peer Address:Port
    udp  UNCONN 0      0        192.168.0.26%ens18:68          0.0.0.0:*
    udp  UNCONN 0      0                 127.0.0.1:323         0.0.0.0:*
    udp  UNCONN 0      0                     [::1]:323            [::]:*
    tcp  LISTEN 0      128                 0.0.0.0:22          0.0.0.0:*
    tcp  LISTEN 0      128                    [::]:22             [::]:*

On voit le service SSH qui est activé et qui écoute sur le port 22

### Récupérer **la liste des DNS utilisés par la machine**

    [root@localhost ~]# dig
    
    ; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>>
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41014
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 6
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4000
    ;; QUESTION SECTION:
    ;.                              IN      NS
    
    ;; ANSWER SECTION:
    .                       492409  IN      NS      g.root-servers.net.
    .                       492409  IN      NS      h.root-servers.net.
    .                       492409  IN      NS      i.root-servers.net.
    .                       492409  IN      NS      j.root-servers.net.
    .                       492409  IN      NS      k.root-servers.net.
    .                       492409  IN      NS      l.root-servers.net.
    .                       492409  IN      NS      m.root-servers.net.
    .                       492409  IN      NS      a.root-servers.net.
    .                       492409  IN      NS      b.root-servers.net.
    .                       492409  IN      NS      c.root-servers.net.
    .                       492409  IN      NS      d.root-servers.net.
    .                       492409  IN      NS      e.root-servers.net.
    .                       492409  IN      NS      f.root-servers.net.
    
    ;; ADDITIONAL SECTION:
    m.root-servers.net.     596276  IN      A       202.12.27.33
    m.root-servers.net.     598207  IN      AAAA    2001:dc3::35
    a.root-servers.net.     335381  IN      A       198.41.0.4
    a.root-servers.net.     349633  IN      AAAA    2001:503:ba3e::2:30
    b.root-servers.net.     568897  IN      A       199.9.14.201
    
    ;; Query time: 7 msec
    ;; SERVER: 89.2.0.1#53(89.2.0.1)
    ;; WHEN: Thu Sep 26 05:46:34 EDT 2019
    ;; MSG SIZE  rcvd: 343

### Requête DNS de www.reddit.com

    [root@localhost ~]# dig www.reddit.com
    
    ; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>> www.reddit.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 5025
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4000
    ;; QUESTION SECTION:
    ;www.reddit.com.                        IN      A
    
    ;; ANSWER SECTION:
    www.reddit.com.         90      IN      CNAME   reddit.map.fastly.net.
    reddit.map.fastly.net.  26      IN      A       151.101.121.140
    
    ;; Query time: 8 msec
    ;; SERVER: 89.2.0.1#53(89.2.0.1)
    ;; WHEN: Thu Sep 26 05:47:31 EDT 2019
    ;; MSG SIZE  rcvd: 94

### Afficher **l'état actuel du firewall**

    [root@localhost ~]# firewall-cmd --list-all
    public (active)
      target: default
      icmp-block-inversion: no
      interfaces: ens18
      sources:
      services: cockpit dhcpv6-client ssh
      ports:
      protocols:
      masquerade: no
      forward-ports:
      source-ports:
      icmp-blocks:
      rich rules:

L'interface filtré est la "ens18"
Les ports filtrés sont : 9090, 546, 22

## II. Edit configuration



<!--stackedit_data:
eyJoaXN0b3J5IjpbNDEyNzAyMjk2LC0yMzQ4MDQ3MDUsLTIwNT
Y3NTQxNjEsLTE0NTYxMjQ5MTMsLTkxMzQzNTExMSwxMzkyMTEy
NjMxLC0xNTU0ODg1NTQsODEzMDQwMTM1LDEzMzk4ODcxMjMsLT
MzNDc5OTg4MiwxMDgxMDYyNzQzLDEwNjA3MDIzNzUsMTg5NTQz
MTIyNF19
-->