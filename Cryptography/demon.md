
OSI LAYERS
07 - Application Layer
06 - Presentation Layer
05 - Session Layer
04 - Transport Layer
03 - Network Layer
	When a device sends data, the Network Layer encapsulates it into packets.
	It assigns a source and destination IP address to each packet.
	Routers use these IP addresses to forward packets across networks.
	At the receiving end, the packets are reassembled and passed to the Transport Layer
	Key Protocols in network layer
	IP (Internet Protocol) – Defines addressing and packet delivery (IPv4, IPv6).
	ICMP (Internet Control Message Protocol) – Used for error reporting (e.g., ping).
	ARP (Address Resolution Protocol) – Resolves IP addresses to MAC addresses.
	RARP (Reverse ARP) – Maps MAC addresses to IP addresses.
	NAT (Network Address Translation) – Translates private IP addresses to public ones.
	OSPF (Open Shortest Path First) & RIP (Routing Information Protocol) – Routing protocols for path selection

02 - Data Link Layer 
	This layer transfers data between nodes on a network segment across the physical layer.
	It is concerned with the local delivery of frames between nodes in same level of network
	It divides the data into frames
	Uses CRC like checks to detect errors in transmitted data 
	Uses MAC addresses to identify devices within a local network

01 - Physical Layer 
	The actual physical connections between devices
	Handles Bit synchronization
	Bit rate control 
	Physical topologies
	Transmission mode


### OSI Model Lab day 3

![](./sc/1.png)

SW2 sends out a STP 

STP is the spanning tree protocol which is used to prevent loops in layer 2. It is used to ensure there is only one active path between any two devices in the network. 

How it works? 

STP uses the 802.1D IEEE algorithm to exchange messages between switches. 
The algorithm detects loops by exchanging BPDU messages. 
If a loop is detected, STP removes it by shutting down bridge interfaces. 
STP creates a tree topology of the network, with a single path from the root to each leaf. 
All other paths are put into a standby state

![](./sc/2.png)

The frame received by PC1 is dropped since it does not have a service to accept it. 

In the switch SW1 this is what happens

![](./sc/3.png)

![](./sc/4.png)


After STP convergance is completed, the router R1 could initiate OSPF to start dynamic routing operation

![](./sc/5.png)

OSPF finds the shortest path between routers using dijkstra's algorithm. 

This is a layer 3 protocol 

Next if we type ipconfig /release on the cli of pc1 then it will release its ip address .
This commands basically disconnects the device by releasing its ip address using DHCP protocol.
DHCP is dynamic host configuration protocol. 
It automatically assigns ip addresses and other configuration information to devices on the network.
This is a layer 7 protocol.

![](./sc/6.png)



### Device security Lab day 4

In the cli 
Router > (User Exec mode)
Type `enable` to move into next mode

Router# (Privelaged Exec mode)
Type `configure terminal` to move into global configure terminal mode

Router(config)#

Setting up passwords
	
	Router(config)#enable password ccna
	Router(config)#show
	Router(config)#sho
	Router(config)#exit
	Router#exit
	Router>enable
	Password: ccna (not visible)
	Router#

This method of setting up password is not secure as it can be viewed in plain text using show `show running-config` command.

To level up the security use command `service password-encryption`
Now the password will be written like this 

	enable password 7 08224F400

The number 7 represents the type of encryption of encryption 
7 represents that it is using cisco's proprietery encryption 
Which is also not secure.

So now we can use command `enable secret ccna` to use a more secure encryption algorithm such as MD5

	enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m

Number 5 means it is MD5 encrypted.


### Lan switching day 6

![](./sc/7.png)

Use ping command to send some data to another ip address
This command

	ping 192.168.1.3 

Will create two packets nammed ARP(Address resolution protocol), and ICMP(Internet control message protocol)

ICMP
![](./sc/8.png)

ARP
![](./sc/9.png)

First we will follow ARP 

ARP packet is received by SW1

![](./sc/10.png)

The switch saves the source MAC address in its MAC address table
Next the switch broadcasts the frame.

The packet received by PC2 and SW2. 
Since ARP's destination IP address does not match with IP address of pc2, it drops it. 

SW2 broadcasts it again to PC3 and PC4.

PC4 drops it. 

PC3's ip address matches with ARP's destination ip address so it process it and prepares a response packet containing it's own MAC address.

![](./sc/11.png)

The response from PC3 is sent straight to PC1 because both the switches now know the mac address. So it does not have to broadcast it.

This whole process is basically like `Who is 192.168.1.3? 192.168.1.0 is asking.`

Now the ICMP packet will be transmitted 

