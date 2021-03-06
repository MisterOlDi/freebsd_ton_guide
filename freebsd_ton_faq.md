# FreeBSD Telegram Open Network FAQ
Attention: This section is big chaos and WIP more then anything else...

# Common node error messages
### [Error : 651 : no nodes]
This happens if your new node cannot connect to any DHT server defined in *global configuration* of your network. It basically means that either your node has no access to internet or the network you are trying to connect to experiences problems.

### [Error : 651 : state not found]
**[TODO]**

### [Error : 652 : net error:  adnl query timeout]
Very common error to see in logs, **[TODO]**: describe what exactly happens here.

# Useful commands
### Convert key HEX representation to Base64
> echo "##HEX##" | xxd -r -p | openssl base64

Example:
> echo "0DF90396788B6CEC38585A980DD346A0FD5122710338B0B014A94059D0B08CB6" | xxd -r -p | openssl base64

Result: `DfkDlniLbOw4WFqYDdNGoP1RInEDOLCwFKlAWdCwjLY=`

### Convert key base64  representation to HEX
> echo "##BASE64##" | openssl base64 -d | xxd -p -u -c 34

Example:
> echo "DfkDlniLbOw4WFqYDdNGoP1RInEDOLCwFKlAWdCwjLY=" | openssl base64 -d | xxd -p -u -c 34

Result: `0DF90396788B6CEC38585A980DD346A0FD5122710338B0B014A94059D0B08CB6`

# System Requirements
## Hardware
### Disk space
Data / work directories should be located on very fast disks (NVMe SSDs), but the space on such drives is expensive, luckily there are several large directories you can safely offload to slower spinner drives:
* _nodeWorkDir/log_
* _nodeWorkDir/db/archive_

## Connectivity
* Fixed IP Address:
Either directly on the net or via NAT in 1:1 mapping. It is also possible to implement port forwarting NAT in this case you can share one fixed IP with other hosts as long as all incoming connections to chosen UDP port are forwarted to your Node. Please consult your Firewall documentation on how to setup this.

#### Note on link speed
TON node traffic is close to symmetric, so you will have bandwidth usage in both up and downlink. If you have 1gbps down and 100mbps up line it will more or less handicap you at 100mbps.

On `testnet2` it was quite common to see bandwidth usage of 120mbps (up and down, so 240mbps total) during day-to-day operations and spikes of up to 800mbps.

# Troubleshooting
## Connectivity
### Validate IP address
In order to find out your node's public IP Address from CLI do:
> host myip.opendns.com resolver1.opendns.com

# Node configuration
## Changing IP address of a node
You can public IP Address of your node by editting edit your *local configuration* file and adjusting value of *addrs[].ip* field.

***Attention***: This field uses decimal format to represent IP address, you can use [ip2dec.sh](./support/ip2dec.sh) shell script to convert IPV4 format into decimal number. There is also a [dec2ip.sh](./support/dec2ip.sh) script to convert decimal into IPV4.

**TODO**: Will change of IP cause problems with availability of validator? Must find out.

## Changing port of a node
You can public port of your node by editting edit your *local configuration* file and adjusting value of *addrs[].port* field.

**TODO**: Will change of port cause problems with availability of validator? Must find out.

# Security
# Hardening
#### Note on security
I highly advise to take security seriously on PoS validator nodes, it would be very advisable to start doing this even on testnet, it is much more difficult to harden instances then to start them hardened in the first place.

This means:
* Place your validator behind firewall (remember, all what TON node needs are incoming UDP connections on some preselected ports, all other connections should be denied).
* Run your validator node in a container or dedicated FreeBSD jail
* Run your validator process under non-priveledged user, it does not require root!

* Outgoing UDP connections to all
* Incoming UDP connections to port(s) chosen as node ports
