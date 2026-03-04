# Netsh Command Reference Guide

> **Note:** Most `netsh` commands require an **elevated (Administrator) command prompt or PowerShell session.**

---

## Table of Contents

- [1. General Usage & Help](#1-general-usage--help)
- [2. Interface (IP Configuration)](#2-interface-ip-configuration)
- [3. IP Address & Gateway Management](#3-ip-address--gateway-management)
- [4. DNS Configuration](#4-dns-configuration)
- [5. DHCP](#5-dhcp)
- [6. Firewall (Windows Defender Firewall)](#6-firewall-windows-defender-firewall)
- [7. WLAN (Wi-Fi)](#7-wlan-wi-fi)
- [8. Routing](#8-routing)
- [9. Port Proxy (Port Forwarding)](#9-port-proxy-port-forwarding)
- [10. HTTP.sys (Web Server Settings)](#10-httpsys-web-server-settings)
- [11. Namespace / WinSock](#11-namespace--winsock)
- [12. Diagnostics & Exporting Config](#12-diagnostics--exporting-config)

---

## 1. General Usage & Help

```c
:: Display top-level help
netsh /?

:: List all available netsh contexts (namespaces)
netsh show helper

:: Enter an interactive netsh shell session
:: From inside the shell, type commands without the 'netsh' prefix
netsh

:: Get help for a specific context
netsh interface /?
netsh advfirewall /?
```

---

## 2. Interface (IP Configuration)

```c
:: List all network interfaces and their state (enabled/disabled)
netsh interface show interface

:: Show detailed IP configuration for all interfaces
netsh interface ip show config

:: Show IP addresses assigned to all interfaces
netsh interface ip show addresses

:: Enable a network interface by name
netsh interface set interface name="Ethernet" admin=enabled

:: Disable a network interface by name
netsh interface set interface name="Ethernet" admin=disabled

:: Rename a network interface
netsh interface set interface name="Ethernet" newname="LAN"
```

---

## 3. IP Address & Gateway Management

```c
:: Assign a static IP address to an interface
:: Syntax: name=<interface> source=static address=<IP> mask=<subnet> gateway=<GW>
netsh interface ip set address name="Ethernet" source=static address=192.168.1.100 mask=255.255.255.0 gateway=192.168.1.1

:: Switch an interface back to DHCP (dynamic IP)
netsh interface ip set address name="Ethernet" source=dhcp

:: Add a secondary (additional) IP address to an interface
netsh interface ip add address name="Ethernet" address=10.0.0.5 mask=255.255.255.0

:: Remove a specific IP address from an interface
netsh interface ip delete address name="Ethernet" address=10.0.0.5

:: Show the routing table (all IP routes)
netsh interface ip show route

:: Add a persistent static route
:: Syntax: add route prefix=<dest/mask> interface=<name> nexthop=<gateway>
netsh interface ip add route prefix=10.10.0.0/16 interface="Ethernet" nexthop=192.168.1.1

:: Delete a static route
netsh interface ip delete route prefix=10.10.0.0/16 interface="Ethernet" nexthop=192.168.1.1
```

---

## 4. DNS Configuration

```c
:: Show current DNS server settings for an interface
netsh interface ip show dns name="Ethernet"

:: Set a primary static DNS server
netsh interface ip set dns name="Ethernet" source=static address=8.8.8.8

:: Add a secondary DNS server (index=2 means second in the list)
netsh interface ip add dns name="Ethernet" address=8.8.4.4 index=2

:: Revert DNS to automatic (DHCP-assigned)
netsh interface ip set dns name="Ethernet" source=dhcp

:: Flush (clear) the local DNS resolver cache
netsh interface ip delete arpcache
:: Note: for DNS cache specifically, use: ipconfig /flushdns
```

---

## 5. DHCP

```c
:: Check whether an interface is using DHCP or a static address
netsh interface ip show address name="Ethernet"

:: Force an interface to use DHCP for both IP and DNS
netsh interface ip set address name="Ethernet" source=dhcp
netsh interface ip set dns name="Ethernet" source=dhcp

:: Show all DHCP-related settings across all interfaces
netsh interface ip show config
```

---

## 6. Firewall (Windows Defender Firewall)

> Use `netsh advfirewall` for modern Windows (Vista+). The legacy `netsh firewall` context is deprecated.

```c
:: Show the current firewall state for all profiles (domain, private, public)
netsh advfirewall show allprofiles

:: Turn the firewall ON for all profiles
netsh advfirewall set allprofiles state on

:: Turn the firewall OFF for all profiles (use with caution)
netsh advfirewall set allprofiles state off

:: Set default inbound/outbound policy to block for all profiles
netsh advfirewall set allprofiles firewallpolicy blockinbound,blockoutbound

:: Allow inbound traffic and block outbound (common hardened stance)
netsh advfirewall set allprofiles firewallpolicy allowinbound,blockoutbound

:: --- INBOUND RULES ---

:: Allow inbound TCP traffic on port 80 (HTTP)
netsh advfirewall firewall add rule name="Allow HTTP" protocol=TCP dir=in localport=80 action=allow

:: Allow inbound TCP traffic on port 443 (HTTPS)
netsh advfirewall firewall add rule name="Allow HTTPS" protocol=TCP dir=in localport=443 action=allow

:: Block inbound traffic from a specific IP address
netsh advfirewall firewall add rule name="Block Bad IP" dir=in remoteip=203.0.113.5 action=block

:: Allow inbound RDP (port 3389) from a specific subnet only
netsh advfirewall firewall add rule name="RDP from LAN" protocol=TCP dir=in localport=3389 remoteip=192.168.1.0/24 action=allow

:: --- OUTBOUND RULES ---

:: Block outbound traffic to a specific IP address
netsh advfirewall firewall add rule name="Block Outbound IP" dir=out remoteip=198.51.100.10 action=block

:: Allow a specific program through the firewall (inbound + outbound)
netsh advfirewall firewall add rule name="Allow MyApp" dir=in action=allow program="C:\MyApp\myapp.exe" enable=yes
netsh advfirewall firewall add rule name="Allow MyApp Out" dir=out action=allow program="C:\MyApp\myapp.exe" enable=yes

:: --- MANAGING RULES ---

:: List all firewall rules
netsh advfirewall firewall show rule name=all

:: Show a specific rule by name
netsh advfirewall firewall show rule name="Allow HTTP"

:: Delete a firewall rule by name
netsh advfirewall firewall delete rule name="Allow HTTP"

:: Enable or disable an existing rule without deleting it
netsh advfirewall firewall set rule name="Allow HTTP" new enable=yes
netsh advfirewall firewall set rule name="Allow HTTP" new enable=no

:: --- PROFILES ---

:: Configure settings for only the Public profile
netsh advfirewall set publicprofile state on
netsh advfirewall set publicprofile firewallpolicy blockinbound,allowoutbound

:: Export firewall policy to a file (useful for backup/replication)
netsh advfirewall export "C:\Backups\firewall-policy.wfw"

:: Import a previously exported firewall policy
netsh advfirewall import "C:\Backups\firewall-policy.wfw"

:: Reset firewall to factory defaults (removes all custom rules)
netsh advfirewall reset
```

---

## 7. WLAN (Wi-Fi)

```c
:: Show the status of the WLAN AutoConfig service and radio state
netsh wlan show all

:: List all available Wi-Fi networks (SSIDs) within range
netsh wlan show networks mode=bssid

:: Show all saved (remembered) Wi-Fi profiles on this machine
netsh wlan show profiles

:: Show detailed info for a saved profile, including security type
netsh wlan show profile name="MyNetwork"

:: Export a Wi-Fi profile to XML (includes settings, but NOT the password by default)
netsh wlan export profile name="MyNetwork" folder="C:\WiFiBackup"

:: Export profile WITH the plaintext key (password) — handle with care
netsh wlan export profile name="MyNetwork" key=clear folder="C:\WiFiBackup"

:: Import/add a Wi-Fi profile from an XML file
netsh wlan add profile filename="C:\WiFiBackup\MyNetwork.xml"

:: Connect to a saved Wi-Fi network
netsh wlan connect name="MyNetwork"

:: Disconnect from the current Wi-Fi network
netsh wlan disconnect

:: Delete a saved Wi-Fi profile
netsh wlan delete profile name="MyNetwork"

:: Show the current Wi-Fi connection details (SSID, signal, channel, etc.)
netsh wlan show interfaces

:: Enable or disable the wireless adapter radio
netsh wlan set radiostate interface="Wi-Fi" enabled
netsh wlan set radiostate interface="Wi-Fi" disabled

:: View Wi-Fi driver capabilities (supported bands, modes, etc.)
netsh wlan show drivers

:: Show Wi-Fi usage statistics per interface
netsh wlan show statistics
```

---

## 8. Routing

```c
:: Show the full IP routing table
netsh interface ip show route

:: Add a persistent static route to a remote subnet
:: Reach 172.16.0.0/12 via gateway 192.168.1.254 on Ethernet
netsh interface ip add route prefix=172.16.0.0/12 interface="Ethernet" nexthop=192.168.1.254

:: Add a host route (route to a single IP, /32 mask)
netsh interface ip add route prefix=10.0.0.1/32 interface="Ethernet" nexthop=192.168.1.1

:: Delete a static route
netsh interface ip delete route prefix=172.16.0.0/12 interface="Ethernet" nexthop=192.168.1.254

:: Show IPv6 routes
netsh interface ipv6 show route
```

---

## 9. Port Proxy (Port Forwarding)

> `netsh interface portproxy` allows you to forward traffic from one IP:port to another — useful for WSL2, VMs, or redirecting services.

```c
:: Forward inbound traffic on local port 8080 to a remote host on port 80
:: (v4tov4 = IPv4 source to IPv4 destination)
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=192.168.1.50

:: Forward a port into a WSL2 instance (common use case)
:: Replace <WSL_IP> with the IP shown by 'wsl hostname -I'
netsh interface portproxy add v4tov4 listenport=3000 listenaddress=0.0.0.0 connectport=3000 connectaddress=<WSL_IP>

:: List all active port proxy rules
netsh interface portproxy show all

:: Remove a specific port proxy rule
netsh interface portproxy delete v4tov4 listenport=8080 listenaddress=0.0.0.0

:: Remove all port proxy rules at once
netsh interface portproxy reset
```

---

## 10. HTTP.sys (Web Server Settings)

> These commands manage the Windows HTTP kernel driver (HTTP.sys), used by IIS, WCF, Kestrel, and other HTTP servers.

```c
:: List all URL reservations (which users can host on which URLs)
netsh http show urlacl

:: Reserve a URL prefix so a non-admin account can host an HTTP listener
:: Replace <DOMAIN\User> with the actual Windows account
netsh http add urlacl url=http://+:8080/ user="DOMAIN\User"

:: Remove a URL reservation
netsh http delete urlacl url=http://+:8080/

:: List all SSL/TLS certificate bindings (maps port to certificate thumbprint)
netsh http show sslcert

:: Bind an SSL certificate to a port using its thumbprint
:: appid must be a GUID in braces — use any GUID for custom apps
netsh http add sslcert ipport=0.0.0.0:443 certhash=<THUMBPRINT> appid="{00000000-0000-0000-0000-000000000001}"

:: Remove an SSL certificate binding
netsh http delete sslcert ipport=0.0.0.0:443

:: Show active HTTP.sys connections and request queue stats
netsh http show servicestate
```

---

## 11. Namespace / WinSock

```c
:: Reset the Winsock catalog to a clean state
:: Fixes many network connectivity issues caused by corrupt LSP entries
:: A REBOOT IS REQUIRED after running this command
netsh winsock reset

:: Show the current Winsock catalog entries
netsh winsock show catalog

:: Reset TCP/IP stack to defaults (use alongside winsock reset for full network reset)
:: Also requires a reboot
netsh int ip reset

:: Reset IPv6 stack to defaults
netsh int ipv6 reset

:: Full network stack reset — run all four commands, then reboot
netsh winsock reset
netsh int ip reset
netsh int ipv6 reset
ipconfig /flushdns
```

---

## 12. Diagnostics & Exporting Config

```c
:: Export the ENTIRE current netsh configuration to a script file
:: This captures all contexts: interfaces, firewall, routing, proxies, etc.
netsh -c "interface" dump > C:\Backups\interface-config.txt
netsh -c "advfirewall" dump > C:\Backups\firewall-config.txt

:: Dump complete system network config (all contexts at once)
netsh dump > C:\Backups\full-netsh-config.txt

:: Re-apply a previously dumped configuration file
netsh exec C:\Backups\full-netsh-config.txt

:: Run netsh commands from a script file (batch execution)
netsh -f C:\Scripts\network-setup.txt

:: Show network interface statistics (packets sent, errors, etc.)
netsh interface ip show ipstats

:: Show ICMP statistics
netsh interface ip show icmpstats

:: Show TCP connection statistics
netsh interface ip show tcpstats

:: Show UDP statistics
netsh interface ip show udpstats

:: Display the ARP cache (IP-to-MAC address mappings)
netsh interface ip show arpcache

:: Clear the ARP cache for a specific interface
netsh interface ip delete arpcache name="Ethernet"
```

---

_Reference guide generated March 2026. Commands apply to Windows 10/11 and Windows Server 2016+._