# Lab 3 - Show MAC Address Table

## Objective
To observe how switches learn mac addresses at layer2

## Topology 
```
[PC1]──[SW1]──[Router]──[SW2]──[PC2]
                            └──[PC3]
```
## Steps used
1.Used the same topology and the same ip address from the previous labs were used
2.All the ip addresses ,subnetmasks,gateways etc were checked before sending the ping  
3.Before the ping test used the cli command to view the MAC address table of the switch

```
enable
show mac-address-table
```
4. The fields were empty meaning without traffic switch doesn't know where to transmit the message
5. Pinged from PC1 to PC 3
6. After the ping resolved ,ran the cli command viewed the MAC address table, table was created with MAC address mapped to the ports
7. Changed the packet tracer to simulation mode and watched how the packet moved
8. For the first time the switch 2 sends the packet across both pc2 and pc3 and second time the packet took the correct  path of PC3 without flooding both pc's

## Verification
### MAC Address Table Before Ping

<img width="1920" height="1080" alt="Before Ping" src="https://github.com/user-attachments/assets/2214637c-5ce1-4d37-8949-4b42225e193c" />

### MAC Address Table After Ping
<img width="1920" height="1080" alt="After Ping" src="https://github.com/user-attachments/assets/d88b4701-4a92-447b-950e-6668d7acc8cc" />

### Ping from PC1 to PC3:
```
ping 192.168.2.20
Success rate is 100 percent (4/4)
```

## Observation
- When traffic first passes through a switch, the MAC address table is empty and the switch 
  floods the packet to all ports — behaving like a hub.
- As traffic flows, the switch maps each source MAC address to the port it arrived on, 
  building the MAC address table dynamically.
- On subsequent traffic, the switch uses the table to forward packets only to the correct 
  port, eliminating unnecessary flooding.
- Observed this behaviour directly in Packet Tracer simulation mode — first ping flooded 
  to both PC2 and PC3, second ping routed only to PC3.
