# intel-turbo
 Linux scripts to manage Turbo Boost on Intel CPUs

# Description
 These simple scripts are used through two systemd services to enable or disable the Intel Turbo Boost technology on any Linux distribution that supports the intel_pstate frequency driver. To check if your system supports it, open the terminal and run:
 
 ```shell
 $ cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_driver
```

 It should return ```intel_pstate``` for each CPU thread.

# Installation
 It is very easy to install these scripts. The intel folder in opt should be placed under the /opt/ folder of your Linux distribution, while the intel-noturbo and intel-turbo service files should be placed under the system subfolder of the systemd
 directory, located in /etc/systemd/system/. I've also put them inside their correct destination folders right in the repository so you can quickly identify where they should be dropped. 
 If you want to enable or disable Turbo Boost at startup, only one of the two services should be enabled, because they go in conflict with each other. For example, if you are on a laptop and want your fans to be quieter and the battery to last longer,
 you can start the service and disable Turbo Boost automatically at startup with:

 ```shell
 $ sudo systemctl enable --now intel-noturbo.service
```

 While you can check the service status with:

 ```shell
 $ sudo systemctl status intel-noturbo.service
```

 You can also directly ask the system if Turbo Boost is enabled or disabled after applying either of the services with:

```shell
 $ cat /sys/devices/system/cpu/intel_pstate/no_turbo
```
 This returns:
 
   - ```0``` if turbo is enabled;
   
   - ```1``` if turbo is disabled.

# Removal
 If you want to uninstall both services, first you need to disable the one that is currently running and that's active on startup. Assuming that the noturbo service is active, you can disable it with:
 
 ```shell
 $ sudo systemctl disable --now intel-noturbo.service
```

 Once the active one is disabled, you can proceed to mask both services to make sure they cannot be started by any means. You can do so with:

```shell
 $ sudo systemctl mask intel-noturbo.service

 sudo systemctl mask intel-turbo.service
```

 You can now proceed to permanently remove the files from your system. You need to delete both the intel folder from /opt/ and the systemd files for these services.

```shell
 $ sudo rm -r /opt/intel/

 sudo rm /etc/systemd/system/intel-noturbo.service /etc/systemd/system/intel-turbo.service 
```

# License
 - This project is distributed under the [GNU General Public License v3.0](https://github.com/ShyVortex/intel-turbo/blob/main/LICENSE).
 - Copyright of [@ShyVortex](https://github.com/ShyVortex), 2023.
