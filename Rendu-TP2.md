# Rendu TP-2

## I. Simplest setup

### Mettre en place la topologie ci-dessus :

![enter image description here](https://i.imgur.com/g9r27S6.png)


### Faire communiquer les deux PCs :

PC1 vers PC1 :

    PC1> ping 10.2.1.2
    84 bytes from 10.2.1.2 icmp_seq=1 ttl=64 time=0.202 ms
    84 bytes from 10.2.1.2 icmp_seq=2 ttl=64 time=0.371 ms
    84 bytes from 10.2.1.2 icmp_seq=3 ttl=64 time=0.359 ms
    84 bytes from 10.2.1.2 icmp_seq=4 ttl=64 time=0.295 ms
    84 bytes from 10.2.1.2 icmp_seq=5 ttl=64 time=0.469 ms

PC2 vers PC1 :

    PC2> ping 10.2.1.1
    84 bytes from 10.2.1.1 icmp_seq=1 ttl=64 time=0.180 ms
    84 bytes from 10.2.1.1 icmp_seq=2 ttl=64 time=0.335 ms

### Déterminer le protocole utilisé par `ping` à l'aide de Wireshark
Sur WireShark : 

    13	13.918831	10.2.1.1	10.2.1.2	ICMP	98	Echo (ping) request  id=0xd2c3, seq=1/256, ttl=64 (reply in 14)
    14	13.918992	10.2.1.2	10.2.1.1	ICMP	98	Echo (ping) reply    id=0xd2c3, seq=1/256, ttl=64 (request in 13)

On voit que le protocole utilisé est ICMP.

### Analyser les échanges ARP

    13	13.918831	10.2.1.1	10.2.1.2	ICMP	98	Echo (ping) request  id=0xd2c3, seq=1/256, ttl=64 (reply in 14)
    14	13.918992	10.2.1.2	10.2.1.1	ICMP	98	Echo (ping) reply    id=0xd2c3, seq=1/256, ttl=64 (request in 13)

Sur PC1 : 

    PC1> show arp
    
    00:50:79:66:68:01  10.2.1.2 expires in 117 seconds

Sur PC2 : 

    PC2> show arp
    
    00:50:79:66:68:00  10.2.1.1 expires in 48 seconds


On voit bien la corrélation entre les tables ARP ou il y'a connexion entre les 2 PC


### Récapituler toutes les étapes (dans le compte-rendu, à l'écrit) quand `PC1` exécute `ping PC2` pour la première fois

Après avoir fait les prérequis,
On mets en place la topologie demander, on change les ip des PC grâce a la commande `ip  xx.xx.xx.xx/CIDR` , on peux désormais ping les pc entre eux et voir les table ARP que ce soit sur les pc avec la commande `show arp` ou directement sur WireShark.

### Expliquer pourquoi le switch n'a pas besoin d'IP

Parce que il sert uniquement d'interface entre les 2 PC.

### Expliquer pourquoi les machines ont besoin d'une IP pour pouvoir se `ping`

Car l'IP leur sert à être identifiable sur le réseau et ainsi pouvoir communiquer avec les différents objets  connectés. 

## II. More switches

### Mettre en place la topologie ci-dessus

![enter image description here](https://i.imgur.com/OtuPz62.png)

### Faire communiquer les trois PCs

    PC1> ping 10.2.2.2
    84 bytes from 10.2.2.2 icmp_seq=1 ttl=64 time=0.283 ms
    84 bytes from 10.2.2.2 icmp_seq=2 ttl=64 time=0.444 ms
    
    PC1> ping 10.2.2.3
    84 bytes from 10.2.2.3 icmp_seq=1 ttl=64 time=0.584 ms
    84 bytes from 10.2.2.3 icmp_seq=2 ttl=64 time=0.614 ms

### Analyser la table MAC d'un switch

    IOU1#show mac address-table
              Mac Address Table
    -------------------------------------------
    
    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
       1    0050.7966.6800    DYNAMIC     Et0/2
       1    0050.7966.6801    DYNAMIC     Et0/0
       1    0050.7966.6802    DYNAMIC     Et0/1
       1    aabb.cc00.0210    DYNAMIC     Et0/1
       1    aabb.cc00.0310    DYNAMIC     Et0/2
       1    aabb.cc00.0320    DYNAMIC     Et0/1
    Total Mac Addresses for this criterion: 6

Chaque ligne montre l'adresse MAC connecter sur le port dans quel VLAN et de quel type

## Mise en évidence du Spanning Tree Protocol

### Déterminer les informations STP



    IOU1#show spanning-tree summary
    Switch is in rapid-pvst mode
    Root bridge for: VLAN0001
    Extended system ID                      is enabled
    Portfast Default                        is disabled
    Portfast Edge BPDU Guard Default        is disabled
    Portfast Edge BPDU Filter Default       is disabled
    Loopguard Default                       is disabled
    PVST Simulation Default                 is enabled but inactive in rapid-pvst mode
    Bridge Assurance                        is enabled
    EtherChannel misconfig guard            is enabled
    Configured Pathcost method used is short
    UplinkFast                              is disabled
    BackboneFast                            is disabled
    
    Name                   Blocking Listening Learning Forwarding STP Active
    ---------------------- -------- --------- -------- ---------- ----------
    VLAN0001                     0         0        0         16         16
    ---------------------- -------- --------- -------- ---------- ----------
    1 vlan                       0         0        0         16         16

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY3OTkwNTI1MiwtNzExMjcwODA4LDE0Nj
cxNDY5MCwtMTg4Mjg2MzU2Miw1MDAxMzE3MjksMTY3NzA3ODcy
N119
-->