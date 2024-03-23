# Question
Configure network and set IP Address
- IP Address 172.20.10.10
- Netmask 255.255.255.0
- Gateway 172.20.10.1
- DNS 172.20.10.1
- Hostname servera.lab.randifilan.id
---

Find the iface [ens3]
- using nmcli con show
```
nmcli con show
NAME  UUID                                  TYPE      DEVICE 
ens3  99783413-b465-3df3-a908-d4570af31af2  ethernet  ens3   
lo    4e07cc32-ff6d-43a9-b7dd-d37b4b3c78dc  loopback  lo 
```

- using nmcli dev status
```
nmcli dev status
DEVICE  TYPE      STATE                   CONNECTION 
ens3    ethernet  connected               ens3       
lo      loopback  connected (externally)  lo 
```

Edit via nmcli
```
nmcli connection modify ens3 ipv4.method manual ipv4.addresses 172.20.10.10/24 ipv4.gateway 172.20.10.1 ipv4.dns 172.20.10.3
```

- Disable ens3
```
nmcli connection down ens3
```

- Enable ens3
```
nmcli connection up ens3
```

Change hostname
```
hostnamectl set-hostname servera.lab.randifilan.id
```
