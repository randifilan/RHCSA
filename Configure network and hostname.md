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

Edit via nmcli
```
nmcli connection modify ens3 ipv4.method manual ipv4.addresses 172.20.10.10/24 ipv4.gateway 172.20.10.1 ipv4.dns 172.20.10.3
```

- Verify using nmcli
```
nmcli connection show ens3
connection.id:                          ens3
connection.uuid:                        99783413-b465-3df3-a908-d4570af31af2
connection.interface-name:              ens3
connection.autoconnect:                 yes
ipv4.method:                            manual
ipv4.dns:                               172.20.10.3
ipv4.dns-search:                        maas
ipv4.addresses:                         172.20.10.10/24
ipv4.gateway:                           172.20.10.1
GENERAL.NAME:                           ens3
GENERAL.UUID:                           99783413-b465-3df3-a908-d4570af31af2
GENERAL.DEVICES:                        ens3
GENERAL.IP-IFACE:                       ens3
GENERAL.STATE:                          activated
GENERAL.DEFAULT:                        yes
IP4.ADDRESS[1]:                         172.20.10.10/24
IP4.GATEWAY:                            172.20.10.1
IP4.DNS[1]:                             172.20.10.1
IP4.SEARCHES[1]:                        maas
```

- verify using ipconfig
```
ifconfig ens3
ens3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.20.10.10  netmask 255.255.255.0  broadcast 172.20.10.255
        ether 52:54:00:60:7f:2c  txqueuelen 1000  (Ethernet)
        RX packets 3092570  bytes 1372889084 (1.2 GiB)
        RX errors 0  dropped 1560841  overruns 0  frame 0
        TX packets 549311  bytes 85671831 (81.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

- using nmcli dev status
```
nmcli dev status
DEVICE  TYPE      STATE                   CONNECTION 
ens3    ethernet  connected               ens3       
lo      loopback  connected (externally)  lo 
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

- Verify Hostname
```
hostnamectl 
 Static hostname: servera.lab.randifilan.id
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: c021f2f2420c41558a083372033d562c
         Boot ID: 615fdd2bd4324e398f8ec6a1a88c4e5d
  Virtualization: kvm
Operating System: Red Hat Enterprise Linux 9.3 (Plow)     
     CPE OS Name: cpe:/o:redhat:enterprise_linux:9::baseos
          Kernel: Linux 5.14.0-362.18.1.el9_3.x86_64
    Architecture: x86-64
 Hardware Vendor: QEMU
  Hardware Model: Standard PC _i440FX + PIIX, 1996_
Firmware Version: Ubuntu-1.8.2-1ubuntu1
```
