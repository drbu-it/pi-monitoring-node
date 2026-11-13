# pi-monitoring-node

DRBU IT monitoring node: one Raspberry Pi 5 that watches things for us so we do not have to walk to them.

Tracker: Notion ticket #39 (Jira WS-28).

## What it does

| # | Function | Folder | Status | Who |
|---|----------|--------|--------|-----|
| 1 | **Print monitoring** - page counts, toner levels, print logs for every DRBU printer, viewable remotely | `print-monitor/` | Starting now | Neeju, Anis |
| 2 | **UPS monitoring (NUT)** - watch battery and line power, shut down cleanly on outage | `nut/` | Planned | Frank |
| 3 | **Protect footage transfer** - copy UniFi Protect camera recordings to Google Drive | `protect-backup/` | Planned | Frank |

The Pi OS install has to suit all three, so it is planned once up front: see [docs/pi-os-setup.md](docs/pi-os-setup.md).

## Hardware

- Raspberry Pi 5
- WD Blue SN580 500 GB NVMe on an M.2 HAT
- Status: arrived, not imaged yet

## How we work

- All work lives here. One GitHub issue per task.
- Branch per issue, pull request to `main`, one reviewer.
- **No secrets in git.** No passwords, SNMP community strings, API keys or `rclone.conf`. Use `.env` files (ignored) and keep an `.env.example` with fake values.
- Printer IPs go in `print-monitor/printers.yml`. This repo is private, keep it that way.

## Docs

- [Kickoff meeting, 2026-11-17](docs/meeting-2026-11-17.md)
- [Pi OS setup plan](docs/pi-os-setup.md)
- [Printer inventory](docs/printer-inventory.md)

## Roadmap

1. Image the Pi, base OS setup
2. Printer inventory across DRBU SW, C-Dorm, SC
3. SNMP proof of concept on one printer
4. Collector for all printers, on a schedule
5. Remote view (decided at kickoff)
6. Alert digest (low toner, faults)
7. NUT
8. Protect footage to Google Drive
