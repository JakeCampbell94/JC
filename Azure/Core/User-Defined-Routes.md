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


