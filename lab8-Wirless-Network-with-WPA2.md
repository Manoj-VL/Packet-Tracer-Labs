# Lab 8 -  Wireless Network with WPA2

## Objective
- Configure a wireless network connect it over a wired network and observe the communication between them

## Topology
<img width="1920" height="1080" alt="Topology- Lab 8" src="https://github.com/user-attachments/assets/9b59389f-9493-44ad-8bd1-3df3595c3898" />



## IP Address Table

| Device   | IP Address    | Subnet Mask   | Default Gateway |
| -------- | ------------- | ------------- | --------------- |
| Laptop 1 | 192.168.0.104 | 255.255.255.0 | 192.168.0.1     |
| Laptop 2 | 192.168.0.102 | 255.255.255.0 | 192.168.0.1     |
| PC1      | 192.168.0.105 | 255.255.255.0 | 192.168.0.1     |


** IP addresses were generated dynamically by Wireless router

**Wireless Router used WRT300N**

## Step Used

### Step 1: Device Configurations
#### Part 1 : Laptop
1. Remove the **Fast Ethernet (PT-LAPTOP-NM-1CE)** module and replace it with the **Wireless (WPC300N)** module for Laptop 1
2. Repeated the same above step for Laptop 2 as well
3. IP Address  configuration was set to DHCP

#### Part 2:  Router
1. Created new an SSID - `Test-WiFI`
2. Selected the WPA2-PSK for authentication and created a PSK pass phrase
		- Password - `admin123`
3. Router IP address was assigned - `192.168.0.1`
4. Enabled the DHCP Server 
		- Start IP address -`192.168.0.100`
		- Maximum IP User - 50
		- IP Address Range - `192.168.0.100` to `192.168.0.149`
		- SSID Broadcast was turned on

#### Part 3 : PC 1
1. IP Address configuration was set to DHCP
### Step 2 : Device Connection
1. Configured the Laptop wireless connection
	1. Selected the `Test-WiFi` from the connection tab
	2. Entered the password and wireless connection was established
	3. Pinged from Laptop 2 to PC1 - successful

## Screenshots
### Installation of wireless module on Laptop

<img width="1920" height="1080" alt="Installation of Wireless module in laptop" src="https://github.com/user-attachments/assets/571d7c50-cde9-443f-81b4-a21f576b03f5" />

### SSID and Authentication settings
<img width="1920" height="1080" alt="SSID and WPA2 Settings" src="https://github.com/user-attachments/assets/d437a3a6-cc5f-4bd2-b3cd-defc054eae96" />


### Details of Wi-Fi connection
<img width="1920" height="1080" alt="Detailed view of Wifi from the connecting device" src="https://github.com/user-attachments/assets/77fc269e-214e-4e56-97f7-3b94284e4472" />

### Ping from Laptop2 to PC1
<img width="1920" height="1080" alt="Ping from laptop to PC" src="https://github.com/user-attachments/assets/ea9481bb-4796-412b-9258-457076e3c9c7" />

## Problems I Hit
- Initial ping failed -  simulation mode revealed the packet was stuck at Laptop2 and when tried to ping from PC1 to laptop 2 packet stuck at switch
- On further inspection switch to router connection was made to the internet port rather than Ethernet port on the router
## Observations
- Wireless and wired devices communicate on the same network because the WRT300N 
  bridges its Wi-Fi and LAN interfaces internally on the same subnet.
- WPA2-PSK authentication ensures only devices with the correct passphrase can join 
  the network — the PSK is hashed and never transmitted in plaintext.
- The built-in DHCP server eliminates the need for manual IP configuration, 
  automatically assigning addresses within the defined range to all connected devices.

## Security Takeaway

- WPA2 replaced WEP and WPA due to their known vulnerabilities. WEP can be cracked 
  in minutes. WPA2 uses AES encryption, making it significantly more resistant to 
  brute force attacks.
