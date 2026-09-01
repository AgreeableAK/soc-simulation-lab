# Modular SOC Simulation Lab

## Project Overview

The **Modular SOC Simulation Lab** is a progressively built cybersecurity home lab designed to simulate the workflow of a Security Operations Center (SOC).

The project starts with a small, resource-conscious environment and expands in phases. Each phase introduces a specific SOC capability, produces a measurable result, and is documented before the lab is expanded.

The primary objective is not simply to deploy security tools. The objective is to demonstrate the practical SOC workflow:

```text
Security Event
    ↓
Telemetry Collection
    ↓
SIEM Ingestion
    ↓
Detection
    ↓
Alert Triage
    ↓
Investigation
    ↓
Classification
    ↓
Incident Response
    ↓
Documentation
    ↓
Detection Improvement
```

The lab is designed around **Splunk** as the primary SIEM and uses Windows, Sysmon, Kali Linux, and later network monitoring, Active Directory, threat intelligence, and Python-based automation.

---

# 1. Project Goals

## Primary Goals

- Build a modular SOC environment using VMware.
- Learn practical Splunk SIEM administration and investigation.
- Collect Windows endpoint telemetry.
- Understand and use Sysmon telemetry.
- Build and test security detections using SPL.
- Generate controlled attack activity.
- Investigate alerts using evidence and timelines.
- Classify alerts as true positives or false positives.
- Document incidents using a repeatable SOC investigation process.
- Gradually introduce network monitoring.
- Build an Active Directory environment for enterprise-style scenarios.
- Develop custom detections and detection engineering practices.
- Introduce threat intelligence enrichment.
- Automate repetitive SOC tasks with Python.

## Secondary Goals

- Create a GitHub portfolio project based on real lab evidence.
- Develop investigation reports that can be shown as portfolio artifacts.
- Build practical experience relevant to entry-level SOC Analyst roles.
- Learn to distinguish between tool deployment and actual security operations.
- Practice reproducible attack and detection scenarios.

---

# 2. Project Philosophy

The lab follows five principles.

### 2.1 Build Small

The lab begins with only the infrastructure necessary to demonstrate the current capability.

### 2.2 Add One Capability at a Time

New technologies are introduced only after the previous phase is functioning and documented.

### 2.3 Generate Our Own Evidence

Whenever possible, attacks and normal activity are generated inside the lab rather than relying entirely on pre-built alerts.

### 2.4 Investigate, Do Not Just Detect

An alert is not the end result. The analyst must determine what happened and whether the activity is actually suspicious.

### 2.5 Document Everything Important

Each meaningful detection and investigation should have reproducible evidence, methodology, conclusions, and lessons learned.

---

# 3. Host Hardware

The initial lab is designed for the following host system:

| Component | Specification |
|---|---|
| Host OS | Windows |
| CPU | AMD Ryzen 7 7435HS |
| RAM | 24 GB |
| GPU | NVIDIA RTX 4050 6 GB |
| Hypervisor | VMware Workstation |
| Primary Constraint | 24 GB system RAM |

The RTX 4050 is not expected to provide significant value for the core SOC workload. The lab is primarily constrained by system RAM, CPU, storage, and virtualization resources.

---

# 4. Initial Lab Architecture

The first useful version of the lab will contain three virtual machines.

```text
                         Windows Host
                 Ryzen 7 / 24 GB RAM / RTX 4050
                              │
                       VMware Workstation
                              │
                    Isolated SOC Network
                              │
              ┌───────────────┼────────────────┐
              │               │                │
              ▼               ▼                ▼
       Splunk Server     Windows Endpoint   Kali Linux
          SIEM               Target          Attacker
              │               │                │
              │        Sysmon + Windows       │
              │             Logs               │
              │               │                │
              └───────────────┴────────────────┘
                              │
                       Security Telemetry
                              │
                              ▼
                           Splunk
```

The architecture will expand in later phases.

---

# 5. Initial VM Resource Strategy

The exact allocation will be adjusted after observing actual resource usage.

## Initial target

| VM | Purpose | RAM Target | CPU Target | Disk Target |
|---|---|---:|---:|---:|
| Splunk Server | SIEM | 8 GB | 4 vCPU | 80-100 GB |
| Windows Endpoint | SOC endpoint | 6 GB | 2-4 vCPU | 60+ GB |
| Kali Linux | Attack simulation | 2-3 GB | 2 vCPU | 40+ GB |

