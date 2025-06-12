# Wide-Area-Network-construction
Network Design Summary
•	2 LANs: Office LAN A and Office LAN B
•	Each LAN:
o	1 Router
o	1 Switch
o	2 PCs

  Devices Needed
•	2 Routers (2811)
•	2 Switches (2960)
•	4 PCs (PC-PT)
•	Cables:
o	(Switch - PC) Copper Straight-through (for connecting different devices)
o	(Router - Router via FastEthernet) Copper Crossover (for connecting same type of devices)
________________________________________
Physical Connections
 LAN A (Left side)
•	PC0 to Switch0: FastEthernet (Straight-through)
•	PC1 to Switch0: FastEthernet (Straight-through)
•	Switch0 to Router0 (Fa0/0): FastEthernet (Straight-through)
LAN B (Right side)
•	PC2 to Switch1: FastEthernet (Straight-through)
•	PC3 to Switch1: FastEthernet (Straight-through)
•	Switch1 to Router1 (Fa0/0): FastEthernet (Straight-through)
WAN (Router to Router)
•	Router0 (Fa0/1) to Router1 (Fa0/1): FastEthernet (Crossover cable)

IP Addressing Plan
Device	Interface	    IP Address	  Subnet Mask
PC0	       —	       192.168.1.10	  255.255.255.0
PC1	       —       	 192.168.1.11	  255.255.255.0
Router0	  Fa0/0	     192.168.1.1	  255.255.255.0
Router0	  Fa0/1	     10.0.0.1	      255.255.255.0
Router1	  Fa0/1	     10.0.0.2	      255.255.255.0
Router1	  Fa0/0	     192.168.2.1	  255.255.255.0
PC2	        —	       192.168.2.10	  255.255.255.0
PC3	        —        192.168.2.11	  255.255.255.0


Step-by-Step Configuration 
Step 1: Assign IP addresses to PCs
Click each PC → Desktop → IP Configuration
PC0:
•	IP: 192.168.1.10
•	Subnet Mask: 255.255.255.0
•	Default Gateway: 192.168.1.1 (Router0 Fa0/0)
PC1:
•	IP: 192.168.1.11, Gateway: 192.168.1.1
PC2:
•	IP: 192.168.2.10, Gateway: 192.168.2.1 (Router1 Fa0/0)
PC3:
•	IP: 192.168.2.11, Gateway: 192.168.2.1

Step 2: Configure Router0
Click Router0 → CLI:

Router> enable
Router# configure terminal
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown      
Router(config-if)# exit

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 10.0.0.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write     

 Step 3: Configure Router1
Click Router1 → CLI:

Router> enable
Router# configure terminal
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.2.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 10.0.0.2 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write

Step 4: Add Static Routes (So both routers know about each other’s LANs)
On Router0:
Router# configure terminal
Router(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
Router(config)# exit
This tells Router0: "To reach 192.168.2.x, send packets via 10.0.0.2"

On Router1:
Router# configure terminal
Router(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1
Router(config)# exit
This tells Router1: "To reach 192.168.1.x, send packets via 10.0.0.1"

Step 5: Test Connectivity (Ping)
Go to PC0 → Desktop → Command Prompt:
ping 192.168.1.11      # Test LAN
ping 192.168.2.10      # Test across WAN

Also test from PC3 to PC0.
If everything is configured properly:
•	All PCs can ping each other
•	Routers can reach both LANs and communicate via WAN

