# packet-tracer-labs
Practical networking labs documenting my progression  from basic connectivity to advanced network security  as a career switcher into cloud security

## Lab 1 Basic Interconnected Network
## Objective 
Configure inter-network communications between two PC on separate subnets using static ip addressing and cisco router

### Topology
PC1--> SWITCH1 --> ROUTER --> PC2

### Steps used
1. Open the packet tracer and put two endpoint devices ie, Pc1,Pc2
2. To show case to how to connect 2 pc in different networks put a switch and router in between
3. Used cables to connect them as show in the topology 
4. Manually configured the 2 pcs with static ip address,gateway and subnet mask

|Device | IP address | Subnetmask     |  Gateway     |
-----------------------------------------------------
| PC1   |192.168.1.10 | 255.255.255.0 | 192.168.1.1  |
| PC2   |192.168.2.10 | 255.255.255.0 | 192.168.2.1  |

5. Router interfaces show red dots by default because they are administratively down and must be enabled manually using the no shutdown command.
6. Use CLI to configure the router,each port were manually configured with gateway subnetmask and manually turning on the router on that side

```
enable
configure terminal
interface GigabitEthernet0/0 # to select which port needs to be configured
ip address 192.168.1.1 255.255.255.0 # static entry of ip address and subnetmask
no shutdown # manually turned on the port
exit
interface GigabitEthernet0/1 # to select which port needs to be configured
ip address 192.168.2.1 255.255.255.0 # static entry of ip address and subnetmask
no shutdown # manually turned on the port
exit
```
### Key Observations
- Router do not automatically turn on like switch
- This is done by design -Routers are more complex devices and Cisco keeps interfaces shut by default to prevent unintended routing until its explicitly configured.
### VERFICATION
-ping from PC1 to PC2
ping 192.168.2.10
successfully transmitted 4 packets and recieved 4 as well
Success rate is 100 percent (4/4)
