**Overview**

GrayBravo represents a sophisticated and evolving threat actor operating a malware-as-a-service (MaaS) platform since March 2025. The threat actor has developed multiple malware families and supports at least four distinct activity clusters, each targeting different sectors with unique tactics and victim profiles.

**Key Findings**

- Custom Malware Development: CastleLoader, CastleBot, CastleRAT with C and Python variants
- Multi-Tiered Infrastructure: 4-tier C2 architecture with backup servers and Tor obfuscation
- Rapid Evolution: Quick response to public exposure, regular tool updates
- MaaS Operation: Multiple customers/affiliates using shared infrastructure

**Activity Clusters**

**TAG-160: Logistics Sector Operations**

- Target: US-based trucking companies, freight brokers, carriers
- Method: Email spoofing, domain re-registration, freight platform abuse
- Unique TTPs: Fraudulent DAT & Loadlink accounts, industry-specific social engineering
- Payloads: CastleLoader -> HijackLoader, Rhadamanthys, zgRAT

**TAG-161: Booking.com Impersonation**

- Target: Hospitality sector, broad victim base
- Method: Custom phishing email management infrastructure ("Booking-Mailer V2.2")
- Unique TTPs: Automated bulk phishing, BEARHOST bulletproof hosting
- Payloads: CastleLoader, Matanbuchus loader

**Cluster 3: Steam Community Dead Drops**

- Target: Hospitality-themed, general users
- Method: Booking.com impersonations, Steam profiles for C2 resolution
- Unique TTPs: Dynamic C2 via Steam Community "About Me" sections
- Payloads: CastleLoader -> CastleRAT

**Cluster 4: Malvertising Campaign**

- Target: IT administrators, general users
- Method: Fake software downloads (Zabbix, RVTools(
- Unique TTPs: EV-signed MSI installers, fake GitHub repositories
- Payloads: CastleLoader -> NetSupport RAT

**Impact Analysis**

**Immediate Consequences**

- Credential Compromise: Browser passwords, email accounts, business applications
- System Surveillance: Keylogging, clipboard monitoring, screen capture
- Data Exfiltration: Sensitive documents, customer information, intellectual property
- Network Pivot: Lateral movement capabiities for broader compromise

**Secondary Threats**

- Ransomware Deployment: Observed second-stage payloads include ransomware capabilities
- Business Email Compromise: Stolen credentials enable financial fraud
- Supply Chain Attack: Logistics sector targeting threatens cargo/shipment integrity
- Long-Term Persistence: RAT capabilities enale extended persistence

**Sector-Specific Risks**

**Logistics**

- Shipment hijacking and cargo theft
- Fraudulent load postings and payment diversion
- Supply chain distruption

**Hospitality**

- Guest data compromise (PII, payment information)
- Brand reputation damage
- Fraudulent booking schemes

**General Enterprise**

- Financial fraud and wire transfer redirection
- Intellectual property theft
- Regulatory compliance violations (GDPR, PCI-DSS)
- Operational distruption

**Strategic Assessment**

- Sophisticated infrastructure segmentation (4-tier architecture)
- Multiple malware variants for different operational needs
- Rapid iteration in response to security research
- MaaS business model indicating established underground presence

**Evolution Trajectory**

- Growing customer/affiliate base (4+ distinct clusters identified)
- Increasing technical capabilities 
- Infrastructure expansion
- Diversification of targets

**Underground Connections**

- Access to EV code signing certificates
- Integration with third-party MaaS (Matanbuchus loader)
- User of BEARHost hosting network

**Recommendations Priority**

**Critical**

- Block known IoCs
- Deploy detection rules - Implemet rules for CastleLoader/CastleRAT activity
- Email filtering - Block known phishing domains
- User Awareness - Train staff on social engineering techniques

**High**

- Endpoint Detection: Deploy monitoring for PowerShell, Python execution
- Network Monitoring: Alet on suspicious C2 patterns (/service/settings/,ip-api.com)
- Credential Review: Check for compromised credentials in breach databases

**Medium**

- Incident Response Planning: Update playbooks for CastleLoader scenarios
- PowerShell Hardening
- Application Whitelisting: Restrict execution from %TEMP% directories

**Conclusion** 

