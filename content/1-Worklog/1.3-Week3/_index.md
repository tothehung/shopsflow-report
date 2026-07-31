---
title: "Worklog Week 3"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

- **Start Date:** June 15, 2026
- **Completion Date:** June 20, 2026

### Objectives for Week 3:
- Theoretical research on Custom VPC virtual networks, CIDR IP blocks, Public/Private Subnets, and Internet Gateways
- Study network firewall security: Stateful Security Groups vs Stateless Network ACLs (NACLs) & VPC Flow Logs
- Hands-on deployment of Multi-AZ Custom VPC, NAT Gateway for Private Subnets, and SSH Bastion Host

### Tasks to implement this week:
| Day | Task |
| --- | --- |
| 2 | Study Amazon VPC theory: CIDR Block IP allocation (`10.0.0.0/16`), Public vs Private Subnet topology, Route Tables, and Internet Gateway (IGW) mechanics. |
| 3 | Research network security enforcement: Stateful Security Groups (instance-level firewall) vs Stateless Network ACLs (subnet-level firewall) and Inbound/Outbound rules. |
| 4 | Hands-on AWS Lab (Custom VPC Multi-AZ Architecture): Create Custom VPC, split into 2 Public Subnets & 2 Private Subnets across 2 distinct Availability Zones. |
| 5 | Hands-on AWS Lab (NAT Gateway & Bastion Host Setup): Allocate Elastic IP (EIP), deploy NAT Gateway enabling Private Subnet outbound internet, and setup SSH Bastion Host. |
| 6 | Enable VPC Flow Logs streaming to CloudWatch Log Groups, audit SSH security access paths, and compile Week 3 progress report. |

### Results achieved in Week 3:
* Completed Week 3 tasks on schedule (Networking & Custom VPC Architecture Labs).
* Designed and built a secure Multi-AZ Custom VPC virtual network following AWS best practices.

### References & Study Materials:
- [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)
- [AWS Security Groups & Network ACLs Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)
- [AWS NAT Gateway Setup & Administration](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [AWS VPC Flow Logs Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
