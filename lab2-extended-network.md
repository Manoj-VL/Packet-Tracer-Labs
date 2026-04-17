# Lab2 Extended Interconnected Network
# Objective
Configure an extended version of network based on the previous lab by adding an switch and another pc thus forming a star topology

## Topology
 PC1--> SWITCH1-->ROUTER-->SWITCH2-->PC2
                               | 
                               PC3

### Steps Used
1.From the lab1 topology,deleted the connection between the router and PC2 
2.Added 2nd switch and new endpoint PC3
3.Connections were made between the router to switch and switch to the 2 different PC
4.Configured the new pc with new ip address,subnetmask,default gateway

| DEVICE | IP ADDRESS | SUBNET MASK | DEFAULT GATEWAY|
|--------|------------|-------------|----------------|
| PC1    |192.168.1.10|255.255.255.0|192.168.1.1|
| PC2    |192.168.2.10|255.255.255.0|192.168.2.1|
| PC3    |192.168.2.20|255.255.255.0|192.168.2.1|

5.No additional router configuration was needed as switch2 is connected to the already- configured GigabitEthernet0/1 interface

## Key Observation
- Devices on different subnets need a router to communicate
  Router operate on Layer 3 makes forwarding decisions based on IP addresses
- Devices on the same subnet communicate directly
  Through the switch using MAC address on Layer2,without needing to reach the gateway
- A single router can manage multiple subnets
  by assigning each interface to a different subnet, acting as the gateway for each


## Verification
```
Ping from PC1 to PC2:
ping 192.168.2.10
Success rate is 100 percent (4/4)

Ping from PC1 to PC3:
ping 192.168.2.20
Success rate is 100 percent (4/4)

Ping from PC2 to PC3:
ping 192.168.2.20
Success rate is 100 percent (4/4)
```

## Screenshots

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/7a1a028b-0c6d-4e9f-ae6c-28f06e2087f5" />