PC1 now knows the mac address of PC3 so this transmission will be simple

![](./sc/12.png)

PC3 receives the packet and responds back since the request was an echo message.

![](./sc/13.png)


Both the switches' mac address table are now filled

	SW1>enable
	SW1#show mac
	SW1#show mac add
	SW1#show mac address-table 
	          Mac Address Table
	----------------------------------------
	Vlan    Mac Address       Type        Ports
	----    -----------       --------    -----
	
	   1    0001.647b.3119    DYNAMIC     Gig0/1
	   1    0004.9a6e.d870    DYNAMIC     Gig0/1
	   1    00d0.d3ad.9cab    DYNAMIC     Fa0/


### IPv4 Addresses day 8

And ip address is 32 bits. 

We represent an ip address as `192.168.1.0/24` where 24 specified the network bits and the rest are the host bits.

a packet to ip address 127.0.0.0 and 127.255.255.255 is simple processed and sent back up the layer as if the host had received it from some other device.
These two addresses are used to loop back the packet

The first usable address is one above the network address

192.168.1.0 is network address 
192.168.1.1 is broadcast address used to broadcast message
Or the last address is broadcast address

Now we will configure routers 
![](./sc/14.png)

Router 1

	Router#show ip interface brief
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     unassigned      YES unset  administratively down down 
	GigabitEthernet0/1     unassigned      YES unset  administratively down down 
	GigabitEthernet0/2     unassigned      YES unset  administratively down down 
	Vlan1                  unassigned      YES unset  administratively down dow
	
These routers are administritavely down because cisco routers have shutdown command enabled by default.

Use command  `Interface GigabitEthernet0` to enter the interface of the router

Configure it all manually using command
	
	Router(config-if)#ip address 15.255.255.254 255.0.0.0

Viewing the interface again 

	Router(config-if)#do show ip interface brief
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     15.255.255.254  YES manual up                    up 
	GigabitEthernet0/1     182.98.255.254  YES manual up                    up 
	GigabitEthernet0/2     201.191.20.254  YES manual up                    up 
	Vlan1                  unassigned      YES unset  administratively down dow

We can view the config using 

	Router#show interfaces g0/0
	GigabitEthernet0/0 is up, line protocol is up (connected)
	  Hardware is CN Gigabit Ethernet, address is 0060.3e72.3101 (bia 0060.3e72.3101)
	  Internet address is 15.255.255.254/8
	  MTU 1500 bytes, BW 1000000 Kbit, DLY 10 usec,
	     reliability 255/255, txload 1/255, rxload 1/255
	  Encapsulation ARPA, loopback not set
	  Keepalive set (10 sec)
	  Full-duplex, 100Mb/s, media type is RJ45
	  output flow-control is unsupported, input flow-control is unsupported
	  ARP type: ARPA, ARP Timeout 04:00:00, 
	  Last input 00:00:08, output 00:00:05, output hang never
	  Last clearing of "show interface" counters never
	  Input queue: 0/75/0 (size/max/drops); Total output drops: 0
	  Queueing strategy: fifo
	  Output queue :0/40 (size/max)
	  5 minute input rate 0 bits/sec, 0 packets/sec
	  5 minute output rate 0 bits/sec, 0 packets/sec
	     0 packets input, 0 bytes, 0 no buffer
	     Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
	     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
	     0 watchdog, 1017 multicast, 0 pause input
	     0 input packets with dribble condition detected
	     0 packets output, 0 bytes, 0 underruns
	     0 output errors, 0 collisions, 2 interface resets
	     0 unknown protocol drops
	     0 babbles, 0 late collision, 0 deferred
	     0 lost carrier, 0 no carrier
	     0 output buffer failures, 0 output buffers swapped ou

Change description

	Router(config-if)#description ## to SW1 ##	

Disable shutdown

	Router(config-if)#no shutdown
	Router(config-if)#
	%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
	
	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up


Next step is configure other two interfaces on the router.

	Router(config)#interface gigabitEthernet 0/1
	Router(config-if)#ip address 182.98.255.254 255.255.0.0
	Router(config-if)#description ## to SW2 ##
	Router(config-if)#no shutdown
	
	Router(config-if)#
	%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up
	
	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

Now the routers are configured 

	Router#show ip interface brief 
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     15.255.255.254  YES manual up                    up 
	GigabitEthernet0/1     182.98.255.254  YES manual up                    up 
	GigabitEthernet0/2     201.191.20.254  YES manual up                    up 
	Vlan1                  unassigned      YES unset  administratively down down

Next step is to configure the PC's ip address and then ping to check connectivity.

