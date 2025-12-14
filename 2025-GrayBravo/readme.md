**GrayBravo Campaign Analysis**

**Overview**

This report provides an analysis of GrayBravo (formerly TAG-150), a malware-as-a-service (MaaS) operator active since March 2025. The threat actor has developed malware families including CastleLoader, CastleRAT, and tools with enable multiple cybercriminal campaigns across diverse sectors.

**Key Highlights**

- Threat Level: High
- Active Since: March 2025
- Primary Malware: CastleLoader, CastleRAT, Matanbuchus
- Target Sectors: Hospitality, General Enterprise, Logistics
- Geographic Focus: Global (US concentration)Add campaign analysis for GrayBravo

**Malware Families**

- Castle Loader - Initial access loader
- CastleRAT - Remote access trojan
- CastleBOT - Early iteration bot
- Matanbuchus - Third party loader MaaS

**Activity Clusters**

- TAG-160 - Logistics sector targeting
- TAG-161 - Booking.com impersonation with custom phishing infrastructure
- Cluster 3 - Steam Community dead drop resolvers
- Cluster 4 - Malvertising with EV-signed installers

**MITRE ATT&CK Coverage**

- Initial Access: T1566.002 (Spearphishing Link)
- Execution: T1059.001 (PowerShell), T1204.002 (User Execution)
- Persistence: T1547.001 (Registry Key Runs)
- Defense Evasion: T1070.004 (File Deletion), T1574.002 (DLL Side-Loading)
- Credential Access: T1056.001 (Keylogging), T1555.003 (Browser Credentials)
- Discovery: T1082 (System Information Discovery)
- Collection: T1115 (Clipboard Data)
- Command and Control: T1071.001 (Web Protocols), T1102.001 (Dead Drop Resolvers)

**References**

- https://www.recordedfuture.com/research/graybravos-castleloader-activity-clusters-target-multiple-industries
- https://www.ibm.com/think/x-force/dissecting-castlebot-maas-operation
- https://catalyst.prodaft.com/public/report/understanding-current-castleloader-campaigns/overview#heading-1000|0
- https://www.morphisec.com/blog/ransomware-threat-matanbuchus-3-0-maas-levels-up/
