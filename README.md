# \# Active Directory Home Lab

# 

# Isolated Active Directory lab for Red Team attack simulation and Blue Team log analysis, built on VirtualBox.

# 

# \## 🎯 Project Goals

# \- Build a realistic, isolated AD environment

# \- Simulate common attack techniques (Kerberoasting, LLMNR Poisoning)

# \- Detect and analyze these attacks from a Blue Team perspective

# \- Document the full process for portfolio purposes

# 

# \## 🧱 Lab Architecture

# | Machine | Role | OS | IP Address |

# |---|---|---|---|

# | DC01 | Domain Controller | Windows Server 2025 | 192.168.10.10 |

# | Kali | Attacker | Kali Linux | 192.168.10.20 |

# 

# Network mode: VirtualBox Internal Network (`LabNetwork`)

# 

# \## 📂 Documentation

# \- \[01 - Network Setup](docs/01-network-setup.md)

# \- \[02 - Domain Controller Setup](docs/02-domain-controller-setup.md)

# \- \[03 - AD Users \& OUs](docs/03-ad-users-and-ous.md)

# \- \[04 - Kerberoasting Attack](docs/04-kerberoasting-attack.md)

# \- \[05 - LLMNR Poisoning Attack](docs/05-llmnr-poisoning-attack.md)

# \- \[06 - Detection (Blue Team)](docs/06-detection-blue-team.md)

# 

# \## 🛠️ Tools Used

# VirtualBox, Windows Server 2025, Kali Linux, PowerShell, Impacket, Responder, Hashcat

# 

# \## 🚧 Status

# 🟢 In progress

