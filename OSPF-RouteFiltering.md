## Topología
![](image.png)
## Configuración OSPF (Cisco)
**R1**
```go
!
interface GigabitEthernet0/0
 description CNX-R2
 ip address 192.168.12.1 255.255.255.0
 ip ospf network point-to-point
 ip ospf 1 area 0
!
interface GigabitEthernet0/1
 description CNX-CX3
 ip address 192.168.13.1 255.255.255.0
 ip ospf network point-to-point
 ip ospf 1 area 11
!
router ospf 1
 router-id 1.1.1.1
!
```
**R2**
```go
!
interface GigabitEthernet0/0
 description CNX-R1
 ip address 192.168.12.2 255.255.255.0
 ip ospf network point-to-point
 ip ospf 1 area 0
!
interface GigabitEthernet0/1
 description CNX-CX4
 ip address 192.168.24.2 255.255.255.0
 ip ospf network point-to-point
 ip ospf 1 area 22
!
router ospf 1
 router-id 2.2.2.2
!
```
## Configuración OSPF - Authentication (Cisco)
**R1**
```go
!
interface GigabitEthernet0/0
 ip ospf message-digest-key 1 md5 Ospf123!
!
interface GigabitEthernet0/1
 ip ospf message-digest-key 1 md5 Ospf123!
!
router ospf 1
 area 0 authentication message-digest
 area 11 authentication message-digest
!
```
**R2**
```go
!
interface GigabitEthernet0/0
 ip ospf message-digest-key 1 md5 Ospf123!
!
interface GigabitEthernet0/1
 ip ospf message-digest-key 1 md5 Ospf123!
!
router ospf 1
 area 0 authentication message-digest
 area 22 authentication message-digest
!
```