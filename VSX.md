## Topología
![](image.png)
## Configuración ISL
**CX-1**
```go
interface lag 254
    description ISL
    no shutdown
    no routing
    vlan trunk native 1
    vlan trunk allowed all
    lacp mode active

interface 1/1/5
    description CNX_CX-2-PORT_1/1/5
    no shutdown
    lag 254
interface 1/1/6
    description CNX_CX-2-PORT_1/1/6
    no shutdown
    lag 254
```
**CX-2**
```go
interface lag 254
    description ISL
    no shutdown
    no routing
    vlan trunk native 1
    vlan trunk allowed all
    lacp mode active

interface 1/1/5
    description CNX_CX-1-PORT_1/1/5
    no shutdown
    lag 254
interface 1/1/6
    description CNX_CX-1-PORT_1/1/6
    no shutdown
    lag 254    
```
## Configuración KEEPALIVE
**CX-1**
```go
vrf KA

interface 1/1/7
    description KeepAlive
    no shutdown
    vrf attach KA
    ip address 10.0.254.1/30
```
**CX-2**
```go
vrf KA

interface 1/1/7
    description KeepAlive
    no shutdown
    vrf attach KA
    ip address 10.0.254.2/30
```
## Configuración VSX
**CX-1**
```go
vsx
    system-mac 02:01:00:00:fe:01
    inter-switch-link lag 254
    role primary
    keepalive peer 10.0.254.2 source 10.0.254.1 vrf KA
    vsx-sync vsx-global
```
**CX-2**
```go
vsx
    inter-switch-link lag 254
    role secondary
    keepalive peer 10.0.254.1 source 10.0.254.2 vrf KA  
```
## Configuración VLANS
**CX-1**
```go
vlan 10
    vsx-sync
vlan 20
    vsx-sync
```
##Configuración MCLAG
**CX-1**
```go
interface lag 1 multi-chassis
    description CNX_CX-3-LAG1
    no shutdown
    no routing
    vlan trunk native 1
    vlan trunk allowed all
    lacp mode active
interface lag 2 multi-chassis
    description CNX_CX-4_LAG2
    no shutdown
    no routing
    vlan trunk native 1
    vlan trunk allowed all
    lacp mode active

interface 1/1/1                                                
    description CNX_CX-3-PORT_1/1/1
    no shutdown
    lag 1
interface 1/1/2
    description CNX_CX-4-PORT_1/1/1
    no shutdown
    lag 2
```
**CX-2**
```go
interface lag 1 multi-chassis
    description CNX_CX-3_LAG1
    no shutdown
    no routing
    vlan trunk native 1
    vlan trunk allowed all
    lacp mode active
interface lag 2 multi-chassis
    description CNX_CX-4-LAG2
    no shutdown
    no routing
    vlan trunk native 1
    vlan trunk allowed all
    lacp mode active

interface 1/1/1                                                
    description CNX_CX-3_PORT_1/1/2
    no shutdown
    lag 1
interface 1/1/2
    description CNX_CX-4-PORT_1/1/2
    no shutdown
    lag 2
```
## Configuración SVI
**CX-1**
```go
interface vlan 10
    vsx-sync active-gateways
    ip address 192.168.10.2/24                                 
    active-gateway ip mac 12:01:00:00:fe:10
    active-gateway ip 192.168.10.1
interface vlan 20
    vsx-sync active-gateways
    ip address 192.168.20.2/24
    active-gateway ip mac 12:01:00:00:fe:20
    active-gateway ip 192.168.20.1
```
**CX-2**
```go
interface vlan 10
    ip address 192.168.10.3/24                                 
interface vlan 20
    ip address 192.168.20.3/24
```