Source: https://learn.microsoft.com/en-us/training/modules/network-fundamentals/1-introduction

## Network types and topologies to use when you design a network
A network is a collection of network-enabled devices.

### Network types
#### personal area network (PAN)
PAN provides networking needs around an individual.
- example devices: laptop, smarkphone, smartwatch
- typically connected through bluetootch

#### local area network (LAN)
LAN provides networking needs around a single location, such as an office, a school, and a university.
- typically privately owned and requires authentication and authoriation
- most commonly used network type

#### Metropolitan area network (MAN)
MAN provides networking capabilities between different location with a large area such city and metropolitan area.
- MAN is scope label, not a required tier, that is, LAN can directly connect to WAN without MAN.

#### wide area network (WAN)
WAN provides networking capabilities between different geographical locations locally or worldwide.
- LANs join a WAN through ISP, internet service provider

### Network topologies
A network topoloy describes the physical composition of a network.

#### bus
In a bus topology, each network device is connected to a single network cable.

#### ring
In a ring toplogy, each network device is connected to its neighbore to form a  ring.

#### mesh
**Physical mesh**: each network device connects to **every** other device in the network.

**Logical mesh**: each device can see any other device in the network by using network **protocals**

#### star
Each device connects to a centralized hub or switch. The most commonly used topology.

### Ethernet
Ethernet is the networking standard that is synonymous with wire-based LAN networks, and is also used in MAN and WAN networks.


## Types of network devices to use when you build a network
https://learn.microsoft.com/en-us/training/modules/network-fundamentals/3-network-infrastructure

### Network standards
Network standards provide a framework that enables the interoperability between devices.

The 802 family of standards.

### Network infrastructure

#### MAC (media access control) address
MAC address is a unique identifier assigned to every network-enabled device at the time of manufacture, also called
- burned-in address
- ethernet hardward address, or
- physical address

#### repeater
Signal amplifier

#### bridge
A bridge divides a network into network segments and can filter and forward data packets between these segments.

#### hub
A hub acts as a multiport repeater on a network.

#### switch
A switch combines the functionality of a bridge and a hub.

#### router
Routers link networkds with different rangeed addresses together. 
- also called gateway
- config router with a default gateway IP address, usually `192.168.1.1`


## Network protocols to use when you implement a network
https://learn.microsoft.com/en-us/training/modules/network-fundamentals/4-network-protocols


