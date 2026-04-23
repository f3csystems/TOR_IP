# TOR IP Blacklist

Automatically updated IP blacklist from TOR Usage alerts (Sekoia.io).
Contains destination IPs of TOR nodes observed in network traffic.

**Last updated:** 2026-04-23 09:22
**Total active IPs:** 331
**Retention policy:** 30 days — IPs not seen for 30+ days are automatically removed

## Rules monitored
- `TOR Usage`
- `TOR Usage Generic Rule`
- `TOR Usage Generic Rule - Wifi Guest/ Eduroam`

## Files
- `blacklist.csv` - Full blacklist with metadata (ip, first_seen, last_seen, alert_count, country)
- `blacklist.txt` - Plain text IP list (1 IP per line, for External Dynamic List / Threat Feed)

## Top 10 TOR IPs
| IP | Alerts | Country |
|----|--------|---------|
| 185.243.218.138 | 17 | NO |
| 31.6.1.164 | 15 | RO |
| 38.190.150.2 | 12 | RS |
| 185.116.236.48 | 12 | US |
| 66.92.214.63 | 11 | US |
| 50.118.225.16 | 11 | NL |
| 148.251.236.209 | 10 | DE |
| 45.41.207.167 | 10 | US |
| 198.55.103.184 | 10 | IE |
| 195.206.234.89 | 10 | NL |

## Firewall Integration — External Dynamic Lists / Threat Feeds

> **Important:** This blacklist **must** be consumed via External Dynamic Lists (EDL) or Threat Feeds.
> Do **not** import the IPs manually — only dynamic feeds ensure automatic updates
> and respect the 30-day retention policy (expired IPs are automatically removed).

The file `blacklist.txt` contains one IP per line and is updated every hour.
IPs not seen for 30+ days are automatically purged to keep the list relevant.

### FortiGate — External Threat Feed

```
config system external-resource
    edit "TOR-Blacklist"
        set type address
        set resource "https://raw.githubusercontent.com/f3csystems/TOR_IP/main/blacklist.txt"
        set refresh-rate 60
    next
end

config firewall policy
    edit 0
        set name "Block-TOR"
        set srcintf "any"
        set dstintf "any"
        set srcaddr "TOR-Blacklist"
        set dstaddr "all"
        set action deny
        set schedule "always"
        set service "ALL"
        set logtraffic all
    next
end
```

The FortiGate will automatically fetch and refresh the IP list every hour.

### Palo Alto — External Dynamic List (EDL)

**GUI:**

1. Go to **Objects > External Dynamic Lists**
2. Click **Add** and configure:
   - **Name:** `TOR-Blacklist`
   - **Type:** IP List
   - **Source:** `https://raw.githubusercontent.com/f3csystems/TOR_IP/main/blacklist.txt`
   - **Repeat:** Every 30 minutes
3. Create a **Security Policy** referencing this EDL as source address with action **Deny**

### Check Point — Network Feed (R80.10+)

1. In **SmartConsole**, go to **New > More > Network Feed**
2. Configure:
   - **Name:** `TOR-Blacklist`
   - **URL:** `https://raw.githubusercontent.com/f3csystems/TOR_IP/main/blacklist.txt`
   - **Update interval:** 30 minutes
   - **Content type:** IP Address
3. Use this object as **Source** in a **Drop** rule
4. **Install Policy**

---
*Updated automatically every hour — IPs expire after 30 days without activity*
