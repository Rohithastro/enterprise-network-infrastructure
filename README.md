# Project 1 — Enterprise Network Infrastructure

## Goal

Build a simulated company campus with **3 routers, 4 switches, 4 VLANs, inter-VLAN routing, OSPF, DHCP and an ACL**. This project includes a topology, port map, IP plan, and IOS-style startup configurations for every device.

## Simulator and images

Use GNS3 with Cisco IOSv routers and IOSvL2 switches, or equivalent lab images that support 802.1Q, OSPF and DHCP. Network OS images are licensed by their vendors and are not included. Use only images you are authorized to use. The configs assume IOS-style interface names; adjust names to match your image.

Create seven nodes and connect the interfaces exactly as in `topology.md`. Paste each matching file from `configs/` into that device's console. Save with `copy running-config startup-config`.

## What to demonstrate

1. Verify VLAN/access/trunk state on all switches.
2. Verify clients in each VLAN receive DHCP addresses and their `.1` gateway.
3. Ping between VLANs to demonstrate router-on-a-stick inter-VLAN routing.
4. Show OSPF neighbors and routes across all three routers.
5. Show that the Guest VLAN cannot reach private company address space while permitted non-private destinations are allowed by the guest ACL.
6. Shut one router link in the simulator; observe OSPF reconvergence and restore it.

Commands, addressing plan and expected results are in `topology.md`. The lab has no physical or production links.

