## VPC

- minimum `/28` (16 ip address)
- maximum `/16` (65,536 ip address)

## DNS

- provide by Route53
- vpc `Base IP + 2` Address
- `enableDnsHostname` - give instances DNS names
- `enableDnsSupport` - enables DNS resolution in VPC (the +2 address)

## Subnet IP addressing

- 5 total reserved IP address
- 10.16.16.0/20
- `Network` address (10.16.16.0/20)
- `Network + 1` (10.16.16.1) - VPC router
- `Network + 2` (10.16.16.2) - Reserved DNS
- `Network + 3` (10.16.16.3) - Reserved for future use
- `Broadcast` addresss 10.16.31.255 (Last IP in subnet)

**Note:** If a subnet should have 16 usable IP address, it really only has 11.

## DHCP option set

- CANNOT be modified (must create new)
