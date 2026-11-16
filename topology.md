# Enterprise Campus Lab Topology

## Device and port map

| Link | Side A | Side B |
|---|---|---|
| VLAN 10 access | SW1 `Gi0/1-20` | Clients: 10.10.10.0/24 |
| SW1 trunk | SW1 `Gi0/0` | R1 `Gi0/0` |
| VLAN 20 access | SW2 `Gi0/1-20` | Clients: 10.10.20.0/24 |
| SW2 trunk | SW2 `Gi0/0` | R1 `Gi0/1` |
| VLAN 30 access | SW3 `Gi0/1-20` | Clients: 10.10.30.0/24 |
| SW3 trunk | SW3 `Gi0/0` | R1 `Gi0/2` |
| VLAN 40 access | SW4 `Gi0/1-20` | Guest clients: 10.10.40.0/24 |
| SW4 trunk | SW4 `Gi0/0` | R1 `Gi0/3` |
| OSPF | R1 `Gi0/4` 10.0.12.1/30 | R2 `Gi0/0` 10.0.12.2/30 |
| OSPF | R2 `Gi0/1` 10.0.23.1/30 | R3 `Gi0/0` 10.0.23.2/30 |
| OSPF | R3 `Gi0/1` 10.0.13.2/30 | R1 `Gi0/5` 10.0.13.1/30 |

```text
                              R2
                         /          \
                        /            \
             R1 ---------------------- R3
          /   |   |   \
        SW1  SW2 SW3  SW4
        V10  V20 V30  V40 (Guest)
```

Each access switch has one access VLAN and a trunk to one R1 subinterface. R1 performs router-on-a-stick routing and DHCP. The three routers form a triangle and advertise their connected networks with OSPF process 10, area 0.

## Addressing and policy

| VLAN | Purpose | Network | Gateway | DHCP range |
|---|---|---|---|---|
| 10 | Corporate users | 10.10.10.0/24 | 10.10.10.1 | .50–.200 |
| 20 | Engineering | 10.10.20.0/24 | 10.10.20.1 | .50–.200 |
| 30 | Operations | 10.10.30.0/24 | 10.10.30.1 | .50–.200 |
| 40 | Guest | 10.10.40.0/24 | 10.10.40.1 | .50–.200 |

The Guest inbound ACL denies RFC1918 destinations, then permits other IPv4 traffic. The simulated topology has no Internet exit, so that final permit demonstrates policy intent only; it does not create Internet access.

## Validation commands

On switches: `show vlan brief`, `show interfaces trunk`, `show interfaces status`.

On R1: `show ip interface brief`, `show ip dhcp binding`, `show access-lists`, `show ip ospf neighbor`, `show ip route ospf`.

On R2/R3: `show ip ospf neighbor`, `show ip route ospf`, `ping <remote-router-link-address>`.

Expected: two OSPF neighbors per router when all links are up; all four VLAN subnets appear in the routing domain; DHCP clients receive the right subnet and gateway; Guest cannot ping 10.10.10.1, 10.10.20.1, 10.10.30.1 or router transit addresses.

## Troubleshooting path

Check physical/link state → switch access VLAN and trunk allowed/native VLAN → R1 subinterface encapsulation/address → DHCP pool and excluded range → OSPF adjacency → route table → ACL counters. Record evidence before changing configuration.
