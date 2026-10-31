# 🛡️ Active Directory Home Lab

Isolated Active Directory lab built for Red Team attack simulation and Blue Team log analysis — fully self-hosted on VirtualBox, with no internet-facing components.

![Status](https://img.shields.io/badge/status-in%20progress-yellow)
![Platform](https://img.shields.io/badge/platform-VirtualBox-blue)
![OS](https://img.shields.io/badge/DC-Windows%20Server%202025-0078D6)
![Attacker](https://img.shields.io/badge/Attacker-Kali%20Linux-557C94)
![SIEM](https://img.shields.io/badge/SIEM-Splunk%20(in%20progress)-orange)

## 🎯 Project Goals

- Build a realistic, isolated Active Directory environment from scratch
- Simulate common real-world attack techniques (Kerberoasting, LLMNR Poisoning)
- Investigate and resolve modern hardening obstacles (e.g. AES-only Kerberos on Server 2025)
- Analyze and detect these attacks from a Blue Team perspective, first by hand in Event Viewer, then automatically in a SIEM (Splunk)
- Document the full process — including dead ends and fixes — for portfolio purposes

## 🧱 Lab Architecture

| Machine | Role | OS | IP Address |
|---|---|---|---|
| DC01 | Domain Controller | Windows Server 2025 | 192.168.10.10 |
| Kali | Attacker | Kali Linux | 192.168.10.20 |
| Splunk01 | SIEM (Splunk Enterprise) | Ubuntu Server 24.04 LTS | 192.168.10.30 |

**Domain:** `corp.local` (NetBIOS: `CORP`)
**Network mode:** VirtualBox Internal Network (`intnet`) — isolated, no external connectivity by design. Splunk01 also has a NAT adapter, used to download installers and, through port forwarding bound to `127.0.0.1` only, to reach SSH and Splunk Web from the host computer.

**Note on names:** DC01 is the lab's name for the Domain Controller. Its Windows computer name is `root` (`root.corp.local`), which is why its events show `host=root` in Splunk.

Logs are collected on a separate machine (Splunk01) instead of the monitored host, so an attacker on DC01 cannot easily tamper with the evidence.

## 📂 Documentation

Numbers show the order in which the documents were written; the groups show how they fit together.

### 🧱 Foundation
| # | Document | Summary |
|---|---|---|
| 01 | [Network Setup](docs/01-network-setup.md) | VirtualBox Internal Network configuration for DC01, Kali and Splunk01 |
| 02 | [Domain Controller Setup](docs/02-domain-controller-setup.md) | AD DS install & forest promotion on Server 2025 |
| 03 | [AD Users & OUs](docs/03-ad-users-and-ous.md) | OU structure, fictional users, Kerberoastable service account |
| 07 | [Splunk SIEM Setup](docs/07-Splunk-SIEM-setup.md) | Splunk Enterprise on a separate Ubuntu VM, Universal Forwarder on DC01 sending the Security log |

### ⚔️ Attacks
| # | Document | Summary |
|---|---|---|
| 04 | [Kerberoasting Attack](docs/04-kerberoasting-attack.md) | SPN discovery → AES256 TGS extraction → offline crack |
| 05 | [LLMNR Poisoning Attack](docs/05-llmnr-poisoning-attack.md) | Responder-based credential capture |

### 🔍 Detection (Blue Team)
| # | Document | Summary |
|---|---|---|
| 06 | [Detection in Event Viewer](docs/06-Blue-Team-detection-write-up.md) | Event 4769/4020 analysis, encryption-type pitfall, tested LLMNR/NBT-NS/mDNS mitigation |
| 08 | Detection in Splunk *(in progress)* | The same detections rebuilt as automated Splunk searches |

## 🔑 Key Learnings & Skills Demonstrated

- **AD infrastructure from zero:** forest/domain promotion, OU design, GPO troubleshooting
- **Modern Kerberos hardening:** diagnosed and resolved `KDC_ERR_ETYPE_NOSUPP` caused by Server 2025's AES-only enforcement — traced to both an outdated Impacket build and domain GPO encryption-type policy
- **Offensive tooling:** Impacket (`GetUserSPNs`), Hashcat (mode `19700` for AES256 Kerberos tickets)
- **Real-world debugging:** resolved VirtualBox networking issues (NAT vs. Internal Network adapters, DHCP/static IP conflicts under NetworkManager)
- **Operational hygiene:** redacting plaintext credentials from documentation before publishing
- **Broadcast poisoning:** used Responder to capture NTLMv2 credentials by exploiting LLMNR/NBT-NS fallback, and identified password reuse as a result
- **Blue Team detection:** identified Event IDs 4769 (Kerberoasting) and 4020 (NTLM fallback from LLMNR/NBT-NS poisoning) as evidence sources, showed why the standard RC4-encryption-type heuristic fails under AES-only hardening, and iteratively tested a defensive fix — disabling only LLMNR/NBT-NS left an mDNS-shaped gap, which was then closed and verified end-to-end
- **SIEM deployment:** installed Splunk Enterprise on a dedicated Ubuntu VM, running as a non-root `splunk` user under `systemd`, and forwarded the DC's Security log with the Universal Forwarder — the installer was delivered over the isolated network, so DC01 never needed internet access

## 🛠️ Tools Used

VirtualBox · Windows Server 2025 · Kali Linux · Ubuntu Server · PowerShell · Impacket · Hashcat · Responder · Splunk Enterprise · Splunk Universal Forwarder

## ✅ Status
🟡 **In progress / actively developed**

✅ Domain Controller deployed and verified
✅ AD structure and Kerberoastable service account configured
✅ Kerberoasting attack completed end-to-end (AES256 TGS extraction + offline crack)
✅ LLMNR/NBT-NS poisoning attack completed end-to-end (broadcast capture + offline crack)
✅ Blue Team detection write-up completed for both attacks (log analysis + tested, verified mitigations)
✅ Splunk Enterprise installed and DC01 Security log forwarded to it
🚧 Splunk detection searches in progress

## 🔜 Roadmap

- [x] Splunk01 VM (Ubuntu Server 24.04 LTS) added to the isolated network with a static IP (192.168.10.30)
- [x] Install Splunk Enterprise on Splunk01
- [x] Forward DC01 Windows event logs to Splunk with the Universal Forwarder
- [ ] Write Splunk detection searches for Kerberoasting (Event 4769) and LLMNR/NBT-NS poisoning

---

*This lab runs entirely within an isolated VirtualBox network with no internet-facing services. All credentials referenced are lab-only test values, generated solely for this project — including any intentional password reuse between accounts, which is part of the attack scenario, not a real-world credential leak.*
