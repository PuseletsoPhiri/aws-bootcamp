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
<img width="1910" height="931" alt="Screenshot 2026-07-26 134129" src="https://github.com/user-attachments/assets/64de1013-9b70-4b42-bbb3-007058956b41" />
<img width="1916" height="966" alt="Screenshot 2026-07-26 134233" src="https://github.com/user-attachments/assets/cb124f8b-d2d9-4826-a10a-0f755d226be1" />

**Public and private subnets in each VPC**
<img width="1917" height="957" alt="Screenshot 2026-07-27 093653" src="https://github.com/user-attachments/assets/4532274f-f3d0-46ee-aca4-43889aef3aae" />

**VPC peering connection — Active status**
<img width="1912" height="961" alt="Screenshot 2026-07-27 094000" src="https://github.com/user-attachments/assets/bd866cf3-a0e8-4fa6-b6a9-d988a8c7d7f7" />

**Route tables with routes to the peer VPC's CIDR block**
<img width="1917" height="965" alt="Screenshot 2026-07-27 094217" src="https://github.com/user-attachments/assets/1c4a1a35-5036-45b5-9115-92357b038bc6" />
<img width="1915" height="967" alt="Screenshot 2026-07-27 094155" src="https://github.com/user-attachments/assets/427ccfbc-d7cf-4dd7-8c43-5e2a4bd45387" />
