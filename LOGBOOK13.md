# SEED Labs - Sniffing and Spoofing

## Task 1

### Setup

Começamos por utilizar o comando ```dcbuild```, de seguid do comando ```dcup``` para incializarmos o docker.
De seguida utilizamos o comando ```dockps```

### Task 1.1.A

- Depois de termos começado o docker, incializamos dois novos terminais. No 1º utilizamos o comando ```docksh seed-attacker``` e no 2º o comando ```docksh hostA-10.9.0.5```.
- No terminal do attacker utilizamos o comando ``ìfconfig``` percebendo que a noss interface será br-95e4991e848b.

- Screenshot A


- De seguida utilizamos o seguinte script de python no terminal do attacker atribuíndo-lhe as seguintes permissões ```chmod a+x task1.1.py```:
```#!/usr/bin/env python3

from scapy.all import *
def print_pkt(pkt):
	pkt.show()

pkt = sniff(iface='br-95e4991e848b', filter='icmp', prn=print_pkt)```

- Depois de colocarmos o attacker em "Sniffing", no segundo terminal demos ping em 10.9.0.6 e percebemos que o terminal do attacker capturou os packets transmitidos.

- Screenshot B

- Experimentamos por fim efetuar o mesmo script mas sem permissões de admistrador e concluímos que não foi possível caputar os packets a partir do attacker.


### Task 1.1.B