The remaining host memory should be left available for Windows and VMware overhead.

The lab should avoid unnecessarily running every VM at the same time.

---

# 6. Network Architecture

The attack environment should be isolated from the normal home network.

Initial concept:

```text
                Normal Host Network
                       │
                    Windows
                       │
                VMware Workstation
                       │
             Isolated Lab Network
                       │
       ┌───────────────┼────────────────┐
       │               │                │
    Splunk          Windows            Kali
    Server          Endpoint          Attacker
```

The exact VMware network configuration will be finalized during Phase 0.

## Security requirements

- Do not expose intentionally vulnerable systems directly to the home LAN.
- Prefer isolated or host-only networking for attack simulations.
- Use controlled NAT only when a VM needs Internet access for legitimate installation or updates.
- Do not bridge vulnerable lab systems to the physical network unless there is a specific reason and the risks are understood.
- Maintain clean snapshots before major experiments.

---

# 7. Project Phases

The project consists of 13 phases.

```text
Phase 0   Lab Foundation
Phase 1   Splunk SIEM
Phase 2   Windows Endpoint Telemetry
Phase 3   Log Collection
Phase 4   Detection Engineering
Phase 5   Attack Simulation
Phase 6   SOC Investigation
Phase 7   Incident Response
Phase 8   Network Security Monitoring
Phase 9   Active Directory
Phase 10  Advanced Detection Engineering
Phase 11  Threat Intelligence
Phase 12  SOC Automation
```

The project does not require completing every phase before it becomes portfolio-ready.

A strong first portfolio version can be produced around Phases 0-6 if the implementation and documentation are detailed.

---

# 8. Phase 0: Lab Foundation

## Objective

Create a safe, reproducible VMware environment.

## Components

- VMware Workstation
- Splunk Server VM
- Windows Endpoint VM
- Kali Linux VM
- Isolated VMware network

## Tasks

1. Plan the network.
2. Define VM names.
3. Allocate CPU, RAM, and disk.
4. Install the operating systems.
5. Configure hostnames.
6. Configure IP addressing.
7. Test connectivity.
8. Create clean snapshots.
9. Document the architecture.
10. Verify that the attack network is isolated.

## Expected Result

All lab machines can communicate as intended while remaining separated from the normal network.

## Deliverables

```text
architecture/
├── network-diagram.png
└── architecture.md
```

## Completion Criteria

- All required VMs boot successfully.
- IP addressing is documented.
- Connectivity has been tested.
- Snapshots exist.
- Network isolation has been verified.

---

# 9. Phase 1: Splunk SIEM

## Objective

Deploy Splunk as the central security monitoring platform.

## Components

```text
Splunk Enterprise
      │
      ├── Indexes
      ├── Searches
      ├── Dashboards
      ├── Alerts
      └── Users / Roles
```

## Tasks

- Install Splunk.
- Configure the initial Splunk instance.
- Understand Splunk indexes and sources.
- Create a security-focused index structure.
- Learn basic SPL.
- Perform test searches.
- Configure basic dashboards.
- Understand time ranges and event fields.
- Document the Splunk configuration.

## Core Skills

- SIEM deployment
- Log indexing
- SPL fundamentals
- Event searching
- Basic dashboard creation
- Security monitoring concepts

## Deliverable

A functioning Splunk instance ready to receive endpoint telemetry.

## Completion Criteria

- Splunk is accessible.
- Searches work.
- Test data can be indexed.
- Basic security searches can be executed.

---

# 10. Phase 2: Windows Endpoint Telemetry

## Objective

Turn the Windows VM into a useful SOC endpoint.

## Telemetry Sources

```text
Windows Endpoint
      │
      ├── Windows Security Logs
      ├── System Logs
      ├── PowerShell Logs
      └── Sysmon
```

## Windows Logging

The lab should enable appropriate Windows auditing for activities such as:

- Successful logons
- Failed logons
- Process creation
- Account activity
- Privilege-related activity
- PowerShell activity
- System changes

## Sysmon

Sysmon will provide additional endpoint visibility.

Relevant telemetry may include:

- Process creation
- Network connections
- File creation
- Registry activity
- DNS activity
- Process relationships
- Other endpoint events supported by the chosen configuration

## Important Principle

Sysmon should not be treated as a magic detection tool.

The goal is to understand:

```text
Event
  ↓
What does it mean?
  ↓
Why does a SOC analyst care?
  ↓
What detection can use it?
```

