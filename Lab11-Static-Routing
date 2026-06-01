# Lab 11 - Static Routing 

## Objective
- Configure all static routes on every router with router 2 central backbone router and verify full end to end connectivity across all networks

## Topology
(screenshot)

## Scenario
- Given four routers in hub and spoke topology  - 1 backbone router connected to 3 router each serving separate LAN segments -5 LAN's
- Configure all routers to achieve interconnectivity between the various subnets and access to internet connection via public IP address

## Steps Used
### Part 1: Initial Configuration
- Statically assigned the IP address , default gateway, subnet mask for the PC in each subnet

| Device | IP address   | Subnet Mask   | Default Gateway |
| ------ | ------------ | ------------- | --------------- |
| PC 1   | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| PC 2   | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| PC 3   | 172.16.1.10  | 255.255.255.0 | 172.16.1.1      |
| PC 4   | 172.16.2.10  | 255.255.255.0 | 172.16.2.1      |
| PC 5   | 172.16.3.10  | 255.255.255.0 | 172.16.3.1      |

- Configured each router interface which are directly connected to their respective LAN by assigning IP address to each interface with CLI command
- For Router1 (assigning IP to interface Gigabit Ethernet 0/0)

```
Router>enable
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gig0/1 #Interface Selection
Router(config-if)#ip address 192.168.1.1 255.255.255.0 #Assigning IP and Subnet mask
Router(config-if)no shutdown #Enabling the interface 
- 
```
  - The above process was repeated for each router with interface connected to each LAN 


| Router   | Interface | IP address  | Subnet Mask     | Connection                            |
| -------- | --------- | ----------- | --------------- | ------------------------------------- |
| Router1  | Gig 0/1   | 192.168.1.1 | 255.255.255.0   | Directly connected to PC 1            |
| Router 1 | Gig 0/2   | 192.168.2.1 | 255.255.255.0   | Directly connected to PC 2            |
| Router 3 | Gig 0/1   | 172.16.1.1  | 255.255.255.0   | Directly connected to PC 3            |
| Router 3 | Gig 0/2   | 172.16.2.1  | 255.255.255.0   | Directly connected to PC 4            |
| Router 4 | Gig 0/1   | 172.16.3.1  | 255.255.255.0   | Directly connected to PC 5            |
| Router 4 | Gig 0/2   | 203.0.113.1 | 255.255.255.252 | Point to point connection to Internet |

### Part 2: Configuring the WAN Links

- Each router to router connection was point to point connection
	- A /30 subnet mask (255.255.255.252) was chosen for all point-to-point WAN links — each /30 provides 4 total addresses with 2 usable host addresses, which is the minimum required for a router-to-router link
- Used the above CLI command to configure the interface of each of the WAN Links 


| Router   | Interface | IP Address | Subnet mask     | Connection |
| -------- | --------- | ---------- | --------------- | ---------- |
| Router 1 | Gig 0/0   | 10.0.0.1   | 255.255.255.252 | Router 2   |
| Router 2 | Gig 0/0   | 10.0.0.2   | 255.255.255.252 | Router 1   |
| Router 2 | Gig 0/1   | 10.0.0.5   | 255.255.255.252 | Router 3   |
| Router 2 | Gig 0/2   | 10.0.0.9   | 255.255.255.252 | Router 4   |
| Router 3 | Gig 0/0   | 10.0.0.6   | 255.255.255.252 | Router 2   |
| Router 4 | Gig 0/0   | 10.0.0.10  | 255.255.255.252 | Router 2   |


### Part 3: Static Routing

- In order for proper interconnection between all the LAN Connections as well as proper internet connectivity - static routing is needed
- Used the CLI command for each router to individually assign static routes

#### Router 2

- For router 2 which is the backbone router - requires individual separate routes to each LAN connections and default route was set to internet
```
Router(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1 #route to PC1 and the hop
Router(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.1 
Router(config)# ip route 172.16.1.0 255.255.255.0 10.0.0.6
Router(config)# ip route 172.16.2.0 255.255.255.0 10.0.0.6
Router(config)# ip route 172.16.3.0 255.255.255.0 10.0.0.10
Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.10 #default route to internet 
```
(screenshot) IP interface brief
(screenshot) Routing table router 2
#### Router 3&1

- For Router 3,1 -Based on the topology there is only one  path to take to get to any other network other than the directly connections so default route was set to optimize to have only a default path which points towards the backbone routers interface
```
*Router 1*
Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2

*Router 3*
Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.5
```
(screenshot) Routing table router 3
(screenshot) Routing table router 1
#### Router 4

