# Network Security Groups (NSGs)
## What's an NSG
Basically an Extended ACL which you can attach to either a VM's NIC or to a VNET. You can allow or block traffic based on different criteria.

NSGs can filter based on:
- Destintation and Destination Port
- Source and Source Port
- Protocol
- Inbound or Outbound

## NSG Priority
NSGs use priority to process the rules, lower the priority numbers win.
- Priority can be a value between 100 and 4096
- You can't have two rules with the same priority and direction (You could have two rules withe the same priority but one is for inbound and the other for outbound)

## Default NSG Rules
By default, Azure will create the following rules:
### Default Inbound Rules
AllowVNetInBound, Priority 65000, Source VirtualNetwork, Source Ports 0-65535, Destination VirtualNetwork, Destiantion Ports 0-65535, Protocol Any, Acess Allow

