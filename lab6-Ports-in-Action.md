# Lab 6 - Ports in Action
## Objective
To understand how port numbers are used in network communication by observing 
HTTP and FTP traffic using Cisco Packet Tracer simulation mode.

## Topology
<img width="1920" height="1080" alt="Topology Lab 4" src="https://github.com/user-attachments/assets/898e0ed3-a576-4602-b750-a59bed9bac2e" />


## IP Addressing Table
| Device      | IP Address   | Subnet Mask   | Default Gateway |
|-------------|--------------|---------------|-----------------|
| PC1         | 192.168.1.10 | 255.255.255.0 | 192.168.1.1     |
| PC2         | 192.168.2.10 | 255.255.255.0 | 192.168.2.1     |
| PC3         | 192.168.2.20 | 255.255.255.0 | 192.168.2.1     |
| Server0     | 192.168.2.50 | 255.255.255.0 | 192.168.2.1     |
| PC0         | 192.168.3.10 | 255.255.255.0 | 192.168.3.1     |
| Router G0/0 | 192.168.1.1  | 255.255.255.0 | —               |
| Router G0/1 | 192.168.2.1  | 255.255.255.0 | —               |
| Router G0/2 | 192.168.3.1  | 255.255.255.0 | —               |

## Steps Used

### Step 1 - Verify HTTP Service
1. Enabled HTTP on Server0 → Services tab → HTTP → ON
2. Pinged Server0 from PC1 to confirm connectivity.
3. Opened web browser on PC1 → entered `192.168.2.50` in URL bar → webpage loaded successfully.

### Step 2 - Verify FTP Service
1. Enabled FTP on Server0 → Services tab → FTP → ON
2. Added login credentials on the server:
   - Username: `admin`
   - Password: `admin123`
3. Connected to FTP server from PC1 command prompt:
  ftp 192.168.2.50
4. Entered credentials — server responded with:
230-Logged in
(passive mode on)
### Step 3 - Observe Ports in Simulation Mode
1. Switched Packet Tracer to simulation mode.
2. Filtered protocols to show only HTTP and FTP packets.
3. Triggered HTTP traffic by loading the webpage from PC1 browser.
4. Triggered FTP traffic by connecting via command prompt.
5. Clicked on each packet to open PDU details and verify port numbers.

## Verification
### HTTP Web page loaded
<img width="1920" height="1080" alt="Webpage being loaded from an web server" src="https://github.com/user-attachments/assets/8396bec1-54da-4c2b-8ac1-388a20300eb5" />

### HTTP - Port 80

<img width="1920" height="1080" alt="Proof of Port 80 being used in HTTP" src="https://github.com/user-attachments/assets/e72405c3-b36f-4cbb-abc9-d3903927d501" />

### FTP Login
<img width="1920" height="1080" alt="FTP connection betwenn PC1 and Sever" src="https://github.com/user-attachments/assets/62455bc9-d23d-46fc-90f0-24292a72e949" />


### FTP - Port 21
<img width="1920" height="1080" alt="Proof of Port 21 used in FTP" src="https://github.com/user-attachments/assets/ddca13a9-5dce-4f72-891d-a2da1388d525" />


## Key Observations
- Port 80 was visible in the HTTP packet PDU, confirming HTTP operates on port 80.
- Port 21 was visible in the FTP packet PDU, confirming FTP operates on port 21.
- Port numbers operate at Layer 4 - the Transport Layer.
- IP addresses are responsible for delivering data to the correct device (Layer 3).
- Port numbers are responsible for delivering data to the correct service on that device (Layer 4).
- Both HTTP and FTP transmit data in plaintext — credentials and page content are fully 
  visible inside the PDU. This is why HTTPS (port 443) uses TLS encryption and SFTP 
  replaces FTP for secure file transfer.
