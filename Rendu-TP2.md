# Rendu TP-2

## I. Simplest setup

### Mettre en place la topologie ci-dessus :

![enter image description here](https://i.imgur.com/g9r27S6.png)


### Faire communiquer les deux PCs :

PC1 vers PC2 :

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

IOU1 est en Forwarding

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

On voit aussi graçe a la commande `show spanning-tree` que l'IOU1 est le root bridge

    Switch>show spanning-tree
    
    VLAN0001
      Spanning tree enabled protocol rstp
      Root ID    Priority    32769
                 Address     aabb.cc00.0100
                 This bridge is the root
                 Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
    
      Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                 Address     aabb.cc00.0100
                 Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
                 Aging Time  300 sec
    
    Interface           Role Sts Cost      Prio.Nbr Type
    ------------------- ---- --- --------- -------- --------------------------------
    Et0/0               Desg FWD 100       128.1    Shr
    Et0/1               Desg FWD 100       128.2    Shr
    Et0/2               Desg FWD 100       128.3    Shr
    Et0/3               Desg FWD 100       128.4    Shr
    Et1/0               Desg FWD 100       128.5    Shr
    Et1/1               Desg FWD 100       128.6    Shr
    Et1/2               Desg FWD 100       128.7    Shr
    Et1/3               Desg FWD 100       128.8    Shr


IOU2 est en Forwarding

    IOU2#show spanning-tree summary
    Switch is in rapid-pvst mode
    Root bridge for: none
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
    1 vlan 

IOU3 est en Blocking 

    IOU3#show spanning-tree summary
    Switch is in rapid-pvst mode
    Root bridge for: none
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
    VLAN0001                     1         0        0         15         16
    ---------------------- -------- --------- -------- ---------- ----------
    1 vlan                       1         0        0         15         16


### Confirmer les informations STP

Effectuer un `ping` d'une machine à une autre et vérifier que les trames passent bien par le chemin attendu (Wireshark)

    19	23.101799	10.2.2.1	10.2.2.2	ICMP	98	Echo (ping) request  id=0x17f7, seq=1/256, ttl=64 (reply in 20)
    20	23.102392	10.2.2.2	10.2.2.1	ICMP	98	Echo (ping) reply    id=0x17f7, seq=1/256, ttl=64 (request in 19)


## Reconfigurer STP

### Changer la priorité d'un switch qui n'est pas le root bridge

    IOU2(config)#spanning-tree vlan 1 priority 32768



<!--stackedit_data:
eyJoaXN0b3J5IjpbMzQ0Mzc3NTAsLTM4MTg3MTcwMCw1OTQxNj
kyNDUsMTg1OTA5NzU5OCwtNDI3MDEzOTMwLC03MTEyNzA4MDgs
MTQ2NzE0NjkwLC0xODgyODYzNTYyLDUwMDEzMTcyOSwxNjc3MD
c4NzI3XX0=
-->