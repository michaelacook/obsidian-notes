- Networks = 2$^n$ where n = network bits
- Hosts   = 2$^h$ - 2 where h = host bits
-------------------------------------------------------------------

### Determine Subnet ID:

1. Determine interesting octet of the subnet mask. This is the octet that is neither 255 nor 0.
2. Everything to the left of the interesting octet in the IP address is part of the subnet ID.
3. Everything to the right of the interesting octet in the IP address is part of the subnet ID. Typically a zero.
4. Magic Number = 256 - Interesting Octet value.
5. Multiply the magic number until it is the highest possible value that is less than or equal to the octet of the IP address that corresponds to the interesting octet of the mask.
6. Write out all values. You have the subnet ID.

E.g. 172.21.92.254
     255.255.240.0

1. Interesting octet is 240
2. 255.255.x.x
3. 255.255.x.0
4. Magic Number = 256 - 240 = 16
5. 16 x 5 = 80, which is >= to 92
6. Subnet ID = 172.21.80.0

### Determine broadcast address:

1. Add the magic number to the interesting octet of the subnet ID and subtract 1.
2. Everything to the right of the interesting octet is 255.

E.g. (80 + 16) - 1 = 95

Broadcast address is 172.21.95.255

Determine first address in the subnet:

1. First address = subnet ID + 1

E.g. Broadcast address is 172.21.80.0 + 1 = 172.21.80.1

Determine usable IP range for the subnet:

1. Subnet ID + 1 to broadcast address - 1

E.g. 172.21.80.0 + 1 = 172.21.80.255 - 1
Usable IPs are 172.21.80.1 - 172.21.95.254
