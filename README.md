# Welcome to FireMyPi!

[![Lint](https://github.com/zerty-gif/firemypi/actions/workflows/lint.yml/badge.svg)](https://github.com/zerty-gif/firemypi/actions/workflows/lint.yml)

## Overview
[FireMyPi](https://github.com/FireMyPi) is a configuration tool designed to help you to configure and install the [IPFire](https://www.ipfire.org) firewall for your home network on a Raspberry Pi.  It allows you to create a micro sd card installation of IPFire that will boot up and run on your Raspberry Pi "out of the box".

Everyone should have a firewall between their home network and the internet.  You may already have a cable modem or router device that includes a built in firewall, but those are usually black boxes with limited options that are difficult to configure, maintain, and monitor.

FireMyPi allows you to easily create a low cost firewall on a Raspberry Pi that "just works" without extensive configuration.  You can be in control, monitor your network traffic and customize your firewall to meet your needs.  For the price of a Raspberry Pi a USB ethernet dongle and a couple of micro sd cards, FireMyPi allows you to do just that.

FireMyPi configures IPFire for the Raspberry Pi without running the IPFire setup tool.  It disables the serial interface for IPFire, so no need to edit any U-Boot files, and automatically identifies and configures the internal and external interfaces on the Raspberry Pi for IPFire.  It also includes the basic settings that would be set with the IPFire setup program as well as advanced options that would be set with the IPFire Web GUI.  This saves time for configuration and allows you to have a **reviewable and reproducible** configuration separate from the firewall.  If your micro sd card breaks or gets corrupted, (it happens), you can just rebuild the configuration and get your firewall running again.  In fact, we recommend that you create two micro sd's with your firewall configuration so that you always have a backup ready at hand.

FireMyPi was designed with the ability to configure and connect multiple separate firewall nodes with Ipsec VPN tunnels.  If you need to connect multiple home networks, FireMyPi allows you to do this easily.

## FireMyPi includes the following basic settings:

- Turns off the serial console for the Raspberry Pi in U-Boot.
- Automatically configures the IPFire green0 and red0 interfaces on boot.
- Allows you to use either a USB ethernet dongle or the Raspberry Pi
  internal wireless nic as the red0 interface.
- Changes the U-Boot configuration so that Rasperry Pi 4B
  models boot correctly. (See Notes)
- Turns on file system journaling for the IPFire root file system on the
  micro sd card.
- Turns on SSH on the green0 interface for headless access to the firewall
  from the internal network only.
- Limits the IPFire Web GUI to the green0 interface for internal access only.
- Configures timezone, hostname, keyboard and language.
- Configures the root password and the IPFire Web GUI admin password.
- Configures and activates the DHCP server in IPFire.


## FireMyPi also includes configuration options that allow you to easily:

- Include fixed IP addresses in the DHCP configuration.
- Include local hostnames in the DNS configuration.
- Configure dynamic DNS.
- Add additional internet DNS servers.
- Turn on Location Blocking.
- Turn on IP Address Blocklist.
- Configure Ipsec VPN tunnels between firewall nodes.
- Configure OpenVPN for remote (Road Warrior) access.
- Turn on the Fireinfo service.
- Monitor and optionally restart the red0 interface to
  recover from ISP network connection issues.
- Create a small portable build that can be transferred to
  a remote node location.


## Quickstart

### Prerequisites

Install the required dependencies:

**On Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install ansible apache2-utils openssl pwgen u-boot-tools
```

**On macOS:**
```bash
brew install ansible pwgen u-boot-tools
```

### Quick Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/zerty-gif/firemypi.git
   cd firemypi
   ```

2. **Accept the license:**
   ```bash
   ./accept-license.sh
   ```

3. **Create node configuration:**
   ```bash
   ./mk-node-config.sh --node 1
   ```

4. **Create secrets (passwords):**
   ```bash
   ./mk-root-secret.sh --node 1
   ./mk-webadmin-secret.sh --node 1
   ```

5. **Verify the build environment:**
   ```bash
   ./show-build-environment.sh --node 1
   ```

6. **Build a test configuration:**
   ```bash
   ./build-firemypi.sh --node 1 --test
   ```

7. **Build a production image:**
   ```bash
   ./build-firemypi.sh --node 1 --prod --image
   ```

8. **Write the image to a micro SD card** using `rpi-imager` or similar tool.

For detailed configuration options, see the [Configuration](#configuration) section below or refer to the [Administrator's Guide](doc/fmp-admin-guide.html).

## Configuration

### System Configuration (`config/system_vars.yml`)

Key configuration options:

| Option | Description | Default |
|--------|-------------|---------|
| `prefix` | Hostname prefix for all firewalls | `"firemypi"` |
| `timezone` | System timezone | `"US/Mountain"` |
| `language` | System language | `"en"` |
| `domain` | Public domain name | `"localdomain"` |
| `green_host` | Production IP host number | `254` |
| `green_testhost` | Test IP host number | `245` |

### Optional Features

Enable these in `config/system_vars.yml`:

| Feature | Variable | Description |
|---------|----------|-------------|
| Dynamic DNS | `include_ddns` | Update DNS records automatically |
| DHCP Fixed Leases | `include_dhcp_fixleases` | Static IP assignments |
| IP Blocklist | `include_ipblocklist` | Block known bad IP addresses |
| Location Blocking | `include_locationblock` | Block traffic by country |
| OpenVPN | `include_ovpn` | Remote access VPN |
| IPsec VPN | `include_vpn` | Site-to-site VPN tunnels |
| Fireinfo | `include_fireinfo` | Send anonymous usage data to IPFire |

### Node Configuration (`config/nodeN_vars.yml`)

Each firewall node has its own configuration file with settings specific to that node:

- `wireless_red0` - Use wireless interface for WAN connection
- `fixleases_mode` - DHCP fixed lease configuration mode
- `hosts_mode` - Local DNS hostname configuration mode

### Secrets Directory

The `secrets/` directory contains sensitive configuration files:

| File | Created By | Purpose |
|------|------------|---------|
| `root_secrets.yml` | `mk-root-secret.sh` | Root password |
| `webadmin_secrets.yml` | `mk-webadmin-secret.sh` | Web GUI admin password |
| `ddns_secret.yml` | `mk-ddns-secret.sh` | Dynamic DNS credentials |
| `vpnpsk_secrets.yml` | `mk-vpnpsk-secret.sh` | VPN pre-shared key |
| `wireless_secrets.yml` | `mk-wireless-secret.sh` | WiFi credentials |

> **Important:** Never commit secrets to version control!

## Common Issues

### Build fails with "something is missing in the configuration"

Run `./show-build-environment.sh --node N` to identify missing configuration files or secrets.

### "Wrong directory" error

Ensure you are running scripts from the FireMyPi root directory.

### Image creation requires sudo password

This is expected - creating disk images requires root privileges for mounting.

### USB Ethernet dongle not detected

Verify your dongle uses one of the supported drivers (see [Hardware Compatibility](#hardware-compatibility)).

## Hardware Compatibility

### Supported Raspberry Pi Models
- Raspberry Pi 3 (all variants)
- Raspberry Pi 4 Model B (all revisions)
- Raspberry Pi 400

### Supported USB Ethernet Dongles
- TP-Link USB 3.0 Gigabit (RTL8153)
- Anker USB 3.0 Gigabit (CDC NCM)
- UGREEN USB 3.0 Gigabit (AX88179)
- Generic ASIX-based dongles (AX88179/AX88178a)
- StarTech USB 3.0 dongles

## How FireMyPi Works

FireMyPi is built on top of the IPFire firewall program and provides automated configuration to manage one or more firewall machines in a network.  The [network diagram](doc/fmp-network-diagram.png) shows the key parts.

![](doc/fmp-network-diagram.png)

The configuration information for FireMyPi consists of a set of YAML files stored in the *config* directory and in the *secrets* directory.

When a build is run, FireMyPi uses the configuration files to create a *config* and *image* for the node being built.

The *config* portion of the build creates an overlay that will be extracted onto the /var/ipfire directory and overwrite the default IPFire configuration files.  The *config* creation includes a one-time run script that installs the configuration the first time the firewall is booted.

The *image* creation writes the *config* to an IPFire core image and installs the hooks necessary to run the configuration script.  When the *image* creation is complete, the result is a pre-configured image that can be written to a micro sd card and booted on the Raspberry Pi.

## IPFire Compatibility
FireMyPi v1.7 is compatible with IPFire 2.29 Core Update 186 and later versions, including Core Updates 190, 194, and beyond. The configuration system is designed to work with the latest stable releases of IPFire for Raspberry Pi (aarch64/ARM64).

## Documentation
Refer to the [FireMyPi Administrator's Guide](doc/fmp-admin-guide.html) for complete instructions and guidance.

## Copyright and License
* Copyright © 2020-2024 David Čuka and Stephen Čuka All Rights Reserved.
* FireMyPi is licensed under the Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License (CC BY-NC-ND 4.0).
* The full text of the license can be found in the included LICENSE file or at https://creativecommons.org/licenses/by-nc-nd/4.0/legalcode.en.
* For the avoidance of doubt, FireMyPi is for personal use only and may not be used by or for any business in any way.

## Version
|          |       |
| -------- |:----- |
|Version:  |v1.7|
|Date:     |Mon Dec 9 16:27:24 2024 -0700  |
