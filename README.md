# Segment Routing Lab: SRv6 l3VPN Demo

This lab is showing a simple configuration and verification of SRv6 on Nokia routers to signal both IGP’s shortest path and Algorithms.

All router network interfaces for transport are <b>using IPv6 and IPv4</b>

## Network Setup

See topology on the next image:

![Segment Routing SRvv6 l3vpn demo Containlerlab no ISIS Path](images/mau-rojas-nokia-srv6-sros-containerlab-l3vpn-demo-with-no-isis-path.png)

Next image shows the same with one option of the shortest path using ISIS.
![Segment Routing SRvv6 l3vpn demo Containlerlab with shortest ISIS Path](images/mau-rojas-nokia-srv6-sros-containerlab-l3vpn-demo-with-isis-path.png)

## Requeriments
Versions used are:
* containerlab 0.25.1
* vr-sros 22.5.R2 (requires license)

SROS image was created using [VR Network Lab](https://github.com/vrnetlab/vrnetlab)
IMPORTANT: vr-sos must be set as an image in docker to be pull directly by containerlab
```
# docker images | grep vr-sros
vr-sros                               22.5.R2                         f33cd7a3732a        3 months ago        965MB
```

## Overview



If you are new with container lab, we recommed to check the post regading [Nokia SROS in the manual](https://containerlab.dev/manual/kinds/vr-sros/)


## Setting the lab

Create Bridges to use for delay testing and trigger Algorithm 128.

```
brctl addbr R2-R4
brctl addbr R3-R5
ip link set R2-R4 up
ip link set R3-R5 up
```

Use containerlab a specified topology
```
clab deploy --topo topo.yml
```
Checking elements in the lab
```
# clab inspect --topo topo.yml
```
Preping origin and destination containers:
```
docker exec -i clab-srv6-demo-destination1 sh -s < destination1_config.sh
docker exec -i clab-srv6-demo-origin1 sh -s < origin1_config.sh
```
Test connectivity (origin is 10.1.4.101 and destination is using 10.6.4.101)
```
# docker exec -ti clab-srv6-demo-origin1 ping 10.6.4.101
PING 10.6.4.101 (10.6.4.101): 56 data bytes
64 bytes from 10.6.4.101: seq=0 ttl=60 time=11.766 ms
64 bytes from 10.6.4.101: seq=1 ttl=60 time=8.953 ms
64 bytes from 10.6.4.101: seq=2 ttl=60 time=8.865 ms
64 bytes from 10.6.4.101: seq=3 ttl=60 time=8.695 ms
```
