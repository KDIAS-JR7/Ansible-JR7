# 🌐 Enterprise Network Automation Showcase (Cisco IOS)

Welcome to my network automation portfolio. This repository features a production-ready suite of **18 Ansible playbooks** engineered to automate configuration management, high availability states, device telemetry, and change management across Cisco IOS/IOS-XE infrastructures. 

As an aspiring Network Architect, I designed these playbooks to replace manual CLI screen-scraping with structured, predictable Infrastructure-as-Code (IaC) workflows.

---

## ⚠️ Execution Risk & Impact Categories

Before executing any playbook, please note the blast-radius classifications used below:
* 🟢 **Low Impact:** Read-only verification or minor, non-disruptive global configuration flags.
* 🟡 **Medium Impact:** Interface-specific or localized configuration changes on edge/access nodes.
* 🔴 **High Impact:** Global state mutations or dynamic routing alterations capable of reshaping network topology.
* 🔥 **Critical Impact:** Permanent state persistence changes (NVRAM writes) or system baselining.

---

## 📋 Automated Workflows & Playbooks

### 🛡️ Core Infrastructure, Routing & High Availability
| Playbook | Target Group | Description | Impact/Severity |
| :--- | :--- | :--- | :--- |
| [`ospf.yml`](./playbooks/ospf.yml) | `Distribution_Switches` | Configures OSPF dynamic routing parameters to manage internal topology distribution. | 🔴 High |
| [`VlanHSRP.yml`](./playbooks/VlanHSRP.yml) | `Core_Switches`, `Distribution_Switches` | Orchestrates high-availability failover topologies by pairing VLAN provisioning with HSRP deployment. | 🔴 High |
| [`HSRP_active.yml`](./playbooks/HSRP_active.yml) | `Edge_routers` | Enforces HSRP priority and preemption settings specifically on designated Active gateway routers. | 🟡 Medium |
| [`defaultGateway.yml`](./playbooks/defaultGateway.yml) | `allHosts` / `Access_Switches` | Standardizes management default gateways across networking devices to ensure reachability. | 🟡 Medium |

### 🔌 Layer 2 Switching & Interface Management
| Playbook | Target Group | Description | Impact/Severity |
| :--- | :--- | :--- | :--- |
| [`vlans.yml`](./playbooks/vlans.yml) | `Access_Switches`, `Distribution_Switches` | Deploys broadcast domains and maps inter-switch trunk links dynamically across the switching fabric. | 🔴 High |
| [`VLANDist.yml`](./playbooks/VLANDist.yml) | `Distribution_Switches` | Isolate-manages localized VLAN database and trunking updates specifically for distribution layer nodes. | 🔴 High |
| [`endDevice.yml`](./playbooks/endDevice.yml) | `Access_Switches` | Provisions edge interfaces, binding them to specific VLAN memberships and setting appropriate access modes. | 🟡 Medium |
| [`cdp.yml`](./playbooks/cdp.yml) | `allHosts` | Enforces global Cisco Discovery Protocol (CDP) properties across the environment for baseline topology mapping. | 🟢 Low |
| [`enableCDP.yml`](./playbooks/enableCDP.yml) | `allHosts` | Alternative targeted interface-level configuration adjustments for Cisco Discovery Protocol. | 🟢 Low |

### 🛠️ Network Telemetry, Time Sync & Device Hardening
| Playbook | Target Group | Description | Impact/Severity |
| :--- | :--- | :--- | :--- |
| [`snmp.yml`](./playbooks/snmp.yml) | `allHosts` | Deploys Simple Network Management Protocol configuration matrices to enable monitoring engine hooks. | 🟡 Medium |
| [`NTP.yml`](./playbooks/NTP.yml) | `allHosts` | Purges unapproved time-sources and synchronizes global fleet clocks via standardized internal NTP v3 pools. | 🟡 Medium |
| [`NTP_edge.yml`](./playbooks/NTP_edge.yml) | `Edge_routers` | Provisions dedicated edge-layer NTP sync (v4) adjustments incorporating regional time-zone configurations. | 🟡 Medium |
| [`LocalTime.yml`](./playbooks/LocalTime.yml) | `allHosts` | Hardens local logging strategies by overriding UTC syslog standards with localized system time tracking. | 🟢 Low |
| [`syslog.yml`](./playbooks/syslog.yml) | `allHosts` | Provisions remote logging sinks, routing system alert hooks to a centralized collector matrix. | 🟢 Low |

### 💾 Change Management, Backups & Verification
| Playbook | Target Group | Description | Impact/Severity |
| :--- | :--- | :--- | :--- |
| [`write.yml`](./playbooks/write.yml) | `allHosts` | **CRITICAL RUN TIME.** Performs multi-directory dated/timed pre-backups before committing configurations permanently to NVRAM. | 🔥 Critical |
| [`goldenState.yml`](./playbooks/goldenState.yml) | `allHosts` | Captures historical reference running-configs (`sh run`) locally to empower configuration drift detection. | 🔥 Critical |
| [`get_facts.yml`](./playbooks/get_facts.yml) | `allHosts` | Executes inventory validation routines, gathering structure maps and active software versions. | 🟢 Low |
| [`showCommand.yml`](./playbooks/showCommand.yml) | `allHosts` | A foundational scratchpad validation playbook designed to execute explicit operational testing and diagnostics. | 🟡 Medium |

---

## 🚀 Getting Started

### Prerequisites
1. Ansible 2.15 or higher installed.
2. The `cisco.ios` community collection installed:
   ```bash
   ansible-galaxy collection install cisco.ios
