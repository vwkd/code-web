---
title: Tailscale
author: vwkd
index: 1
tags:
  - the-web
---

- mesh VPN
- point-to-point connections, connects everything to everything else
- private, end-to-end encrypted
- overlay network, in parallel to physical network
- can think of like private internet, big encrypted LAN
software-defined network

note:
no unencrypted traffic over LAN
only routes internal traffic through VPN by default, can use exit node
routes only `100.x.y.z` addresses and subnets by default
traffic between two devices on the same LAN does not leave that LAN



## VPN

- built on WireGuard, lightweight, efficient, reliable
- everything gets static IP address `100.x.y.z` and human-readable DNS name `hostname`
- endpoints scale quadratically with number of nodes, since everyone with everyone, $n \cdot (n-1)$ endpoints for $n$ nodes
- over UDP and fancyports, except when blocked over TCP relays with HTTPS
- uses coordination server



## Coordination server

- central server by Tailscale
- manages access control of nodes
- distributes keys to every node, shared drop box for public keys
- distributes firewall to every node for incoming connections at decryption time
- drop box for logs from each node, asynchronously streamed



## Resources

- [How Tailscale works](https://tailscale.com/blog/how-tailscale-works)
