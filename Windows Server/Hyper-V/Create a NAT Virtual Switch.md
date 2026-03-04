- How to create a virtual switch that performs network address translation
- Can only be done with PowerShell

```powershell

# --- STEP 1 - Create the Internal Virtual Switch ---
#
# Creates a new virtual switch named "NAT-vSwitch" with type Internal.
# An Internal switch allows communication between the Hyper-V host and its VMs,
# but does not directly bind to a physical NIC - which is required for NAT to work correctly.
New-VMSwitch -SwitchName "NAT-vSwitch" -SwitchType Internal


# --- STEP 2 - Find the Interface Index ---
#
# Lists all network adapters visible to the host OS, including the virtual adapter
# just created by New-VMSwitch. You need the ifIndex value of the "NAT-vSwitch" adapter
# for Step 3, as it tells Windows which interface to assign the gateway IP to.
Get-NetAdapter


# --- STEP 3 - Assign the Gateway IP Address ---
#
# Assigns an IP address to the Hyper-V host's virtual network adapter (identified by InterfaceIndex).
# This IP (192.168.1.1) becomes the default gateway for VMs on this virtual switch.
# PrefixLength 24 defines the subnet mask (/24 = 255.255.255.0), giving you the range 192.168.1.0-192.168.1.254.
# NOTE: Replace InterfaceIndex 19 with the actual ifIndex value returned by Get-NetAdapter in Step 2.
New-NetIPAddress -IPAddress 192.168.1.1 -PrefixLength 24 -InterfaceIndex 19


# --- STEP 4 - Create the NAT Object ---
#
# Creates the NAT object in Windows, named "NATNetwork".
# InternalIPInterfaceAddressPrefix defines the internal subnet that NAT will service (192.168.1.0/24).
# Any VM on this subnet sending traffic outside will have its source IP translated to the host's
# external IP address - this is what provides internet/network access to the VMs.
New-NetNat -Name "NATNetwork" -InternalIPInterfaceAddressPrefix 192.168.1.0/24


########################################
# REMINDER: Adjust IP addresses, subnet,
# switch name, and InterfaceIndex to
# suit your environment.
########################################

# View NAT sessions
Get-NetNatSession
```
