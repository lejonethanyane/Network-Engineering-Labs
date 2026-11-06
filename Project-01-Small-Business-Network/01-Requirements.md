
# Project 01 — Business Requirements Document

**Project:** Small Business Network Design, VLAN Segmentation & Inter-VLAN Routing  
**Client:** Northstar Business Solutions (Fictional)  
**Location:** Johannesburg, South Africa  
**Environment:** Cisco Packet Tracer Simulation  
**Document Status:** Initial Requirements — Pending Validation

---

## 1. Project Overview

Northstar Business Solutions is a fictional company opening a new two-floor office in Johannesburg.

The company requires a secure, reliable and scalable network infrastructure supporting 60 employees, business applications, wired and wireless connectivity, and future expansion.

The purpose of this project is to design, implement, test and document a network solution using a structured network engineering methodology.

All requirements have been established through a simulated client interview. Physical site-survey findings remain unverified.

## 2. Business Requirements

| Requirement | Details |
|---|---|
| Initial Employees | 60 |
| Project Budget | R350,000 |
| Office Location | Johannesburg |
| Number of Floors | 2 |
| Office Size | Approximately 350 m² per floor |
| Expected Growth | 25% |
| Planned Employee Capacity | 75 |
| Connectivity | Wired and wireless |
| Availability | Minimise business-hour downtime |
| Security | Departmental segmentation and controlled access |
| Guest Access | Internet only |
| Remote Access | Secure access to selected internal resources |
| Business Hours | Monday–Friday, 08:00–17:00 |

The initial project budget covers network equipment, cabling and installation. Internet subscriptions and ongoing operational costs are funded separately.

## 3. Departmental Distribution

### Ground Floor — 35 Employees

| Department | Employees |
|---|---:|
| Sales | 20 |
| Operations | 10 |
| IT | 5 |
| **Total** | **35** |

Additional facilities:
- Reception
- Meeting rooms
- Main server and communications room

### First Floor — 25 Employees

| Department | Employees |
|---|---:|
| Finance | 10 |
| Operations | 15 |
| **Total** | **25** |

Additional facilities:
- Boardroom
- Communications closet

## 4. Network Device Requirements

The following equipment was identified during initial requirements gathering.

| Device | Quantity |
|---|---:|
| Laptops | 45 |
| Desktop Computers | 15 |
| IP Phones | 25 |
| Network Printers | 6 |
| CCTV Cameras | 8 |
| Physical Servers | 2 |
| Wireless Access Points | To Be Determined |
| Guest Devices | Up to 20 simultaneously |

The distribution of devices between floors must be confirmed before final switch capacity planning.

Not all devices necessarily require dedicated wired connections. The final design must account for simultaneous connectivity, switch uplinks and Power over Ethernet (PoE) requirements.

## 5. Business Applications

| Application / Service | Hosting |
|---|---|
| Microsoft 365 | Cloud |
| CRM System | Cloud |
| Accounting / ERP System | On-Premises |
| File Server | On-Premises |
| Active Directory | On-Premises |
| DNS and DHCP | On-Premises |
| VoIP Phone System | Cloud |

The deployment and resource requirements of the on-premises applications must be confirmed during detailed planning.

## 6. Network Availability Requirements

The business has provided the following maximum acceptable downtime targets.

| Service | Downtime Target | Priority |
|---|---|---|
| Finance / ERP | 30 minutes | Critical |
| Core Network | 30 minutes | Critical |
| Active Directory / DNS | 30 minutes | Critical |
| Internet Connectivity | 1 hour | High |
| Microsoft 365 / CRM Access | 1 hour | High |
| VoIP Services | 1 hour | High |
| File Server | 2 hours | Medium |
| Guest Wi-Fi | 4 hours | Low |

These are business requirements, not guaranteed recovery times. The network design must evaluate whether they are achievable within the allocated budget.

Scheduled maintenance should preferably occur outside business hours.

## 7. Security Requirements

The network should support:

- Separation of departments through appropriate network segmentation.
- Restricted access to Finance systems.
- Guest Wi-Fi isolated from internal business resources.
- Secure remote access for authorised employees.
- Controlled management access to networking equipment.
- Appropriate protection of internal servers and services.

Detailed access-control policies will be established during the design phase.

## 8. Existing Infrastructure

Information obtained during the simulated client meeting:

| Infrastructure | Current Status |
|---|---|
| Main Server Room | Available on ground floor |
| Communications Closet | Available on first floor |
| Internet Entry Point | Ground-floor service area |
| Network Cabling | Not yet installed or certified |
| Vertical Cable Riser | Available, suitability unverified |
| Electrical Supply | Commercial power available |
| UPS Equipment | Not yet purchased |
| Wireless Coverage | Survey required |
| Architectural Floor Plans | Available from building management |

No physical site survey has been completed.

The previously discussed 70-metre fibre-link and EMI scenario was a training example, not a verified finding for this building.

## 9. Outstanding Information

The following items require further investigation before the final design can be approved:

- [ ] Obtain and review architectural floor plans.
- [ ] Measure actual cable routes and verify riser suitability.
- [ ] Assess communications room power, cooling and security.
- [ ] Confirm available internet service providers and connection requirements.
- [ ] Determine whether backup internet connectivity is required.
- [ ] Confirm the distribution of wired devices between floors.
- [ ] Calculate switch port capacity and PoE requirements.
- [ ] Determine wireless coverage and access point requirements.
- [ ] Confirm server and application deployment requirements.
- [ ] Define departmental access-control rules.
- [ ] Confirm remote access requirements.
- [ ] Evaluate equipment and installation costs against the budget.

## 10. Next Project Phase

The next phase is **Network Design and IP Address Planning**.

Activities will include:

1. Developing a logical network topology.
2. Evaluating the physical network architecture.
3. Determining VLAN and subnet requirements.
4. Creating an IP addressing plan.
5. Evaluating routing and inter-VLAN connectivity.
6. Identifying security and availability requirements.
7. Preparing an equipment and capacity plan.

Design decisions will be recorded with their technical justification.

---

## Project Disclaimer

This document forms part of an independently developed network engineering training project.

Northstar Business Solutions, its requirements and its infrastructure are fictional.

The purpose is to demonstrate professional network planning, design, implementation, verification and documentation practices.
