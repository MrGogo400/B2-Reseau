# Rendu TP-1

## 1. Gather Informations

### Récupérer une **liste des cartes réseau** avec leur nom, leur IP et leur adresse MAC

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
    2: ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 5a:ba:02:0e:f0:a8 brd ff:ff:ff:ff:ff:ff
        inet 192.168.0.26/24 brd 192.168.0.255 scope global dynamic noprefixroute ens18
           valid_lft 55419sec preferred_lft 55419sec
        inet6 fe80::efea:9eca:a3c:235b/64 scope link noprefixroute
           valid_lft forever preferred_lft forever

### Déterminer si les cartes réseaux ont récupéré une **IP en DHCP** 

Non les cartes réseaux n'ont pas récupéré une IP en DHCP



## II. Edit configuration

<!--stackedit_data:
eyJoaXN0b3J5IjpbNjk5NTIwMzA3LDEwODEwNjI3NDMsMTA2MD
cwMjM3NSwxODk1NDMxMjI0XX0=
-->