Now pc1 can reach both pc2 and pc3

![](./sc/15.png)



### Switch interfaces Lab Day 9


The main difference between switches and routers is that switches have shutdown command disabled and routers have it enabled

	Switch#show ip interface brief 
	Interface              IP-Address      OK? Method Status                Protocol 
	FastEthernet0/1        unassigned      YES manual up                    up 
	FastEthernet0/2        unassigned      YES manual up                    up 
	GigabitEthernet0/1     unassigned      YES manual down                  down 
	GigabitEthernet0/2     unassigned      YES manual up                    up 
	Vlan1                  unassigned      YES manual administratively down down

The ip addresses are unasigned because these are layer 2 switch ports and they do not need an ip address

To view the status of switches 

	Switch#show interfaces status 
	Port      Name               Status       Vlan       Duplex  Speed Type
	Fa0/1                        connected    1          auto    auto  10/100BaseTX
	Fa0/2                        connected    1          auto    auto  10/100BaseTX
	Gig0/1                       notconnect   1          auto    auto  10/100BaseTX
	Gig0/2                       connected    1          auto    auto  10/100BaseTX


##### Speed and Duplex auto negotiation

Interfaces advertise their capabilities to the neighbouring devices and they negotiate the best speed and duplex settings

The PC's will use their max speed and switch will adjust its speed accordingly 

If auto negotiation is disabled then the switch will try to sens the speed. If it cannot sense it then it chooses the slowest possible speed. 
If speed of PC 10 or 100 Mbps then half duplex otherwise full duplex

Also without auto negotiation there can be duplex mismatch which can lead to collision.


![](./sc/16.png)

##### R1

	R1(config)#interface gigabitEthernet 0/0
	R1(config-if)#ip address 172.16.255.254 255.255.0.0
	R1(config-if)#speed 1000
	R1(config-if)#duplex full
	R1(config-if)#description ## to SW1 ##
	R1(config-if)#no shutdown

##### SW1
	SW1(config)#interface gigabitEthernet 0/1
	SW1(config-if)#speed 1000
	SW1(config-if)#duplex full
	SW1(config-if)#
	%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up
	
	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up
	
	SW1(config-if)#description ## to R1 ##
	SW1(config-if)#int g0/2
	SW1(config-if)#speed 1000
	SW1(config-if)#duplex full
	SW1(config-if)#
	%LINK-3-UPDOWN: Interface GigabitEthernet0/2, changed state to down
	
	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down
	
	SW1(config-if)#desc
	SW1(config-if)#description ## to SW2 ##
	SW1(config-if)#int range f0/3 - f0/24
	SW1(config-if-range)#description ## not in use ##
	SW1(config-if-range)#shutdown

##### SW2

Same as SW1 


### IPv4 header Lab Day 10

OSI model PDU units

L2 trailer -> data -> L4 header -> L3 header -> L2 header
This whole this is called frame

data -> (end)
This is called packet

data -> L4 header
Segment


![](./sc/17.png)

###### Version 
IPv4 = 4 (0100)
IPv6 = 6 (0110)

###### IHL
Identifies the length of the header in 4 byte increments 
Length of header = 4 * IHL Bytes

###### DSCP
Used for quality of service 
Prioritize delay sensitive data 

###### ECN 
Provides end to end notification of network congestion withour dropping packets (optional)

###### Total Length
Indicates total length of packet 
Measured in bytes

###### Identification
If a packet is fragmented for being too large this field is used to identify which packet it belongs to 

###### Flag
Bit 0 always 0
Bit 1 don't fragment
Bit 2 There are more fragments. For last fragment it is set to 0

###### Fragment offset
Indicate the position of fragment within the original unfragmented packet

###### Time to live 
Used to prevent loop
Router will drop packet if TTL is 0

###### Protocol
Indicates the protocol of L4 PDU
6 - TCP
17 - UDP
1 - ICMP
89 - OSPF

###### Header checksum 
Check for errors in IPv4 header


### Routing Lab Day 11

Static routes are not added to the routing table and must be configured manually.

![](./sc/18.png)

We will have to manually configure R1 to send packets meant for PC2 to G0/0.

###### R1 
	R1(config-if)#do sh ip int bri
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     192.168.12.1    YES manual up                    down 
	GigabitEthernet0/1     192.168.1.254   YES manual up                    up 

###### R2 
	R2(config-if)#do sh ip int br
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     192.168.12.2    YES manual up                    up 
	GigabitEthernet0/1     192.168.13.2    YES manual up                    down 

###### R3
	R3(config-if)#do sh ip int br
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     192.168.13.3    YES manual up                    up 
	GigabitEthernet0/1     192.168.3.254   YES manual up                    up 


