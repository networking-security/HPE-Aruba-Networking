## Configuración VLAN
#### SW2L3
```go
SW2L3(config)# vlan 150
SW2L3(config-vlan-150)# name SALES
SW2L3(config-vlan-150)# exit
```
#### SW3L3
```go
SW3L3(config)# vlan 150
SW3L3(config-vlan-150)# name SALES
SW3L3(config-vlan-150)# exit
```
## Asignación interfaz a la VLAN
#### SW2L3
```go
SW2L3(config)# int 1/1/1
SW2L3(config-if)# no shutdown
SW2L3(config-if)# no routing
SW2L3(config-if)# vlan access 150
SW2L3(config-if)# exit
```
#### SW3L3
```go
SW3L3(config)# int 1/1/1
SW3L3(config-if)# no shutdown
SW3L3(config-if)# no routing
SW3L3(config-if)# vlan access 150
SW3L3(config-if)# exit
```
## Enlace troncal SW2L3 -> SW3L3
#### SW2L3
```go
SW2L3(config)# int 1/1/2
SW2L3(config-if)# no shutdown
SW2L3(config-if)# no routing
SW2L3(config-if)# vlan trunk allowed 150
SW2L3(config-if)# exit
```
#### SW3L3
```go
SW3L3(config)# int 1/1/2
SW3L3(config-if)# no shutdown
SW3L3(config-if)# no routing
SW3L3(config-if)# vlan trunk allowed 150
SW3L3(config-if)# exit
```
#### Prueba ping PC1 -> PC2
```go
root@PC-1:~# ping 192.168.150.102
PING 192.168.150.102 (192.168.150.102) 56(84) bytes of data.
64 bytes from 192.168.150.102: icmp_seq=1 ttl=64 time=17.8 ms
64 bytes from 192.168.150.102: icmp_seq=2 ttl=64 time=10.0 ms
64 bytes from 192.168.150.102: icmp_seq=3 ttl=64 time=5.49 ms
64 bytes from 192.168.150.102: icmp_seq=4 ttl=64 time=6.18 ms
64 bytes from 192.168.150.102: icmp_seq=5 ttl=64 time=6.45 ms
^C
--- 192.168.150.102 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4004ms
rtt min/avg/max/mdev = 5.492/9.201/17.878/4.613 ms
```
## Configuración VLAN Nativa 99
#### SW2L3
```go
SW2L3(config)# vlan 99
SW2L3(config-vlan-99)# name NATIVE 
SW2L3(config-vlan-99)# exit
SW2L3(config)# int 1/1/1           
SW2L3(config-if)# vlan trunk native 99
SW2L3(config-if)# exit
```
#### SW3L3
```go
SW3L3(config)# vlan 99
SW3L3(config-vlan-99)# name NATIVE 
SW3L3(config-vlan-99)# exit
SW3L3(config)# int 1/1/1           
SW3L3(config-if)# vlan trunk native 99
SW3L3(config-if)# exit
```
## Show commands
#### SW2L3
```go
SW2L3# show vlan summary 
Number of existing VLANs: 3
Number of static VLANs:   3
Number of dynamic VLANs:  0
```
```go
SW2L3# show vlan 

----------------------------------------------------------------------------------------------------------------
VLAN  Name                              Status  Reason                  Type      Interfaces                    
----------------------------------------------------------------------------------------------------------------
1     DEFAULT_VLAN_1                    down    no_member_port          default   
99    NATIVE                            down    no_member_port          static    
150   SALES                             up      ok                      static    1/1/1-1/1/2
#### SW3L3
SW3L3# show vlan summary 
Number of existing VLANs: 3
Number of static VLANs:   3
Number of dynamic VLANs:  0
SW3L3# show vlan

----------------------------------------------------------------------------------------------------------------
VLAN  Name                              Status  Reason                  Type      Interfaces                    
----------------------------------------------------------------------------------------------------------------
1     DEFAULT_VLAN_1                    down    no_member_port          default   
99    NATIVE                            down    no_member_port          static    
150   SALES                             up      ok                      static    1/1/1-1/1/2
```