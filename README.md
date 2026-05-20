# azure-networking-subnets
Hands-on lab: Creating and configuring Azure Subnets inside a Virtual Network, and deploying a VM into a specific subnet for AZ-104 exam preparation.

# Azure Networking - Subnets Lab

## Objective
Deploy and manage Azure Subnets inside a Virtual Network including creating 
multiple subnets, understanding IP address ranges, and assigning a Virtual 
Machine to a specific subnet for AZ-104 exam preparation.

## Environment
- Microsoft Azure Free Account
- Resource Group: rg1
- Region: Australia East
- Subscription: Azure Subscription 1

## VNet & Subnet Configuration

| Setting | Value |
|---------|-------|
| VNet Name | vnet1 |
| Resource Group | rg1 |
| Region | Australia East (Asia Pacific) |
| Subscription | Azure Subscription 1 |
| Subnet 1 | sub-net1 — 10.0.0.0/24 |
| Subnet 2 | sub-net2 — 10.0.1.0/24 |
| VM Subnet | sub-net2 (vm1 deployed here) |
| Status | Deployed Successfully |

## What is an Azure Subnet?
A subnet is a logical division of an Azure Virtual Network. It segments the VNet 
address space into smaller ranges so you can isolate workloads, apply security 
rules, and control routing independently per segment. When a resource like a VM 
is deployed into a subnet, Azure automatically assigns it a private IP address 
from that subnet's address range.

## Tasks Completed
- [x] Created Virtual Network (vnet1)
- [x] Configured address space
- [x] Created Resource Group (rg1)
- [x] Renamed default subnet to sub-net1 (10.0.0.0/24)
- [x] Created second subnet sub-net2 (10.0.1.0/24)
- [x] Deployed VM (vm1) into sub-net2
- [x] Verified private IP assignment (10.0.1.4) from sub-net2 range
- [ ] Configure Network Security Group on subnets
- [ ] Test connectivity between sub-net1 and sub-net2
- [ ] Configure VNet Peering

## Screenshots

### 1. Create Virtual Network - Basic Configuration

<img width="953" height="536" alt="Screenshot 2026-05-19 165317" src="https://github.com/user-attachments/assets/02a2c5e3-f021-4f44-90c9-9b5b467edd97" />

### 2. VNet Deployment Complete

<img width="959" height="535" alt="Screenshot 2026-05-19 165416" src="https://github.com/user-attachments/assets/f712a5b3-6daa-4b3b-a2cd-69c519f93e61" />

### 3. Subnets View — sub-net1 and sub-net2 Created

<img width="955" height="539" alt="Screenshot 2026-05-20 114741" src="https://github.com/user-attachments/assets/9921b5e0-db54-45b3-b60f-9ee370f61a94" />

### 4. Create VM — Basics Tab

<img width="955" height="535" alt="Screenshot 2026-05-20 115423" src="https://github.com/user-attachments/assets/22275fb8-ce1d-44f6-ae00-90f9661b64d6" />

### 5. VM Networking — Selecting sub-net2

<img width="959" height="533" alt="Screenshot 2026-05-20 115510" src="https://github.com/user-attachments/assets/332cdba3-3460-49e9-986d-191a489f1f04" />

### 6. VM Networking — sub-net2 Confirmed

<img width="959" height="536" alt="Screenshot 2026-05-20 115529" src="https://github.com/user-attachments/assets/bf54a846-b9a9-4ece-82be-517e6805a560" />

### 7. VM Network Settings — Subnet Assignment Verified

<img width="959" height="536" alt="Screenshot 2026-05-20 115758" src="https://github.com/user-attachments/assets/a10f0e49-2412-40d9-ab2a-8de0b5765d1e" />


## Key Concepts Learned

### What is a Subnet?
Subnets divide a VNet into smaller segments for isolation, security and routing control.