Routers are setup now 

Now we have to setup a static route 

###### R1 
	R1(config)#ip route 192.168.3.0 255.255.255.0 192.168.12.2
	Gateway of last resort is not set

    192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
	C       192.168.1.0/24 is directly connected, GigabitEthernet0/1
	L       192.168.1.254/32 is directly connected, GigabitEthernet0/1
	S    192.168.3.0/24 [1/0] via 192.168.12.2
	     192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
	C       192.168.12.0/24 is directly connected, GigabitEthernet0/0
	L       192.168.12.1/32 is directly connected, GigabitEthernet0/0
###### R2
	R2(config)#ip route 192.168.1.0 255.255.255.0 g0/0
	%Default route without gateway, if not a point-to-point interface, may impact performance
	R2(config)#ip route 192.168.3.0 255.255.255.0 g0/1
	%Default route without gateway, if not a point-to-point interface, may impact performance

	S    192.168.1.0/24 is directly connected, GigabitEthernet0/0
	S    192.168.3.0/24 is directly connected, GigabitEthernet0/1


###### R3
	R3(config)#ip route 192.168.1.0 255.255.255.0 192.168.13.2

	S    192.168.1.0/24 [1/0] via 192.168.13.2
     192.168.3.0/24 is variably subnetted, 2 subnets, 2 masks

![](./sc/19.png)


### The life of a packet Lab Day 12

![](./sc/20.png)


ping PC4 using PC1

###### PC1-SW1 
	Dest Addr - FFFFF
	SRC Addr - PC1 

###### SW1-R1
	In layer
		Dest Addr - FFFF
		SRC Addr SW1
	Out Layer
		Dest Addr - SW1
		SRC Addr - R1
This goes back to PC1.
Now PC1 knows the MAC address of R1. It sends ICMP packet to it. 
R1 drops it because the Dest IP address is remote.
R1 creates an ARP frame to learn the MAC address of neighbouring device the remote network
###### R1-R2
	In Layer
		Dest Addr - FFFFF
		SRC Addr - R1
	Out Layer
		Dest Addr - R1
		SRC Addr - R2

PC1 sends ICMP again and it reaches till R2. Then R2 sends and ARP 

###### R2-R3
	In Layer
		Dest Addr - FFFFF
		SRC Addr - R2
	Out Layer
		Dest Addr - R2
		SRC Addr - R3

PC1 sends ICMP again and it reaches till R3. Then R3 sends an ARP

###### R3-SW2
	Dest Addr - FFFFF
	SRC Addr - R3

Switch floods all the ports since it does not know the dest addr
PC5, 6 rejects it because ip address mismatch 

###### SW2-PC4
	In layer
		Dest Addr - FFFFF
		SRC Addr - R3
	Out Lyaer
		Dest Addr - R3
		SRC Addr - PC4

Now all the routers and switches know their neighbours mac address. 
PC1 sends ICMP again and it will be sent to PC4 correctly


### Subnetting

It is dividing a larger network into smaller network 

Instead just 3 classes of network A(8), B(16), C(24) 

We can use 25-31 also depending on the number of hosts in the network.

If a network has only one point to point connection then /30 or /31 is ideal for it.

##### Subnetting class C network

192.168.1.0/24 
Divide this network into 4 subnets with 45 hosts in each

Subnet 1 = 192.168.1.0/26 (64 addresses)
Its range 192.168.1.0 - 192.168.1.63

Subnet 2 = 192.168.1.64/26 
Its range 192.168.1.64 - 192.168.1.127

Subnet 3 = 192.168.1.128/26 
Its range 192.168.1.128 - 192.168.1.191

Subnet 4 = 192.168.1.193/26 
Its range 192.168.1.192 - 192.168.1.255

#### VLSM Lab Day 15 

Variable length subnet mask

Assign first subnet that has the maximum number of hosts and so on.

![](./sc/21.png)

LAN2 - 192.168.5.0 /25
Total 128 hosts
126 usable addresses 

LAN1 - 192.168.5.128 /26
Total 64 hosts
62 usable addresses


### VLAN Lab Day 16

A broadcast domain is a group of devices that will receive the broadcast frame FFFFFF sent by one of the member.

VLAN is used to separate hosts in layer 2.

If a broadcast frame comes from a VLAN, the switch will only forward the frame in that VLAN.

The switch does not perform inter VLAN routing. It must send it through router.

Lab 16
![](./sc/22.png)

