# Screenshots

Add the following screenshots to this folder and reference them from the main README.

## Suggested Files

- `subnets-overview.png`
  - Show `DMZ-Subnet`, `Internal-Subnet`, and `Security-Subnet`
  - Show each subnet associated with its NSG

- `dmz-inbound-ssh-allow.png`
  - Show the inbound SSH rule on `dmz-subnet-nsg`
  - Show source restricted to trusted public IP `/32`

- `dmz-outbound-deny-rules.png`
  - Show the outbound deny rules blocking traffic to:
    - `10.0.2.0/24`
    - `10.0.3.0/24`

- `ipflow-ssh-allowed.png`
  - Show IP Flow Verify allowing inbound SSH from the trusted public IP

- `ipflow-dmz-to-internal-denied.png`
  - Show IP Flow Verify denying DMZ to Internal traffic

- `ipflow-dmz-to-security-denied.png`
  - Show IP Flow Verify denying DMZ to Security traffic

## Screenshot Goal

The screenshots should prove:

1. the network was segmented
2. access to the DMZ was restricted
3. lateral movement from DMZ was blocked
4. the controls were validated with Azure tooling
