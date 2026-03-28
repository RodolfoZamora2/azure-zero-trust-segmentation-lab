# Architecture

## Overview

This lab was built to model a simple Zero Trust network segmentation pattern in Azure.

The design separates resources into three trust zones inside a single Azure virtual network:

- **DMZ**
- **Internal**
- **Security**

The goal was to allow tightly controlled administrative access to the DMZ while preventing that subnet from communicating freely with more sensitive segments.

## Resource Layout

### Resource Group
- `ZeroTrust-RG`

### Virtual Network
- `ZeroTrust-VNet`

### Subnets

| Subnet | Address Range | Purpose |
|---|---|---|
| DMZ-Subnet | 10.0.1.0/24 | Externally reachable zone for administrative testing |
| Internal-Subnet | 10.0.2.0/24 | Protected internal segment |
| Security-Subnet | 10.0.3.0/24 | Isolated security segment |

### Workload
- `attacker-VM` in `DMZ-Subnet`

## NSG Design

### DMZ NSG
**NSG:** `dmz-subnet-nsg`

Purpose:
- allow SSH only from a trusted public IP
- deny outbound communication from DMZ to Internal and Security subnets

Implemented logic:
- Allow inbound TCP/22 from trusted public IP `/32`
- Deny outbound to `10.0.2.0/24`
- Deny outbound to `10.0.3.0/24`

### Internal NSG
**NSG:** `internal-subnet-nsg`

Purpose:
- block inbound traffic originating from the DMZ subnet

Implemented logic:
- Deny inbound traffic from `10.0.1.0/24`

### Security NSG
**NSG:** `security-subnet-nsg`

Purpose:
- block inbound traffic originating from the DMZ subnet

Implemented logic:
- Deny inbound traffic from `10.0.1.0/24`

## Enforcement Model

Originally, the VM had a NIC-level NSG. This was removed to avoid overlapping enforcement and to make the subnet the primary security boundary.

Final enforcement model:
- **Subnet NSGs active**
- **NIC NSG removed**
- **DMZ subnet acts as the controlled access zone**

## Traffic Intent

### Allowed
- Trusted public IP → DMZ VM on TCP/22

### Denied
- DMZ → Internal
- DMZ → Security

## Validation Method

Azure Network Watcher IP Flow Verify was used to test and confirm:

1. inbound SSH from the trusted public IP to the DMZ VM was allowed
2. outbound DMZ-to-Internal traffic was denied
3. outbound DMZ-to-Security traffic was denied

## Security Rationale

This architecture follows key Zero Trust ideas:

- assume network segments should not trust each other by default
- reduce administrative exposure
- limit east-west movement
- validate policy outcomes rather than assuming they work

## Diagram Summary

```text
Internet (trusted public IP only)
        |
        v
   DMZ-Subnet (10.0.1.0/24)
        |
   [dmz-test-vm]
        X
        | blocked to Internal
        X
        | blocked to Security

Internal-Subnet (10.0.2.0/24)
Security-Subnet (10.0.3.0/24)