## Deliverable

A Windows telemetry reference documenting:

| Telemetry | Purpose | SOC Relevance |
|---|---|---|
| Windows Security Events | Authentication and security activity | Account investigation |
| Sysmon Process Events | Process execution | Endpoint investigation |
| Sysmon Network Events | Network connections | Network/process correlation |
| PowerShell Events | Script activity | Suspicious execution investigation |

## Completion Criteria

- Windows logging is working.
- Sysmon is generating events.
- Normal activity is visible.
- Important event types are understood.

---

# 11. Phase 3: Log Collection

## Objective

Build the end-to-end telemetry pipeline.

```text
Windows
   │
   ├── Security Logs
   ├── System Logs
   ├── PowerShell
   └── Sysmon
          │
          ▼
Splunk Universal Forwarder
          │
          ▼
       Splunk
          │
          ▼
     Search / Alert
```

## Tasks

- Install Splunk Universal Forwarder on Windows.
- Configure required inputs.
- Configure forwarding.
- Configure appropriate Splunk indexes.
- Verify data arrival.
- Troubleshoot missing events.
- Validate timestamps.
- Validate host/source/sourcetype information.
- Create searches for Windows and Sysmon events.

## Deliverable

A functioning telemetry pipeline:

```text
Endpoint → Forwarder → Splunk → Search
```

## Completion Criteria

- Windows events arrive in Splunk.
- Sysmon events arrive in Splunk.
- Events contain useful host and source information.
- Searches can reliably retrieve the expected events.

This is the first major technical milestone.

---

# 12. Phase 4: Detection Engineering

## Objective

Create the first security detections.

The focus is on understanding why a detection exists rather than copying large collections of rules.

## Initial Detection Categories

### Authentication

- Multiple failed logons
- Suspicious authentication patterns
- Authentication anomalies

### Account Activity

- Unexpected account creation
- Account changes
- Privilege-related changes

### Process Activity

- Suspicious process execution
- Unusual parent-child relationships
- Potentially suspicious administrative tools

### PowerShell

- Suspicious PowerShell execution
- Encoded or unusual command activity
- PowerShell launched by unexpected processes

### Persistence

- Scheduled task creation
- Registry-based persistence simulations
- Other controlled persistence mechanisms

## Detection Documentation

Every detection should contain:

```text
Detection Name
Objective
Data Source
Required Telemetry
SPL Query
Trigger Condition
Severity
False Positive Conditions
MITRE ATT&CK Mapping
Test Procedure
Expected Result
Tuning Notes
```

## Deliverable

A detection library:

```text
detections/
├── authentication/
├── account-activity/
├── process/
├── powershell/
└── persistence/
```

## Completion Criteria

- Detections trigger on controlled test activity.
- Normal activity does not produce excessive false positives.
- Each detection has documented logic.
- Detection limitations are documented.

---

# 13. Phase 5: Attack Simulation

## Objective

Introduce an attacker into the environment and generate controlled security activity.

Kali Linux becomes active during this phase.

```text
Kali Linux
   │
   │ Controlled attack activity
   ▼
Windows Endpoint
   │
   │ Telemetry
   ▼
Splunk
   │
   ▼
Detection
   │
   ▼
SOC Analyst
```

## Initial Scenarios

### Scenario 1: Network Reconnaissance

Generate controlled reconnaissance activity against the lab endpoint.

Objective:

- Understand network reconnaissance telemetry.
- Determine what the endpoint can see.
- Identify what Splunk can detect.

### Scenario 2: Port Scanning

Generate a controlled port scan.

Objective:

- Observe network-related activity.
- Determine available telemetry.
- Develop an investigation workflow.

### Scenario 3: Authentication Attack Simulation

Generate controlled failed authentication attempts.

Objective:

- Test authentication detections.
- Identify source IP.
- Establish an event timeline.
- Determine whether the activity represents malicious behavior.

### Scenario 4: PowerShell Activity

Generate controlled PowerShell activity.

Objective:

- Understand PowerShell telemetry.
- Test process and script-related detections.
- Compare normal and suspicious-looking activity.

### Scenario 5: Persistence Simulation

Create a controlled persistence mechanism.

Objective:

- Generate endpoint telemetry.
- Test persistence detections.
- Investigate the process that created the persistence mechanism.

## Safety

All attack activity must remain inside the isolated lab environment.

The project should prioritize controlled simulations and test commands rather than deploying uncontrolled malware or targeting external systems.

