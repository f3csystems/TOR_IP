# TOR IP Blacklist

Automatically updated IP blacklist from TOR Usage alerts (Sekoia.io).
Contains destination IPs of TOR nodes observed in network traffic.

**Last updated:** 2026-05-16 00:22
**Total active IPs:** 137
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
| 95.216.33.58 | 67 | NL |
| 50.28.86.149 | 67 | NL |
| 94.23.76.244 | 67 | NL |
| 31.172.65.197 | 67 | NL |
| 91.196.82.247 | 67 | NL |
| 217.154.145.43 | 67 | NL |
| 62.112.9.92 | 67 | NL |
| 85.23.8.178 | 67 | NL |
| 108.62.119.77 | 67 | NL |
| 51.222.15.8 | 67 | NL |

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
