# Enterprise Security Incident - DFIR Training Exercise

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Difficulty](https://img.shields.io/badge/difficulty-beginner%20to%20advanced-orange.svg)
![Type](https://img.shields.io/badge/type-DFIR%20training-green.svg)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-21%2B%20techniques-red.svg)

A realistic, enterprise-grade Digital Forensics and Incident Response (DFIR) training scenario based on actual APT and ransomware attack patterns. This exercise provides hands-on experience investigating a sophisticated multi-stage cyberattack from initial compromise through ransomware deployment.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Scenario Background](#scenario-background)
- [Learning Objectives](#learning-objectives)
- [What's Included](#whats-included)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Difficulty Levels](#difficulty-levels)
- [Exercise Structure](#exercise-structure)
- [Success Criteria](#success-criteria)
- [Tips for Success](#tips-for-success)
- [Educational Philosophy](#educational-philosophy)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## 🎯 Overview

This repository contains a comprehensive DFIR training exercise simulating a **real-world enterprise breach**. On February 3, 2026, CONTOSO Corporation experienced a sophisticated cyberattack involving:

### Key Features

✅ **Realistic Timeline:** 5,629 real-world formatted events spanning 43 minutes of attack activity  
✅ **Authentic TTPs:** Based on actual APT and ransomware-as-a-service (RaaS) operations  
✅ **Comprehensive Logging:** Windows Security, Sysmon, PowerShell, and EDR logs  
✅ **MITRE ATT&CK Mapped:** 21+ techniques across all kill chain phases  
✅ **Multiple Difficulty Levels:** Suitable for beginners through advanced practitioners  
✅ **Professional Deliverables:** Sample incident response reports and IOC documentation  

---

## 📖 Scenario Background

### The Attack

**Organization:** CONTOSO Corporation (Technology/Software Development, ~1,500 employees)  
**Date:** February 3, 2026  
**Attack Duration:** ~43 minutes (10:47:33 - 11:30:00 UTC)  
**Attack Type:** Advanced Persistent Threat (APT) with Ransomware Deployment  

---

## 🎓 Learning Objectives

This exercise is designed to develop practical skills in:

### Technical Skills
- 🔍 **Log Analysis** - Parse and correlate Windows Security, Sysmon, and PowerShell logs
- 📊 **Timeline Reconstruction** - Build comprehensive attack timelines from raw event data
- 🎯 **Threat Hunting** - Identify malicious activity within normal enterprise noise (1:139.7 signal-to-noise ratio)
- 🗺️ **MITRE ATT&CK Mapping** - Map observed techniques to the ATT&CK framework
- 🚨 **IOC Extraction** - Identify and document indicators of compromise
- 🛡️ **EDR Analysis** - Understand security control bypass techniques

### Professional Skills
- 📝 **Incident Reporting** - Write executive and technical incident response reports
- 💼 **Business Impact Assessment** - Evaluate security incidents from a business perspective
- 🎯 **Detection Engineering** - Identify gaps and develop detection improvements
- 🤝 **Communication** - Present findings to technical and non-technical stakeholders

### Knowledge Areas
- Understanding of Windows authentication mechanisms (NTLM, Kerberos)
- Active Directory attack techniques
- EDR bypass methods
- Ransomware operations and double extortion models
- Enterprise logging architecture and SIEM concepts

---

## 📦 What's Included

### Core Exercise Files

```
📁 DFIR-Training-Exercise/
├── 📄 unified_timeline_enterprise_CLASSIFIED.csv     # Main investigation file (5,629 events)
├── 📄 Cyber_Security_Exercise_Environment_Documentation.md  # Environment guide
└── 📁 supporting_materials/
    ├── 📄 DFIR_Incident_Response_Report.docx        # Sample professional report
```

### Dataset Specifications

**Timeline File:** `unified_timeline_enterprise_CLASSIFIED.csv`
- **Total Events:** 5,629
- **Malicious Events:** 40 (0.71%)
- **Benign Events:** 5,589 (99.29%)
- **Signal-to-Noise Ratio:** 1:139.7 (realistic enterprise environment)
- **Systems Covered:** 58 (workstations, servers, domain controllers)
- **Time Span:** February 3, 2026 (incident day)
- **Log Sources:** Windows Security, Sysmon, PowerShell, CrowdStrike Falcon

**CSV Columns:**
- Timestamp, System, Log_Type, Event_ID, Event_Category
- User, Process, Description, Source_IP, Dest_IP, Details
- Classification (Malicious/Benign - for validation only)

---

## 🔧 Prerequisites

### Required Knowledge

**Beginner Level:**
- Basic understanding of Windows operating system
- Familiarity with spreadsheet software (Excel/Google Sheets)
- Understanding of common network concepts (IP addresses, ports)
- Awareness of cybersecurity fundamentals

**Intermediate Level:**
- Windows Event Log structure (Event IDs, log sources)
- Basic understanding of Active Directory
- Command-line proficiency (grep, awk, PowerShell)
- Familiarity with common attack techniques

**Advanced Level:**
- Deep understanding of Windows internals
- Active Directory security architecture
- Python programming with data analysis libraries
- Experience with SIEM platforms
- Knowledge of MITRE ATT&CK framework

### Tools Required

#### Beginner Tools
- **Spreadsheet Software:** Excel, LibreOffice Calc, or Google Sheets
- **Text Editor:** Notepad++, VSCode, or any capable text editor
- **CSV Viewer:** Any tool that can handle 5,600+ row CSV files

#### Intermediate Tools
- **Spreadsheet Software:** Excel or LibreOffice
- **Command-Line Tools:** `grep`, `awk`, `sort`, `cut` (built-in on Linux/Mac, available via WSL/Cygwin on Windows)
- **PowerShell:** For advanced filtering (`Import-Csv`, `Where-Object`, `Group-Object`)
- **Python (Optional):** For scripting custom analysis

#### Advanced Tools
- **Python + pandas:** **REQUIRED** for efficient large-scale analysis
- **grep/awk:** For quick initial filtering
- **SIEM Platform:** Splunk, ELK Stack, or similar (highly recommended)
- **Jupyter Notebook:** For interactive analysis and documentation
- **Timeline Tools:** Plaso/log2timeline, TimeSketch
- **Visualization:** Matplotlib, Plotly for timeline visualization
- **Custom Scripts:** For automated correlation and pattern detection

---

## 🚀 Getting Started

### Quick Start (Beginner)

1. **Clone or Download** this repository
   ```bash
   git clone https://github.com/amacready/Contoso-DFIR-Exercise.git
   cd DFIR-Training-Exercise
   ```

2. **Read the Environment Documentation**
   - Open `Cyber_Security_Exercise_Environment_Documentation.md`
   - Familiarize yourself with the CONTOSO Corporation infrastructure
   - Note the security controls and logging capabilities

3. **Open the Timeline**
   - Load `incident_timeline.csv` in Excel or Timepline Explorer (Eric Zimmerman tool)

4. **Start Investigating**
   - Indetify Initial Access - follow the initial alert that indicated a problem, then follow the bouncing ball.


5. **Document Your Findings**
   - Create a separate document for your investigation notes
   - Track: Compromised accounts, compromised systems, IOCs, timeline events

### Quick Start (Intermediate)

1. **Load Data into Analysis Tool and Examples ONLY**
   ```bash
   # Using command-line tools
   grep -i "powershell" incident_timeline.csv | less
   awk -F',' '$4 == "4624" {print $1,$6}' incident_timeline.csv
   ```

   ```powershell
   # Using PowerShell
   $events = Import-Csv incident_timeline.csv
   $events | Where-Object {$_.Event_ID -eq "4688"} | Select Timestamp,System,User,Process
   ```

2. **Focus on Key Systems**
   - Start with systems showing suspicious activity - investigate the initail Crowdstrike alert


3. **Build Correlation Queries**
   - Link process creation to network connections
   - Correlate logons with suspicious activity
   - Map lateral movement paths

### Quick Start (Advanced)

1. **Python Analysis Setup**
   ```python
   import pandas as pd
   import numpy as np
   from datetime import datetime
   
   # Load timeline
   df = pd.read_csv('incident_timeline.csv')
   
   
   # Quick overview
   print(f"Total events: {len(df)}")
   print(f"Systems: {df['System'].nunique()}")
   print(f"Users: {df['User'].nunique()}")
   print(f"Date range: {df['Timestamp'].min()} to {df['Timestamp'].max()}")
   ```

2. **SIEM Import**
   - Import CSV into Splunk, ELK, or your preferred SIEM
   - Create custom dashboards for timeline visualization
   - Set up correlation rules for technique detection

3. **Advanced Analysis**
   - Use pandas for cross-system correlation
   - Build attack graphs showing lateral movement
   - Develop automated detection rules
   - Generate MITRE ATT&CK Navigator layers

---

## 📊 Difficulty Levels

### 🟢 Beginner (4-6 hours)

**Objective:** Understand the basics of incident investigation

**Tasks:**
- Identify compromised systems
- Find the initial access method
- Discover compromised user accounts
- Build a basic attack timeline
- Extract primary IOCs

**Recommended Approach:**
- Start by filtering for Event_ID related to process creation
- Look for PowerShell with unusual command lines
- Document what you find as you go

**Expected Deliverable:**
- Simple timeline document (Word/Google Docs)
- List of compromised systems and accounts
- Basic IOC list (IPs, filenames, usernames)

---

### 🟡 Intermediate (2-4 hours)

**Objective:** Conduct a thorough forensic investigation

**Tasks:**
- Complete attack timeline with all phases
- Map all MITRE ATT&CK techniques used
- Extract comprehensive IOC list
- Identify security control bypasses
- Assess business impact

**Recommended Approach:**
- Use filtering to track specific users across systems
- Correlate network connections with process execution
- Identify parent-child process relationships
- Document attack techniques with evidence
- Cross-reference with MITRE ATT&CK framework

**Expected Deliverable:**
- Detailed timeline with phase breakdown
- MITRE ATT&CK technique mapping (all 21+ techniques)
- Comprehensive IOC documentation
- Security gap analysis

---

### 🔴 Advanced (1-2 hours investigation + 2-3 hours reporting)

**Objective:** Produce professional-grade incident response deliverables

**Tasks:**
- Automated detection rule development
- Gap analysis of security controls
- Executive incident response report
- Technical incident response report
- Detection improvement recommendations
- Custom threat hunting queries

**Recommended Approach:**
- Use Python/pandas for automated correlation
- Build detection signatures for each technique
- Create MITRE ATT&CK Navigator layer
- Write both executive summary and technical deep-dive
- Develop preventive control recommendations

**Expected Deliverable:**
- Executive incident response report (5-10 pages)
- Technical incident response report (20-30 pages)
- Detection rules in Sigma/YARA/Suricata format
- MITRE ATT&CK Navigator JSON layer
- Strategic security recommendations

---

## 🎯 Exercise Structure

### Phase 1: Environment Familiarization (15-30 min)

Read the environment documentation to understand:
- Network topology (VLANs, IP addressing)
- Active Directory structure
- Security controls deployed (CrowdStrike, Sysmon)
- Key systems and users
- Logging capabilities

### Phase 2: Initial Triage (30-60 min)

Identify signs of compromise:
- Suspicious process execution
- Unusual network connections
- Privilege escalation indicators
- Credential access attempts

### Phase 3: Timeline Reconstruction (1-3 hours)

Build comprehensive attack timeline:
- Identify initial access vector
- Track lateral movement
- Document privilege escalation
- Identify data exfiltration
- Determine impact

### Phase 4: Analysis & Mapping (30-90 min)

Deep analysis:
- Map to MITRE ATT&CK framework
- Identify all techniques used
- Extract comprehensive IOCs
- Assess security control effectiveness

### Phase 5: Reporting (30 min - 3 hours depending on level)

Document findings:
- **Beginner:** Simple timeline and IOC list
- **Intermediate:** Detailed technical report
- **Advanced:** Executive brief + technical deep-dive

---

## ✅ Success Criteria

To successfully complete this exercise, you should be able to answer:

### Initial Access
✅ **How did the attacker initially compromise the environment?**
- Attack vector identified
- Compromised user identified
- Initial payload mechanism understood

### Credential Theft
✅ **Which accounts were compromised and how?**
- All compromised accounts documented
- Credential theft technique identified (Mimikatz)
- Scope of credential access determined

### Lateral Movement
✅ **How did the attacker move through the network?**
- Lateral movement path documented
- Techniques identified 
- Compromised systems tracked

### Privilege Escalation
✅ **How did the attacker gain Domain Admin access?**
- Privilege escalation method identified
- Domain compromise technique documented
- Persistence mechanisms discovered 

### Data Impact
✅ **What data was accessed, stolen, or destroyed?**
- Exfiltrated data quantified
- Encrypted files counted
- Sensitive data exposure assessed

### Techniques Used
✅ **What MITRE ATT&CK techniques were employed?**
- All 21+ techniques identified and mapped
- Kill chain phases understood
- Tactic progression documented

### Indicators of Compromise
✅ **What IOCs can prevent similar attacks?**
- Network IOCs extracted
- File IOCs documented
- Behavioral IOCs identified

### Recommendations
✅ **How can this be prevented in the future?**
- Security gaps identified
- Preventive controls recommended
- Detection improvements proposed

---

## 💡 Tips for Success

### Investigation Methodology

1. **Don't Rush**
   - Take 15-30 minutes to understand the environment first
   - Read the documentation thoroughly before analyzing logs
   - Understand what "normal" looks like before hunting for "abnormal"

2. **Think Like an Attacker**
   - After initial access, what would YOU do next?
   - How would YOU avoid detection?
   - What would YOUR objectives be?

3. **Correlate Events**
   - Single events rarely tell the full story
   - Look for patterns across time and systems
   - Connect process execution → network connections → file operations

4. **Use Multiple Log Sources**
   - Cross-reference Windows Security logs with Sysmon
   - PowerShell logs reveal command execution details
   - EDR alerts show what was detected (and what bypassed controls)

5. **Document as You Go**
   - Keep running notes in a separate document
   - Track your hypotheses and evidence
   - Build your timeline incrementally - don't wait until the end

6. **Ask "Why?"**
   - Why is PowerShell spawning from Compromised APplication
   - Why is bcdedit being run?
   - Why is there network traffic going to?

7. **Stay Organized**
   - Create separate lists for: IOCs, compromised accounts, compromised systems, techniques
   - Use tabs/sections in your notes
   - Color-code events by severity or phase

8. **Real World Mindset**
   - Treat this like an actual incident
   - Your "client" is waiting for answers
   - Your report may be read by executives, lawyers, or regulators

9. **Focus on Anomalies**
   - In 5,629 events, 99.29% are benign
   - Don't try to understand every event
   - Focus on what stands out as unusual or suspicious

10. **Validate Assumptions**
    - Don't assume - verify with evidence
    - If you think user X did action Y, find the log entry that proves it
    - Back up every claim with specific evidence

### Common Pitfalls to Avoid

❌ **Skipping Environment Documentation** - You'll miss critical context  
❌ **Trying to Analyze Everything** - Focus on suspicious activity  
❌ **Not Correlating Across Systems** - The attack spans multiple hosts  
❌ **Ignoring Timestamps** - Timeline sequence is critical  
❌ **Overlooking "Boring" Events** - Logons, network shares matter  
❌ **Not Documenting Evidence** - You'll forget your reasoning later  

---

## 🎓 Educational Philosophy

### Why This Exercise Exists

**This exercise is different:**
- ✅ Based on actual APT and ransomware operations
- ✅ Realistic enterprise environment (1,500 employees, 1,600 systems)
- ✅ Real-world signal-to-noise ratio (1:139.7)
- ✅ Authentic logging (5,629 events from multiple sources)
- ✅ Professional-grade deliverables expected

### Design Principles

**1. Realistic Complexity**

The timeline contains:
- 5,629 total events (not 50, not 500,000)
- 99.29% benign enterprise activity
- Multiple systems generating events simultaneously
- Normal user behavior mixed with attacker activity
- Incomplete information requiring correlation

**Why?** Real investigations don't hand you a silver platter of only malicious events. You need to find needles in haystacks.

**2. Scalable Difficulty**

- **Beginners** can succeed by filtering for obvious indicators (PowerShell, known IPs)
- **Intermediate** practitioners reconstruct the full timeline
- **Advanced** responders develop detection rules and write executive reports

**Why?** The same dataset serves learners at all levels, allowing natural progression.

**3. Practical Skill Development**

You'll practice the exact skills needed in real incident response:
- Log analysis and correlation
- Timeline reconstruction
- MITRE ATT&CK mapping
- IOC extraction
- Business impact assessment
- Executive communication
- Technical report writing

**Why?** These are the skills that employers value and real-world incidents demand.

### What Makes This "Realistic"?

**✅ Authentic Attack Pattern**
- Based on actual BlackCat/ALPHV, LockBit, and Hive ransomware operations
- Uses documented EDR bypass techniques 
- Follows real APT playbooks (credential dumping → lateral movement → domain compromise

**✅ Real-World Logging**
- Events formatted exactly as Windows/Sysmon/EDR generate them
- Realistic Event IDs (4624, 4688, Sysmon Event 1, 10, etc.)
- Proper parent-child process relationships
- Accurate network connection logging

**✅ Enterprise Environment**
- 1,500 employee organization
- Active Directory domain (contoso.local)
- Multiple VLANs and network segments
- Realistic security controls (CrowdStrike Falcon, Sysmon)
- Normal business operations generating background noise

**✅ Forensically Sound**
- Every action requires proper authentication (Event 4624)
- Process execution follows Windows architecture
- Network connections have proper source/destination
- Timeline is internally consistent
- No "magic" events that couldn't happen in reality

### Learning Philosophy

**"Learn by Doing, Not by Reading"**

You won't learn DFIR by reading about it. This exercise throws you into the deep end:
- No hand-holding (beyond environment documentation)
- No step-by-step walkthrough (intentionally)
- No "Click here to see the answer" buttons
- You have to figure it out, just like in real incidents

**"Fail Fast, Learn Faster"**

Your first attempt won't be perfect. That's expected and encouraged:
- Try different filtering approaches
- Test hypotheses and discard wrong ones
- Make mistakes and learn from them
- Compare your findings to the ground truth afterward

**"Professional Standards"**

This isn't just about finding the answer—it's about:
- **Documenting your methodology** - Can someone reproduce your analysis?
- **Supporting claims with evidence** - Where's the proof?
- **Communicating clearly** - Can a non-technical executive understand?
- **Meeting deadlines** - Real incidents have time pressure

---

## 🤝 Contributing

Contributions to improve this training exercise are welcome! Here's how you can help:

### Ways to Contribute

- 🐛 **Report Issues:** Found a data quality problem? Inconsistency? Open an issue.
- 📝 **Improve Documentation:** Clearer explanations, better examples, additional guidance
- 🔧 **Add Tools:** Scripts for analysis, visualization tools, automation helpers
- 📊 **Share Solutions:** Did you develop an interesting analysis approach? Share it!
- 🎓 **Educational Content:** Tutorials, walkthrough videos, teaching materials
- 🌍 **Translations:** Help make this accessible to non-English speakers

### Reporting Issues

If you find problems with the dataset
- **Data Quality Issues:** Events that violate Windows architecture, impossible timelines, authentication errors
- **Documentation Errors:** Wrong IP addresses, incorrect system descriptions, outdated information
- **Tool Incompatibility:** Analysis scripts that don't work on certain platforms

Please open an issue with:
- Clear description of the problem
- Evidence (specific event, row number, timestamp)
- Suggested fix if you have one

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

**In Plain English:**
- ✅ Use this for training, education, and personal skill development
- ✅ Modify and adapt for your organization's needs
- ✅ Share with colleagues, students, and the community
- ✅ Use in corporate training programs
- ❌ Don't claim you created this
- ❌ Don't sell this as a commercial product without significant modification

### Attribution

If you use this exercise in training, presentations, or educational content, please credit:
```
Enterprise Security Incident - DFIR Training Exercise
Created by Andy Macready
https://github.com/amacready/Contoso-DFIR-Exercise
```

---

## 🙏 Acknowledgments

This training exercise was developed to address the gap between theoretical cybersecurity education and practical incident response skills. It represents hundreds of hours of work to create a realistic, educational, and challenging learning experience.

### Inspiration

This exercise draws from:
- Real-world APT campaigns and ransomware operations
- Actual incident response engagements (anonymized and sanitized)
- MITRE ATT&CK framework and threat intelligence
- Enterprise security architecture best practices
- Professional DFIR community knowledge

### Special Thanks

- The **DFIR community** for sharing techniques, tools, and knowledge
- **MITRE** for the ATT&CK framework providing common language for threat analysis
- **Microsoft** for comprehensive Windows logging documentation
- **Sysmon community** for advanced logging capabilities
- **CrowdStrike**, **Splunk**, and other security vendors for detailed attack reports
- **Ransomware victims** who shared their stories to help others learn

### Educational Mission

This project is released as **free, open educational material** because:
- 🎓 **Cybersecurity skills shouldn't be behind paywalls**
- 🌍 **Everyone deserves access to quality training**
- 💪 **We're stronger when we learn together**
- 🛡️ **Better-trained defenders make us all safer**

---

## 📞 Contact & Support

### Questions?

- 📖 **Read the Docs First:** Most questions are answered in `Cyber_Security_Exercise_Environment_Documentation.md`
- 💬 **Discussions:** Use GitHub Discussions for general questions, tips, and community support
- 🐛 **Issues:** Report bugs or data quality problems via GitHub Issues
- 📧 **Direct Contact:** [Your contact method if you want to provide one]

### Resources

- 🔗 **MITRE ATT&CK:** https://attack.mitre.org/
- 🔗 **Sysmon Configuration:** https://github.com/SwiftOnSecurity/sysmon-config
- 🔗 **Windows Event Logging:** https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/
- 🔗 **SANS DFIR:** https://www.sans.org/cyber-security-courses/advanced-incident-response-threat-hunting-training/

### Stay Updated

- ⭐ **Star this repo** to get notifications of updates
- 👁️ **Watch this repo** for new releases
- 🍴 **Fork this repo** to create your own variations

---

## 🎯 Final Thoughts

**This is more than just a training exercise—it's a simulation of what you'll face in real incidents.**

When you work through this timeline:
- You're practicing the same analysis actual incident responders do
- You're developing skills that employers desperately need
- You're learning to recognize patterns that could save your organization
- You're building the mindset of a professional DFIR analyst

**The techniques in this exercise are real. The tools are real. The challenge is real.**

The only thing that's not real is the consequences. Use this opportunity to make mistakes, try different approaches, and learn without the pressure of a live incident.

**When (not if) you face a real breach, you'll be ready.**

---

**Good luck with your investigation! 🔍🛡️**

**Remember:** The best way to learn DFIR is by doing DFIR. You've got 5,629 events waiting for analysis. Your investigation starts now.

---

<p align="center">
  <strong>If this exercise helped you learn, please ⭐ star this repository and share it with others!</strong>
</p>

<p align="center">
  Made with 🔐 for the DFIR community
</p>
