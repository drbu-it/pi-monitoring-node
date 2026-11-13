# Pi OS setup plan

One install has to serve print monitoring now, and NUT plus the Protect footage transfer later. These are the choices that are hard to change after imaging.

## Base

| Item | Choice | Why |
|------|--------|-----|
| OS | Raspberry Pi OS Lite, 64-bit | Headless, no desktop needed |
| Boot | NVMe (no SD card in normal use) | SD cards wear out under constant writes |
| Network | Wired Ethernet, DHCP reservation | Printers and the NVR need a stable address to talk to |
| Hostname | `drbu-monitor` (proposed) | |
| Access | SSH keys only, one user per person | No shared password |
| Remote | Tailscale (proposed) | No inbound ports opened |
| Updates | `unattended-upgrades` for security patches | |
| Time | NTP on, timezone America/Los_Angeles | Logs and digests need correct time |

## Disk layout (500 GB NVMe)

| Partition | Size | Use |
|-----------|------|-----|
| boot | default | firmware |
| root | 64 GB | OS, services, print data |
| `/srv/protect` | remainder | Protect footage staging before upload |

Footage gets its own partition so a full staging area cannot fill root and take down print monitoring and NUT.

## Considerations for NUT

- The Pi must be powered **from the UPS** so it stays up to report the outage.
- One USB port reserved for the UPS data cable.
- UPS model must be on the NUT hardware compatibility list. Check before relying on it.
- Pi runs as NUT server; other machines can be clients later.

## Considerations for the Protect footage transfer

- Tool: `rclone` to a Google shared drive.
- Auth: service account or OAuth token, stored outside git.
- Run overnight with `--bwlimit` so uploads do not hurt daytime internet.
- Google Drive caps uploads at 750 GB per user per day.
- Open question: how footage gets from the NVR to the Pi (Protect export, API archiver, or network share).

## Considerations for print monitoring

- Pi needs to reach every printer on UDP 161 (SNMP). Check VLAN and firewall rules for C-Dorm and SC.
- Python 3 with a virtualenv per service.
- Services run under `systemd` timers, not a logged-in session.

## Imaging steps

1. Put the NVMe in a USB enclosure, flash with Raspberry Pi Imager.
2. In Imager settings: set hostname, user, SSH key, timezone.
3. Seat the NVMe in the HAT, connect Ethernet, power on.
4. If it does not boot from NVMe: boot from SD once, run `sudo rpi-eeprom-update -a`, set boot order in `raspi-config`.
5. Create the `/srv/protect` partition.
6. Install base packages: `git python3-venv snmp unattended-upgrades`.
