# Disable Laptop Lid Suspend on Proxmox VE 9.x

## Goal

Keep the Proxmox server running when the laptop lid is closed.

---

## 1. Edit the systemd logind configuration

Open the configuration file:

```bash
nano /etc/systemd/logind.conf
```
or if you use micro

```bash
micro /etc/systemd/logind.conf
```

Find or add these lines and set =ignore:

```ini
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
```

> Make sure these lines are **not commented out** (no `#` at the beginning).

Save the file.

---

## 2. Restart the logind service

```bash
systemctl restart systemd-logind
```

Or reboot:

```bash
reboot
```

---

## 3. Verify the configuration

Check the configuration file:

```bash
grep HandleLidSwitch /etc/systemd/logind.conf
```

Expected output:

```text
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
```

---

## 4. Check for override files (optional)

If the laptop still suspends, check whether another configuration overrides the settings:

```bash
grep -R "HandleLidSwitch" /etc/systemd /usr/lib/systemd 2>/dev/null
```

Only your edited `/etc/systemd/logind.conf` should contain these settings.

---

## 5. Test

Close the laptop lid.

Expected result:

- Proxmox continues running.
- Virtual Machines remain online.
- LXC containers remain online.
- SSH remains accessible.
- Web interface remains accessible.

---

## Troubleshooting

If the laptop still suspends:

Check the active log:

```bash
systemctl status systemd-logind
```

If you see:

```text
Lid closed.
Suspending...
```

then the settings have not been applied correctly.

Verify:

- The `HandleLidSwitch` lines are **not commented**.
- `systemd-logind` has been restarted or the system rebooted.
- No override files exist in `/etc/systemd/` or `/usr/lib/systemd/`

---

## Optional: Disable Suspend Completely

For a dedicated Proxmox server, you can prevent any suspend operation:

```bash
systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

To restore suspend functionality later:

```bash
systemctl unmask sleep.target suspend.target hibernate.target hybrid-sleep.target
```
