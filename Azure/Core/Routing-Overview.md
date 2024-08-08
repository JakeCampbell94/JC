# How Azure Select Routes
Outbound traffic is routed using the longest prefix match alogrithm. For example, if you had a route table with two routes, 10.0.0.0/16 and 10.0.0.0/24, traffic with a destination IP of 10.0.0.5 would be routed to the /24 subnet instead of the /16 because it has a longer prefix that the destiantion IP falls under.

For cases where there are multiple routes with the same prefix, Azure selects routes based on these priorities:
1. User-defined routes
2. BGP Routes
3. System Routes

NOTE: System routes for VNet or VNet Peering related traffic is preferred over BGP even if the BGP ones are more specific.

# System Routes
Below are the default system routes added to each subnet route table.

| Num | Address Prefix | Next Hop Type | Route Type |
|---|---|---|---|
| 1 | VNet Address Space | Virtual Network | System Route |
| 2 | 0.0.0.0/0 | Internet | System Route | 
| 3 | 10.0.0.0/8 | None | System Route |
| 4 | 172.16.0.0/12 | None | System Route |
| 5 | 192.168.0.0/16 | None | System Route |
| 6 | 100.64.0.0/10 | None | System Route |
| 7 | 20.35.252.0/22 | None | System Route |
| 8 | 23.103.0.0/18 | None | System Route |
| 9 | 25.4.0.0/14 | None | System Route |
| 10 | 25.33.0.0/16 | None | System Route |
| 11 | 25.41.0.0/20 | None | System Route |
| 12 | 25.48.0.0/12 | None | System Route |
| 13 | 40.108.0.0/17 | None | System Route |
| 14 | 40.109.0.0/16 | None | System Route |
| 15 | 104.147.0.0/16 | None | System Route |
| 16 | 104.146.0.0/17 | None | System Route |
| 17 | 157.59.0.0/16 | None | System Route |
| 18 | 198.18.0.0/15 | None | System Route |

- Entry 1: This is for routing to internal VNet addresses, the next hop is Vitual Network whcih means traffic is routed internally within the VNet. It will go to the gateway and then tothe internal subnet resource.
  - For VNets with multiple address spaces, an entry is added for each space.
- Entry 2: Basically a default route, anything not matching another route uses this. It will forward the traffic to the internet externally of your Azure environment.
  - This has exceptions if the address it is going to is actually hosted in Azure then it won't leave the backbone.
- Entry 3 to 5: These add in the RFC 1918 addresses to stop them being routed over the internet.
- Entry 6: This stops RFC 6598 address space from being routed on the internet.
- Entry 7 to 18: These are the Azure Management public IP ranges aren't routed over the internet.

### System Routes for Dual-Stack Subnets
If you're running IPv6, it will add two extra IPv6 system routes. One for the IPv6 address space with the VNet as the next hop and another as ::/0 for any internet/non-Azure traffic.

### Additional System Routes
For VNet Peering, Vnet Gateways and Service Endpoints, they automatically create custom system routes for those services when you set them up for your subnet.
- VNet Peering, this added the address spaces in the adjacent VNet as routes in your route table.
- VNet Gateways, these are routes added in by the VNet Gateway it has learnt.
- Service Endpoints, public IP address for certain services are added to the route table for subnets where service endpoints are enabled on.

# User Defined Routing (Custom Route Tables)
You can use UDRs or BGP to change teh default routing behaviour done by the system routes. You could maybe direct traffic through a specific appliance, send certain traffic to your on-prem network or block specific traffic.

## User Defined Routes (UDRs)
There's an Azure resource called a Route Table where you can create your custom routing rules which basically behave like static routes. 

When you setup a route table and associate it with a subnet, it will merge the routes with the system routes to a custom route table with both sets of rules in. Please note,
- A subnet can only be associated with ONE custom route table.
- You cannot delete route tables that are actively associated with a subnet.

When creating your route tables, you create rules which will cover:
- Route name, a nice friendly name that describes the purpose of the route (Redirect-To-Firewall-A)
- Destination Type, here you can either use an IP range or service tag.
- Next Hop Type, what do you want to set the next hop as for matching traffic?
- Next Hop Address, only used if you select a virtual appliance as the next hop to specify the IP of the appliance.

You can then associate it with a subnet under the Route Table resource within the settings menu then subnets. You can confirm it works by going to a NIC in that subnet and under Effective Routes you will see the routes added with the source being "User" ratehr than Default.

## Service Tags for UDRs
You can use a service tag as the destiantion in place of an IP range, these represent a range of IPs from a specific Azure service like storage or specific regions. Microsoft look after the IP ranges that are included in the service tag so your rules will auto-update as and when the IPs used by the service change.

Azure gives preference to the route with the explicit prefix when there's a 
