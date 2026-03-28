
## `docs/lessons_learned.md`

```md
# Lessons Learned

## 1. A segmented VNet is not automatically a secure VNet
Creating multiple subnets looks secure on paper, but Azure default NSG behavior still allows virtual network traffic unless custom rules override it.

## 2. Subnet-level controls create a cleaner security model
Using NSGs at the subnet layer made the project easier to reason about than mixing NIC-level and subnet-level controls.

## 3. Administrative access should be narrowed aggressively
Restricting SSH to a single trusted public IP was one of the simplest and highest-value improvements in the lab.

## 4. Validation matters as much as configuration
It is not enough to create rules and assume they work. Azure Network Watcher IP Flow Verify provided direct proof of which traffic was allowed or denied.

## 5. Zero Trust is about reducing implicit trust
The main security gain in this lab came from removing default-style trust between subnets and enforcing explicit policy boundaries.

## 6. Cloud projects benefit from clear documentation
The technical work became much more useful once it was turned into screenshots, architecture notes, and test evidence that could be shared in GitHub or discussed in interviews.

## What I Would Improve Next

- add an internal VM for live segmentation testing
- enable NSG flow logs for stronger monitoring evidence
- integrate Defender for Cloud or Sentinel in a follow-on phase
- expand the lab into a broader Azure monitoring and detection project
