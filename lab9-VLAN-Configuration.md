# Lab - 9 VLAN Configuration

## Objective
- Configure VLANs  on a switch and observe traffic separation and also use inter VLAN routing to connect two different VLANs

## Topology
<img width="1920" height="1080" alt="Topology Lab 9" src="https://github.com/user-attachments/assets/cbb516f7-fc8d-4ac6-90b9-90b4b75809e2" />


## VLAN Table
1. VLAN 10 - HR - Department
2. VLAN 20 - Finance Department


| Device | VLAN | IP Address    | Sub net mask  | Default gateway |
| ------ | ---- | ------------- | ------------- | --------------- |
| PC 1   | 10   | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| PC 2   | 10   | 192.168.10.20 | 255.255.255.0 | 192.168.10.1    |
| PC 3   | 20   | 192.168.20.10 | 255.255.255.0 | 192.168.20.1    |
| PC 4   | 20   | 192.168.20.20 | 255.255.255.0 | 192.168.20.1    |

## Steps used
### Initial Configuration

1. Used 4 PC ,1 Switch and Router to connect as shown in topology
2. Statically assigned the IP address,, Subnet mask, Default gateway  for PC1 to 4 

### Part 1: Create VLANs  on Switch

1. Used switch CLI mode in order to create VLANs
**Switch CLI Mode**
```
Switch(config)# vlan 10 
Switch(config-vlan)# name HR-Department
Switch(config-vlan)# vlan 20
Switch(config-vlan)# name Finance-Department
Switch(config-vlan)#end
```
2. Created two different VLANs
### Part 2: Assign ports to VLAN

1.To manually assign the created VLANs to their respective access ports
**Switch CLI Mode**
```
Switch# configure terminal  
Switch(config)#interface f0/1 #
Switch(config-if)#switchport mode access # access port selection
Switch(config-if)#switchport access vlan 10 #  binding the port to VLAN 10
Switch(config-if)#exit

Switch(config)#interface f0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit

Switch(config)#interface f0/3
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#exit

Switch(config)#interface f0/4
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#exit

```


#### Screenshot

##### VLAN Brief - VLAN 10, 20
<img width="1920" height="1080" alt="VLAN Brief" src="https://github.com/user-attachments/assets/b1fbe33c-99a1-4a7c-a3bd-6ed23d84fe90" />



### Part 3: Testing VLAN Isolation
1. Pinged PC1 to PC2, ping was successful
2. Pinged PC1 to PC3 ,ping was unsuccessful
3. Both ping confirms two VLAN are working and segmented properly

#### Screenshot
#### VLAN Isolation B
<img width="1920" height="1080" alt="Ping failing due to Vlan Separation" src="https://github.com/user-attachments/assets/4c4721af-c30c-4b27-ab12-ebb45300b670" />


### Part 4:Inter VLAN Routing
1. To configure inter VLAN routing so that PC1 can connect to PC3 ,which are both in two different VLANs
2. Configuring the switch in order to trunk the port to router
**Switch CLI Mode**
```
Switch# configure terminal
Switch(config)#interface f0/5 # port connected to router
Switch(config-if)#switchport mode trunk  # switch port to be trunk mode
Switch(config-if)#no shutdown
Switch(config-if)#exit
```
3. Configure the router to do the inter VLAN routing
**Router CLI Mode**
```
Sub interface for VLAN 10
Router(config)# interface g0/0.10 # virtual slice of the port
Router(config-subif)# encapsulation dot1Q 10 #vtag the vlan tag to the packet
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit


Sub interface for VLAN 20
Router(config)# interface g0/0.20 
Router(config-subif)# encapsulation dot1Q 20 
Router(config-subif)# ip address 192.168.20.1 255.255.255.0 
Router(config-subif)# exit


Turn on the physical interface
Router(config)# interface g0/0
Router(config-if)# no shutdown
Router(config-if)# exit
```
4. After the configuration checked the IP brief to confirm the inter VLAN routing
5. Pinged from PC1 to PC3 was successful

#### Screenshot
##### Router's IP Brief showing the inter VLAN routing
<img width="1920" height="1080" alt="Router ip brief" src="https://github.com/user-attachments/assets/598c8798-2c30-4f89-bfac-0627416df167" />


##### Ping from PC1 to PC3

<img width="1920" height="1080" alt="Ping from PC1 to PC3 using inter VLAN routing" src="https://github.com/user-attachments/assets/df9e6ac2-7890-414a-9232-94bba47e4b11" />


## Key Observations

- VLANs create network separation — making each network logically isolated, segmented, 
  and secure without any additional hardware.
- Access ports connect a switch to endpoint devices and carry only one VLAN. Trunk ports 
  connect switch-to-switch or switch-to-router, carrying multiple VLANs over a single 
  physical connection.
- dot1Q encapsulation adds an 802.1Q header to untagged frames, providing a VLAN ID so 
  multiple VLANs can travel across a single trunk port.
- Layer 3 devices are required for inter-VLAN routing — each VLAN is considered a 
  separate network, so only a Layer 3 device can route traffic between them.

### Security Takeaway

- VLANs are a fundamental network security control — separating HR and Finance traffic 
  means a compromised device in one VLAN cannot directly reach devices in another, 
  reducing lateral movement risk.
