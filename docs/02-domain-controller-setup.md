\# 02 - Domain Controller Setup



This document covers the installation of Active Directory Domain Services (AD DS) on the Windows Server 2025 VM, and its promotion to the first Domain Controller in a new forest.



\## 1. Installing the AD DS Role



Using Server Manager, the \*\*Active Directory Domain Services\*\* role was installed via \*Add Roles and Features Wizard\*.



!\[AD DS installation confirmation](../images/ad-ds-install-confirmation.png)



\## 2. Promoting the Server to a Domain Controller



After the role installation, the server was promoted to a Domain Controller using the \*Active Directory Domain Services Configuration Wizard\*.



\*\*Deployment configuration:\*\*

\- Deployment operation: Add a new forest

\- Root domain name: `corp.local`



\*\*Domain Controller options:\*\*

\- Forest functional level: Windows Server 2025

\- Domain functional level: Windows Server 2025

\- DNS Server: enabled

\- Global Catalog (GC): enabled

\- DSRM password set (not shown/stored anywhere in this repository)



!\[Domain Controller options](../images/dc-options.png)



\## 3. Prerequisites Check



Before installation, the wizard validates prerequisites. A warning about DNS delegation appeared:



> A delegation for this DNS server cannot be created because the authoritative parent zone cannot be found or it does not run Windows DNS server.



This is expected behavior in an isolated lab environment, since `corp.local` does not integrate with any parent DNS infrastructure. No action was required.



!\[Prerequisites check](../images/dc-prerequisites-check.png)



\## 4. Review \& Installation



Final configuration summary before installation:



\- New forest/domain name: `corp.local`

\- NetBIOS name: `CORP`

\- Forest Functional Level: Windows Server 2025

\- Domain Functional Level: Windows Server 2025

\- Global Catalog: Yes

\- DNS Server: Yes

\- Create DNS Delegation: No



!\[Review options](../images/dc-review-options.png)



The server rebooted automatically to complete the promotion.



\## 5. Verification



After reboot, the domain was verified using \*\*Active Directory Users and Computers (ADUC)\*\*, confirming that `corp.local` is visible and operational as the domain root.



\## Status

✅ Domain Controller successfully deployed and verified.

