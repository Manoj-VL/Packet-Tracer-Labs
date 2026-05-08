# Lab 10 - Subnetting
## Objective 
-Apply subnetting practically in Cisco Packet Tracer for a specific scenario and to observe how subnetting separates the network connection

## Scenario
- **Given IP Address**: `192.168.1.0/24`
- **Task:** Create 4 subnets

## Topology

<img width="1920" height="1080" alt="Lab 10 Topology" src="https://github.com/user-attachments/assets/5608fcec-7187-4eb5-805d-2171bc92c384" />


## Step Used

### Part 1:Calculation of subnets


- Given IP address is `192.168.1.0/24` is a default class **C** address with 256 host
- To break it into 4 parts that is 4 subnets
- **/26** = 4 subnets (64 IPs each)

- Subnet mask : `255.255.255.192`


Each of the 4 Subnets IP ranges were given below

| No    | Network IP       | Broadcast IP      | First IP         | Last IP           |
| ----- | ---------------- | ----------------- | ---------------- | ----------------- |
| 1 | 192.168.1.0  | 192.168.1.63  | 192.168.1.1  | 192.168.1.62  |
| 2 | 192.168.1.64 | 192.168.1.127 | 192.168.1.65 | 192.168.1.126 |
| 3     | 192.168.1.128    | 192.168.1.191     | 192.168.1.129    | 192.168.1.190     |
| 4     | 192.168.1.192    | 192.168.1.255     | 192.168.1.193    | 192.168.1.254     |



1. For the scenario of `192.168.1.0/24` to have 4 subnets  with each block size of 64 
	1. IP address : `192.168.1.0/26`
	2. Subnet mask: `255.255.255.192`
2. The IP ranges are shown in the table above with Network, Broadcast, First & Last Usable IP ranges shown
3. From the 4 subnets created, selected `192.168.1.0/26 and 192.168.1.64/26`  were chosen for the lab 
	- **Subnet 1** -`192.168.1.0  to 192.168.1.63
	- **Subnet 2** - `192.168.1.64 to 192.168.1.127
### Part 2 : Configuring Devices
1. Based on the subnet selection configured the end point devices and router with IP address shown below

Assigning IP values from the  selected subnet ranges to all the network devices

| Device | IP Address      | Default Gateway |
| ------ | --------------- | --------------- |
| PC1    | 192.168.1.10/26 | 192.168.1.1     |
| PC2    | 192.168.1.20/26 | 192.168.1.1     |
| PC3    | 192.168.1.70/26 | 192.168.1.65    |
| PC4    | 192.168.1.80/26 | 192.168.1.65    |

**Router1**

| Port                 | IP Address   | Subnet mask     |
| -------------------- | ------------ | --------------- |
| Gigabit Ethernet 0/0 | 192.168.1.1  | 255.255.255.192 |
| Gigabit Ethernet 0/1 | 192.168.1.65 | 255.255.255.192 |
*Rule of thumb: For each subnet, router must use 1st usable IP available for that particular subnet*

2. Both Network and Broadcast IP were left unassigned
3. IP address for PC's were randomly selected and Router IP was selected from the first usable IP range for each subnet

### Part 3 -Verification

1. Ping from PC 1 to PC 2 - Successful (Same Subnet)
2. Ping from PC 1 to PC3 - Successful (Different Subnet)

- To confirm that inter-subnet communication depends on the router, 
  the router interface was disabled and the ping was repeated — 
  proving that without a Layer 3 device, devices on different 
  subnets cannot communicate.

3. Ping from PC 1 to PC3 with router disabled - Unsuccessful

### Screenshots

### Ping from PC 1 to PC2
<img width="1920" height="1080" alt="Ping PC1 - PC2  Same Subnet" src="https://github.com/user-attachments/assets/f02f909f-e819-44f7-8e7d-eb744ecb45e3" />

### Ping from PC 1 to PC 3
<img width="1920" height="1080" alt="Ping PC1 to PC3  Differnet Subnet" src="https://github.com/user-attachments/assets/ec3da651-2b7b-4495-ab72-2a28892c5b29" />

### Ping from PC1 to PC 3 (Disabled Router)
<img width="1920" height="1080" alt="Ping PC1 to PC3  Different Subnet , Disabled Router" src="https://github.com/user-attachments/assets/1876492c-7614-449f-9ac6-eca21649f93c" />



## Problems I Hit
- Was confused about assigning Routers IP address - randomly or special rule
- Googled it - Found the rule of thumb that first usable IP from the subnets range should be selected. 
-  Rule of thumb is used because it is predictable and consistent across all subnets, making troubleshooting faster."


## Observations
- Subnetting divides larger networks into smaller subnetworks - isolated, separate and segmented similar to VLAN
- Subnetting offers diverse options for creation of subnetworks like 2,4,8,16 etc. equal parts or can create variable size subnets using VLSM(Variable - Length Subnet Mask )
- VLANs provide logical separation at Layer 2 and require inter-VLAN routing to 
  communicate. Subnetting provides logical separation at Layer 3 — both achieve 
  network segmentation but at different layers of the OSI model.

