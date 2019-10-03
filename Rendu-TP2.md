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
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA5NDM5MTQ3NSwxNDY3MTQ2OTAsLTE4OD
I4NjM1NjIsNTAwMTMxNzI5LDE2NzcwNzg3MjddfQ==
-->