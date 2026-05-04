# Lab 5 — Data Encapsulation in Simulation Mode

**OSI Layers:** All 7 layers — full stack observation

## Topology
```
Same as Lab 4 — no new devices added
Simulation Mode used to observe existing network
```

## What Simulation Mode Shows
```
Packet Tracer Simulation Mode slows down
all network traffic so you can watch
each packet travel hop by hop
across the network in real time
```

## How to Enter Simulation Mode
```
Bottom right of Packet Tracer screen
Click "Simulation" button
OR press Shift + S
```

## Steps I Followed
```
1. Switched to Simulation Mode
2. Sent ping from PC1 to Server (192.168.2.50)
3. Pressed Play to watch packet travel
4. Clicked on travelling packet
5. Opened PDU Information window
6. Observed OSI layer details at each hop
7. Watched encapsulation leaving PC1
8. Watched decapsulation arriving at Server
```

## What I Observed

**Encapsulation — Sending Side (PC1)**
```
Layer 7 Application  → Data created (HTTP/ICMP)
Layer 4 Transport    → Segment — TCP header added
                       Source port + Destination port
Layer 3 Network      → Packet — IP header added
                       Source IP + Destination IP
Layer 2 Data Link    → Frame — MAC header added
                       Source MAC + Destination MAC
Layer 1 Physical     → Bits — converted to
                       electrical signals on cable
```

**Decapsulation — Receiving Side (Server)**
```
Layer 1 Physical     → Bits received from cable
Layer 2 Data Link    → Frame — MAC header removed
                       Switch checks MAC address
Layer 3 Network      → Packet — IP header removed
                       Router checks IP address
Layer 4 Transport    → Segment — TCP header removed
                       Port number checked
Layer 7 Application  → Data delivered to application
                       HTTP webpage served
```

## PDU Names at Each Layer
| OSI Layer | PDU Name | Header Added |
|-----------|---------|--------------|
| Layer 7-5 | Data | Application data |
| Layer 4 | Segment | TCP/UDP header |
| Layer 3 | Packet | IP header |
| Layer 2 | Frame | MAC header |
| Layer 1 | Bits | Electrical signal |

## Key Takeaways
- Every layer adds its own header going DOWN
- Every layer removes that header going UP
- Router only reads up to Layer 3 IP header
  it does not care about Layer 7 content
- Switch only reads up to Layer 2 MAC header
  it does not care about IP addresses
- This is why layered networking is powerful
  each device only does its specific job

## Screenshots

**PDU Information Window — Layer Details**
<img width="1920" height="1080" alt="PDU Window  - Layer Details" src="https://github.com/user-attachments/assets/9e822700-cc37-428d-b092-0648714beb4a" />