R1

	R1(config-if)#do sh ip int br
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     10.0.0.62       YES manual up                    up 
	GigabitEthernet0/1     10.0.0.126      YES manual up                    up 
	GigabitEthernet0/2     10.0.0.190      YES manual up                    up 

Configuring VLAN on switch 
Use command `switchport access vlan 10`
SW1 
	SW1(config-if-range)#do show vlan br
	
	VLAN Name                             Status    Ports
	---- -------------------------------- --------- -------------------------------
	1    default                          active    Fa9/1
	10   VLAN0010                         active    Gig0/1, Fa3/1, Fa4/1
	20   VLAN0020                         active    Gig1/1, Fa5/1, Fa6/1
	30   VLAN0030                         active    Gig2/1, Fa7/1, Fa8/1
	1002 fddi-default                     active    
	1003 token-ring-default               active    
	1004 fddinet-default                  active    
	1005 trnet-default                    active   

Now the switch will only flood the virtual lan.


### Trunk Ports Lab Day 17

They are used to carry traffic from multiple VLAN's over a single interface.

VLAN tagging

802.1Q tag(4 bytes) is inserted between Source and type fields of the ethernet header frame 

The tag consists of Tag protocol identifier (TPID) and tag control identifier (TCI) 

![](./sc/23.png)

###### TPID 
Always set to 0x81000
This indicates that the frame is 802.1Q tagged.

###### PCP 
Priority code point
Used for class of service.
It prioritizes congested traffic in the network.

###### DEI
Used to indicate frames that can be dropped if the network is congested. 

###### VID
Vlan ID
Identifies the VLAN the frame belongs to 
first and last address are not used.

#### Native VLAN
When a switch receives an untagged frame on a trunk port it assumes the frame belongs to the native vlan.



![](./sc/24.png)

After configuring the VLAN 

We have to configure the trunk ports.

In SW1

	SW1(config-if-range)#int g0/1
	SW1(config-if)#switchport mode trunk
	SW1(config-if)#
	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
	
	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up
	SW1(config-if)#switchport trunk allowed vlan 10,30
	SW1(config-if)#sw trunk native vlan 100

In SW2

	SW2(config-if)#int g0/1
	SW2(config-if)#switchport trunk allowed vlan 10,30
	SW2(config-if)#switchport trunk native vlan 100

Next we have to configure SW2 and R1 using router on a stick.

	R1(config-if)#int g0/0.10
	R1(config-subif)#encapsulation dot1q 10
	R1(config-subif)#
	R1(config-subif)#ip address 10.0.0.62 255.255.255.192

	R1(config-subif)#int g0/0.20
	R1(config-subif)#encapsulation dot1q 20
	R1(config-subif)#
	R1(config-subif)#ip address 10.0.0.126 255.255.255.192

	R1(config-subif)#int g0/0.30
	R1(config-subif)#encapsulation dot1q 30
	R1(config-subif)#ip add
	R1(config-subif)#ip address 10.0.0.190 255.255.255.192

All connections are working.


### Inter Vlan switching Lab Day 18

![](./sc/25.png)


	R1#show ip int br
	Interface              IP-Address      OK? Method Status                Protocol 
	GigabitEthernet0/0     unassigned      YES NVRAM  up                    up 
	GigabitEthernet0/0.10  10.0.0.62       YES manual up                    up 
	GigabitEthernet0/0.20  10.0.0.126      YES manual up                    up 
	GigabitEthernet0/0.30  10.0.0.190      YES manual up                    up 
	GigabitEthernet0/1     unassigned      YES NVRAM  administratively down down 
	GigabitEthernet0/2     unassigned      YES NVRAM  administratively down down 
	GigabitEthernet0/0/0   1.1.1.2         YES manual up                    up 
	Vlan1                  unassigned      YES unset  administratively down down

Remove these sub interfaces

After we have to assign `10.0.0.194 255.255.255.252` ip address to g0/0 on R1 

Next we have to configure SW2

SW2 will not have an option to set ip address because its in layer 2 mode. However we can use command `no switchport` to jump to layer 3

After that configure g1/0/2 ip address to 10.0.0.193

Next enable ip routing SW2
configure default route on SW2 to ip address of router ie 10.0.0.194

	Gateway of last resort is 10.0.0.194 to network 0.0.0.0

Next configure SVI's on SW2

SVI's wont have up status if its vlan does not exist

To create an SVI just enter `interface vlan`

Next assign the ip address to that SVI

	Vlan10                 10.0.0.62       YES manual up                    up 
	Vlan20                 10.0.0.126      YES manual up                    up 
	Vlan30                 10.0.0.190      YES manual up                    up

Now the inter vlan routing happens using SW2 instead of R1.
