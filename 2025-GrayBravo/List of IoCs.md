# Indicators of Compromise (IoCs)

**Last Updated:** December 21, 2025  

## IoC Summary Statistics

| Type | Count | Primary Use |
|------|-------|-------------|
| Domains | 200+ | Phishing, C2 infrastructure |
| IP Addresses | 100+ | C2 servers, hosting |
| File Hashes (SHA256) | 50+ | Malware samples |
| Email Addresses | 20+ | Phishing senders |
| URLs | 100+ | Phishing links, payload delivery |
| ASNs | 5 | Malicious hosting networks |
| Steam Profiles | 5 | Dead drop resolvers |
| RC4 Keys | 3+ | CastleRAT encryption |

## Critical Tier 1 Block List (Immediate Action)

These indicators represent active C2 infrastructure and should be blocked immediately at network perimeter.

### CastleLoader C2 Infrastructure

**Domains - BLOCK:**
```
wereatwar.com
oldspicenotsogood.shop
castlppwnd.com
donttouchme.life
icantseeyou.icu
anotherproject.icu
touchmeplease.icu
donttouchthisisuseless.icu
doyoureallyseeme.icu
rcpeformse.com
roject0.com
bethschwier.com
speatly.com
campanyasoft.com
alafair.net
dpeformse.com
```

**IP Addresses - BLOCK:**
```
31.58.50.160
31.58.87.132
45.11.183.19
45.11.183.45
45.11.183.165
45.155.249.121
80.77.25.88
80.77.25.114
80.77.25.239
107.158.128.26
147.45.177.127
170.130.165.201
172.86.90.58
173.44.141.52
185.121.234.141
```

### CastleRAT C2 Infrastructure

**Domains - BLOCK:**
```
programsbookss.com
justnewdmain.com
autryjones.com
gabesworld.com
miteamss.com
kakapupuneww.com
treetankists.com
tdbfvgwe456yt.com
```

**IP Addresses - BLOCK:**
```
5.35.44.176
45.11.180.174
45.11.181.59
45.134.26.41
104.225.129.171
144.208.126.50
185.196.9.80
185.208.158.250
192.109.138.102
195.85.115.44
```

## Tier 2: Phishing Infrastructure

### TAG-160 Logistics-Themed Domains

**Domain Block List:**
```
loadsschedule.com
loadstracking.com
loadstrucking.com
rateconfirmations.com
cdlfreightlogistics.com
dperforms.info
englandloglstics.com
englanglogistlcs.com
hometownlogisticsllc.com
leemanlogisticsinc.com
loadplannig.com
loads.icu
loadsplanning.com
tenderloads.com
trucksscheduling.com
```

**IP Addresses:**
```
74.119.239.234
162.215.230.96
162.215.230.150
162.251.80.108
185.236.20.154
199.79.62.141
207.174.212.141
```

### TAG-161 Booking.com Phishing Domains

**Domain Block List:**
```
checkinstayverify.com
guestformsafe.com
confirmstayon.com
guestverifyportal.com
guestverifyhub.com
confirmhotelstay.com
guestaformahub.com
guestaportalverify.com
verifyhubguest.com
servicehotelonline.com
```

**IP Addresses:**
```
77.83.207.55
185.39.19.180
185.39.19.181
```

### Cluster 3 Booking.com Domains

**Domain Block List:**
```
boiksal.com
update-info4468765.com
guest-request64533.com
bookingnewprice109034.icu
```

**IP Addresses:**
```
178.17.57.102
178.17.57.103
192.109.138.102
```

## Malicious ASNs

Monitor all traffic to/from these autonomous systems:

```
AS211659 - STIMUL-AS, Russia (BEARHOST-affiliated)
AS216341 - OPTIMA-AS, Russia (BEARHOST-affiliated)
AS215540 - Global Connectivity Solutions
```

## Steam Community Dead Drop Profiles

**URLs to Monitor/Block:**
```
https://steamcommunity.com/id/tfy5d6gohu8tgy687r7
https://steamcommunity.com/id/desdsfds34324y3g
https://steamcommunity.com/id/fio34h8dsh3iufs
https://steamcommunity.com/id/jeg238r7staf378s
https://steamcommunity.com/id/krouvhsin34287f7h3
```