## Deliverable

```text
attack-scenarios/
├── 001-port-scan/
├── 002-authentication/
├── 003-powershell/
├── 004-process-execution/
└── 005-persistence/
```

Each scenario should contain:

```text
README.md
attack-notes.md
evidence/
screenshots/
```

---

# 14. Phase 6: SOC Investigation

## Objective

Investigate generated alerts like a SOC analyst.

This is one of the most important phases of the project.

The workflow should be:

```text
Alert
  ↓
Triage
  ↓
Validate
  ↓
Identify Affected Asset
  ↓
Identify User
  ↓
Establish Timeline
  ↓
Correlate Events
  ↓
Determine Technique
  ↓
Classify
  ↓
Assign Severity
  ↓
Recommend Response
  ↓
Document
```

## Investigation Questions

For each alert:

1. What happened?
2. When did it happen?
3. Which host was affected?
4. Which user was involved?
5. What was the source IP?
6. What process was involved?
7. What network activity occurred?
8. What events happened immediately before and after?
9. Is the activity expected?
10. What evidence supports the conclusion?
11. Is this a true positive or false positive?
12. What is the impact?
13. What should the SOC do next?

## True Positive Example

```text
Repeated authentication failures
        ↓
Same source IP
        ↓
Short time window
        ↓
Multiple targeted attempts
        ↓
No legitimate explanation
        ↓
True Positive
```

## False Positive Example

```text
Several failed logons
        ↓
User accidentally entered wrong password
        ↓
No suspicious follow-up activity
        ↓
User confirms activity
        ↓
False Positive
```

## Investigation Report Template

Each investigation should record:

```text
Incident ID
Title
Date / Time
Affected Host
Affected User
Source IP
Detection
Severity
Summary
Timeline
Evidence
Analysis
Classification
MITRE ATT&CK Mapping
Recommended Response
Lessons Learned
```

## Deliverable

```text
investigations/
├── INC-001/
├── INC-002/
└── INC-003/
```

---

# 15. Phase 7: Incident Response

## Objective

Move from detection and investigation into response.

The basic lifecycle is:

```text
Preparation
    ↓
Detection & Analysis
    ↓
Containment
    ↓
Eradication
    ↓
Recovery
    ↓
Lessons Learned
```

## Example

For a compromised Windows account:

```text
Detection
    ↓
Investigate authentication activity
    ↓
Confirm compromise
    ↓
Contain account
    ↓
Terminate malicious activity
    ↓
Remove persistence if present
    ↓
Verify endpoint
    ↓
Recover normal operation
    ↓
Document lessons learned
```

## Deliverable

Incident response reports documenting:

- Incident summary
- Evidence
- Timeline
- Impact
- Classification
- Containment
- Eradication
- Recovery
- Lessons learned

---

# 16. Phase 8: Network Security Monitoring

## Objective

Add network-level visibility and correlate it with endpoint telemetry.

Potential technologies:

- Suricata
- Zeek
- Other appropriate network monitoring tools

Architecture:

```text
Network Traffic
      │
      ▼
Network Monitoring
      │
      ▼
    Splunk
      │
      ├──────────────┐
      │              │
      ▼              ▼
Network Events   Windows Events
      │              │
      └──────┬───────┘
             ▼
       Correlated SOC
       Investigation
```

## Investigation Examples

- Port scans
- Suspicious network connections
- DNS activity
- Reconnaissance
- Network and endpoint correlation

## Deliverable

Network monitoring documentation and correlated investigations.

---

# 17. Phase 9: Active Directory

## Objective

Expand the lab into a small enterprise-style Windows environment.

Architecture:

```text
                 Domain Controller
                       │
              ┌────────┴────────┐
              │                 │
       Windows Client     Windows Client
              │                 │
              └────────┬────────┘
                       │
                     Splunk
```

## Potential Scenarios

- Account compromise
- Password spraying simulation
- Privilege escalation
- Group membership changes
- Domain reconnaissance
- Kerberos-related investigation
- Lateral movement simulation

## Why This Phase Comes Later

Active Directory adds significant infrastructure and troubleshooting complexity.

It should be introduced only after the analyst understands:

- Windows telemetry
- Splunk
- Endpoint investigation
- Detection engineering
- Attack simulation

---

# 18. Phase 10: Advanced Detection Engineering

## Objective

Develop custom detections based on behavior rather than simply relying on default alerts.

