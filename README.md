# AirConnect package for Synology NAS and Synology Router

[![Latest release](https://img.shields.io/github/release/bandesz/AirConnect-Synology.svg)](https://github.com/bandesz/AirConnect-Synology/releases/latest)

A minimal Synology package for [AirConnect](https://github.com/philippe44/AirConnect
). It allows you to use AirPlay to stream to Chromecast devices. This is a fork of @bandesz's [original version](https://github.com/bandesz/AirConnect-Synology) for UPnP/Sonos devices.

## How to install

### Download the pre-built Synology package

You can find the available packages under [Releases](https://github.com/bandesz/AirConnect-Synology/releases) for three different architecture groups:
 * **ARMv7**: ipq806x armada370 armadaxp armada375 armada38x alpine alpine4k monaco comcerto2k
 * **Intel - 32-bit**: x86 cedarview bromolow evansport avoton braswell broadwell apollolake
 * **Intel - 64-bit (DSM 6.0+)**: x86_6

You can check which architecture you have [here](https://www.synology.com/en-uk/knowledgebase/DSM/tutorial/General/What_kind_of_CPU_does_my_NAS_have).

For the Synology Routers you should use the ARM version.

### Install in Package Center

Open the Package Center app.

As this package is not an official Synology package you may have to allow packages from any publisher. Go to Settings and set the Trust Level to "Any publisher".

Click on Manual Install and upload the package you just downloaded.

Don't forget to change back the Trust level to "Synology Inc." for additional security.

## How it works

It runs the aircast process with the following options:

```
aircast -b [router local ip] -z -f /tmp/aircast.log -d all=error -d main=info
```

The process is running with a low-privilege user.

The aircast process will only recognise your devices if it's bound to the appropriate local network IP, but as there are various Synology devices and network setups this is not trivial.
The start script will check all your local network interfaces (with ip 192.168.* or 10.*) and checks if the aircast process adds any devices (based on the logs). It there are no devices added in 3 seconds it will try the next interface. For the automatic IP discovery to work you should have at least one Chromecast device on your network.

If the start script is not able to find the right IP automatically you can fix it in `scripts/start-stop-status` by setting your own local IP and building your own package. Look for the following lines:

```
# If you want to start the aircast process on a specific IP please uncomment the following lines
# and set your own local IP address:
#
# start_aircast "1.2.3.4" || true
# return  0
```

Alternatively you can open an issue ticket and include your network interface list and your local IP.

## Build

Optionally run shellcheck:

```
make shellcheck
```

Build packages for all architectures:

```
make clean build-all
```

Build a package for only one architecture:

```
ARCH=arm make clean build
```

Possible values for ARCH: arm, x86, x86-64

You can find the built packages in the dist directory.

## Debugging

You can see the error logs by going to the Package Center and clicking **View Log** on the package's page.
If you want to see more logs then change the ```-d all=error``` parameter in scripts/start-stop-status and rebuild the package, then install it again from the dist directory.

## License

See https://github.com/philippe44/AirConnect/blob/master/LICENSE.
