#gnulinux_support common

##Target
Provide common codes or files needed for the gnulinx_support project like :
- sdroid (schroot for Android manager)
- android-init (Android init manager for GNU/Linux)
- *.service (some systemd service unit files)
- *.target (some systemd target like bootloader, recovery, ...)

##Sources
###init
####android-init

android-init command permit to manage Android /init process.

This can be managed with systemd unit file hybris-ready.service.
If enabled, this service is part of the basic.target which is a default dependency for every units (except those with DefaultDependencies=no).

hybris-ready.service can start/stop Android system ONLY ONE TIME between each reboot due to Android init constraint.

###schroot
####sdroid

sdroid permit to easily manage an Android schroot session with sdroid command.
If the Android schroot session is not started, it will do it for you.

Example:

Without sdroid:
```
schroot -b -c android -n droid
schroot -r -c droid -- ls -l /
schroot -r -c droid
schroot -e -c droid

```
With sdroid:
```
sdroid ls -l /
sdroid
sdroid -e
```

sdroid will restart a session if the session was not cleaned properly after a reboot.

sdroid will automatically execute itself to the cgroup systemd/system.slice/hybris-ready.service if the cgroup exist and to permit a better tracking of Android running processes by systemd when you are using hybris-ready.service (see hybris-ready package)

####android-example

Example of schroot configuration for Android chroot.
Those files can be used by hybris-device-?-? packages (eg: see hybris-device-sony-nozomi)

###systemd

Support service for hybris-ready and others.

Reboot into recovery or bootloader (fastboot) is supported with new target files.
```
eg: systemctl isolate recovery
eg: systemctl isolate bootloader
```