The detection engineering process:

```text
Threat Behavior
      ↓
Detection Hypothesis
      ↓
Required Telemetry
      ↓
SPL Logic
      ↓
Test
      ↓
Evaluate False Positives
      ↓
Tune
      ↓
Document
```

## Detection Documentation

Each detection should include:

- Detection objective
- Threat behavior
- Data sources
- SPL
- Trigger logic
- Severity
- Expected false positives
- Testing method
- Tuning history
- MITRE ATT&CK mapping
- Limitations

## Deliverable

```text
detections/
├── authentication/
├── endpoint/
├── powershell/
├── persistence/
├── network/
└── active-directory/
```

---

# 19. Phase 11: Threat Intelligence

## Objective

Use external intelligence to add context to an investigation.

Architecture:

```text
Splunk Alert
     │
     ├── IP
     ├── Domain
     ├── Hash
     └── URL
           │
           ▼
   Threat Intelligence
           │
           ▼
      Enrichment
           │
           ▼
      Investigation
```

Potential services and sources may include:

- VirusTotal
- AbuseIPDB
- AlienVault OTX
- NVD

The goal is not to collect as many feeds as possible.

The goal is to answer:

> Does the intelligence materially improve the investigation?

## Example

```text
Suspicious IP
    ↓
External reputation lookup
    ↓
Known malicious reputation
    ↓
Additional context
    ↓
Higher confidence in classification
```

---

# 20. Phase 12: SOC Automation

## Objective

Automate repetitive SOC tasks using Python.

Architecture:

```text
Splunk Alert
     │
     ▼
Python
     │
     ├── Extract Indicators
     ├── Enrich IP / Domain
     ├── Query Intelligence
     ├── Format Evidence
     ├── Generate Report
     └── Produce Analyst Context
```

## Potential Automation Projects

### Alert Enrichment

Input:

```text
IP Address
```

Output:

```text
IP
Reputation
Country
ASN
Known Abuse
External Context
```

### Investigation Report Generator

Input:

```text
Alert details
Timeline
Evidence
Classification
```

Output:

```text
Structured investigation report
```

### Indicator Extraction

Extract:

- IP addresses
- Domains
- URLs
- Hashes

from alert data.

## Important Principle

Automation should support the analyst.

It should not blindly make high-impact decisions without appropriate validation.

---

# 21. GitHub Repository Structure

The repository should grow with the lab.

Initial structure:

```text
soc-simulation-lab/
│
├── README.md
│
├── architecture/
│   ├── network-diagram.png
│   └── architecture.md
│
├── modules/
│   ├── 00-infrastructure/
│   ├── 01-splunk/
│   ├── 02-windows-telemetry/
│   ├── 03-log-collection/
│   ├── 04-detection-engineering/
│   ├── 05-attack-simulation/
│   ├── 06-investigation/
│   ├── 07-incident-response/
│   ├── 08-network-monitoring/
│   ├── 09-active-directory/
│   ├── 10-advanced-detection/
│   ├── 11-threat-intelligence/
│   └── 12-automation/
│
├── detections/
│
├── attack-scenarios/
│
├── investigations/
│
├── incident-reports/
│
├── scripts/
│
├── screenshots/
│
└── documentation/
```

The repository should not contain every directory before the work exists. Folders should be added as each phase is implemented.

---

# 22. Evidence Collection

The project should maintain evidence for important scenarios.

Examples:

- Splunk search screenshots
- Alert screenshots
- Event details
- Timeline screenshots
- Network monitoring output
- Detection test results
- Investigation reports
- Architecture diagrams
- Detection tuning notes

Evidence should be sanitized before publication.

Do not publish:

- Personal IP addresses
- Real credentials
- API keys
- Tokens
- Passwords
- Private host information
- Sensitive personal information

---

# 23. Documentation Standards

Each major scenario should answer:

### What happened?

Describe the event.

### How was it detected?

Identify the telemetry and detection.

### What evidence was collected?

Reference relevant events and searches.

### What did the analyst determine?

Explain the reasoning.

### Was it a true or false positive?

State the classification and supporting evidence.

### What should happen next?

Describe containment or other recommended response.

### What was learned?

Record improvements to the detection or investigation process.

---

# 24. MITRE ATT&CK Mapping

MITRE ATT&CK mapping should be used where it adds value.

Example structure:

```text
Technique
Tactic
Technique ID
Evidence
Detection
Investigation Notes
```

