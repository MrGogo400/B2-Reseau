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


<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEwNjQ4NDQwOSwxNjc3MDc4NzI3XX0=
-->