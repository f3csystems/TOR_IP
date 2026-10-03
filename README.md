# TOR Exit/Relay Node Blacklist

Automatically updated blacklist of TOR node IP addresses observed in corporate traffic.

**Last updated:** 2026-10-03 22:42
**Total active IPs:** 234
**Retention policy:** 30 days

## Files
- `blacklist.csv` - ip, first_seen, last_seen, alert_count, country
- `blacklist.txt` - Plain text IP list (1 per line, for EDL / Threat Feed)

## Firewall Integration — External Dynamic Lists / Threat Feeds

> Consume via EDL / Threat Feed only (auto-refresh + retention).

### FortiGate
```
config system external-resource
    edit "TOR-Blacklist"
        set type address
        set resource "https://raw.githubusercontent.com/f3csystems/TOR_IP/main/blacklist.txt"
        set refresh-rate 30
    next
end
```

### Palo Alto — EDL
- Objects > External Dynamic Lists > IP List > Source: `https://raw.githubusercontent.com/f3csystems/TOR_IP/main/blacklist.txt`

---
*Updated automatically — IPs expire after 30 days without activity*
