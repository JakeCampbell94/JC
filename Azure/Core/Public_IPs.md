# Public IP Addresses
Public IPs can be allocated to:
- Virtual machine network interfaces
- Virtual machine scale sets
- Public Load Balancers
- Virtual Network Gateways (VPN/ER)
- NAT gateways
- Application Gateways
- Azure Firewall
- Bastion Host
- Route Server

Public IPs can also either be dynamic and static.
- Dynamic is the default, it dynamically assigns a public IP from a pool when the resource is created or started and then removed when it is stopped or deleted, returning to the pool.
- Static will keep the public IP assocated with the resource, even when it is turned off, until it is un-associated with the resource by someone deleting the resource or changing it back to dynamic.

Each Azure Region has a pool of Public IPs you can use for your resources. There are IPv4 and IPv6 public IPs available as well as zone redundancy depending on your public IP SKU and region you are in. You set up a public IP prefix with the amount of IPs you need defined by the mask and then once created you can allocate out the public IPs you have out to your resources.

There is BYOIP (Bring Your Own IP) where you can add in any public IPs you own to use within Azure
