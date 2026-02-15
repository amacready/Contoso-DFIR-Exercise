# Cyber Security Exercise - Environment Documentation
## Enterprise Environment & Initial Alerts
**Cyber Exercise Author:** Andy Macready
**Exercise ID:** CYBER-EX-2026-001  
**Scenario Incident Date:** February 3, 2026  
**Organization:** CONTOSO Corporation  
**Industry:** Technology / Software Development  
**Employee Count:** ~1,500 employees  
**Domain:** contoso.local


---

## Table of Contents

1. [Network Architecture](#network-architecture)
2. [Active Directory Structure](#active-directory-structure)
3. [Systems Inventory](#systems-inventory)
4. [User Accounts & Privileges](#user-accounts--privileges)
5. [Security Infrastructure](#security-infrastructure)
6. [Initial Alerts & Detection](#initial-alerts--detection)
7. [Exercise Objectives](#exercise-objectives)
8. [Investigation Starting Points](#investigation-starting-points)

---

## Network Architecture

### Network Segments

| VLAN | Network | Purpose | Systems |
|------|---------|---------|---------|
| **VLAN 10** | 10.50.10.0/24 | Corporate Workstations | User desktops, laptops |
| **VLAN 20** | 10.50.20.0/24 | Server Infrastructure | File servers, app servers |
| **VLAN 5** | 10.50.5.0/24 | Domain Controllers | DC01, DC02 |
| **VLAN 50** | 10.50.50.0/24 | IT Administration | Admin workstations, jump boxes |
| **VLAN 100** | 10.50.100.0/24 | DMZ | Web servers, email gateway |

### Key Network Details

- **Domain Name:** contoso.local
- **Forest Functional Level:** Windows Server 2022
- **Domain Functional Level:** Windows Server 2022
- **DNS Servers:** 10.50.5.5 (DC01), 10.50.5.6 (DC02)
- **Gateway:** 10.50.10.1
- **DHCP Server:** 10.50.5.5 (DC01)
- **Internet Egress:** 203.0.113.100 (NAT gateway)

### External Connections

- **Internet Access:** All workstations have filtered internet access
- **Email:** Microsoft 365 (Exchange Online)
- **Cloud Services:** Azure AD Connect, OneDrive, SharePoint Online
- **VPN:** Cisco AnyConnect (10.50.200.0/24 pool)

---

## Active Directory Structure

### Domain Information

- **Domain:** CONTOSO
- **Full FQDN:** contoso.local
- **NetBIOS Name:** CONTOSO
- **Domain Controllers:** 2 (DC01, DC02)
- **Password Policy:** 
  - Minimum Length: 12 characters
  - Complexity: Enabled
  - Max Age: 90 days
  - Lockout Threshold: 5 attempts
  - Lockout Duration: 30 minutes

### Organizational Unit (OU) Structure

```
CONTOSO.local
├── Domain Controllers
├── Corporate
│   ├── Workstations
│   │   ├── Executive
│   │   ├── Marketing
│   │   ├── Sales
│   │   ├── Engineering
│   │   └── Finance
│   ├── Servers
│   │   ├── File Servers
│   │   ├── Application Servers
│   │   └── Web Servers
│   └── Users
│       ├── Executives
│       ├── IT Staff
│       ├── Marketing
│       ├── Sales
│       ├── Engineering
│       └── Finance
└── IT
    ├── Admins
    ├── Service Accounts
    └── Admin Workstations
```

### Security Groups (Relevant to Incident)

| Group Name | Type | Members | Purpose |
|------------|------|---------|---------|
| **Domain Admins** | Security (Global) | Administrator, jennifer.hayes, Backup Admin* | Full domain control |
| **Enterprise Admins** | Security (Universal) | Administrator | Forest-wide control |
| **IT Staff** | Security (Global) | david.palmer, jennifer.hayes, michael.chen | Standard IT privileges |
| **Helpdesk** | Security (Global) | robert.kim, lisa.wong | Password reset, unlock accounts |
| **Server Admins** | Security (Global) | jennifer.hayes, kevin.torres | Server local admin |
| **Marketing** | Security (Global) | sarah.mitchell, emma.davis, ryan.lopez | Marketing department |
| **Finance** | Security (Global) | laura.martinez, kevin.anderson | Finance access |
* Backup Admin added to DA as persistence by Jennifer Haynes


---

## Systems Inventory

### Domain Controllers

#### DC01 (Primary Domain Controller)
- **Hostname:** DC01.contoso.local
- **IP Address:** 10.50.5.5
- **Operating System:** Windows Server 2022 Datacenter
- **Roles:** 
  - Active Directory Domain Services
  - DNS Server
  - DHCP Server
  - Global Catalog
- **CPU:** 8 vCPUs
- **RAM:** 32 GB
- **Storage:** 500 GB
- **Last Patched:** January 15, 2026
- **Backup Schedule:** Daily at 02:00 UTC

#### DC02 (Secondary Domain Controller)
- **Hostname:** DC02.contoso.local
- **IP Address:** 10.50.5.6
- **Operating System:** Windows Server 2022 Datacenter
- **Roles:**
  - Active Directory Domain Services
  - DNS Server
  - Global Catalog
- **CPU:** 8 vCPUs
- **RAM:** 32 GB
- **Storage:** 500 GB
- **Last Patched:** January 15, 2026
- **Backup Schedule:** Daily at 03:00 UTC

---

### File Servers

#### FILESERVER01 (Primary File Server - COMPROMISED)
- **Hostname:** FILESERVER01.contoso.local
- **IP Address:** 10.50.10.30
- **Operating System:** Windows Server 2022 Standard
- **CPU:** 16 vCPUs
- **RAM:** 64 GB
- **Storage:** 10 TB (RAID 6)
- **Purpose:** Primary file storage for all departments
- **Shares:**
  - `\\FILESERVER01\CompanyData` - Main company data share
  - `\\FILESERVER01\Marketing` - Marketing department files
  - `\\FILESERVER01\Finance` - Financial documents (restricted)
  - `\\FILESERVER01\HR` - HR documents (restricted)
  - `\\FILESERVER01\Engineering` - Engineering projects
- **Last Patched:** January 20, 2026
- **Backup Schedule:** Daily at 01:00 UTC (last successful: Feb 2, 2026 23:45 UTC)


#### Sensitive Directories on FILESERVER01
```
D:\Shares\CompanyData\
├── Board\                          (Board of Directors materials)
│   ├── Board_Meeting_Agenda_Feb_2026.docx
│   ├── Strategic_Plan_2026-2028_CONFIDENTIAL.pptx
│   └── Q4_2025_Financial_Results.xlsx
├── MergersAcquisitions\            (M&A confidential documents)
│   ├── Acquisition_Target_Analysis_TechCorp.xlsx
│   ├── Due_Diligence_Report_CONFIDENTIAL.pdf
│   └── Project_Phoenix_Overview.docx
├── ResearchDevelopment\            (R&D intellectual property)
│   ├── Product_Roadmap_2026-2027_SECRET.xlsx
│   ├── Patent_Application_AI_Algorithm_2026.pdf
│   └── Prototype_Specifications_Confidential.docx
└── IT_Architecture\                (IT infrastructure documentation)
    ├── Network_Topology_Diagram_2026.vsdx
    ├── Active_Directory_Design_Document.docx
    ├── Firewall_Rules_and_ACLs.xlsx
    └── Disaster_Recovery_Plan_2026.pdf
```

---

### Workstations

#### WORKSTATION-01 (COMPROMISED - Initial Access)
- **Hostname:** WORKSTATION-01.contoso.local
- **IP Address:** 10.50.10.15
- **Operating System:** Windows 11 Enterprise (23H2)
- **Primary User:** sarah.mitchell
- **Department:** Marketing
- **CPU:** Intel Core i7-13700
- **RAM:** 16 GB
- **Storage:** 512 GB NVMe SSD
- **Security:** CrowdStrike Falcon (disabled by attacker), Windows Defender disabled
- **Last Patched:** January 25, 2026
- **Applications:**
  - Microsoft Office 365 (Outlook, Word, Excel, PowerPoint)
  - Adobe Creative Cloud
  - Chrome, Edge browsers
  - Slack, Microsoft Teams

#### WORKSTATION-02 (COMPROMISED - Lateral Movement)
- **Hostname:** WORKSTATION-02.contoso.local
- **IP Address:** 10.50.10.22
- **Operating System:** Windows 11 Enterprise (23H2)
- **Primary User:** david.palmer
- **Department:** IT Administration
- **CPU:** Intel Core i7-13700
- **RAM:** 32 GB
- **Storage:** 1 TB NVMe SSD
- **Security:** CrowdStrike Falcon installed
- **Last Patched:** January 28, 2026
- **Applications:**
  - Microsoft Office 365
  - Remote Server Administration Tools (RSAT)
  - PowerShell 7
  - Visual Studio Code
  - PuTTY, WinSCP
  - Wireshark

#### Representative Benign Workstations

| Hostname | IP | User | Department | Purpose |
|----------|-----|------|------------|---------|
| WORKSTATION-03 | 10.50.10.17 | emma.davis | Marketing | Design workstation |
| WORKSTATION-07 | 10.50.10.21 | michael.brown | Sales | Sales CRM access |
| WORKSTATION-10 | 10.50.10.24 | olivia.taylor | Engineering | Development workstation |
| WORKSTATION-17 | 10.50.10.31 | andrew.white | Finance | Financial analysis |
| WORKSTATION-21 | 10.50.10.35 | sophia.martinez | HR | HR systems access |
| WORKSTATION-25 | 10.50.10.39 | joshua.thomas | Marketing | Marketing automation |
| WORKSTATION-27 | 10.50.10.41 | isabella.harris | Sales | Inside sales |
| LAPTOP-01 | DHCP | jennifer.hayes | IT Administration | IT Director laptop |
| LAPTOP-08 | DHCP | ryan.martin | Executive | VP of Operations |
| LAPTOP-10 | DHCP | charlotte.robinson | Executive | CFO laptop |

---

### Application Servers

#### APPSERVER01
- **Hostname:** APPSERVER01.contoso.local
- **IP Address:** 10.50.20.10
- **Operating System:** Windows Server 2022 Standard
- **Purpose:** Internal CRM application (SalesForce connector)
- **CPU:** 8 vCPUs
- **RAM:** 32 GB
- **Status:** Not affected by incident

#### APPSERVER02
- **Hostname:** APPSERVER02.contoso.local
- **IP Address:** 10.50.20.11
- **Operating System:** Windows Server 2022 Standard
- **Purpose:** SharePoint On-Premises (legacy)
- **CPU:** 16 vCPUs
- **RAM:** 48 GB
- **Status:** Not affected by incident

---

## User Accounts & Privileges

#### sarah.mitchell (Initial Victim)
- **Username:** sarah.mitchell
- **Email:** sarah.mitchell@contoso.com
- **Department:** Marketing
- **Title:** Marketing Specialist
- **Groups:** 
  - Domain Users
  - Marketing
  - CompanyData_ReadWrite
- **Privileges:** Standard user (no elevated privileges)
- **Primary Workstation:** WORKSTATION-01
- **Hire Date:** March 2024
- **Last Password Change:** December 15, 2025
- **MFA Status:** ❌ Not enabled


#### david.palmer (Lateral Movement Target)
- **Username:** david.palmer
- **Email:** david.palmer@contoso.com
- **Department:** IT Operations
- **Title:** Systems Administrator
- **Groups:**
  - Domain Users
  - IT Staff
  - Server Admins
  - Local Administrators (workstations)
- **Privileges:** 
  - Local administrator on all workstations
  - Member of Server Admins group
  - Remote Desktop access to servers
  - PowerShell Remoting enabled
- **Primary Workstation:** WORKSTATION-02
- **Hire Date:** January 2022
- **Last Password Change:** January 5, 2026


#### james.rodriguez (Collateral Credential Theft)
- **Username:** james.rodriguez
- **Email:** james.rodriguez@contoso.com
- **Department:** Engineering
- **Title:** Software Developer
- **Groups:**
  - Domain Users
  - Engineering
  - Developers
- **Privileges:** Standard user
- **Compromise Method:** Cached credentials in LSASS on WORKSTATION-01
- **Impact:** Minimal - credentials obtained but not actively used

#### jennifer.hayes (Domain Administrator - CRITICAL)
- **Username:** jennifer.hayes
- **Email:** jennifer.hayes@contoso.com
- **Department:** IT Administration
- **Title:** IT Director
- **Groups:**
  - Domain Users
  - Domain Admins 
  - Enterprise Admins 
  - IT Staff
  - Server Admins
- **Privileges:**
  - **Full domain administrative access**
  - All servers and workstations
  - Active Directory management
  - Schema modifications
  - Group Policy administration
- **Primary Workstation:** LAPTOP-01 (mobile)
- **Also Uses:** WORKSTATION-02 (occasionally for IT tasks)
- **Hire Date:** May 2019
- **Last Password Change:** November 20, 2025


#### BackupAdmin (Malicious Account - Created by Attacker)
- **Username:** BackupAdmin
- **Created:** February 3, 2026 at 11:12:15 UTC
- **Created By:** jennifer.hayes (compromised account)
- **Groups:**
  - Domain Users
  - Domain Admins 
  - Administrators 
- **Privileges:** Full domain administrative access
- **Purpose:** Attacker persistence mechanism


---

### Non-Compromised Users (Representative Sample)

| Username | Department | Title | Privileges | Groups |
|----------|------------|-------|------------|--------|
| michael.brown | Sales | Account Executive | Standard user | Domain Users, Sales |
| emily.davis | Marketing | Marketing Manager | Standard user | Domain Users, Marketing |
| olivia.taylor | Engineering | Senior Developer | Standard user | Domain Users, Engineering, Developers |
| matthew.anderson | IT Operations | Network Engineer | Local admin | IT Staff, Network Admins |
| andrew.white | Finance | Financial Analyst | Standard user | Domain Users, Finance |
| sophia.martinez | HR | HR Manager | Standard user | Domain Users, HR, HR_Managers |
| charlotte.robinson | Executive | CFO | Standard user | Domain Users, Executives, Finance_ReadAll |
| ryan.martin | Executive | VP Operations | Standard user | Domain Users, Executives |

---

### Service Accounts

| Account | Purpose | Privileges | Password Change |
|---------|---------|------------|-----------------|
| svc_backup | Veeam backup service | Backup Operators, Domain Users | 90 days |
| svc_sql | SQL Server service account | Domain Users, specific DB access | 90 days |
| svc_monitoring | Monitoring tool (SolarWinds) | Read-only domain | 90 days |
| svc_adconnect | Azure AD Connect sync | Domain Users, replication rights | 90 days |

---

## Security Infrastructure

### Endpoint Detection & Response (EDR)

**Product:** CrowdStrike Falcon Sensor  
**Version:** 7.10.16303.0  
**Deployment:** 95% of endpoints (1,425 of 1,500)  
**Configuration:**
- Real-time protection: Enabled
- Cloud delivery: Enabled
- Tamper Protection: ✅ Enabled
- Behavioral blocking: Enabled
- Script scanning: Enabled
- Network protection: Enabled

**Coverage:**
- ✅ All workstations (WORKSTATION-01 through WORKSTATION-35, LAPTOP-01 through LAPTOP-15)
- ✅ File servers (FILESERVER01, FILESERVER02)
- ✅ Application servers (APPSERVER01, APPSERVER02)
- ❌ Domain controllers (excluded per vendor recommendation)
- ❌ Legacy systems (5% of environment)

**Monitoring:**
- Security Operations Center (SOC) monitors CrowdStrike console
- Alerts routed to SIEM (Splunk Enterprise Security)
- High/Critical detections trigger immediate analyst review

---

### SIEM (Security Information & Event Management)

**Product:** Splunk Enterprise Security  
**Version:** 8.2.1  
**Log Sources:**
- Windows Event Logs (Security, System, Application)
- Sysmon (comprehensive configuration)
- PowerShell Logging (Module, Script Block)
- CrowdStrike Falcon events
- Firewall logs (Palo Alto Networks)
- DNS logs
- DHCP logs
- Azure AD logs (via Azure AD Connect)

**Retention:** 90 days online, 1 year archive

**Notable Events Configuration:**
- Failed logon attempts (>5 in 15 minutes)
- Privileged account usage
- Service account interactive logons
- Account lockouts
- Group membership changes
- LSASS process access
- PowerShell execution (suspicious patterns)
- Rare process execution
- Lateral movement indicators

---

### Sysmon Configuration

**Version:** 15.0  
**Configuration:** SwiftOnSecurity configuration with custom rules  
**Deployment:** All workstations and servers

**Key Events Logged:**
- Event 1: Process Creation (full command line)
- Event 3: Network Connection
- Event 7: Image Loaded
- Event 8: CreateRemoteThread (process injection detection)
- Event 10: Process Access (LSASS access detection)
- Event 11: File Create
- Event 12: Registry Object Added/Deleted
- Event 13: Registry Value Set
- Event 22: DNS Query
- Event 23: File Delete

---

### Network Security

**Firewall:** Palo Alto Networks PA-5220  
**Configuration:**
- Inbound: Deny all except required services
- Outbound: Allow HTTP/HTTPS, block known malicious IPs
- Lateral movement: Restricted via internal zones
- Logging: All traffic logged and sent to SIEM

**IDS/IPS:** Suricata (inline mode)  
**DNS Filtering:** Cisco Umbrella  
**Email Security:** Proofpoint Email Protection  
**Web Proxy:** Zscaler Internet Access

---

### Backup Infrastructure

**Product:** Veeam Backup & Replication v12  
**Configuration:**
- Daily backups at 01:00-05:00 UTC
- Retention: 14 daily, 8 weekly, 12 monthly
- Backup Repository: On-premises NAS (isolated network)
- Offsite Copy: AWS S3 (Glacier)
- Immutable Backups: ✅ Enabled (14-day immutability)

**Last Successful Backups (Pre-Incident):**
- FILESERVER01: February 2, 2026 23:45 UTC ✅
- DC01: February 3, 2026 02:15 UTC ✅
- DC02: February 3, 2026 03:10 UTC ✅
- WORKSTATION-01: February 1, 2026 18:30 UTC ✅
- WORKSTATION-02: February 2, 2026 19:15 UTC ✅

**Recovery Time Objective (RTO):** 4 hours  
**Recovery Point Objective (RPO):** 24 hours

---

## Initial Alerts & Detection

### Timeline of Alerts Received by SOC

#### 10:49:25 UTC - CrowdStrike Alert #1 (HIGH Severity)
```
Alert: Tamper Protection Violation
Source: CrowdStrike Falcon
Severity: HIGH
Hostname: WORKSTATION-01.contoso.local
User: CONTOSO\sarah.mitchell
Process: powershell.exe
Detection: Attempt to stop CSFalconService blocked
Technique: T1562.001 (Disable or Modify Tools)
Action Taken: BLOCKED
Status: Requires investigation
```

**SOC Response:** Ticket created (INC-2026-0203-001), assigned to Tier 1 analyst


## Exercise Objectives

### Primary Objectives

1. **Timeline Reconstruction**
   - Reconstruct complete attack timeline from initial access through impact
   - Identify all compromised systems and accounts
   - Map attacker movements through the environment

2. **Forensic Analysis**
   - Analyze Windows Event Logs (Security, System, Application)
   - Correlate Sysmon events for process execution chains
   - Extract PowerShell execution artifacts
   - Identify credential harvesting evidence

3. **MITRE ATT&CK Mapping**
   - Map all attacker techniques to MITRE ATT&CK framework
   - Identify tactics used across the kill chain
   - Document evidence for each technique

4. **IOC Extraction**
   - Identify network indicators (IPs, domains, URLs)
   - Document file-based indicators (hashes, paths, names)
   - Extract process execution indicators
   - Identify registry and service modifications

5. **Impact Assessment**
   - Determine scope of data breach
   - Assess domain compromise level
   - Evaluate encryption impact
   - Calculate business disruption

### Secondary Objectives

6. **Detection Improvement**
   - Identify detection gaps that allowed attack progression
   - Recommend detection rule improvements
   - Propose monitoring enhancements

7. **Response Planning**
   - Develop containment strategy
   - Plan eradication steps
   - Design recovery procedures
   - Create communication plan

8. **EDR Evasion Analysis**
   - Understand how the threat actor evaded EDR
   - Research real-world usage by ransomware groups
   - Evaluate detection opportunities
   - Propose preventive controls

---

## Investigation Starting Points

### For Analysts New to the Exercise

#### Starting Point #1: Initial Alert Investigation
**Where to Start:** Review CrowdStrike alerts from 10:49:25 UTC onwards

**Key Questions:**
- What triggered the first tamper protection alert?
- What process attempted to stop CrowdStrike service?
- Who was logged into WORKSTATION-01 at that time?
- What activity preceded the tamper attempt?

**Files to Examine:**
- `unified_timeline_enterprise.csv` - Filter for WORKSTATION-01 around 10:49:00
- PowerShell logs (Event 4103, 4104) on WORKSTATION-01
- CrowdStrike detection alerts

---

#### Starting Point #2: User Activity Analysis
**Where to Start:** Investigate sarah.mitchell account activity on February 3, 2026

**Key Questions:**
- What was sarah.mitchell's normal behavior pattern?
- Were any suspicious PowerShell commands executed?

*
### Difficulty Levels

#### **Beginner Level:**
- Use the IOC list above to search the CSV timeline
- Focus on key events: phishing, credential dumping, encryption
- Answer: What systems were compromised? What accounts were compromised?

#### **Intermediate Level:**
- Reconstruct complete attack timeline without IOC hints
- Identify all MITRE ATT&CK techniques used
- Explain the EDR bypass technique
- Calculate total data stolen and encrypted

#### **Advanced Level:**
- Perform full forensic analysis from raw logs (not just CSV)
- Identify detection gaps and propose improvements
- Build detection rules to prevent similar attacks
- Write executive report suitable for board presentation

---

### Files Provided for Investigation

1. **unified_timeline_enterprise.csv** - Main investigation file (5,880 events)
2. **DATA_EXFILTRATION_PHASE_SUMMARY.md** - Attack phase documentation
3. **EDR_EVASION_TECHNIQUE.md** - EDR bypass technical details
4. **EXERCISE_DIFFICULTY_ANALYSIS.md** - Signal-to-noise analysis
5. **DFIR_Incident_Response_Report_Professional.docx** - Reference answer key

---

### Success Criteria

**Minimum Requirements:**
- ✅ Identify initial access vector (phishing)
- ✅ Identify compromised systems (4 systems)
- ✅ Identify compromised accounts (6 accounts)
- ✅ Explain EDR bypass method (Safe Mode boot)
- ✅ Calculate data theft (156 MB) and encryption (1,247 files)

**Full Credit:**
- ✅ Complete timeline reconstruction (all 7 phases)
- ✅ MITRE ATT&CK mapping (21+ techniques)
- ✅ Comprehensive IOC list
- ✅ Detection gap analysis
- ✅ Professional incident report

---

**Exercise Version:** 1.4
**Exercise Author:** Andy Macready
**Creation Date:** February 3, 2026  
**Last Updated:** February 15, 2026  
**Difficulty:** HARD (Professional Level)  
**Estimated Time:** 2-4 hours (intermediate), 4-6 hours (beginner)

---

**Good luck with your investigation! Remember: Real incidents are just as complex, if not more so. This exercise mirrors actual APT and ransomware attack patterns observed in the wild.**