# Python and Bash scripting

Automation, parsing, and operational shortcuts — not full applications.

## Automated log parsing

Python scans daily logs for disconnect error codes and writes a daily summary.

```python
# Local file only. No hosts, no credentials.
codes = ("DISCONNECT", "35=5")
counts = {code: 0 for code in codes}
with open("gateway.log", encoding="utf-8") as handle:
    for line in handle:
        for code in codes:
            if code in line:
                counts[code] += 1
print(counts)
```

## Health checks

Bash via cron checks key processes and alerts if one drops.

```bash
# cron: */5 * * * * /opt/ops/check-gateway.sh
# Process names only. No hosts, no credentials.
for svc in fix-gateway md-feed; do
  if ! ps aux | grep -v grep | grep -q "$svc"; then
    echo "ALERT: $svc is down"
  fi
done
```

## Data transformation

Client config files or order CSVs into a database import format: map the columns, normalise timestamps, write the load file.

**What good looks like:** You walk the script in one pass: the file it reads, the check or column map, and the summary or import file it writes.