Do not map techniques simply to make the project look more advanced.

A technique should be mapped because the simulated behavior actually represents that technique.

---

# 25. Detection Quality

Detection quality should be evaluated using:

```text
Detection
   │
   ├── Does it trigger?
   │
   ├── Does it trigger on the intended behavior?
   │
   ├── Does it produce excessive false positives?
   │
   ├── Is enough context available?
   │
   └── Can an analyst investigate it?
```

A detection that triggers constantly without useful context is not considered successful.

---

# 26. Investigation Quality

A strong investigation should be:

- Evidence-based
- Reproducible
- Timeline-driven
- Explicit about uncertainty
- Clear about assumptions
- Able to distinguish normal from suspicious behavior
- Focused on the affected asset and user
- Supported by relevant telemetry

The lab should avoid conclusions such as:

> "This is malicious because the alert says so."

Instead:

> "The activity was classified as a true positive because the authentication attempts originated from the simulated attacker host, occurred repeatedly within a short time window, targeted multiple accounts, and were followed by successful authentication activity."

The exact conclusion will depend on the evidence generated by the lab.

---

# 27. Milestones

## Milestone 1: Working SIEM

```text
Splunk installed
        ↓
Basic searches working
```

## Milestone 2: Working Endpoint Telemetry

```text
Windows
   ↓
Security Logs + Sysmon
   ↓
Splunk
```

## Milestone 3: Working Detection

```text
Security Event
   ↓
Splunk SPL
   ↓
Alert
```

## Milestone 4: Working Attack Simulation

```text
Kali
   ↓
Controlled Activity
   ↓
Windows
   ↓
Splunk
   ↓
Alert
```

## Milestone 5: Working Investigation

```text
Alert
   ↓
Evidence
   ↓
Timeline
   ↓
Classification
   ↓
Investigation Report
```

## Milestone 6: Portfolio-Ready SOC

At this point the project should contain:

- Working Splunk environment
- Windows telemetry
- Sysmon
- Kali attack scenarios
- Multiple detections
- Multiple investigations
- True-positive and false-positive examples
- MITRE ATT&CK mappings
- Screenshots
- Architecture diagram
- GitHub documentation

---

# 28. Resume Positioning

The project should be described based on what has actually been completed.

A potential future project description, once the relevant phases are genuinely completed:

> **Modular SOC Simulation Lab**  
> Built a virtualized SOC environment using Splunk, Windows, Sysmon, and Kali Linux to collect endpoint telemetry, develop SPL-based detections, simulate controlled security activity, and investigate alerts using timeline analysis, evidence correlation, true/false-positive classification, and MITRE ATT&CK mapping.

Additional bullets should only be added after those capabilities are implemented and tested.

Examples of evidence-based additions could include:

- Developed and tested custom SPL detections for authentication, process, PowerShell, and persistence-related activity.
- Investigated controlled attack scenarios using Windows and Sysmon telemetry and documented findings in structured incident reports.
- Added network monitoring and correlated network events with endpoint telemetry.
- Built an Active Directory test environment for authentication, privilege, and lateral-movement investigations.
- Developed Python utilities for alert enrichment and investigation workflow automation.

These should not be claimed until demonstrated by the lab.

---

# 29. Portfolio Strategy

The GitHub repository should demonstrate three layers.

## Layer 1: Infrastructure

Shows that the environment was actually built.

Examples:

- Architecture
- VMware configuration
- Network design
- Installation notes

## Layer 2: Security Operations

Shows actual SOC ability.

Examples:

- Detections
- SPL
- Attack scenarios
- Investigations
- Incident reports
- False-positive analysis

## Layer 3: Engineering

Shows progression beyond basic alert monitoring.

Examples:

- Detection engineering
- Network monitoring
- Threat intelligence
- Python automation
- Detection tuning

The strongest portfolio value will come from Layers 2 and 3, not from screenshots showing that software was installed.

---

# 30. Recommended Initial Scope

Because the host has 24 GB RAM, the initial implementation should stop at:

```text
Phase 0
   ↓
Phase 1
   ↓
Phase 2
   ↓
Phase 3
   ↓
Phase 4
   ↓
Phase 5
   ↓
Phase 6
```

Initial VMs:

