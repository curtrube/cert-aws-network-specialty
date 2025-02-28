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

## Stateful vs. Stateless Firewall

- Network Access Control Lists (NACL) is `stateless`
- Security Groups (SG) are `stateful`

## Network Access Control Lists (NACL)

- stateless
- protects at the subnet boundary
- rules are evalulatd from top to bottom
- NACL's are associated with subnets (1 or more)

## Security Groups (SG's)

- stateful
- no explicit deny
- attached to ENI's not instances
- logical references (sg can reference another sg)

## VPC Flow Logs

- can be applied at `VPC`, `Subnet`, or `ENI`
- flow logs are not `realtime`
- capture flow log records (not content)

## IPv4

- addresses are `private` or `public`


## IPv6

- no need for NAT (use a `Egress-Only IGW` to achieve `private` subnet behavior)
- IGW can handle both IPv4 & IPv6 traffic


## Internet Gateway (IGW)

- (IPv4) 1:1 `Static NAT` - Private IPv4 <=> Public IPv4
- (IPv6) `Routes` IPv6 packets IN and OUT of a VPC

## Egress-Only Internet Gateway

- Egress-Only is `outbound-only` for IPv6

## NAT Gateway

- 5Gbps by default... can scale to 45Gbps
- Want more? ... split resources into mulitple subnets and use multiple `NATGW's`
- Elastic IP cannot be changed on a `NAT GW`
- CANNOT be used accross `VPC Peers`, `Site-to-Site VPN` connections, or `AWS Direct Connect`
- 55,000 simultaneous connections to each unique destionation
- `ErrorPortAllocation`
- `NO Security Groups on NAT Gateway`

## AWS Private Link

- Highly-Available via `multiple endpoints`
- IPv4 & TCP Only (IPv6 isn't supported)
- Private DNS is supported (verified domains)
- `Direct Connect`, `Site-to-Site VPN` and `VPC Peering` are supported

## VPC Endpoints

### Gateway Endpoints

- Provide `private access` to `S3` and `DynamoDB`
- Prefix List added to route table => Gateway Endpoint
- Highly Available (HA) accross all AZ's in a region by default
- Endpoint policy is used to control what it can access
- Regional ... can't access cross-region services
- Prevent Leaky Buckets - S3 buckets can be set to private only by allowing access ONLY from a gateway endpoint
- Only accessible from within the VPC

### Interface Endpoints

- Provide `private access` to AWS Public Services
- historically... any service NOT `S3` and `DynamoDB`... but `S3` is now supported by interface endpoints
- Not `HA` must add to specific subnets in specific AZs to achieve `HA`
- Added to specific subnets - an ENI - not `HA`
- For HA, add one endpoint, to one subnet, per AZ used in the VPC
- Network access controlled via `Security Groups`
- `Endpoint Policies` - restrict what can be done with the endpoint
- `TCP` and `IPv4` Only
- Uses `PrivateLink`

## ELB

- ELB is a DNS A Record pointing at 1+ Nodes per AZ
- Nodes (in one subnet per AZ) can scale to more
- Internet-facing allocated with public IPv4 IP's
- Internal is private IP's
- EC2 doesn't need to be public to work with a ELB
- Listener configuration controls WHAT the LB does
- 8+ free IP's per subnet, and /27 subnet to allow for scaling







