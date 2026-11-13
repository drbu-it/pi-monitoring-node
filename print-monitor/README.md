# print-monitor

Polls every DRBU printer over SNMP and records page counts and supply levels.

## What to read from a printer

Standard Printer MIB (works on most networked printers):

| Value | OID |
|-------|-----|
| Total page count | `1.3.6.1.2.1.43.10.2.1.4.1.1` |
| Supply name | `1.3.6.1.2.1.43.11.1.1.6.1.x` |
| Supply max capacity | `1.3.6.1.2.1.43.11.1.1.8.1.x` |
| Supply current level | `1.3.6.1.2.1.43.11.1.1.9.1.x` |
| Device status | `1.3.6.1.2.1.25.3.5.1.1.1` |

Toner % = current level / max capacity x 100. Some printers report -2 or -3 (unknown / "some left") instead of a number.

Try it:

```
snmpwalk -v2c -c <community> <printer-ip> 1.3.6.1.2.1.43.11.1.1
```

## Planned pieces

- `printers.yml` - list of printers (name, site, IP)
- `collect.py` - poll each printer, write one row per printer per run
- output - decided at kickoff (Sheet / database / both)
- `digest.py` - low-toner and fault summary email
