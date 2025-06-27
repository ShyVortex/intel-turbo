# intel-turbo

A simple Linux script to disable Intel Turbo Boost on systems using the `intel_pstate` frequency driver.

---

## Description

This script disables Intel Turbo Boost technology on supported Linux distributions. It can be used with **systemd**, **SysVinit**, or manually through other startup mechanisms.

To check if your system supports the `intel_pstate` driver, run:

```bash
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_driver
```

If all outputs return `intel_pstate`, your system is compatible.

---

## Installation

### Systemd-Based Systems

1. Copy the `intel` folder to `/opt/`:

   ```bash
   sudo cp -r intel /opt/
   ```

2. Copy the `intel-noturbo.service` file to the systemd directory:

   ```bash
   sudo cp systemd/intel-noturbo.service /etc/systemd/system/
   ```

3. Ensure the script has execution permissions:

   ```bash
   sudo chmod +x /opt/intel/intel_noturbo.sh
   ```

4. To automatically disable Turbo Boost at startup:

   ```bash
   sudo systemctl enable --now intel-noturbo.service
   ```

   Or, to disable Turbo Boost manually on demand:

   ```bash
   sudo systemctl start intel-noturbo.service
   ```

5. Check the service status:

   ```bash
   sudo systemctl status intel-noturbo.service
   ```

6. You can verify the Turbo Boost status at any time with:

   ```bash
   cat /sys/devices/system/cpu/intel_pstate/no_turbo
   ```

   * `0`: Turbo Boost is **enabled**
   * `1`: Turbo Boost is **disabled**

---

### SysVinit-Based Systems

1. Copy the `intel` folder to `/opt/`:

   ```bash
   sudo cp -r intel /opt/
   ```

2. Copy the init script to `/etc/init.d/`:

   ```bash
   sudo cp sysvinit/intel_noturbo /etc/init.d/
   sudo chmod +x /etc/init.d/intel_noturbo
   ```

3. Enable the script to run at boot:

   ```bash
   sudo update-rc.d intel_noturbo defaults
   ```

4. To disable Turbo Boost immediately without rebooting:

   ```bash
   sudo /etc/init.d/intel_noturbo start
   ```

   Or:

   ```bash
   sudo service intel_noturbo start
   ```

---

## Manual or Alternative Startup Methods

If your system uses **OpenRC**, **runit**, **s6**, or other init systems, you can still use the script manually or set it to run at startup via alternative methods.

### Manual Execution

```bash
sudo /opt/intel/intel_noturbo.sh
```

### Automatic Execution via Cron (Example)

1. Ensure `cronie` is installed:

   * **Debian/Ubuntu**:

     ```bash
     sudo apt install cronie
     ```

   * **Fedora/RHEL**:

     ```bash
     sudo yum install cronie
     ```

   * **Arch Linux**:

     ```bash
     sudo pacman -S cronie
     ```

2. Verify installation:

   ```bash
   crond -V
   ```

3. Edit the crontab:

   ```bash
   crontab -e
   ```

4. Add the following line to disable Turbo Boost at startup:

   ```bash
   @reboot sh /opt/intel/intel_noturbo.sh
   ```

---

## Removal

### Systemd

1. Disable and stop the service:

   ```bash
   sudo systemctl disable --now intel-noturbo.service
   ```

2. Mask the service to prevent accidental activation:

   ```bash
   sudo systemctl mask intel-noturbo.service
   ```

3. Remove installed files:

   ```bash
   sudo rm -r /opt/intel/
   sudo rm /etc/systemd/system/intel-noturbo.service
   ```

4. Reboot to apply changes.

---

### SysVinit

1. Remove the service from startup:

   ```bash
   sudo update-rc.d -f intel_noturbo remove
   ```

2. Delete installed files:

   ```bash
   sudo rm -r /opt/intel/
   sudo rm /etc/init.d/intel_noturbo
   ```

3. Reboot to apply changes.

---

### Cron or Manual Installation

1. Remove the `@reboot` line from the crontab:

   ```bash
   crontab -e
   ```

2. Delete installed files:

   ```bash
   sudo rm -r /opt/intel/
   ```

---

## License

* Licensed under the [GNU General Public License v3.0](https://github.com/ShyVortex/intel-turbo/blob/main/LICENSE).
* Copyright © [@ShyVortex](https://github.com/ShyVortex), 2023.
