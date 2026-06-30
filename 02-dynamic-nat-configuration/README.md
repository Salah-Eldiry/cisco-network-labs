# Enterprise NAT Overload (PAT) Implementation Lab

A hands-on Cisco Packet Tracer project expanding on foundational routing by implementing Port Address Translation (PAT). This lab simulates hiding an entire private LAN space behind a single public IPv4 address using transport-layer port mappings.

## 📌 Project Overview

Building directly upon the Branch-to-HQ topology, this project transitions the Branch network from plain routing to a production-style NAT Overload edge deployment. By specifying inside and outside boundaries and mapping an Access Control List (ACL) to the WAN interface, multiple internal hosts share a single public IP address, enforcing edge security and conserving IPv4 address space.

### Key Objectives Achieved:
* **Boundary Definition:** Designated `FastEthernet0/0` as `ip nat inside` and `FastEthernet0/1` as `ip nat outside`.
* **Traffic Matching:** Configured standard Access Control List 1 to identify permissible private LAN traffic (`10.1.1.0/24`).
* **NAT Overload (PAT) Activation:** Bound the ACL to the public interface with the `overload` parameter to enable port-level tracking.
* **Verification:** Confirmed translation states using deep-inspection IOS verification commands.

---

## 🗺️ Updated Addressing & PAT Architecture

The topology layout remains structurally consistent with the Branch-to-HQ infrastructure, but local traffic traversing the WAN interface (`Fa0/1`) now undergoes translation:

* **Private Inside LAN Subnet:** `10.1.1.0/24`
* **Public Inside Global IP (Shared Interface):** `209.165.201.1`

---

## ⚙️ Core NAT Overload Configuration (Branch Router)

The following running configuration was applied to the edge router to establish boundaries and bind traffic to the interface with port translation enabled:

```text
hostname Branch
!
interface FastEthernet0/0
 description Link to LAN Switch (SW1)
 ip address 10.1.1.1 255.255.255.0
 ip nat inside
 duplex auto
 speed auto
!
interface FastEthernet0/1
 description Link to HQ Router (WAN)
 ip address 209.165.201.1 255.255.255.224
 ip nat outside
 duplex auto
 speed auto
!
access-list 1 permit 10.1.1.0 0.0.0.255
ip nat inside source list 1 interface FastEthernet0/1 overload
!
ip classless
ip route 0.0.0.0 0.0.0.0 209.165.201.2
