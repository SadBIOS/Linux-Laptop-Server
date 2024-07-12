# Disable Lid Action Linux Laptop Server aka Portable Server

This guide explains how to disable the action triggered by closing the laptop lid on Arch Linux and Debian systems.

## Arch Linux

1. **Edit `logind.conf`:**
   - Open a terminal.
   - Edit the `logind.conf` file with root privileges:
   - 
     ```bash
     sudo nano /etc/systemd/logind.conf
     ```
     
   - Find the line `#HandleLidSwitch=suspend` and uncomment it (remove the `#`), then change `suspend` to `ignore`:
  
   - 
     ```text
     HandleLidSwitch=ignore
     ```
     
   - Save the file (`Ctrl + O`, then `Enter`) and exit (`Ctrl + X`).

2. **Apply Changes:**
   - After saving the changes, you need to restart the `systemd-logind` service to apply the new configuration:
   - 
     ```bash
     sudo systemctl restart systemd-logind
     ```

## Debian

1. **Edit `UPower.conf`:**
   - Open a terminal.
   - Edit the `UPower.conf` file with root privileges:
   - 
     ```bash
     sudo nano /etc/UPower/UPower.conf
     ```
     
   - Find the line `IgnoreLid=false` and change it to:
   - 
     ```text
     IgnoreLid=true
     ```
     
   - Save the file (`Ctrl + O`, then `Enter`) and exit (`Ctrl + X`).

2. **Reboot:**
   - After making the change, reboot your system to apply the new configuration:
   - 
     ```bash
     sudo reboot
     ```

By following these steps, you will disable the action taken when closing the laptop lid on Arch Linux and Debian systems.
