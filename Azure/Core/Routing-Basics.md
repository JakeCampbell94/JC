# Routing Basics
Azure will create a route table per subnet which will have routing rules that are automatically setup (These are called system routes) and you can add in customer User-Defined Routes to control routing. The default system routes cannot be modified or removed but you can over-ride them with custom routes.

## System Routes
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

Entry 1: This is for routing to internal VNet addresses, the next hop is Vitual Network whcih means traffic is routed internally within the VNet. It will go to the gateway and then tothe internal subnet resource.
For VNets with multiple address spaces, an entry is added for each space.
Entry 2: Basically a default route, anything not matching another route uses this. It will forward the traffic to the internet externally of your Azure environment.
This has exceptions if the address it is going to is actually hosted in Azure then it won't leave the backbone.
Entry 3 to 5: These add in the RFC 1918 addresses to stop them being routed over the internet.
Entry 6: This stops RFC 6598 address space from being routed on the internet.
Entry 7 to 18: These are the Azure Management public IP ranges aren't routed over the internet.
