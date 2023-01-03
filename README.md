# Segment Routing Lab: SRv6 Demo
# WORK IN PROGRESS: DO NOT USE IT!!

This lab is showing a simple configuration and verification of SR on Nokia routers to signal both IGP’s shortest path and TE LSPs similar to the capabilities of LDP and RSVP-TE MPLS transport label signaling protocols respectively. SR’s built-in capability for Fast ReRoute (FRR) using Loop-Free Alternate (LFA) is also shown

All router network interfaces for transport are <b>using IPv6</b>

## Network Setup

See topology on the next image:

![Segment Routing ISIS TE LSP Path Containlerlab](images/segment-routing-isis-te-lsp-path-lab-containerlab-sros-nokia-model-driven-mdm-ipv6.png)


We created two ePipes using different configurations
* non-TE LSP — R1 — R2 — R4 (i.e., the ISIS’ shortest path)
* TE LSP — R1 — R3 — R2 — R4 (i.e., the desired TE network path by the carrier for the ePipe service)

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

This lab is showing a simple configuration and verification of SR on Nokia routers to signal both IGP’s shortest path and TE LSPs similar to the capabilities of LDP and RSVP-TE MPLS transport label signaling protocols respectively. SR’s built-in capability for Fast ReRoute (FRR) using Loop-Free Alternate (LFA) is also shown

Lab built with containerlab to test sr-mpls using sr-isis.
<b>All network and system interfaces are IPv6 only.</b>

An ePipe 100 is setup between R1 and R4 so that ORG1 and DEST1 connected to the Service Access Ports (SAPs) of the ePipe 100 can communicate with each other as the two PCs are on the same IP subnet of 10.10.10.0/24. Another ePipe 101 was created via TE LSP via a longer path between R1-R2-R3-R4 routers. Traffic Engineering (TE) is the capability where the path of each LSP can be manually defined hop-by-hop or signalled based on meeting certain QoS requirements instead of just following the IGP’s shortest path as in the case of LDP. 

More details of SR-MPLS, TE-LSP and Fast ReRoute (FRR) capability by pre-computing Loop-Free Alternate (LFA) paths, and other technologies used on this demo, check post: [Segment Routing with Nokia 7750 on GNS3 by Derek Cheung](https://derekcheung.medium.com/segment-routing-b69f6ea2e3f5)

If you are new with container lab, we recommed to check the post regading [Nokia SROS in the manual](https://containerlab.dev/manual/kinds/vr-sros/)


## Setting the lab

Create Bridges:

```
brctl addbr VSR-NRC
brctl addbr VPN-BR1-R1
brctl addbr VPN-BR6-R6
brctl addbr R2-R4
brctl addbr R3-R5
```

Use containerlab a specified topology
```
clab deploy --topo topo.yml
```
Checking elements in the lab
```
# clab inspect --topo topo.yml