- For Router 4 -  cannot assign a default path like  in the case of router 3 and 1 it has an internal connection going towards the backbone router and external connection to the internet
- For external connections we can assign just assign a default path to internet IP address
```
Router(config)#ip route 0.0.0.0 0.0.0.0 203.0.113.2
```
- For the internal connection - if we assign specific routes it will be 4 different routes plus the 2 point to point connection that  we will be 6 routes
- Configuring six individual static routes on Router4 is functional but inefficient — route summarization reduces this to two entries, minimizing routing table size and forwarding overhead
#### Route Summarization for Router 4
###### Left
Network          1st Octet      2nd Octet   3rd Octet
192.168.1.0  ->  11000000 . 10101000 . 000000  01 . 00000000
192.168.2.0  ->  11000000 . 10101000 . 000000  10 . 00000000

- The 1st and 2nd octet are the same - 8+8 = 16 bits
- 3rd octet first 6 bits are the same -
- So 8+8+8 =22 bits 
- So  our new subnet mask `/22` which in Cisco standard `255.255.252.0`
- Range - 192.168.0.0 - 192.168.3.255
```
Router(config) # ip route 192.168.0.0 255.255.252.0 10.0.0.9
```
###### Right
Network          1st Octet   2nd Octet   3rd Octet
172.16.1.0   ->  10101100 . 00010000 . 000000 01 . 00000000
172.16.2.0   ->  10101100 . 00010000 . 000000 10 . 00000000

- The 1st and 2nd octet are the same - 8+8 = 16 bits
- 3rd octet first 6 bits are the same -
- So 8+8+8 =22 bits 
- So  our new subnet mask `/22` which in Cisco standard `255.255.252.0`
- Range - 172.16.0.0 - 172.16.3.255
- Note: In broader network designs, we can use an even wider `/21` or `255.255.248.0` mask here to leave room for future expansion up to `172.16.7.255`, but a `/22` is the tightest mathematical fit for just these two specific subnet

```
Router(config) # ip route 172.16.0.0 255.255.252.0 10.0.0.9
```

**(screenshot) Routing table router 4**
## Verification
- Ping from PC 1 to PC 2  (same router, different subnets ) - Successful 
(Screenshot)

- Ping from PC 1 to PC 3  (via backbone router) - Successful
(screenshot)

- Ping from PC 1 to PC 5  (multi hop) - Successful 
(screenshot)

- Ping from PC 3 to PC 4  (same router, different subnets) - Successful 
(screenshot)
- Ping from PC 5 to PC 1  (multi hop - reverse path) - Successful 
(screenshot)


## Observation
### 1. Routing Table Efficiency via Summarization (Supernetting)

- **Observation:** Instead of configuring Router4 with six separate static routes to reach every individual internal subnet, route summarization was used to collapse them into two clean entries (`192.168.0.0/22` and `172.16.0.0/22`).
    
- **Significance:** This drastically reduces the size of Router4’s routing table. In production environments, smaller routing tables conserve router memory (RAM) and reduce CPU overhead, because the router has fewer rows to scan before forwarding a packet.
    

### 2. Validation of the Longest Prefix Match (LPM) Rule

- **Observation:** Even though Router4 is directly connected to `172.16.3.0/24`, it safely uses a summarized route of `172.16.0.0/22` pointing up to the Backbone for other traffic.
    
- **Significance:** This proves the operation of the Longest Prefix Match rule. When traffic is destined for a local user on Switch5, the router matches the more specific `/24` connected route over the broader `/22` static route, preventing local traffic from being accidentally forwarded to the backbone.
    

### 3. Optimization of Stub Routers

- **Observation:** Router1 and Router3 are "stub routers"—they only have a single physical path connecting them to the rest of the corporate network (via the Backbone).
    
- **Significance:** Configuring individual static routes on these routers is inefficient. Utilizing a single default route (0.0.0.0/0) pointing to the Backbone next-hop interface achieves 100% connectivity with minimal configuration overhead.
    

### 4. Edge Router Traffic Isolation and Loop Prevention

- **Observation:** Router4 serves as the corporate gateway to the external internet (`Cloud0`). Therefore, its default route (0.0.0.0/0) must point toward the ISP next-hop IP (`203.0.113.2`), rather than internal routers.
    
- **Significance:** If the default route pointed to the Backbone, creates a routing loop between Router4 and the Backbone, as neither router would have a more specific route to forward internet-bound traffic. Combining specific internal summaries with an external default route ensures proper traffic segregation between the corporate intranet and the public internet.
    