| Property | Detail |
|----------|--------|
| Purpose | Isolate and segment network traffic |
| Security | Apply NSGs to control inbound/outbound traffic |
| Azure Reserved IPs | 5 IPs reserved per subnet |
| Delegation | Can be delegated to PaaS services |

### The 5 Reserved IP Addresses Per Subnet
Azure automatically reserves 5 IP addresses in every subnet.

| Address | Purpose |
|---------|---------|
| x.x.x.0 | Network address |
| x.x.x.1 | Default gateway |
| x.x.x.2 | Azure DNS |
| x.x.x.3 | Azure DNS |
| x.x.x.255 | Broadcast address |

So a /24 subnet gives you 256 - 5 = **251 usable IP addresses**

### Subnets Created in This Lab

| Subnet Name | IPv4 CIDR | Available IPs | Used By |
|-------------|-----------|---------------|---------|
| sub-net1 | 10.0.0.0/24 | 251 | Available |
| sub-net2 | 10.0.1.0/24 | 251 | vm1 (10.0.1.4) |

### Private IP Address Ranges (RFC1918)

| Range | CIDR |
|-------|------|
| 10.0.0.0 to 10.255.255.255 | 10.0.0.0/8 |
| 172.16.0.0 to 172.31.255.255 | 172.16.0.0/12 |
| 192.168.0.0 to 192.168.255.255 | 192.168.0.0/16 |

### VNet vs Subnet

| Feature | VNet | Subnet |
|---------|------|--------|
| Scope | Entire private network | Segment of VNet |
| Address space | e.g. 10.0.0.0/16 | e.g. 10.0.1.0/24 |
| Security | VNet level isolation | NSG applied here |
| Resources | Contains subnets | Contains NICs/VMs |

### Private vs Public IP

| Type | Purpose | Allocation |
|------|---------|------------|
| Private IP | Internal communication inside VNet | Dynamic or Static |
| Public IP | Internet reachability | Static (Standard SKU) |

## Key Exam Scenarios

**Scenario 1:**
You have two VMs in different subnets of the same VNet.
Can they communicate?
- Answer: Yes, by default resources in the same VNet can communicate

**Scenario 2:**
How many usable IP addresses are in a /24 subnet?
- Answer: 256 minus 5 reserved = 251 usable addresses

**Scenario 3:**
You need to isolate traffic between two subnets. What do you use?
- Answer: Network Security Group (NSG)

**Scenario 4:**
A VM is deployed into sub-net2 (10.0.1.0/24). What private IP will it receive?
- Answer: An address from 10.0.1.4 onwards (first 3 are reserved after .0, .1, .2, .3)

**Scenario 5:**
You need to connect two VNets privately without going over the internet.
What do you configure?
- Answer: VNet Peering

**Scenario 6:**
A VM needs to be deployed into a VNet. What is required?
- Answer: The VM and VNet must be in the same region

## Important Notes
- VNets are region-specific — cannot span multiple regions
- VNet and VM must be in the same region to connect
- Each subnet must have a unique, non-overlapping address range within the VNet
- Peering is non-transitive — A peers B, B peers C does NOT mean A can reach C
- VNets are completely free — you only pay for connected resources
- Always plan your IP address space before creating subnets to avoid overlapping ranges
- A VM placed in sub-net2 (10.0.1.0/24) gets a 10.0.1.x address, NOT 10.0.0.x

## Next Steps
- [ ] Configure NSGs on sub-net1 and sub-net2
- [ ] Deploy a second VM into sub-net1 and test connectivity
- [ ] Set up VNet Peering between two VNets
- [ ] Configure Azure Bastion for secure VM access without public IP
- [ ] Set up Private DNS Zone

## References
- [Azure VNet Documentation](https://learn.microsoft.com/en-us/azure/virtual-network/)
- [Subnet Planning](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-vnet-plan-design-arm)
- [VNet Peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)
- [AZ-104 Study Guide](https://learn.microsoft.com/en-us/certifications/exams/az-104)
