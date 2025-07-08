## Aruba AOS-CX - VRF
## Escenario
![](./Imagenes/Aruba%20AOS-CX%20Basics%207%20-%20VRF.png)
## Configuramos las interfaces
```go
SW1(config)# int 1/1/1
SW1(config-if)# no shutdown 
SW1(config-if)# routing
SW1(config-if)# ip address 10.1.1.1/24
SW1(config-if)# int 1/1/2
SW1(config-if)# no shutdown 
SW1(config-if)# routing 
SW1(config-if)# ip address 10.1.2.1/24
SW1(config-if)# int 1/1/3
SW1(config-if)# no shutdown 
SW1(config-if)# routing
SW1(config-if)# ip address 10.1.3.1/24
SW1(config-if)# int 1/1/4
SW1(config-if)# no shutdown 
SW1(config-if)# routing 
SW1(config-if)# ip address 10.1.4.1/24
SW1(config-if)# end
```
```go
SW1# sh ip int br | inc up
1/1/1            10.1.1.1/24               up/up
1/1/2            10.1.2.1/24               up/up
1/1/3            10.1.3.1/24               up/up
1/1/4            10.1.4.1/24               up/up
```
## Hacemos pruebas de ping
```go
VPCS2> ping 10.1.2.10

84 bytes from 10.1.2.10 icmp_seq=1 ttl=63 time=20.076 ms
84 bytes from 10.1.2.10 icmp_seq=2 ttl=63 time=2.528 ms
84 bytes from 10.1.2.10 icmp_seq=3 ttl=63 time=2.256 ms
84 bytes from 10.1.2.10 icmp_seq=4 ttl=63 time=1.975 ms
84 bytes from 10.1.2.10 icmp_seq=5 ttl=63 time=2.012 ms

VPCS2> ping 10.1.3.10

84 bytes from 10.1.3.10 icmp_seq=1 ttl=63 time=9.796 ms
84 bytes from 10.1.3.10 icmp_seq=2 ttl=63 time=2.056 ms
84 bytes from 10.1.3.10 icmp_seq=3 ttl=63 time=1.871 ms
84 bytes from 10.1.3.10 icmp_seq=4 ttl=63 time=2.134 ms
84 bytes from 10.1.3.10 icmp_seq=5 ttl=63 time=3.219 ms

VPCS2> ping 10.1.4.10

84 bytes from 10.1.4.10 icmp_seq=1 ttl=63 time=8.523 ms
84 bytes from 10.1.4.10 icmp_seq=2 ttl=63 time=2.169 ms
84 bytes from 10.1.4.10 icmp_seq=3 ttl=63 time=1.999 ms
84 bytes from 10.1.4.10 icmp_seq=4 ttl=63 time=2.640 ms
84 bytes from 10.1.4.10 icmp_seq=5 ttl=63 time=2.634 ms
```
> Se observa que hay ping entre todas las PCs.
## Configuramos las VRF
```go
SW1(config)# vrf Red
SW1(config-vrf)# exit
SW1(config)# vrf Blue
SW1(config-vrf)# exit
SW1(config)# int 1/1/1,1/1/3  
SW1(config-if-<1/1/1,1/1/3>)# vrf attach Red
SW1(config-if-<1/1/1,1/1/3>)# exit
SW1(config)# int 1/1/2,1/1/4 
SW1(config-if-<1/1/2,1/1/4>)# vrf attach Blue
SW1(config-if-<1/1/2,1/1/4>)# exit
```
```go
SW1(config)# sh run int 1/1/1
interface 1/1/1 
    no shutdown 
    vrf attach Red
    exit
```
> Se observa que cuando se une una interfaz a una VRF la IP configurada se borra.
## Configuramos nuevamente las IPs
```go
SW1(config)# int 1/1/1
SW1(config-if)# routing
SW1(config-if)# ip address 10.1.1.1/24
SW1(config-if)# int 1/1/2
SW1(config-if)# routing
SW1(config-if)# ip address 10.1.2.1/24 
SW1(config-if)# int 1/1/3
SW1(config-if)# routing
SW1(config-if)# ip address 10.1.3.1/24 
SW1(config-if)# int 1/1/4
SW1(config-if)# routing
SW1(config-if)# ip address 10.1.4.1/24 
SW1(config-if)# exit
```
## Hacemos pruebas de ping
```go
VPCS2> ping 10.1.3.10

84 bytes from 10.1.3.10 icmp_seq=1 ttl=63 time=19.058 ms
84 bytes from 10.1.3.10 icmp_seq=2 ttl=63 time=2.732 ms
84 bytes from 10.1.3.10 icmp_seq=3 ttl=63 time=2.843 ms
84 bytes from 10.1.3.10 icmp_seq=4 ttl=63 time=2.709 ms
84 bytes from 10.1.3.10 icmp_seq=5 ttl=63 time=2.356 ms

VPCS2> ping 10.1.2.10

*10.1.1.1 icmp_seq=1 ttl=64 time=2.478 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.1.1 icmp_seq=2 ttl=64 time=2.078 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.1.1 icmp_seq=3 ttl=64 time=2.026 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.1.1 icmp_seq=4 ttl=64 time=2.089 ms (ICMP type:3, code:0, Destination network unreachable)
10.1.2.10 icmp_seq=5 timeout

VPCS2> ping 10.1.4.10

*10.1.1.1 icmp_seq=1 ttl=64 time=2.560 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.1.1 icmp_seq=2 ttl=64 time=4.304 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.1.1 icmp_seq=3 ttl=64 time=2.040 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.1.1 icmp_seq=4 ttl=64 time=1.915 ms (ICMP type:3, code:0, Destination network unreachable)
10.1.4.10 icmp_seq=5 timeout
```
```go
SW1# sh ip route vrf Red

Displaying ipv4 routes selected for forwarding

Origin Codes: C - connected, S - static, L - local
              R - RIP, B - BGP, O - OSPF
Type Codes:   E - External BGP, I - Internal BGP, V - VPN, EV - EVPN
              IA - OSPF internal area, E1 - OSPF external type 1
              E2 - OSPF external type 2

VRF: Red

Prefix              Nexthop          Interface     VRF(egress)       Origin/   Distance/    Age
                                                                     Type      Metric
---------------------------------------------------------------------------------------------------------
10.1.1.0/24         -                1/1/1         -                 C         [0/0]        -            
10.1.1.1/32         -                1/1/1         -                 L         [0/0]        -            
10.1.3.0/24         -                1/1/3         -                 C         [0/0]        -            
10.1.3.1/32         -                1/1/3         -                 L         [0/0]        -            

Total Route Count : 4
```
```go
VPCS> ping 10.1.4.10

84 bytes from 10.1.4.10 icmp_seq=1 ttl=63 time=17.714 ms
84 bytes from 10.1.4.10 icmp_seq=2 ttl=63 time=2.527 ms
84 bytes from 10.1.4.10 icmp_seq=3 ttl=63 time=2.588 ms
84 bytes from 10.1.4.10 icmp_seq=4 ttl=63 time=2.853 ms
84 bytes from 10.1.4.10 icmp_seq=5 ttl=63 time=2.739 ms

VPCS> ping 10.1.1.10

*10.1.2.1 icmp_seq=1 ttl=64 time=1.744 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.2.1 icmp_seq=2 ttl=64 time=2.794 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.2.1 icmp_seq=3 ttl=64 time=2.320 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.2.1 icmp_seq=4 ttl=64 time=2.233 ms (ICMP type:3, code:0, Destination network unreachable)
10.1.1.10 icmp_seq=5 timeout

VPCS> ping 10.1.3.10

*10.1.2.1 icmp_seq=1 ttl=64 time=1.581 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.2.1 icmp_seq=2 ttl=64 time=1.809 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.2.1 icmp_seq=3 ttl=64 time=1.737 ms (ICMP type:3, code:0, Destination network unreachable)
*10.1.2.1 icmp_seq=4 ttl=64 time=2.466 ms (ICMP type:3, code:0, Destination network unreachable)
10.1.3.10 icmp_seq=5 timeout
```
```go
SW1# sh ip route vrf Blue

Displaying ipv4 routes selected for forwarding

Origin Codes: C - connected, S - static, L - local
              R - RIP, B - BGP, O - OSPF
Type Codes:   E - External BGP, I - Internal BGP, V - VPN, EV - EVPN
              IA - OSPF internal area, E1 - OSPF external type 1
              E2 - OSPF external type 2

VRF: Blue

Prefix              Nexthop          Interface     VRF(egress)       Origin/   Distance/    Age
                                                                     Type      Metric
---------------------------------------------------------------------------------------------------------
10.1.2.0/24         -                1/1/2         -                 C         [0/0]        -            
10.1.2.1/32         -                1/1/2         -                 L         [0/0]        -            
10.1.4.0/24         -                1/1/4         -                 C         [0/0]        -            
10.1.4.1/32         -                1/1/4         -                 L         [0/0]        -            

Total Route Count : 4
```
> Se observa que solo hay comunicación entre las PCs que pertenecen a la misma VRF, esto debido a que la tabla de enrutamiento es independiente para cada VRF.