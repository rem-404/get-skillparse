# Get-SkillParse
*This script is for a lab environment and meant for learning purposes only*
> **This script is tide to other metrics in job hunting script**
  - get-emotionaldamage (the main script that aggregates the data)
  - invoke-ghostdetector (the script that changes the status of "uncertain" to "ghosted" after 3 days)
  - CsvOfDisappointment.csv (the main data that holds the object, this file should be manually created by the user)
  - get-skillparse (this script... a helper script for getting the skills on a job post and readily available from the clipboard to be added to the csv)

## What does it do
- Parses a job description text file against a curated pool of known IT skills and copies the matches directly to your clipboard — ready to paste into the Skills column of the CSV of Disappointment. No manual typing, no missed keywords, no copy-paste fatigue.
- Uses regex word boundaries (`\b`) to ensure accurate matching — so `'it'` doesn't accidentally match inside `'monitoring'`, and `'vpn'` doesn't match inside `'development'`.

## What does it solve
- Manually reading a job listing and typing every skill into a CSV is tedious, inconsistent, and eats into gaming time. This automates the extraction so you can focus on spamming the apply button instead.
- When paired with `Get-EmotionalDamageAnalytics`, the captured skills feed the Skill Leaderboard — turning your own application data into a personal market intelligence report on what skills are actually in demand.

## Who's it for
Anyone tracking a job hunt campaign who is too lazy to type skills manually but not too lazy to build a script that does it instead.

## Requirements

- PowerShell 5.1+
- A job description saved as a plain text file (default: `C:\Logs\text\jobdesc.txt`)
- No additional modules needed

## Usage

```powershell
# Default - parses jobdesc.txt
Get-SkillParse

# Custom file path
Get-SkillParse -FilePath "C:\Logs\text\otherjob.txt"

# Full workflow
# 1. Copy job description → paste into jobdesc.txt
# 2. Run Get-SkillParse
# 3. Switch to Excel → Ctrl+V into Skills column
# 4. Done
```

## Skill Pool Coverage
The built-in skill pool covers:
- Core Systems & OS — Windows Server, Active Directory, Linux, PowerShell, DNS, DHCP
- Virtualization & Infrastructure — VMware, Hyper-V, Proxmox, Veeam, SAN/NAS
- Networking — TCP/IP, VLANs, Routing, Cisco, Firewall, VPN, CCNA
- Microsoft 365 & Cloud Identity — Intune, Entra ID, Exchange, MFA, SharePoint
- Cloud Platforms — Azure, AWS, GCP, IaaS/PaaS/SaaS
- Security & Compliance — ITIL, ITSM, RBAC, PKI, ISO 27001
- Automation & Scripting — Ansible, Terraform, REST API, DevOps
- Remote & Endpoint — RDP, MDT, SCCM, SSH
- General IT & Support — L1/L2/L3, Helpdesk, Monitoring, SOP

## Warning

- Skill pool uses regex word boundary matching — multi-word skills like `'active directory'` match correctly but the job description must contain the exact phrase
- The skill pool is opinionated and IT-focused — marketing, finance, or other domain skills won't be detected
- Output goes directly to clipboard — if clipboard is in use, it will be overwritten without warning
- Does not deduplicate if the same skill appears multiple times in the job listing

## Limitations
- Only reads from a single text file per run — no bulk processing of multiple job descriptions
- Skill pool requires manual updates as new technologies emerge
- No fuzzy matching — `'powershell scripting'` won't match `'powershell'` in the pool
- `END {}` block is empty — placeholder for future use

## Notes
Peak laziness driven development — built specifically to avoid typing skills manually into a CSV. Pairs with `Get-EmotionalDamageAnalytics` and `Invoke-GhostDetector` to form the complete CSV of Disappointment ecosystem. The more job descriptions parsed, the more accurate the Skill Leaderboard becomes. 

## Sample Output
```powershell
PS C:\Logs> Get-SkillParse
[+] Skills Captured and Copied to Clipboard:
    active directory, group policy, dns, dhcp, disaster recovery, backup, networking, remote access, wireless, microsoft 365, exchange, exchange online, mfa, sharepoint, onedrive, teams, it support, technical support, infrastructure, troubleshooting, documentation
PS C:\Logs>
```

## Combined sample output of three scripts at work
```powershell
PS C:\Logs> import-csv .\CsvOfDisappointment.csv | Get-EmotionalDamageAnalytics

CampaignStartDate : 2026-06-03
DaysSinceStart    : 5
TotalApplications : 21
Hired             : 0
Denied            : 0
Responded         : 2
Uncertain         : 14
Ghosted           : 5

WARNING: high uncertainty detected: 66.67%
Ghosting rate : 23.81%
Uncertain rate: 66.67%

--- MARKET ANALYSIS ON SKILLSETS ---

Skill             Count
-----             -----
teams                 6
compliance            6
troubleshooting       5
technical support     5
security              5
networking            4
documentation         4
infrastructure        4
it operations         4
it support            4
microsoft 365         3
active directory      3
automation            3
data analist          3
exchange online       2
```
