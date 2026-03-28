# Screenshots

This folder contains validation evidence and supporting visuals for the Azure Zero Trust Segmentation Lab.

## Screenshot Index

### Network Build
- `subnets-overview.png` — Azure virtual network and segmented subnet layout
- `dmz-inbound-ssh-allow.png` — NSG rule allowing inbound SSH only from the trusted public IP
- `dmz-outbound-deny-rules.png` — NSG deny rules blocking outbound traffic from the DMZ to protected segments

### Validation / Testing
- `ipflow-ssh-allowed.png` — Azure Network Watcher IP Flow Verify showing trusted SSH access is allowed
- `ipflow-dmz-to-internal-denied.png` — IP Flow Verify showing DMZ-to-Internal traffic is denied
- `ipflow-dmz-to-security-denied.png` — IP Flow Verify showing DMZ-to-Security traffic is denied

## Notes
These screenshots are included to demonstrate segmentation design, access control hardening, and validation of Zero Trust network boundaries in Azure.
