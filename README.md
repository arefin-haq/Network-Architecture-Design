# Scalable-Network-Infrastructure-RHEL-Windows-Server-Lab-Environment
3-Tier Network Isolation with Hardened Security & Standardized IPAM
This repository documents a production-mimicked network architecture designed for Red Hat Enterprise Linux 9 (RHEL 9) and Windows Server (Active Directory) workloads. The design prioritizes Zero-Trust principles, Anti-Reconnaissance strategies, and resource optimization using repurposed hardware.

Network Topology & Segmentation:
The infrastructure is logically segmented into three distinct zones to ensure that laboratory experiments, guest traffic, and core management remain isolated.

[Internet]
    |
[Core Gateway] 10.0.0.1 (ISP Uplink)
    |
    +--- [Guest Router] 10.0.0.4 (Wired) ----> Subnet: 192.168.2.0/24
    |
    +--- [TP-Link Extender] 10.0.0.2 (Wired Bridge)
              |
              +--- [Lab Router] 10.0.0.3 (Wired) ----> Subnet: 192.168.1.0/24
                                                        (RHEL & Windows Lab)


Logical Address Management (IPAM)

CORE ZONE (10.0.0.0/24)
Primary Gateway: 10.0.0.1
Infrastructure Bridge: 10.0.0.2 (TP-Link Extender)
Lab Environment Uplink: 10.0.0.3 (Lab Router WAN)
Guest Segment Uplink: 10.0.0.4 (Guest Router WAN)

LAB ZONE (192.168.1.0/24)
Workloads: RHEL 9 Nodes & Windows Server 2022 Clusters
DHCP Range: .100 to .254 (Static reservations below .100)
DNS Hierarchy: Internal Active Directory Domain Services (AD DS)

GUEST ZONE (192.168.2.0/24)
Primary Role: Isolated Public/Family Access
Security: Client Isolation Enabled (L2 Isolation)
Content Filtering: Cloudflare Family DNS (1.1.1.3)                             

Hardened Security & Anti-Reconnaissance Logic:
1. Deception & Anti-Hacker Strategy
By implementing a custom 10.0.0.0/24 range on the core backbone, the infrastructure neutralizes automated reconnaissance scripts, CSRF exploits, and malware that specifically target default consumer-grade gateways (192.168.0.1 or 192.168.1.1). This adds a critical layer of Security by Obscurity.

2. Invisible Infrastructure (RF Silence):
Both the Extender and the Lab Router operate with SSID and Radio Frequency (RF) disabled. This eliminates wireless interference and ensures the laboratory environment remains invisible to unauthorised Wi-Fi scanners, thereby enforcing a strictly wired, high-stability environment.

3. Segmented Traffic Isolation:
Guest Network: Connected directly to the Core via Ethernet to prevent congestion on the Lab backbone. Features Client Isolation and Cloudflare Family DNS (1.1.1.3) for real-time malware filtering.
Lab Network: A dedicated environment optimized for Active Directory (AD DS) synchronization and RHEL 9 node management. Includes a 4-hour DHCP Lease Time for efficient IP recycling.

Infrastructure Note on Evolution:
Note: Currently, due to the absence of a dedicated Managed/Unmanaged Switch, I have repurposed a legacy TP-Link router as a hardened Network Extender. By assigning it a Static Management IP (10.0.0.2), disabling DHCP/SSID, and completely deactivating the Radio Frequency (RF), I have successfully achieved Layer 2 connectivity with Layer 3 visibility. This allows for infrastructure monitoring and a stable wired backbone for the server cluster while maintaining maximum cost-efficiency.

Lab Goals:
Server Administration: Mastering RHEL 9 and Windows Server 2022 environments.
Directory Services: Implementing Active Directory (AD DS), DNS, and Group Policy management.
Network Engineering: Proficiency in IPAM, Static Routing, and Hardware Hardening.

Pro-Tip for Reviewers:
The use of non-standard IP addressing and RF hardening demonstrates a proactive security mindset, while the repurposing of legacy hardware as a management-enabled bridge highlights resourceful engineering and cost-effective infrastructure scaling.
