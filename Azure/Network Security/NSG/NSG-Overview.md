# Network Security Groups (NSGs)
## What's an NSG
Basically an Extended ACL which you can attach to either a VM's NIC or to a VNET. You can allow or block traffic based on different criteria.

NSGs can filter based on:
- Destintation and Destination Port
- Source and Source Port
- Protocol

Rules can be applied either Inbound or Outbound

## NSG Priority
NSGs use priority to process the rules, lower the priority numbers win.
- Priority can be a value between 100 and 4096
- You can't have two rules with the same priority and direction (You could have two rules with the same priority but one is for inbound and the other for outbound)

## Default NSG Rules
By default, Azure will create the following rules:
### Default Inbound Rules

| Rule | Priority | Source | Source Ports | Destination | Destination Ports | Protocol | Access |
| ---|---|---|---|---|---|---|---|
| AllowVNetInBound | 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Any | Allow |
| AllowAzureLoadBalancerInBound | 65001 | AzureLoadBalancer | 0-65535 | 0.0.0.0/0 | 0-65535 | Any | Allow |
| DenyAllInbound | 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Any | Deny |

### Default Outbound Rules

| Rule | Priority | Source | Source Ports | Destination | Destination Ports | Protocol | Access |
| ---|---|---|---|---|---|---|---|
| AllowVNetOutBound | 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Any | Allow |
| AllowInternetOutBound | 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Any | Allow |
| DenyAllOutbound | 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Any | Deny |


## Routing Behaviours with NSGs
NSGs can be applied to NICs and VNETs which will hav an affect on how the rules are processed and the order.
- If there is an NSG linked to the VNET, inbound traffic to that VNET will hit that first and be processed.