## File Hashes

### CastleLoader Samples (SHA256)

```
1b6befc65b19a63b4131ce5bcc6e8c0552fe1e1d136ab94bc7d81b3924056156
202f6b6631ade2c41e4762e5877ce0063a3beabce0c3f8564b6499a1164c1e04
25e0008aba82690e0f58c9d9fcfbc5d49820aa78d2f7bfcd0b85fb969180fc04
fb9de7448e9e30f717c171f1d1c90ac72828803a16ad385757aeecc853479d3c
```

### CastleRAT Python Variant (SHA256)

```
94dc0f696a46f3c225b0aa741fbd3b8997a92126d66d7bc7c9dd8097af0de52a
53775af67e9df206ed3f9c0a3756dbbc4968a77b1df164e9baddb51e61ac82df
```

### CastleRAT C Variant (SHA256)

```
1ff6ee23b4cd9ac90ee569067b9e649c76dafac234761706724ae0c1943e4a75
e6bcdf375649a7cbf092fcab65a24d832d8725d833e422e28dfa634498b00928
67cf6d5332078ff021865d5fef6dc61e90b89bc411d8344754247ccd194ff65b
```

## Network Indicators

### CastleLoader HTTP Pattern

```http
GET /service/settings/[hash] HTTP/1.1
Host: [c2_domain]
Cache-Control: no-cache
Connection: Keep-Alive
Pragma: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
[NO ACCEPT HEADER]
```

### CastleRAT Geolocation Queries

```
GET /line/?fields=16385 HTTP/1.1 (Python variant)
GET /line/?fields=147457 HTTP/1.1 (C variant)
Host: www.ip-api.com
```

### Matanbuchus C2 Pattern

```http
POST /gate.php HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Body: =eyJhY3Rpb24i... (base64, no & separators)
```

## File System Indicators

### CastleLoader Staging Paths

```
%TEMP%\update.zip
%TEMP%\pythonw.exe
%TEMP%\loader.pyw
%TEMP%\castle.pyw
```

### CastleRAT Log Files

```
%TEMP%\MuuuuuhGer3\keylog.txt
%TEMP%\MuuuuuhGer3\clipboardlog.txt
%TEMP%\PluhhSuk3\keylog.txt
%LOCALAPPDATA%\JohniiDepp\keylog.txt
%LOCALAPPDATA%\LuchiiSvet\keylog.txt
```

## Email Indicators

### Phishing Sender Patterns (TAG-160)

```
From: [firstname]@[logistics-domain].com
Pattern: First name only (david, paul, maritza)
Domains: cdlfreightlogistics.com, mrlogsol.ca
```

### Subject Line Patterns

**TAG-160 (Logistics):**
```
Rate Confirmation - Load #[ID]
Freight Quote - Load [ID]
DPE Form Required - [Load ID]
```

**TAG-161 (Hospitality):**
```
Guest Verification Required - Booking #[ID]
Payment Confirmation Needed - Reservation [ID]
Action Required: Verify Your Stay
```

## Registry Indicators

### Persistence Keys

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run\[random_name]
Common names: WindowsUpdate, SecurityUpdate, SystemCheck
```

## RC4 Encryption Keys (CastleRAT)

These hardcoded strings identify CastleRAT infections:

```
NanuchkaUpyachka
VodkaBalabayka
GrazdanskayaOborona
```

## Complete IoC Lists

For complete IoC lists including all 200+ domains, 100+ IPs, and additional indicators, refer to the original Recorded Future research report.

## IoC Management Recommendations

**Update Frequency:**
- Critical Infrastructure (Tier 1): Daily updates
- Phishing Domains (Tier 2): Weekly updates  
- File Hashes: Continuous via threat feeds
- Network Patterns: Monthly review and tuning

**Distribution:**
- Import into SIEM, EDR, firewall platforms
- Maintain internal documentation

**Validation:**
- Test blocking effectiveness weekly
- Monitor for false positives
- Document exceptions and whitelisting needs
- Track detection rates vs. true positives
