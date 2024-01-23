Configuración EVPN
==================

router bgp 65501
    bgp router-id 192.168.0.7
    neighbor 192.168.0.5 remote-as 65501
    neighbor 192.168.0.5 update-source loopback 0
    neighbor 192.168.0.6 remote-as 65501
    neighbor 192.168.0.6 update-source loopback 0
    address-family l2vpn evpn
        neighbor 192.168.0.5 activate
        neighbor 192.168.0.5 send-community extended
        neighbor 192.168.0.6 activate
        neighbor 192.168.0.6 send-community extended
    exit-address-family

evpn
    vlan 10
        rd auto
        route-target export auto
        route-target import auto
    vlan 20
        rd auto                                                
        route-target export auto
        route-target import auto

Configuración VXLAN
===================

interface vxlan 1
    source ip 192.168.0.7
    no shutdown
    vni 1010
        vlan 10                                                
    vni 1020
        vlan 20

