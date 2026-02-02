# Configuring IPsec for GRE Tunnel

This guide provides simple instructions to configure IPsec for a GRE tunnel between two routers. Replace `router 1 WAN address` and `router 2 WAN address` with the actual WAN IP addresses of the routers.

```
! Step 1: Configure the GRE Tunnel
interface Tunnel0
 ip address 10.0.0.1 255.255.255.252         ! On Router 1
 tunnel source GigabitEthernet0/0/1
 tunnel destination router 2 WAN address
 no shutdown

interface Tunnel0
 ip address 10.0.0.2 255.255.255.252         ! On Router 2
 tunnel source GigabitEthernet0/0/1
 tunnel destination router 1 WAN address
 no shutdown

! Step 2: Configure ISAKMP Policy (Phase 1)
crypto isakmp policy 10
 encryption aes
 hash sha256
 authentication pre-share
 group 2
 lifetime 86400
exit

crypto isakmp key mysharedkey address router 2 WAN address  ! On Router 1
crypto isakmp key mysharedkey address router 1 WAN address  ! On Router 2

! Step 3: Configure IPsec Transform Set (Phase 2)
crypto ipsec transform-set TRANSFORM_SET esp-aes esp-sha-hmac

! Step 4: Create a Crypto Map
crypto map CRYPTO_MAP 10 ipsec-isakmp
 set peer router 2 WAN address                           ! On Router 1
 set transform-set TRANSFORM_SET
 match address 100
exit

crypto map CRYPTO_MAP 10 ipsec-isakmp
 set peer router 1 WAN address                           ! On Router 2
 set transform-set TRANSFORM_SET
 match address 100
exit

! Step 5: Configure the Access List for GRE Traffic
access-list 100 permit gre host router 1 WAN address host router 2 WAN address  ! On Router 1
access-list 100 permit gre host router 2 WAN address host router 1 WAN address  ! On Router 2

! Step 6: Apply the Crypto Map to the WAN Interface
interface GigabitEthernet0/0/1
 crypto map CRYPTO_MAP                                   ! On Both Routers

! Step 7: Verify the Configuration
show crypto isakmp sa
! State should be QM_IDLE.

show crypto ipsec sa
! Look for incrementing counters under #pkts encaps and #pkts decaps.
