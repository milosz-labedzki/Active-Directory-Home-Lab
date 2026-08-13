# 🛡️ Active Directory Home Lab

Isolated Active Directory lab built for Red Team attack simulation and Blue Team log analysis — fully self-hosted on VirtualBox, with no internet-facing components.

![Status](https://img.shields.io/badge/status-in%20progress-yellow)
![Platform](https://img.shields.io/badge/platform-VirtualBox-blue)
![OS](https://img.shields.io/badge/DC-Windows%20Server%202025-0078D6)
![Attacker](https://img.shields.io/badge/Attacker-Kali%20Linux-557C94)

## 🎯 Project Goals

- Build a realistic, isolated Active Directory environment from scratch
- Simulate common real-world attack techniques (Kerberoasting, LLMNR Poisoning)
- Investigate and resolve modern hardening obstacles (e.g. AES-only Kerberos on Server 2025)
- Analyze and detect these attacks from a Blue Team perspective
- Document the full process — including dead ends and fixes — for portfolio purposes

## 🧱 Lab Architecture

| Machine | Role | OS | IP Address |
|---|---|---|---|
| DC01 | Domain Controller | Windows Server 2025 | 192.168.10.10 |
| Kali | Attacker | Kali Linux | 192.168.10.20 |

**Domain:** `corp.local` (NetBIOS: `CORP`)
**Network mode:** VirtualBox Internal Network (`LabNetwork`) — fully isolated, no external connectivity by design

## 📂 Documentation

| # | Document | Summary |
|---|---|---|
| 01 | [Network Setup](docs/01-network-setup.md) | VirtualBox Internal Network configuration |
| 02 | [Domain Controller Setup](docs/02-domain-controller-setup.md) | AD DS install & forest promotion on Server 2025 |
| 03 | [AD Users & OUs](docs/03-ad-users-and-ous.md) | OU structure, fictional users, Kerberoastable service account |
| 04 | [Kerberoasting Attack](docs/04-kerberoasting-attack.md) | SPN discovery → AES256 TGS extraction → offline crack |
| 05 | [LLMNR Poisoning Attack](docs/05-llmnr-poisoning-attack.md) | Responder-based credential capture *(planned)* |
| 06 | [Detection (Blue Team)](docs/06-detection-blue-team.md) | Log analysis & defensive recommendations *(planned)* |

## 🔑 Key Learnings & Skills Demonstrated

- **AD infrastructure from zero:** forest/domain promotion, OU design, GPO troubleshooting
- **Modern Kerberos hardening:** diagnosed and resolved `KDC_ERR_ETYPE_NOSUPP` caused by Server 2025's AES-only enforcement — traced to both an outdated Impacket build and domain GPO encryption-type policy
- **Offensive tooling:** Impacket (`GetUserSPNs`), Hashcat (mode `19700` for AES256 Kerberos tickets)
- **Real-world debugging:** resolved VirtualBox networking issues (NAT vs. Internal Network adapters, DHCP/static IP conflicts under NetworkManager)
- **Operational hygiene:** redacting plaintext credentials from documentation before publishing

## 🛠️ Tools Used

VirtualBox · Windows Server 2025 · Kali Linux · PowerShell · Impacket · Hashcat

## 🚧 Status

🟡 **In progress**
✅ Domain Controller deployed and verified
✅ AD structure and Kerberoastable service account configured
✅ Kerberoasting attack completed end-to-end (AES256 TGS extraction + offline crack)
⏭️ Next: LLMNR/NBT-NS Poisoning attack (Responder) + Blue Team detection write-up

---

*This lab runs entirely within an isolated VirtualBox network with no internet-facing services. All credentials referenced are lab-only test values, generated solely for this project and not reused elsewhere.*
