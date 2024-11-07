# intel-turbo
 Linux script to disable Turbo Boost on Intel CPUs

# Description
 This simple script is used through a systemd service to disable the Intel Turbo Boost technology on any Linux distribution that supports the intel_pstate frequency driver. To check if your system supports it, open the terminal and run:

 ```shell
 cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_driver
```

 It should return ```intel_pstate``` for each CPU thread.

# Installation
 It is very easy to install this script. The intel folder in opt should be placed under the /opt/ folder of your Linux distribution, while the intel-noturbo service file should be placed under the system subfolder of the systemd
 directory, located in /etc/systemd/system/. I've also put it inside its correct destination folder right in the repository so you can quickly identify where it should be dropped.
 If you want to automatically disable Turbo Boost at startup, you can do so with with:

 ```shell
 sudo systemctl enable --now intel-noturbo.service
```

 Or, you can just disable turbo manually at your will and not automatically with:

 ```shell
 sudo systemctl start intel-noturbo.service
```

 While you can check the service status with:

 ```shell
 sudo systemctl status intel-noturbo.service
```

 You can also directly ask the system if Turbo Boost is enabled or disabled before and after applying the service with:

 ```shell
 cat /sys/devices/system/cpu/intel_pstate/no_turbo
 ```
 This returns:

   - ```0``` if turbo is enabled;

   - ```1``` if turbo is disabled.

# Removal
 If you want to uninstall the service, first you need to disable it from running and activating on startup. You can disable it with:

 ```shell
 sudo systemctl disable --now intel-noturbo.service
```

 Once the service is disabled, you can proceed to mask it so it cannot be started by any means. You can do so with:

 ```shell
 sudo systemctl mask intel-noturbo.service
 ```

 You can now proceed to permanently remove the files from your system. You need to delete both the intel folder from /opt/ and the added systemd file.

 ```shell
 sudo rm -r /opt/intel/

 sudo rm /etc/systemd/system/intel-noturbo.service
 ```

 Reboot your system for the changes to take effect.

# Usage outside of systemd
 Not all Linux distributions come with the systemd daemon, but they may use other alternatives like OpenRC, runit or s6. In this case, you can still use the script but you can't enable it at startup with
 the method written above. What you can do instead is copy the intel folder in your system's /opt/ and follow one of the other methods listed [here](https://www.baeldung.com/linux/run-script-on-startup) to
 enable its autoexecution at startup.
 If you only want to execute it manually at your own pace, you can do so by opening the terminal and writing this command.

```shell
sudo ./opt/intel/intel_noturbo.sh
```

 As an example, we can use the Cron Job method to automatically run the script at startup on non-systemd distributions. To do so, you have to first make sure that cronie is installed on your system.
 Open the terminal, and do the following:

- If you are on Debian or Ubuntu, type

  ```shell
  sudo apt install cronie
  ```

- On Fedora or RHEL, type

  ```shell
  sudo yum install cronie
  ```

- On Arch Linux and its derivatives, type

  ```shell
  sudo pacman -S cronie
  ```

 You can check if the installation was successful with:

 ```shell
 crond -V
 ```

 Now we need to edit the cron table to add our task which runs the custom script on startup. Type:

 ```shell
 crontab -e
 ```

 Then add said task using the @reboot expression, which executes the code once at startup:

 ```shell
 @reboot sh /opt/intel/intel_noturbo.sh
 ```

 If you want to remove the script in the future, first you need to delete its task from the cron table, typing again ```crontab -e``` and removing the line previously added.
 You can then proceed to delete all files from your system, following the same method as in [Removal](https://github.com/ShyVortex/intel-turbo/edit/main/README.md#removal):

 ```shell
 sudo rm -r /opt/intel/

 sudo rm /etc/systemd/system/intel-noturbo.service
 ```

# License
 - This project is distributed under the [GNU General Public License v3.0](https://github.com/ShyVortex/intel-turbo/blob/main/LICENSE).
 - Copyright of [@ShyVortex](https://github.com/ShyVortex), 2023.
