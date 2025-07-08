## Management Network Config
## Configuración básica Aruba CX
```go
switch# conf t
switch(config)# hostname SW2L3
SW2L3(config)# user admin password
Enter password: *****
Confirm password: *****
```
## Configuración de la interfaz mgmt Aruba CX
```go
SW2L3(config)# int mgmt
SW2L3(config-if-mgmt)# ip static 192.168.50.201/24
SW2L3(config-if-mgmt)# ip static 2001:db8:a:50::201/64
SW2L3(config-if-mgmt)# default-gateway 192.168.50.1  
SW2L3(config-if-mgmt)# default-gateway 2001:db8:a:50::1   
SW2L3(config-if-mgmt)# exit
```
## Configuración de la interfaz vlan 1 Cisco
```go
Switch(config)#hostname SW1L3
SW1L3(config)#int vlan 1
SW1L3(config-if)#ip add 192.168.50.1 255.255.255.0
SW1L3(config-if)#ipv6 add 2001:db8:a:50::1/64
SW1L3(config-if)#ipv6 add fe80::1 link-local 
SW1L3(config-if)#no sh
SW1L3(config-if)#exit
```
## Ping Aruba CX -> Cisco
```go
SW2L3# ping 192.168.50.1 vrf mgmt
PING 192.168.50.1 (192.168.50.1) 100(128) bytes of data.
108 bytes from 192.168.50.1: icmp_seq=2 ttl=255 time=4.16 ms
108 bytes from 192.168.50.1: icmp_seq=3 ttl=255 time=3.42 ms
108 bytes from 192.168.50.1: icmp_seq=4 ttl=255 time=4.69 ms
108 bytes from 192.168.50.1: icmp_seq=5 ttl=255 time=4.57 ms

--- 192.168.50.1 ping statistics ---
5 packets transmitted, 4 received, 20% packet loss, time 4068ms
rtt min/avg/max/mdev = 3.418/4.209/4.688/0.496 ms
```
```go
SW2L3# ping6 2001:db8:a:50::1 vrf mgmt            
PING 2001:db8:a:50::1(2001:db8:a:50::1) 100 data bytes
108 bytes from 2001:db8:a:50::1: icmp_seq=1 ttl=64 time=13.5 ms
108 bytes from 2001:db8:a:50::1: icmp_seq=2 ttl=64 time=3.45 ms
108 bytes from 2001:db8:a:50::1: icmp_seq=3 ttl=64 time=4.90 ms
108 bytes from 2001:db8:a:50::1: icmp_seq=4 ttl=64 time=3.83 ms
108 bytes from 2001:db8:a:50::1: icmp_seq=5 ttl=64 time=4.92 ms

--- 2001:db8:a:50::1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 3.451/6.115/13.482/3.728 ms
```
## Configuración DNS en Aruba CX
```go
SW2L3(config)# ip dns server-address 192.168.50.10 vrf mgmt 
SW2L3(config)# ip dns domain-name omnisys.net vrf mgmt
```
## Ping Aruba CX -> Servidor NTP ub1
```go
SW2L3# ping ub1 vrf mgmt
PING ub1.omnisys.net (192.168.50.100) 100(128) bytes of data.
108 bytes from 192.168.50.100: icmp_seq=1 ttl=255 time=18.4 ms
108 bytes from 192.168.50.100: icmp_seq=2 ttl=255 time=5.36 ms
108 bytes from 192.168.50.100: icmp_seq=3 ttl=255 time=2.88 ms
108 bytes from 192.168.50.100: icmp_seq=4 ttl=255 time=3.09 ms
108 bytes from 192.168.50.100: icmp_seq=5 ttl=255 time=5.97 ms

--- ub1.omnisys.net ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 2.882/7.145/18.429/5.770 ms
```
## Configuración NTP en Aruba CX
```go
SW2L3(config)# clock timezone america/lima
SW2L3(config)# ntp server ub1 iburst version 4
SW2L3(config)# ntp vrf mgmt 
SW2L3(config)# ntp enable
## Comandos de verificación NTP 
SW2L3# show ntp associations 
----------------------------------------------------------------------
 ID            NAME          REMOTE          REF-ID ST LAST POLL REACH
----------------------------------------------------------------------
  1             ub1  192.168.50.100 162.159.200.123  4   42   64   327
----------------------------------------------------------------------
```