```text
┌───────────────────────────────┐
│ VMware Workstation             │
│                               │
│  ┌────────────┐               │
│  │ Splunk     │  8 GB RAM     │
│  │ Server     │               │
│  └────────────┘               │
│                               │
│  ┌────────────┐               │
│  │ Windows    │  6 GB RAM     │
│  │ Endpoint   │               │
│  └────────────┘               │
│                               │
│  ┌────────────┐               │
│  │ Kali       │  2-3 GB RAM   │
│  │ Attacker   │               │
│  └────────────┘               │
│                               │
└───────────────────────────────┘
```

This is sufficient to demonstrate a complete basic SOC workflow.

---

# 31. Expansion Strategy

After the initial SOC works:

```text
Basic SOC
   │
   ├── Better endpoint telemetry
   │
   ├── More detections
   │
   ├── More attack scenarios
   │
   ├── Better investigations
   │
   ├── Network visibility
   │
   ├── Active Directory
   │
   ├── Threat intelligence
   │
   └── Automation
```

The lab should grow in **capability**, not simply in the number of virtual machines.

---

# 32. Final Architecture Target

The eventual environment may look like:

```text
                           VMware SOC Lab
                                  │
                 ┌────────────────┼────────────────┐
                 │                │                │
                 ▼                ▼                ▼
             Splunk          Windows Hosts       Kali
              SIEM                │             Attacker
                 │                │
                 │        ┌───────┴────────┐
                 │        │                │
                 │      Sysmon         Windows Logs
                 │        │                │
                 │        └───────┬────────┘
                 │                │
                 └───────────────┬┘
                                 │
                         Security Telemetry
                                 │
                 ┌───────────────┴──────────────┐
                 │                              │
                 ▼                              ▼
          Network Monitoring               Active Directory
          Suricata / Zeek                 Domain Controller
                 │                              │
                 └───────────────┬──────────────┘
                                 │
                                 ▼
                              Splunk
                                 │
                  ┌──────────────┼──────────────┐
                  │              │              │
                  ▼              ▼              ▼
              Detection    Investigation   Threat Intel
                  │              │              │
                  └──────────────┼──────────────┘
                                 ▼
                          Incident Response
                                 │
                                 ▼
                             Automation
                                 │
                                 ▼
                               Python
```

---

# 33. Definition of Success

The project is successful when the lab can demonstrate the complete cycle:

```text
1. Generate controlled activity
            ↓
2. Produce telemetry
            ↓
3. Ingest telemetry into Splunk
            ↓
4. Detect suspicious behavior
            ↓
5. Generate an alert
            ↓
6. Triage the alert
            ↓
7. Investigate supporting evidence
            ↓
8. Build a timeline
            ↓
9. Classify the alert
            ↓
10. Map relevant ATT&CK behavior
            ↓
11. Recommend or perform appropriate response
            ↓
12. Document the investigation
            ↓
13. Improve the detection
```

The final portfolio should show this workflow through reproducible lab scenarios rather than simply listing technologies.

---

# 34. Current Project Status

## Planned

- [x] Modular project architecture
- [x] Splunk selected as the SIEM
- [x] VMware selected as the hypervisor
- [x] Initial three-VM architecture defined
- [x] Phase roadmap defined

## Not Yet Implemented

- [ ] Phase 0: Lab Foundation
- [ ] Phase 1: Splunk SIEM
- [ ] Phase 2: Windows Endpoint Telemetry
- [ ] Phase 3: Log Collection
- [ ] Phase 4: Detection Engineering
- [ ] Phase 5: Attack Simulation
- [ ] Phase 6: SOC Investigation
- [ ] Phase 7: Incident Response
- [ ] Phase 8: Network Security Monitoring
- [ ] Phase 9: Active Directory
- [ ] Phase 10: Advanced Detection Engineering
- [ ] Phase 11: Threat Intelligence
- [ ] Phase 12: SOC Automation

---

# 35. Immediate Next Step

The next task is **Phase 0: Lab Foundation**.

Before installing Splunk or the other tools, finalize:

1. VMware network topology.
2. VM names.
3. IP addressing scheme.
4. CPU allocation.
5. RAM allocation.
6. Disk allocation.
7. VMware network adapters.
8. Internet access requirements.
9. Snapshot strategy.
10. Isolation and safety controls.

Only after Phase 0 is verified should the project move to Phase 1.

---

# Project Principle

> **Build small. Generate evidence. Investigate deeply. Document accurately. Expand only when the current capability works.**

The objective is to build a SOC portfolio project that demonstrates actual hands-on security operations rather than a collection of installed security tools.
