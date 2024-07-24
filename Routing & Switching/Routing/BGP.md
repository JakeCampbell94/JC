# BGP
## Overview
BGP is an EGP that talks over TCP Port 179 to neighbours. eBGP is used where neighbours are in a different AS and iBGP for neighbours in the same AS.

BGP uses 4 different message types:
- Open, Used to establish peerings
- Update, Exchange routing information
- Notification, Notify peers about errors
- Keepalives, ensures a neighbour is still active

## iBGP and eBGP
### iBGP Overview
- AD is 200 for iBGP routes
- The next hop address for a route doesn't change (Can be changed in the BGP config)
- Requires a full-mesh setup between neighbours in the AS
- iBGP neighbours can be multiple hops away
- iBGP learnt routes aren't forwarded to other iBGP peers so that's why you'll need a full mesh setup.

### eBGP Overview
- AD of 20 for eBGP routes
- The next hop address is changed
- eBGP routes can be sent on to iBGP and eBGP neighbours

