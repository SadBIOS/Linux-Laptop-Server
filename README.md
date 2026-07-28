# Disable Laptop Lid Close Action (Portable Linux Server)

If you're using a laptop as a portable server, you may want it to continue running even when the lid is closed. This guide shows how to disable the default **lid close action** on **Arch Linux** and **Debian** or **Proxmox VE 9.x** etc.

---

## Arch Linux

### 1. Edit the `systemd-logind` configuration

Open the configuration file with your preferred text editor:

Using **nano**:

```bash
nano /etc/systemd/logind.conf
```

Or using **micro**:

```bash
micro /etc/systemd/logind.conf
```

Find the following line:

```text
#HandleLidSwitch=suspend
```

Uncomment it and change the value to:

```text
HandleLidSwitch=ignore
```

Save the file and exit the editor.

### 2. Apply the changes

Restart the `systemd-logind` service:

```bash
systemctl restart systemd-logind
```

The laptop will now ignore lid close events.

---

## Debian

### 1. Edit the UPower configuration

Open the configuration file:

Using **nano**:

```bash
sudo nano /etc/UPower/UPower.conf
```

Find the following line:

```text
IgnoreLid=false
```

Change it to:

```text
IgnoreLid=true
```

Save the file and exit the editor.

### 2. Reboot the system

Apply the changes by rebooting:

```bash
reboot
```

After rebooting, the system will ignore lid close events.

---

## Proxmox VE 9.x.y
## 1. Edit the systemd logind configuration

Open the configuration file:

```bash
nano /etc/systemd/logind.conf
```
or if you use micro:

```bash
micro /etc/systemd/logind.conf
```

Find or add these lines:

```ini
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
```

> Make sure these lines are **not commented out** (no `#` at the beginning).

Save the file then **Restart the logind service**


Verify:

- The `HandleLidSwitch` lines are **not commented**.
- `systemd-logind` has been restarted or the system rebooted.
- No override files exist in `/etc/systemd/` or `/usr/lib/systemd/`.

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

## More information
> **Note:** Some desktop environments (such as GNOME, KDE Plasma, or XFCE) may have their own power management settings that override these configurations. If the laptop still suspends when the lid is closed, check your desktop environment's power settings as well. You can check additional .md files from this repo, some of tested config with details information may include in future.

You have to edit every logind config file as a root. 
