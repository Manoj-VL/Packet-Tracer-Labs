# Lab 7 DHCP,DNS,SSH,TELNET

## Objective
To understand various services like  DHCP,DNS, SSH , TELNET and their respective ports works in network communication and observe packets in simulation mode while using these services in cisco packet tracer

## Topology
<img width="1920" height="1080" alt="Lab 7 - Topology" src="https://github.com/user-attachments/assets/b270be88-f112-40ef-9f11-5a28240744e5" />


## IP Addressing Table

| Device          | IP Address              | Subnet Mask   | Default Gateway |
| --------------- | ----------------------- | ------------- | --------------- |
| PC1             | 192.168.1.100 (Dynamic) | 255.255.255.0 | 192.168.1.1     |
| PC2             | 192.168.1.101(Dynamic)  | 255.255.255.0 | 192.168.1.1     |
| DHCP/SSH Server | 192.168.1.50            | 255.255.255.0 | 192.168.1.1     |
| PC 3            | 192.168.2.30            | 255.255.255.0 | 192.168.2.1     |
| DNS Server      | 192.168.2.10            | 255.255.255.0 | 192.168.2.1     |
| Web Server      | 192.168.2.20            | 255.255.255.0 | 192.168.2.1     |

Dynamic** - DHCP server dynamically allocated the IP address

## Steps used

### Initial Configuration
-Statically assigned IP address for DHCP,DNS, Web Server and PC 3
### Part 1 : DHCP
1. Configured DHCP Pool on the Server 
	1. Pool name : `serverPool`
	2. Default gateway ,DNS Server IP (as shown in table)
	3. Start IP address:`192.168.1.100 `subnet mask:` 255.255.255.0`
	4. Max users:`50`
2. PC1 and PC2 received IPs automatically
3. Observed DHCP packets on UDP port 67,68
#### Screenshots
#### DHCP automatically assigning IP address to  PC1 and PC2 
<img width="1920" height="1080" alt="DHCP Server auto assigns the IP to PC2" src="https://github.com/user-attachments/assets/2cdc7475-a7c3-47e3-9d91-6c8ee39b8773" />
<img width="1920" height="1080" alt="DHCP Server auto assigns the IP to PC1" src="https://github.com/user-attachments/assets/35260501-5d4f-4eb9-b347-d9075d78c034" />

#### DHCP -Port 67,68
<img width="1920" height="1080" alt="DHCP packets on port 67,68" src="https://github.com/user-attachments/assets/a60961d1-d05a-4e44-b548-b73c7f913cfc" />


### Part 2 : DNS
1. Configured DNS Server
	1. `www.testsite.com --- 192.168.2.20`
	2. PC1 resolved domain to IP successfully
	3. Observed DNS query on port 53
### Screenshots

#### PC1 resolving the domain and webpage loading in 192.168.2.20
<img width="1920" height="1080" alt="Browser resolving domain and loading web page -www testsite, com" src="https://github.com/user-attachments/assets/0105afe4-5b94-4792-bad7-c26e81675630" />

#### DNS packets on port 53
<img width="1920" height="1080" alt="DNS Packet in Simulation Mode to show port 53 in action" src="https://github.com/user-attachments/assets/a2e39fc8-a4e0-4e3a-aa3d-b914f525afd5" />

### Part 3: Telnet
1. Configured Telnet on router
#### Router CLI Commands
```
enable
configure terminal
line vty 0 4 #configure virtual terminal lines
password cisco
login #authentication
transport input telnet #to define portocol
exit

```
2. Used PC3 terminal to access the router1, entered password when asked and generated the routing table

### Screenshots
#### Connection from PC3 to Router via Telnet
<img width="1920" height="1080" alt="Connection via telnet on router from PC3 showing its routing table" src="https://github.com/user-attachments/assets/9f6cb1c4-df46-4224-9d36-4bde40bc58f5" />


#### Telnet - Ports 23 + Plain text
<img width="1920" height="1080" alt="Simulation mode show Telnet uses plain text" src="https://github.com/user-attachments/assets/3ff992a5-64f5-40e4-b094-d4539375cfa3" />


### Part 4: SSH
1. Configured the router for SSH connection
#### Router CLI Command
```
enable
configure terminal
hostname SecureRouter #changed the router name
ip domain-name lab.com # to generate rsa key requires Fully Qualified Domain Name
crypto key generate rsa
--ask key size : 2048 # rsa key size
ip ssh version 2
line vty 0 4
transport input ssh
login local
username admin password admin123
exit

```
2. Used PC3 terminal to access the router1, entered username & password when asked and generated the routing table
##### Screenshots

#### Connection from PC3 to Router via SSH
<img width="1920" height="1080" alt="Connection via ssh on router from  PC3 showing routing table" src="https://github.com/user-attachments/assets/6e07cb9b-1167-4fae-9cbc-e476fea6ce45" />


#### SSH - Ports 22 + Encrypted text
<img width="1920" height="1080" alt="Simulation showing that SSH uses an encrypted connection" src="https://github.com/user-attachments/assets/095d731d-8fd0-462c-aea5-2911d68edbd0" />


### Observations
- Telnet sends everything in plain text
- Anyone capturing traffic can read it 
- SSH encrypts all data end to end 
- Always use SSH use never Telnet in real world
- This is why port 23 is flagged as a vulnerability in security scans


