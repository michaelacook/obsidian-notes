# `ip` Command
## Overview
- `ip` is part of the **iproute2** suite
- `-c` option to colorize output
- Replaces deprecated commands: `ifconfig`, `route`
- Used to:
  - View/configure IP addresses and interfaces
  - Bring interfaces up/down
  - View and edit routes
  - View interface statistics

## Viewing IP addresses / interfaces
### Show all interfaces and IPs
    ip addr show
    # or:
    ip a
- Shows interface names, IPs, netmasks (CIDR), broadcast, MAC, state (`UP`/`DOWN`)

### Limit output to one interface
    ip addr show dev wlo1
- `dev` = device (interface)
- Can’t abbreviate this as `ip a` when using `dev`

## Bringing interfaces up and down
### Bring interface down
    sudo ip link set wlo1 down
- `link` = link layer (interface)
- `set` = change interface state/config
- Disconnects that interface (e.g. Wi-Fi / SSH)

### Bring interface up
    sudo ip link set wlo1 up
- Restores connectivity (DHCP/static must be correct)

## Adding / removing IP addresses
Assume interface: `wlo1`.

### Add an IP address
    sudo ip addr add 192.168.0.123/24 dev wlo1
- Adds an additional IP to the interface

### Remove an IP address
    sudo ip addr del 192.168.0.123/24 dev wlo1
- Removes that specific IP from the interface

### Reset DHCP address by toggling interface
    sudo ip link set wlo1 down
    sudo ip link set wlo1 up
- Often causes DHCP to reassign a correct address

## Viewing and managing routes
### Show routing table
    ip route show
    # or:
    ip r
- Typically shows:
  - Default route, e.g. `default via 172.16.250.1 dev wlo1`
  - Connected network, e.g. `172.16.250.0/24 dev wlo1 proto kernel src 172.16.250.105`
- Use this to troubleshoot reachability (no route = no access)

### Add a route
    sudo ip route add 10.10.10.0/24 via 172.16.250.1 dev wlo1
    sudo ip route add default via <gateway_ip_address>
- `10.10.10.0/24`: destination network
- `via 172.16.250.1`: gateway
- `dev wlo1`: interface

### Delete a route
    sudo ip route del 10.10.10.0/24 via 172.16.250.1 dev wlo1
- Same syntax as add, but with `del`

## Viewing interface statistics
### Show link-layer stats for an interface
    ip -s link show dev wlo1
- `-s` = statistics
- Shows RX/TX packets, errors, dropped packets, etc.

### Watch stats live
    watch "ip -s link show dev wlo1"
- Re-runs the command every 2 seconds
- Useful while testing network changes

## Minimal command cheat sheet
- Show IPs/interfaces:
  - `ip a`
  - `ip addr show dev <iface>`
- Bring interface up/down:
  - `sudo ip link set <iface> down`
  - `sudo ip link set <iface> up`
- Add/remove IP:
  - `sudo ip addr add <ip>/<cidr> dev <iface>`
  - `sudo ip addr del <ip>/<cidr> dev <iface>`
- Routing:
  - `ip r`
  - `sudo ip route add <net>/<cidr> via <gw> dev <iface>`
  - `sudo ip route del <net>/<cidr> via <gw> dev <iface>`
- Statistics:
  - `ip -s link show dev <iface>`
  - `watch "ip -s link show dev <iface>"`
