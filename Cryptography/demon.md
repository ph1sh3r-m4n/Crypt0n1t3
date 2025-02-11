
## OSI Layer Overview

- **Layer 7** - Application Layer  
- **Layer 6** - Presentation Layer  
- **Layer 5** - Session Layer  
- **Layer 4** - Transport Layer  
- **Layer 3** - Network Layer  
- **Layer 2** - Data Link Layer  
- **Layer 1** - Physical Layer  

## Network Layer Deep Dive

### When data is sent across a network:
- Network Layer encapsulates it into packets
- Assigns source and destination IP addresses
- Routers use these addresses for forwarding
- Packets are reassembled at the receiving end

### Key Network Layer Protocols
- **IP (Internet Protocol)** – Addressing and packet delivery (IPv4, IPv6)
- **ICMP (Internet Control Message Protocol)** – Error reporting (ping)
- **ARP (Address Resolution Protocol)** – IP to MAC resolution
- **RARP (Reverse ARP)** – MAC to IP resolution
- **NAT (Network Address Translation)** – Private/public IP translation
- **OSPF & RIP** – Path selection routing protocols

## Data Link Layer Functions
- Transfers data between network nodes
- Handles local frame delivery
- Divides data into frames
- Uses CRC for error detection
- Manages MAC addressing

## Physical Layer Responsibilities
- Physical device connections
- Bit synchronization
- Bit rate control
- Physical topologies
- Transmission modes

## STP Lab (Day 3)

### Understanding STP (Spanning Tree Protocol)
- Prevents layer 2 loops
- Uses 802.1D IEEE algorithm
- Exchanges BPDU messages
- Detects and removes loops
- Creates single-path tree topology

### When PC1 receives a frame:
- If no service exists to accept it -> Frame is dropped

## Device Security Lab (Day 4)

### CLI Navigation
```bash
Router > (User Exec mode)
```
Type 'enable' for:
```bash
Router# (Privileged Exec mode)
```
Type 'configure terminal' for:
```bash
Router(config)#
```

### Password Configuration
Basic password:
```bash
Router(config)#enable password ccna
```
Enhanced security:
```bash
Router(config)#service password-encryption
```
Result:
```bash
enable password 7 08224F400
```
Best security:
```bash
Router(config)#enable secret ccna
```
Result:
```bash
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m
```

## LAN Switching (Day 6)

### Packet Analysis
Using `ping 192.168.1.3` creates:
- ARP packet
- ICMP packet

### ARP Process
#### Initial broadcast:
- Source MAC saved in switch table
- Frame broadcasted

#### PC2 and SW2 receive packet
- PC2 drops (IP mismatch)
- SW2 rebroadcasts
- PC3 matches IP and responds
- Direct response to PC1 (no broadcast needed)

## IPv4 Addressing (Day 8)

### IP Address Structure
- 32-bit length
- Format: `192.168.1.0/24`
- Network/host bit division
- Special addresses:
  - `127.0.0.0/127.255.255.255` (loopback)
  - First usable = network + 1
  - Last address = broadcast

### Router Configuration Example
```bash
Router>show ip interface brief
```
```
Interface              IP-Address      OK? Method Status     Protocol 
GigabitEthernet0/0     unassigned      YES unset  down      down 
GigabitEthernet0/1     unassigned      YES unset  down      down 
GigabitEthernet0/2     unassigned      YES unset  down      down 
Vlan1                  unassigned      YES unset  down      down 
```

## Switch Interfaces (Day 9)

### Switch vs Router Defaults
- **Switches:** shutdown disabled by default
- **Routers:** shutdown enabled by default

### Interface Status Check
```bash
Switch#show interfaces status
```
```
Port      Name     Status    Vlan    Duplex  Speed Type
Fa0/1              connected  1      auto    auto  10/100BaseTX
```

### Speed/Duplex Auto-negotiation
- Interfaces advertise capabilities
- Best settings negotiated automatically
- Without auto-negotiation:
  - Speed sensing attempted
  - Defaults to lowest speed if unsuccessful
  - Potential duplex mismatches

## IPv4 Header (Day 10)

### PDU Units in OSI Model
```bash
L2 trailer -> data -> L4 header -> L3 header -> L2 header
```

### Header Fields
- **Version:** IPv4 (0100) or IPv6 (0110)
- **IHL:** Header length (4-byte increments)
- **DSCP:** Quality of service
- **ECN:** Congestion notification
- **Total Length:** Packet size in bytes
- **TTL:** Loop prevention
- **Protocol:** L4 protocol identifier (TCP=6, UDP=17)

## Routing (Day 11)

### Static Route Configuration
#### R1:
```bash
R1(config)#ip route 192.168.3.0 255.255.255.0 192.168.12.2
```
#### R2:
```bash
R2(config)#ip route 192.168.1.0 255.255.255.0 g0/0
R2(config)#ip route 192.168.3.0 255.255.255.0 g0/1
```
#### R3:
```bash
R3(config)#ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

## Packet Life Cycle (Day 12)

### ARP Process Flow
#### PC1 to SW1:
```bash
Dest: FFFFF
Source: PC1
```
#### SW1 to R1:
```bash
In: Dest: FFFF, Source: SW1
Out: Dest: SW1, Source: R1
```
Subsequent router hops follow similar pattern
Final delivery uses learned MAC addresses

## Subnetting

### Class C Network Division Example
Dividing `192.168.1.0/24` into 4 subnets:
```bash
Subnet 1: 192.168.1.0/26 (0-63)
Subnet 2: 192.168.1.64/26 (64-127)
Subnet 3: 192.168.1.128/26 (128-191)
Subnet 4: 192.168.1.192/26 (192-255)
```

## VLAN Configuration (Day 16)
### VLAN basics:

Separates broadcast domains
Requires router for inter-VLAN communication

### Basic VLAN Setup
```bash
switchport access vlan 10
```

### VLAN Status Check
```bash
SW1#show vlan brief
```

## Trunk Ports (Day 17)
### Key concepts:

Carries multiple VLAN traffic
Uses 802.1Q tagging
Adds 4-byte tag between Source and Type fields
### Configuration Example
```bash
SW1(config-if)#switchport mode trunk
SW1(config-if)#switchport trunk allowed vlan 10,30
SW1(config-if)#switchport trunk native vlan 100
```

## Inter-VLAN Switching (Day 18)

### Layer 3 Switch Configuration
```bash
no switchport
ip routing
interface vlan 10
ip address 10.0.0.62 255.255.255.192
```
### SVI Configuration
```bash
Vlan10                 10.0.0.62       YES manual up                    up 
Vlan20                 10.0.0.126      YES manual up                    up 
Vlan30                 10.0.0.190      YES manual up                    up
```
