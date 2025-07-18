# (IA)NUG Summer 2025 Containerlab Demos
This repository contains the demos used for the (IA)NUG Summer 2025 event. The repository contains five labs:

1. [srl_lab](#srl-lab)
2. [multivendor_lab](#multivendor-lab)
3. [frr_lab](#frr-lab)
4. [srl-telemetry-lab](#srl-telemetry-lab)

Further documentation for Containerlab may be found on the [official Containerlab website](https://containerlab.dev/).


## SRL Lab
This lab has three routers doing OSPF peering between each other and linux hosts attached to the routers.

![SRL Lab](srl_lab/srl_lab.clab.drawio.svg)

You should be able to ping between the linux hosts when fully up.


Ping from host1 to host2:

```
/ # ping -c 5 192.168.20.1
PING 192.168.20.1 (192.168.20.1): 56 data bytes
64 bytes from 192.168.20.1: seq=0 ttl=61 time=1.097 ms
64 bytes from 192.168.20.1: seq=1 ttl=61 time=1.184 ms
64 bytes from 192.168.20.1: seq=2 ttl=61 time=1.040 ms
64 bytes from 192.168.20.1: seq=3 ttl=61 time=1.008 ms
64 bytes from 192.168.20.1: seq=4 ttl=61 time=1.105 ms

--- 192.168.20.1 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 1.008/1.086/1.184 ms
```

## Multivendor Lab
The multivendor lab has the following nodes:

1. Nokia SR Linux Spine
2. Arista cEOS Leaf
3. FRR Leaf

![Multivendor Lab](multivendor_lab/mvlab.clab.drawio.svg)

This lab is using eBGP to interconnect the spines and leafs. The Arista
leaf demonstrates a L3 SVI and access vlan to connect to host1.
The FRR node is using a routed port to connect to host2.

You should be able to ping between the linux hosts when fully up.


Ping from host1 to host2:

```
/ # ping -c 5 192.168.20.1
PING 192.168.20.1 (192.168.20.1): 56 data bytes
64 bytes from 192.168.20.1: seq=0 ttl=61 time=1.880 ms
64 bytes from 192.168.20.1: seq=1 ttl=61 time=1.840 ms
64 bytes from 192.168.20.1: seq=2 ttl=61 time=1.697 ms
64 bytes from 192.168.20.1: seq=3 ttl=61 time=1.546 ms
64 bytes from 192.168.20.1: seq=4 ttl=61 time=1.467 ms

--- 192.168.20.1 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 1.467/1.686/1.880 ms
```

## FRR Lab
This lab has three routers doing simple ebgp peering between each other.
Each router is advertising its loopback address in BGP.

![FRR Lab](frr_lab/frr_lab.clab.drawio.svg)

You should be able to ping between the linux hosts when fully up.


Routing table from spine1:

```
spine1# show ip route
Codes: K - kernel route, C - connected, L - local, S - static,
       R - RIP, O - OSPF, I - IS-IS, B - BGP, E - EIGRP, N - NHRP,
       T - Table, v - VNC, V - VNC-Direct, A - Babel, F - PBR,
       f - OpenFabric, t - Table-Direct,
       > - selected route, * - FIB route, q - queued, r - rejected, b - backup
       t - trapped, o - offload failure

IPv4 unicast VRF default:
K>* 0.0.0.0/0 [0/0] via 172.20.20.1, eth0, weight 1, 00:02:11
B>* 1.1.1.1/32 [20/0] via 10.1.1.2, eth1, weight 1, 00:02:09
B>* 2.2.2.2/32 [20/0] via 10.2.2.2, eth2, weight 1, 00:02:09
C>* 10.1.1.0/24 is directly connected, eth1, weight 1, 00:02:11
L>* 10.1.1.1/32 is directly connected, eth1, weight 1, 00:02:11
C>* 10.2.2.0/24 is directly connected, eth2, weight 1, 00:02:11
L>* 10.2.2.1/32 is directly connected, eth2, weight 1, 00:02:11
L * 100.100.100.100/32 is directly connected, lo, weight 1, 00:02:11
C>* 100.100.100.100/32 is directly connected, lo, weight 1, 00:02:11
C>* 172.20.20.0/24 is directly connected, eth0, weight 1, 00:02:11
L>* 172.20.20.4/32 is directly connected, eth0, weight 1, 00:02:11
```

BGP Summary from Spine1:

```
spine1# show bgp summary

IPv4 Unicast Summary:
BGP router identifier 100.100.100.100, local AS number 100 VRF default vrf-id 0
BGP table version 3
RIB entries 5, using 640 bytes of memory
Peers 2, using 33 KiB of memory

Neighbor        V         AS   MsgRcvd   MsgSent   TblVer  InQ OutQ  Up/Down State/PfxRcd   PfxSnt Desc
10.1.1.2        4          1         8         8        3    0    0 00:02:41            1        3 N/A
10.2.2.2        4          2         8         8        3    0    0 00:02:41            1        3 N/A

Total number of neighbors 2
```


BGP Table from Spine1:

```
spine1# show ip bgp
BGP table version is 3, local router ID is 100.100.100.100, vrf id 0
Default local pref 100, local AS 100
Status codes:  s suppressed, d damped, h history, u unsorted, * valid, > best, = multipath,
               i internal, r RIB-failure, S Stale, R Removed
Nexthop codes: @NNN nexthop's vrf id, < announce-nh-self
Origin codes:  i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  1.1.1.1/32       10.1.1.2                 0             0 1 i
 *>  2.2.2.2/32       10.2.2.2                 0             0 2 i
 *>  100.100.100.100/32
                    0.0.0.0                  0         32768 i

Displayed 3 routes and 3 total paths
```

## SRL Telemetry Lab
This lab is the same SRL Telemetry lab found [here](https://github.com/srl-labs/srl-telemetry-lab)
included as a great example of combining containers available in the larger Docker container
ecosystem with your labs.