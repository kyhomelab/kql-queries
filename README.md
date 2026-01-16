# KQL Queries for Microsoft Sentinel & Defender

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![KQL](https://img.shields.io/badge/Language-KQL-blue.svg)](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/)
[![Microsoft Sentinel](https://img.shields.io/badge/Platform-Microsoft%20Sentinel-0078D4.svg)](https://azure.microsoft.com/en-us/services/azure-sentinel/)

A curated collection of production-ready Kusto Query Language (KQL) queries for security operations, threat hunting, and compliance monitoring in Microsoft Sentinel, Microsoft 365 Defender, and Azure Log Analytics environments.

## Table of Contents

- [Overview](#overview)
- [Query Categories](#query-categories)
  - [Security](#security)
  - [Audit](#audit)
  - [Identity](#identity)
  - [Performance](#performance)
- [Getting Started](#getting-started)
- [Query Sanitization Tool](#query-sanitization-tool)
- [Use Cases](#use-cases)
- [Contributing](#contributing)
- [Author](#author)
- [License](#license)

## Overview

This repository contains KQL queries developed for real-world SOC operations, incident response, and security monitoring. Each query is designed to address specific security challenges and includes:

- Clear documentation and use case descriptions
- Performance-optimized query logic
- Configurable parameters for customization
- Comments explaining query behavior

These queries are ready to deploy in:
- **Microsoft Sentinel** - Cloud-native SIEM platform
- **Microsoft 365 Defender** - Advanced threat protection
- **Azure Log Analytics** - Centralized log management
- **Azure Data Explorer** - Big data analytics

## Query Categories

### Security

Advanced threat detection and vulnerability management queries for proactive security monitoring.

| Query | Description | Use Case |
|-------|-------------|----------|
| **Shared_Device_Identification_V1.kql** | Identifies devices with multiple unique users (excludes whitelisted accounts and device types) | Detect unauthorized device sharing, insider threats, compliance violations |
| **Shared_Device_Identification_V2.kql** | Enhanced version with direct summarization from DeviceLogonEvents | Improved performance for large environments |
| **Critical_Software_Vulnerabilities_V1.kql** | Lists devices with critical CVEs and counts vulnerabilities per device | Prioritize patch management, vulnerability remediation |
| **Critical_Software_Vulnerabilities_V2.kql** | Enhanced vulnerability tracking with additional filtering | Advanced vulnerability management workflows |
| **Suspicious_PowerShell_Execution.kql** | Detects potentially malicious PowerShell commands using encoded commands, web downloads, or obfuscation | Identify fileless malware, script-based attacks, and malicious automation |
| **Impossible_Travel_Detection.kql** | Detects sign-ins from geographically distant locations within a short time window using Haversine distance calculation | Detect credential theft, account compromise, session hijacking |

### Audit

Compliance monitoring and change tracking queries for governance and regulatory requirements.

| Query | Description | Use Case |
|-------|-------------|----------|
| **Conditional_Access_Policy_Changes.kql** | Monitors creation, modification, and deletion of Conditional Access policies | Track security policy drift, compliance auditing, change management |
| **Admin_Role_Assignment_Changes.kql** | Tracks administrative role assignments and privilege escalations | Detect unauthorized privilege escalation, insider threat monitoring |
| **OAuth_Application_Consent_Grants.kql** | Tracks when users or admins grant permissions to OAuth applications | Detect illicit consent grant attacks, monitor third-party app access |

### Identity

Authentication and identity security queries for detecting account compromise and access anomalies.

| Query | Description | Use Case |
|-------|-------------|----------|
| **Failed_Signins_By_User.kql** | Identifies users with multiple failed sign-in attempts in 24 hours | Detect brute-force attacks, credential stuffing, user authentication issues |
| **Account_Lockout_Monitoring.kql** | Tracks user account lockouts over 24 hours to identify attack patterns or support issues | Detect brute-force attacks, credential stuffing, identify users requiring support |

### Performance

Monitoring queries for identifying performance bottlenecks and optimizing user experience.

| Query | Description | Use Case |
|-------|-------------|----------|
| **Signin_Latency_By_Application.kql** | Analyzes sign-in latency patterns by application | Identify authentication performance issues, optimize user experience |

## Getting Started

### Prerequisites

- Access to Microsoft Sentinel, Microsoft 365 Defender, or Azure Log Analytics workspace
- Appropriate permissions to run queries against your log data
- Basic understanding of KQL syntax

### Usage

1. **Clone the repository**
   ```bash
   git clone https://github.com/kyhomelab/kql-queries.git
   cd kql-queries
   ```

2. **Navigate to the relevant category**
   ```bash
   cd Security  # or Audit, Identity, Performance
   ```

3. **Copy the query content** and paste it into:
   - Microsoft Sentinel > Logs
   - Microsoft 365 Defender > Advanced Hunting
   - Azure Log Analytics > Logs

4. **Customize parameters** (if applicable)
   - Update whitelisted accounts
   - Adjust time ranges
   - Modify device name filters
   - Configure thresholds

5. **Run the query** and analyze results

### Example: Detecting Failed Sign-ins

```kql
SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType != 0  // Non-zero means failure
| summarize
    FailedAttempts = count(),
    FailureCodes = make_set(ResultType),
    Locations = make_set(Location),
    Applications = make_set(AppDisplayName)
    by UserPrincipalName
| where FailedAttempts > 5
| order by FailedAttempts desc
```

## Query Sanitization Tool

A Python script (`sanitize_kql.py`) is included to help strip sensitive information from queries before sharing.

### Features

- Removes IP addresses
- Redacts email addresses
- Sanitizes GUIDs and sensitive identifiers
- Preserves query logic and structure

### Usage

**Sanitize a directory of queries:**
```bash
python3 sanitize_kql.py --input unsanitized/ --output sanitized/
```

**Sanitize a single file:**
```bash
python3 sanitize_kql.py --file my_query.kql
```

## Use Cases

### For SOC Analysts
- Investigate security incidents
- Hunt for threats proactively
- Monitor authentication anomalies
- Track compliance violations

### For Security Engineers
- Build detection rules and alerts
- Optimize SIEM performance
- Create custom dashboards
- Develop automated response workflows

### For Compliance Teams
- Audit administrative changes
- Track policy modifications
- Monitor privileged access
- Generate compliance reports

## Contributing

Contributions are welcome! If you have queries to add or improvements to suggest:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-query`)
3. Commit your changes (`git commit -m 'Add new threat hunting query'`)
4. Push to the branch (`git push origin feature/new-query`)
5. Open a Pull Request

Please ensure queries include:
- Descriptive title and category comments
- Clear use case description
- Performance considerations
- Example output or expected results

## Author

**Kyle** - SOC Analyst | Cybersecurity Professional

- Portfolio: [kyhomelab.github.io](https://kyhomelab.github.io)
- GitHub: [@kyhomelab](https://github.com/kyhomelab)

Passionate about threat detection, security automation, and building effective SOC workflows. These queries represent real-world solutions developed for enterprise security operations.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Disclaimer:** These queries are provided as-is for educational and professional use. Always test queries in a non-production environment first and customize them to fit your specific security requirements and organizational policies.

**Keywords:** KQL, Kusto Query Language, Microsoft Sentinel, Azure Sentinel, Microsoft 365 Defender, Threat Hunting, SOC, SIEM, Security Operations, Incident Response, Threat Detection, Cybersecurity, Log Analytics
