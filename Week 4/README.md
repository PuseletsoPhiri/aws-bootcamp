Week 4 — Building a Secure and Scalable AWS Network

Topic: Building a Secure and Scalable AWS Network Using VPCs, Subnets, and Routing Controls
Original session date: July 5, 2025

Goals
- Create VPC-A with CIDR block 10.10.0.0/16
- Create VPC-B with CIDR block 10.20.0.0/16
- Create one public subnet and one private subnet in each VPC
- Establish a VPC peering connection between VPC-A and VPC-B
- Update route tables in each VPC to allow traffic to flow between them via the peering connection

What I did
Created two separate VPCs to simulate a multi-VPC architecture:
- VPC-A — CIDR block `10.10.0.0/16`
- VPC-B— CIDR block `10.20.0.0/16`

In each VPC, created one public subnet and one private subnet. No
Internet Gateway or NAT Gateway was attached, since the task was
focused purely on VPC-to-VPC connectivity rather than internet access.

Set up a VPC peering connection between VPC-A and VPC-B, and
accepted the peering request so the connection became **Active**.

Updated the route tables in both VPCs so each one has a route
pointing to the other VPC's CIDR block via the peering connection —
i.e. VPC-A's route table routes `10.20.0.0/16` traffic through the
peering connection, and VPC-B's route table routes `10.10.0.0/16`
traffic the same way.

Key takeaways
- VPC peering only works one connection at a time (no transitive
  peering) — each VPC pair needs its own explicit peering connection
  and matching route table entries on both sides.
- Peering doesn't require an Internet Gateway or NAT Gateway at all —
  it's a private, direct connection between VPCs, which is why this
  task didn't need either.
- Route tables have to be updated **on both sides** of the peering
  connection — updating only one VPC's route table would let traffic
  flow one way but not the other.
- CIDR blocks for peered VPCs must not overlap, which is why
  10.10.0.0/16 and 10.20.0.0/16 were chosen as distinct ranges.

Screenshots / Evidence
**VPC-A and VPC-B with their CIDR blocks**
![VPCs and CIDR blocks](screenshots/vpcs-cidr-blocks.png)

**Public and private subnets in each VPC**
![Subnets in each VPC](screenshots/subnets-per-vpc.png)

**VPC peering connection — Active status**
![Peering connection active](screenshots/peering-connection-active.png)

**Route tables with routes to the peer VPC's CIDR block**
![Route tables with peering routes](screenshots/route-tables-peering.png)