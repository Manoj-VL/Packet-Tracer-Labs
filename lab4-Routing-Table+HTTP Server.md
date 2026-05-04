# Lab 4 Routing Table + HTTP Server

## Objective
- To understand how routers operate at layer 3 examining the routing table and observing how an HTTP server communicates with clients across different subnets.

### Topology
<img width="1920" height="1080" alt="Topology Lab 4" src="https://github.com/user-attachments/assets/1e554513-c302-48d8-8774-52b5aca568ce" />

### Ip address Table 
| Device        | IP Address   | Subnet Mask   | Default Gateway |
|---------------|--------------|---------------|-----------------|
| PC1           | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| PC2           | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| PC3           | 192.168.2.20 | 255.255.255.0 | 192.168.2.1     |
| Server0       | 192.168.2.50 | 255.255.255.0 | 192.168.2.1     |
| PC0           | 192.168.3.10 | 255.255.255.0 | 192.168.3.1     |
| Router G0/0   | 192.168.1.1  | 255.255.255.0 | —               |
| Router G0/1   | 192.168.2.1  | 255.255.255.0 | —               |
| Router G0/2   | 192.168.3.1  | 255.255.255.0 | —               |

## Steps used
1. Extended the previous topology by adding a third subnet with PC0 and Switch 2 connected to router port gigabitEthernet 0/2, also connected a new server to switch 1.
2. The new server and the PC0 was statically assigned the ip address,subnetmasks,gateway
3. Configured the router port for the new subnet added

```
enable
configure terminal
interface gigabitEthernet0/2
ip address 192.168.3.1 255.255.255.0
no shutdown
exit
```
4. Ran `show ip route` to view the routing table and verify all three subnets were registered.
5. Enabled HTTP service on Server0:
   - Double-clicked Server0 → Services tab → HTTP → ON
6. Accessed the server from PC1 using the web browser:
   - Desktop → Web Browser → entered `192.168.2.50` → page loaded successfully.
7. Used simulation mode to watch the packet closely as it travelled through the router
## Verification
### Routing Table
<img width="1920" height="1080" alt="Routting Table" src="https://github.com/user-attachments/assets/3e020b1b-929c-43f6-98d8-e4904dd818b1" />


### HTTP Access from PC1
<img width="1920" height="1080" alt="Webpage" src="https://github.com/user-attachments/assets/8fb36c71-7ae3-4bcb-8db8-a322ff8a33f0" />


### Ping Tests
Ping from PC1 to Server0:
<img width="1920" height="1080" alt="Ping from Pc 1 to Pc 4" src="https://github.com/user-attachments/assets/401e4516-a361-48f6-8d25-8335c4320f3f" />

## Observation
-- The routing table is automatically populated when router interfaces are configured 
  and enabled — no manual route entries were needed for directly connected subnets.
- Each entry in the routing table shows the subnet, the interface it is reachable through, 
  and whether it is a directly connected (C) or learned route.
- A server is just an endpoint with a service enabled — it uses the same IP addressing 
  and gateway rules as any other device on the subnet.
- HTTP traffic crossed two subnets (PC1 on 192.168.1.0 → Server on 192.168.2.0), 
  with the router handling Layer 3 forwarding transparently.
- When a packet passes through a router, the MAC address changes at each hop — the router 
  strips the incoming frame and builds a new one with its own MAC as the source.
- This confirms that MAC addresses are local to each subnet (Layer 2), while IP addresses 
  carry the end-to-end destination (Layer 3) — the router bridges the two layers.
