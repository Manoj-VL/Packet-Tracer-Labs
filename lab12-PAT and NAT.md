# Lab 12 - NAT & PAT (Network Address Translation & Port Address Translation)

## Objective
- Configure  PAT on a Edge router so - PC's in our internal network can connect to internet via a single  public IP address using PAT
## Topology
<img width="1920" height="1080" alt="Topology - Lab 11" src="https://github.com/user-attachments/assets/60980f8f-7fb4-4262-b548-ff2cb78ee201" />

## Scenario
- Similar to the topology in Lab 11 - 1 backbone router connected to 3 routers each serving 5 separate LAN's  segments
- Router 4 is the edge router - the gateway to internet
- To simulate a external network - wired connection to ISP  router was introduced with another PC simulating an external server
- Static routing from Lab 11 was retained as the base configuration

## Steps Used

### Part 1 : Initial Configuration
- IP configuration that was assigned in lab 11 was also retained

| Device         | IP address   | Subnet Mask   | Default Gateway |
| -------------- | ------------ | ------------- | --------------- |
| PC 1           | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| PC 2           | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| PC 3           | 172.16.1.10  | 255.255.255.0 | 172.16.1.1      |
| PC 4           | 172.16.2.10  | 255.255.255.0 | 172.16.2.1      |
| PC 5           | 172.16.3.10  | 255.255.255.0 | 172.16.3.1      |
| ISP-PC(Server) | 8.8.8.8      | 255.255.255.0 | 8.8.8.1         |

- New entry was added to router 4 to address the interface (Gig0/2)  which is ther outside connection  to the ISP Router

| Router       | Interface   | IP address      | Subnet Mask         | Connection                                  |
| ------------ | ----------- | --------------- | ------------------- | ------------------------------------------- |
| Router1      | Gig 0/1     | 192.168.1.1     | 255.255.255.0       | Directly connected to PC 1                  |
| Router 1     | Gig 0/2     | 192.168.2.1     | 255.255.255.0       | Directly connected to PC 2                  |
| Router 3     | Gig 0/1     | 172.16.1.1      | 255.255.255.0       | Directly connected to PC 3                  |
| Router 3     | Gig 0/2     | 172.16.2.1      | 255.255.255.0       | Directly connected to PC 4                  |
| Router 4     | Gig 0/1     | 172.16.3.1      | 255.255.255.0       | Directly connected to PC 5                  |
| **Router 4** | **Gig 0/2** | **203.0.113.1** | **255.255.255.252** | **Point to point connection to ISP Router** |
| ISP Router   | Gig0/0      | 203.0.113.2     | 255.255.255.252     | Point to point connection to Edge router    |
| ISP Router   | Gig0/1      | 8.8.8.1         | 255.255.255.0       | Connect ISP Router to  ISP Server           |

- WAN Links were also retained as  part of the base configuration

| Router   | Interface | IP Address | Subnet mask     | Connection |
| -------- | --------- | ---------- | --------------- | ---------- |
| Router 1 | Gig 0/0   | 10.0.0.1   | 255.255.255.252 | Router 2   |
| Router 2 | Gig 0/0   | 10.0.0.2   | 255.255.255.252 | Router 1   |
| Router 2 | Gig 0/1   | 10.0.0.5   | 255.255.255.252 | Router 3   |
| Router 2 | Gig 0/2   | 10.0.0.9   | 255.255.255.252 | Router 4   |
| Router 3 | Gig 0/0   | 10.0.0.6   | 255.255.255.252 | Router 2   |
| Router 4 | Gig 0/0   | 10.0.0.10  | 255.255.255.252 | Router 2   |

### Part 2 : Configuring PAT on the edge router

- To properly connect to the ISP router - need to configure PAT on the edge router so that all the LAN segments can ensure connectivity to external network

#### Part 2.1 : Tell the router which interface are inside and outside

-  First the edge router(router 4) needs to configured so that it knows which interface is inside the network and which is outside the network

**Router 4 CLI**
**Inside Interface**
```
Router(config)# interface Gig0/0
Router(config-if)# ip nat inside #assigns the interface as inside 

Router(config)# interface Gig0/1
Router(config-if)# ip nat inside #assigns the interface as inside 
```

**Outside Interface**
```
Router(config)# interface Gig0/2
Router(config-if)# ip nat outside #assigns the interface as outside 
```

#### Part 2.2 Tell the router which internal IP's should be translated

- To tell the the router which internal IP's should be translated - **Access List** is defined

**Router 4 CLI**
```
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255 
Router(config)#access-list 1 permit 192.168.2.0 0.0.0.255
Router(config)#access-list 1 permit 172.16.1.0 0.0.0.255
Router(config)#access-list 1 permit 172.16.2.0 0.0.0.255
Router(config)#access-list 1 permit 172.16.3.0 0.0.0.255
```
- The above command show  an access list with the private IP of all the PC's in our internal network that should be translated to a  public IP when it reaches the Router 4's interface
**Notice: The access list uses a wild card subnet mask opposite of the subnet mask**

##### Show ip access-list
<img width="1920" height="1080" alt="Access - Lists" src="https://github.com/user-attachments/assets/0ebde26a-e2f0-4c6c-bf96-c3eb5e45d2b8" />


#### Part 2.3 Link the access list - to outside interface

- In above step the access list was only defined - need to link the access list with the outside interface so that the PAT translation can occur 

**Router 4 CLI**
```
ip nat inside source list 1
interface Gig0/2
overload
```

- The above command takes all traffic matching access list 1 
- Translate the source IP to Gig0/2 IP which is `203.0.113.1`
- **Overload** keyword - PAT (Number of private IP to one public IP using ports for tracking)


### Part 3 : Verification

- PC1 Pings 8.8.8.8(server outside the internal network via ISP router)
**Screenshot - Ping**
  <img width="1920" height="1080" alt="PC1 Ping - 8 8 8 8" src="https://github.com/user-attachments/assets/c78f3ee4-c154-4138-936a-c64c3d0064dc" />

**Screenshot NAT Translation Table**
<img width="1920" height="1080" alt="PC1 - 8 8 8 8 IP NAT Translation" src="https://github.com/user-attachments/assets/d73b9aed-1405-41cb-8f88-308f52d93695" />


- PC 3 Pings 8.8.8.8(server outside the internal network via ISP router)
**Screenshot NAT Translation Table**
<img width="1920" height="1080" alt="PC3 - 8 8 8 8 IP NAT Translations" src="https://github.com/user-attachments/assets/1a2af1af-2caf-422d-86ae-4d6958a76323" />


## Observation

1. PAT allows multiple devices across different internal networks to share a single public IP address Each session is tracked using unique port numbers
2. The NAT translation table confirmed that PC1 (192.168.1.10) and PC3 (172.16.1.10) both translated to the same public IP 203.0.113.1 with different port numbers assigned to each session
3. Without the correct access list PAT will not function even if inside and outside interfaces are correctly configured The access list defines exactly which traffic gets translated
4. The overload keyword is what distinguishes PAT from NAT Without overload only one inside device can use the public IP at a time With overload unlimited sessions share the same public IP
5. Return traffic is handled automatically by the router using the port number in the translation table to identify which internal device the response belongs to

**screenshot(show ip nat statistics)**
<img width="1920" height="1080" alt="IP NAT Statistics" src="https://github.com/user-attachments/assets/214576c8-ac04-4330-bc0e-99254c23a077" />
