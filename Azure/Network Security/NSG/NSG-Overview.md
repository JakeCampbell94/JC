# Network Security Groups (NSGs)
## What's an NSG
Basically an Extended ACL which you can attach to a VM's NIC or to a VNET to control traffic.

- NSGs can filter on the usual, Destination, Destination Port, Source, Source Port and Protocol.
- Rules can be setup for either inbound or outbound.
- You can also use service tags or Application Security Groups in your rules in place of IP.
- There is a limit of 1000 NSGs per Subscription and they are regional

## Usage of Service Tags and Application Security Groups
You can use service tags to specify a specific Azure service within your NSG rule, say using "Internet" to specify anything outside of Azure or using "PowerBi" to control access to Power Bi.

Application Security Groups can also be used to simply rules instead of multiple rules for specific IPs. You may have 3 frontend VMs that connect to a SQL database, those VMs can be put into an Application Security Group and your NSG rule for accessing the SQL box can specify that ASG rather than each VM IP.

## Flow Records & Connections
NSGs are stateful so if you set a rule to allow traffic out via port 94 then you won't need to define an inbound rule for the same port. The same goes for inbound rules, you won't need to set a outbound one.

When removing a rule, it won't affect active connections - only new connections. Say you had a rule to allow SSH, started a SSH session and then changed the rule to block SSH, it won't affect the active SSH session but will affect new ones.

## NSG Priority
NSGs use priority to process the rules, lower the priority numbers win.
- Priority can be a value between 100 and 4096
- You can't have two rules with the same priority and direction (You could have two rules with the same priority but one is for inbound and the other for outbound)

## Routing Behaviours with NSGs
NSGs can be applied to NICs and VNETs, Microsoft recommend using one or the other and not both. For inbound traffic, NSGs associated to the VNET are processed first, if allowed then it will reach the VMs within where it may hit a second NSG associated to the VM where the traffic is processed against for a second time. Outbound traffic from a VM will be processed by the NSG associated with the VM's NIC and then secondly by the VNET's NSG if the intial NSG allows it through. 

Inter-VNET traffic can be affected if you were to create a rule in your NSG associated with the subnet to block all communication that can then stop the VMs within the subnet reaching one another. You can use the effective routes in the NIC overview, Network Watcher, IP Verify and Connection Troubleshooter to spot any dodgy rules causing routing issues.

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

