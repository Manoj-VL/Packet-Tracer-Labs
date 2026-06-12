
# Lab 13 - DHCP & DNS

## Objective
- Configure a DHCP Server and DNS Server in Packet Tracer and verify the clients receive automatic IP configuration and can resolve domain names

## Topology
<img width="1920" height="1080" alt="Lab 13 - Topology" src="https://github.com/user-attachments/assets/9a179eb7-6cdb-4323-93f4-a22bc3051e7d" />

## Scenario
- The topology has 2 separate network - Client, Sever side
- Client's PC's has no IP configuration 
- Server subnet has 3 separate Severs -DNS, DHCP and Web Server

## Steps Used

### Part 1: Initial Configuration
- For the server side - IP address ,subnet mask, default gateway was statically assigned assigned

| Device      | IP Address   | Subnet Mask   | Default Gateway |
| ----------- | ------------ | ------------- | --------------- |
| DHCP Server | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| DNS Server  | 192.168.2.20 | 255.255.255.0 | 192.168.2.1     |
| Web Server  | 192.168.2.30 | 255.255.255.0 | 192.168.2.1     |
- Configured the router interface with IP address on both sides - Client & Server side

| Router  Interface | IP address  | Subnet mask   |
| ----------------- | ----------- | ------------- |
| Gig 0/0           | 192.168.1.1 | 255.255.255.0 |
| Gig 0/1           | 192.168.2.1 | 255.255.255.0 |

### Part 2.1 : Configuring DHCP Server

- Enabled the DHCP Services  on the Server - to function as a DHCP Sever
- Assigned the DHCP Scope with the parameters as below

| Parameters              |               | Remarks                                   |
| ----------------------- | ------------- | ----------------------------------------- |
| Pool Name               | Client Pool   |                                           |
| Default Gateway         | 192.168.1.1   | Client sides router  interface IP         |
| DNS Server              | 192.168.2.20  |                                           |
| Start IP Addres         | 192.168.1.2   |                                           |
| Subnet Mask             | 255.255.255.0 |                                           |
| Maximum Number of Users | 100           | Can be increased or decreased accordingly |
| Maximum lease time      | 24hrs         |                                           |

**192.168.1.1 is reserved for the router interface — DHCP pool starts at 192.168.1.2 to avoid conflict**

### Part 2.2 : Configuring Web Server

- Enabled the Http and Https on services tab on the server - to function as a Web Server
- Default page was served -The default Packet Tracer HTTP index page was served confirming HTTP functionality
- Both HTTP & HTTPS was enabled -Both services enabled to demonstrate that the server can handle both encrypted and unencrypted web traffic

### Part 2.3 : Configuring DNS Server

- Enabled the  DNS Services on the Server  - to function as a DNS Server
- Created A record  inside the DNS server - to map a domain name to IPv4 Address

| Name        | Type     | Details      |
| ----------- | -------- | ------------ |
| `test.site` | A Record | 192.168.2.30 |
- The A Record point the domain name `test.site` to the IP address of `192.168.2.30` which is the IP address of the Web Server
- An A record was chosen specifically -  because it maps a hostname directly to an IPv4 address

<img width="1920" height="1080" alt="DNS Server - A Record" src="https://github.com/user-attachments/assets/5cf54759-b538-4e2c-b8da-b971f940fa97" />

### Part 3 : Configuring the Router  as DHCP Relay Agent

- Since the client side PC's are in different subnet - Router needs to act as DHCP Relay Agent
- In order to configure the router into DHCP Relay Agent - `ip helper-address` is used
- Router client side interface needs to configured with `ip helper-address`
**Router CLI**
```
Router>enable
Router#config t
Router(config)#interface gig0/0
Router(config-if)#ip helper-address 192.168.2.10 # Fwd DHCP broadcast request as unicast to the server                                                          
Router(config-if)#exit
```


## Verification

1. Client PC's got  the IP Configuration automatically

<img width="1920" height="1080" alt="PC1  IP configuraton" src="https://github.com/user-attachments/assets/2bfa831a-c3b6-4af2-b047-f063752bc6ae" />
<img width="1920" height="1080" alt="PC2 IP configuration" src="https://github.com/user-attachments/assets/43287851-06e5-493a-8497-b154a4c1a1e3" />

2. Navigate to the web address `test.site` from  PC1's web browser  - Page Loaded successfully
<img width="1920" height="1080" alt="test site - Web page loaded" src="https://github.com/user-attachments/assets/9f6093b2-2df0-44fa-9965-39bc9cbf1bc5" />


3. Used simulation mode to watch the packet going through the DNS query and response  and DHCP automatically assigns IP Configuration
4. Ran `nslookup` on `test.site` from PC1 command prompt
<img width="1920" height="1080" alt="Nslookup - test site" src="https://github.com/user-attachments/assets/a60b7f1e-e719-4351-88a7-e1fa61d6026f" />


## Observation

1. Client Lan and Server Lan was in different  subnet but DHCP Server is still  able to  assign  the Client PC's  with an IP Configuration - router  acting as DHCP Relay agent using the `ip helper-address`

2. Client PC was able to load the web page`test.site` successfully -DNS resolved the specific domain name to an IP Address which points to the Web Server IP address

3. Default web page was loaded from the Web Server - Combined effort of DHCP & DNS Server DHCP assigning IP configuration to Client PC's which contain DNS Server IP and DNS Server mapping the domain name to IP of Web Server

4. Separating DHCP, DNS, and Web onto individual servers reflects enterprise design — each service can be managed, scaled, and secured independently 

5. The DORA process: "DHCP assignment follows the DORA process — Discover, Offer, Request, Acknowledge — visible in Packet Tracer simulation mode
