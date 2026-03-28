# Azure Zero Trust Segmentation Lab

Built a segmented Azure lab to simulate a Zero Trust network design using subnet-level Network Security Groups (NSGs), least-access administrative exposure, and validated traffic controls with Azure Network Watcher.

## Project Overview

This lab demonstrates how to reduce lateral movement inside an Azure virtual network by separating workloads into distinct trust zones and enforcing traffic controls at the subnet layer.

The environment was designed around three network segments:

- **DMZ Subnet** for externally reachable systems
- **Internal Subnet** for protected internal workloads
- **Security Subnet** for isolated security-focused resources

Instead of relying on VM-level network controls alone, I implemented **subnet-based NSGs** to enforce segmentation more cleanly and consistently across the environment.

## Objectives

- Create a segmented Azure virtual network
- Restrict administrative access to a single trusted public IP
- Prevent lateral movement from the DMZ into Internal and Security segments
- Validate allow/deny behavior using Azure Network Watcher
- Document the architecture and test results in a GitHub-ready project

## Lab Architecture

### Resource Group
- `ZeroTrust-RG`

### Virtual Network
- `ZeroTrust-VNet`

### Subnets
- `DMZ-Subnet` → `10.0.1.0/24`
- `Internal-Subnet` → `10.0.2.0/24`
- `Security-Subnet` → `10.0.3.0/24`

### VM
- `attacker-VM` deployed in `DMZ-Subnet`

### NSGs
- `dmz-subnet-nsg`
- `internal-subnet-nsg`
- `security-subnet-nsg`

## Security Controls Implemented

### 1. Subnet-Level Segmentation
Each subnet was assigned its own NSG to enforce network policy at the segment level rather than only at the NIC level.

### 2. Restricted SSH Access
Inbound SSH access to the DMZ subnet was restricted to a single trusted public IP using a `/32` source rule.

### 3. Lateral Movement Prevention
Custom deny rules were added to block traffic from the DMZ subnet to:

- `10.0.2.0/24` (Internal)
- `10.0.3.0/24` (Security)

### 4. Cleaner Enforcement Model
The original VM NIC NSG was removed so the active policy enforcement point became the subnet NSG, reducing overlap and simplifying troubleshooting.

## Validation Performed

Azure Network Watcher IP Flow Verify was used to confirm expected traffic behavior.

### Verified Results

- **Inbound SSH from trusted public IP to DMZ VM** → Allowed
- **Outbound traffic from DMZ to Internal subnet** → Denied
- **Outbound traffic from DMZ to Security subnet** → Denied

## Key Takeaways

- Segmentation is stronger when enforced at the subnet layer rather than only at the individual VM level
- Restricting administrative access by source IP is a simple but effective Zero Trust control
- Default virtual network allow rules must be intentionally overridden to prevent east-west movement
- Azure Network Watcher provides useful validation evidence for security rule testing

## Screenshots

Suggested screenshots for this repo:

- `screenshots/subnets-overview.png`
- `screenshots/dmz-inbound-ssh-allow.png`
- `screenshots/dmz-outbound-deny-rules.png`
- `screenshots/ipflow-ssh-allowed.png`
- `screenshots/ipflow-dmz-to-internal-denied.png`
- `screenshots/ipflow-dmz-to-security-denied.png`

## Skills Demonstrated

- Azure Virtual Network design
- Network segmentation
- Network Security Groups (NSGs)
- Administrative access hardening
- East-west traffic restriction
- Azure Network Watcher
- Security validation and documentation

## Resume-Friendly Summary

Designed and validated an Azure Zero Trust segmentation lab using subnet-based Network Security Groups, restricted SSH access to a trusted public IP, and blocked lateral movement from a DMZ segment into protected internal and security subnets using Azure Network Watcher IP Flow Verify.

## Future Improvements

Possible Phase 2 enhancements:

- Add an internal VM in `Internal-Subnet` for live host-to-host validation
- Enable NSG flow logs for additional monitoring visibility
- Add Microsoft Defender for Cloud or Microsoft Sentinel for alerting and telemetry
- Expand the environment with additional protected workloads and logging pipelines
