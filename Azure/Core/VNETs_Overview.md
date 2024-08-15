# Virtual Networks (VNets)
VNets are allow you to create subnets for your resources to work within. You can have a VNet with a overall address space of 10.10.0.0/16 and then within that have subnets such as 10.10.10.0/24 and 10.10.20.0/24.
- VNets work at layer 3 and 4 so you will see TCP, UDP, ICMP for example but no layer 2 concepts like broadcasts.
- VNets are regional and can be used with availablilty zones in specific regions.
- VNet names must be unique within a resource group, you can technically have two different VNets named "MyVnet" if both are part of seperate resource groups (Obviously don't name VNets the same thing though)
- You can connect VNets to other VNets in different regions or subscriptions.

### Blocked Traffic in VNets
There are some stuff blocked in VNets by Azure:
- Multicast and Broadcast
- GRE
- DHCP Unicast, over UDP Ports 67 and 68
- IP-in-IP Encapsulation
- UDP Source Port 65330

## Address Spaces in VNets
When creating a VNet, you define at least one address space and within that you can create subnets.
- You can run single-stack (just IPv4) or dual-stack (Both IPv4 and IPv6)
- Azure doesn't support IPv6 only VNets and subnets (Although they can be configured but you can't attach a NIC to it)
- You can have multiple address ranges in a VNet and those spaces can have subnets too, say run both 10.10.0.0/16 and 192.168.0.0/16 in once VNet.
- You can use the usual RFC 1918 ranges plus 100.64.0.0/10 reserved in RFC 6598 and Azure will treat it as a private range.
- Azure uses /64 for your IPv6 subnets
- Some Azure services need a dedicated subnet

### Reserved Addresses and Blocked Ranges
Each subnet will have 5 addresses reserved from the get go:
- The network address and broadcast address like usual.
- The first usable address automatically becomes the gateway.
- The second and third usable addresses are for Azure DNS
Due to these being reserved by default, the smalled IPv4 subnet you can have is a /29 (Biggest is a /2)

Specific ranges are blocked from being used:
- Multicast (224.0.0.0/4)
- Broadcast (255.255.255.255/32)
- Loopback (127.0.0.0/8)
- Link-Local (169.254.0.0/16)
- Internal DNS (168.63.129.16/32)
