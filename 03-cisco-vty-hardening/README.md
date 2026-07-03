# Lab 03: Basic Network Device Security (VTY & Console Hardening)

This project demonstrates the step-by-step implementation of basic remote management and console security across a small network topology using **Cisco Packet Tracer**. It captures the transition from basic password authentication to secure local user database authentication, enables an encrypted privileged exec mode secret, and enforces global password encryption.

## 📋 Table of Contents
1. [Project Scenario](#-project-scenario)
2. [Network Devices & Credentials](#-network-devices--credentials)
3. [Configuration Implementation](#-configuration-implementation)
4. [Verification & Visual Assets](#-verification--visual-assets)
5. [Key Security Takeaways](#-key-security-takeaways)

---

## 📋 Project Scenario
Securing the management plane of network infrastructure requires restricting both physical (Console) and virtual (VTY) access pathways. This lab demonstrates:
* Establishing local database authentication (`login local`) for unique accountability.
* Setting up secure, hashed enable passwords (`enable secret`).
* Hardening lines against casual observers using Type-7 global password encryption.
* Securing the network gateway (`Branch`) router alongside secondary switches (`SW1`, `SW2`).

---

## 🔐 Network Devices & Credentials
Across all configured devices (**Branch**, **SW1**, and **SW2**), the standardized local administrative credentials implemented are:
* **Username:** `user1`
* **Secret / Password:** `cisco`
* **Enable Secret:** `cisco`

---

## 🛠️ Configuration Implementation

### 1. Router Hardening (Branch Gateway)
The router configuration ensures that both the physical console port and all 16 virtual terminal channels utilize strict local database checks, enhanced by global Type-7 string hashing:

```cisco
! Enable password encryption globally
service password-encryption 

! Configure local user database
username user1 secret cisco
enable secret cisco

! Secure the physical console port with a 60-minute timeout
line con 0
 exec-timeout 60 0
 password cisco
 logging synchronous
 login local

! Secure all VTY lines for remote management
line vty 0 15
 password cisco
 login local
