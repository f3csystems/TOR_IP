# TOR IP Blacklist

Automatically updated IP blacklist from TOR Usage alerts (Sekoia.io).
Contains IPs observed using the TOR network (exit nodes, guard nodes, relay nodes).

**Last updated:** 2026-04-10 14:57
**Total active IPs:** 318
**Retention policy:** 30 days — IPs not seen for 30+ days are automatically removed

## Rules monitored
- `TOR Usage`
- `TOR Usage Generic Rule`
- `TOR Usage Generic Rule - Wifi Guest/ Eduroam`

## Files
- `blacklist.csv` - Full blacklist with metadata (ip, first_seen, last_seen, alert_count, country, tor_node_types)
- `blacklist.txt` - Plain text IP list (1 IP per line, for External Dynamic List / Threat Feed)

## TOR Node Types Distribution
| Node Type | Count |
|-----------|-------|
| unknown | 318 |

## Top 10 TOR IPs
| IP | Alerts | Country | TOR Node Types |
|----|--------|---------|----------------|
| 148.251.236.209 | 10 | N/A | unknown |
| 198.55.103.184 | 10 | N/A | unknown |
| 195.206.234.89 | 10 | N/A | unknown |
| 31.56.7.161 | 9 | N/A | unknown |
| 185.243.218.138 | 9 | N/A | unknown |
| 213.239.213.220 | 7 | N/A | unknown |
| 45.9.156.16 | 7 | N/A | unknown |
| 81.162.55.28 | 7 | N/A | unknown |
| 93.123.118.224 | 7 | N/A | unknown |
| 109.70.100.248 | 6 | N/A | unknown |

## Firewall Integration — External Dynamic Lists / Threat Feeds

> **Important:** This blacklist **must** be consumed via External Dynamic Lists (EDL) or Threat Feeds.
> Do **not** import the IPs manually — only dynamic feeds ensure automatic updates
> and respect the 30-day retention policy (expired IPs are automatically removed).

The file `blacklist.txt` contains one IP per line and is updated every 45 minutes.
IPs not seen for 30+ days are automatically purged to keep the list relevant.

### FortiGate — External Threat Feed

```
config system external-resource
    edit "TOR-Blacklist"
        set type address
        set resource "https://raw.githubusercontent.com/f3csystems/TOR_IP/main/blacklist.txt"
        set refresh-rate 45
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

The FortiGate will automatically fetch and refresh the IP list every 45 minutes.

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
*Updated automatically every 45 minutes — IPs expire after 30 days without activity*
