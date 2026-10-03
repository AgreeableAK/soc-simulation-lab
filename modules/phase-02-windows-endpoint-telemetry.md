# Phase 2: Windows Endpoint Telemetry

## 1. Objective

The objective of Phase 2 was to turn the Windows endpoint `SOC-WIN-01` into a useful SOC telemetry source.

The endpoint was prepared to generate security-relevant Windows telemetry that can later be collected by Splunk and used for detection engineering and SOC investigations.

The telemetry model for the endpoint is:

```text
                    SOC-WIN-01
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
   Windows Logs     PowerShell         Sysmon
        │               │                │
        └───────────────┼────────────────┘
                        │
                        ▼
              Endpoint Telemetry
```


## 2. Lab Endpoint

| Attribute          | Value                    |
| ------------------ | ------------------------ |
| Hostname           | `SOC-WIN-01`             |
| Operating System   | Windows                  |
| Role               | SOC monitoring endpoint  |
| Network            | Isolated SOC lab network |
| Splunk destination | `SOC-SPLUNK-01`          |
| Splunk index       | `soc_security`           |

The endpoint is intentionally isolated from the normal network as part of the lab architecture.

## 3. Windows Telemetry

The endpoint was configured to provide security-relevant Windows event telemetry.

The main Windows event sources considered during Phase 2 were:

* Windows Security
* Windows System
* Windows Application
* PowerShell
* Sysmon

These sources provide visibility into authentication, process activity, system changes, scripting activity, and endpoint behavior.

### 3.1 Windows Security Logs

Security events provide visibility into authentication and security-related activity.

Important examples include:

* Successful logons
* Failed logons
* Account activity
* Privilege-related activity
* Authentication failures
* Security policy activity

SOC relevance:

```text
Windows Security Event
        ↓
Authentication / Account Activity
        ↓
User or Host Investigation
        ↓
Potential Detection
```

Security telemetry is particularly important for investigating account compromise, suspicious authentication behavior, and privilege-related activity.

### 3.2 Windows System Logs

System events provide visibility into operating system and service activity.

These events can help an analyst investigate:

* System changes
* Service activity
* Startup and shutdown activity
* Operating system issues
* Driver and infrastructure-related events

System telemetry provides additional context when investigating endpoint behavior.

### 3.3 Windows Application Logs

Application events provide visibility into software and application-level activity.

These events can be useful when investigating:

* Application failures
* Service/application behavior
* Unexpected application activity
* Errors associated with suspicious activity

### 3.4 PowerShell Telemetry

PowerShell telemetry is important for monitoring script-based activity.

Relevant activity may include:

* PowerShell execution
* Script execution
* Administrative commands
* Suspicious scripting behavior
* Command execution associated with endpoint attacks

SOC relevance:

```text
PowerShell Activity
        ↓
Script / Command Execution
        ↓
Determine Intent
        ↓
Correlate With Other Events
        ↓
Detection / Investigation
```

PowerShell events should be interpreted in context rather than treated as inherently malicious.

## 4. Sysmon Telemetry

Sysmon is intended to provide additional endpoint visibility beyond the standard Windows event logs.

Relevant Sysmon telemetry includes:

| Telemetry             | SOC relevance                                      |
| --------------------- | -------------------------------------------------- |
| Process creation      | Investigating process execution                    |
| Network connections   | Correlating processes with network activity        |
| File creation         | Investigating file-based activity                  |
| Registry activity     | Investigating persistence or configuration changes |
| DNS activity          | Investigating endpoint network behavior            |
| Process relationships | Understanding parent/child execution chains        |

A simplified investigation model is:

```text
Process Creation
      │
      ├── Parent Process
      ├── Child Process
      ├── Command Line
      └── User Context
              │
              ▼
        Analyst Investigation
              │
              ▼
       Detection Opportunity
```

Sysmon is not treated as a standalone detection mechanism.

Its value comes from providing additional context that can be correlated with Windows Security, System, Application, PowerShell, and later network telemetry.

## 5. Endpoint Telemetry Model

The completed endpoint telemetry architecture is:

```text
                     SOC-WIN-01
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
 Windows Event Logs   PowerShell       Sysmon
        │                │                │
        └────────────────┼────────────────┘
                         │
                         ▼
                 Endpoint Visibility
                         │
                         ▼
              Splunk Collection Layer
                         │
                         ▼
                  SOC Investigation
```

Phase 2 focuses on producing and understanding the endpoint telemetry.

Phase 3 builds the collection pipeline that transports this telemetry into Splunk.

## 6. Validation

The endpoint was validated as a functioning telemetry source.

The Windows endpoint successfully generated normal Windows activity that could subsequently be collected and searched from Splunk.

The following Windows event sources were subsequently observed in the Splunk `soc_security` index:

```text
XmlWinEventLog:Security
XmlWinEventLog:Application
XmlWinEventLog:System
```

Example validation query:

```spl
index=soc_security earliest=-15m
| stats count by host sourcetype source
| sort - count
```

Observed endpoint:

```text
host       sourcetype
SOC-WIN-01 XmlWinEventLog:Security
SOC-WIN-01 XmlWinEventLog:Application
SOC-WIN-01 XmlWinEventLog:System
```

This confirms that the Windows endpoint is producing telemetry that can be consumed by the SOC monitoring stack.

## 7. SOC Relevance

Each telemetry source serves a different investigative purpose.

| Telemetry              | Primary use                                |
| ---------------------- | ------------------------------------------ |
| Windows Security       | Authentication and account investigation   |
| Windows System         | System and service investigation           |
| Windows Application    | Application behavior and errors            |
| PowerShell             | Script and command execution investigation |
| Sysmon Process Events  | Process execution investigation            |
| Sysmon Network Events  | Process/network correlation                |
| Sysmon File Events     | File activity investigation                |
| Sysmon Registry Events | Registry activity investigation            |
| Sysmon DNS Events      | Endpoint DNS investigation                 |

## 8. Phase 2 Completion Status

Phase 2 established the Windows endpoint telemetry foundation required by the project.

### Completed

* [x] Windows endpoint prepared for SOC monitoring
* [x] Windows Security telemetry available
* [x] Windows System telemetry available
* [x] Windows Application telemetry available
* [x] PowerShell telemetry considered as an endpoint telemetry source
* [x] Sysmon incorporated into the telemetry design
* [x] Normal Windows activity visible
* [x] Endpoint telemetry understood in terms of SOC relevance
* [x] Endpoint successfully producing telemetry for downstream collection
