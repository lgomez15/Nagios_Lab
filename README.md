# Nagios Lab Repository

This repository contains a pre-configured Nagios lab setup. It only requires importing the virtual machines into VirtualBox to start practicing and experimenting with Nagios Core.

## What is Nagios and What is it Used For?

Nagios is a powerful monitoring system that enables organizations to identify and resolve IT infrastructure problems before they affect critical business processes. It offers monitoring and alerting services for servers, switches, applications, and services. Nagios alerts technicians when something goes wrong and alerts them again when the problem has been resolved.

## Why This Repository?

Setting up Nagios can be complex due to the need for multiple machines and compatibility issues between versions. This repository aims to simplify the process by providing a ready-to-use lab environment.

We recommend using PuTTY for a better experience, as it allows copy and paste operations.

## Specifications

### Virtualization:
- VirtualBox 7.0

### Nagios Server:
- CentOS Linux 7
- Nagios Core 4.4.2
  [Nagios Core Releases](https://github.com/NagiosEnterprises/nagioscore/releases)
- Nagios Plugins 2.2.1
  [Nagios Plugins Releases](https://github.com/nagios-plugins/nagios-plugins/releases)
- NRPE 3.2.1
  [NRPE Releases](https://github.com/NagiosEnterprises/nrpe/releases/)

#### Dependency Package Installation (Already Installed)
```bash
yum install -y gettext wget net-snmp-utils openssl-devel glibc-common unzip perl epel-release gcc php gd automake autoconf httpd make glibc gd-devel net-snmp
yum install perl-Net-SNMP
```

### Linux Client:
- CentOS Linux 7
- NRPE 3.2.1
  [NRPE Releases](https://github.com/NagiosEnterprises/nrpe/releases/)

#### Dependency Package Installation (Already Installed)
```bash
yum install -y gcc glibc glibc-common openssl openssl-devel perl wget
```

## Usage Instructions

The lab is set up so that you only need to follow the steps below to start using Nagios on the server.

1. **Configure Network Settings:** Change the network configuration on both machines to ensure they have internet access. Perform the following changes on both the client and server:
   ```bash
   nano /etc/sysconfig/network-scripts/ifcfg-enp0s3 # Access the network configuration file to assign a network, netmask, default gateway, and DNS server.
   nmcli con down enp0s3 && nmcli con up enp0s3 # Restart network interfaces.
   ```
   Verify internet access, for example, with: `ping www.google.com`

2. **Server Configuration:**
   Go to `/usr/local/nagios/etc/objects/` and edit `linux.cfg`:
   ```bash
   nano linux.cfg
   ```
   ###### Server Definition
   ```
   define host{
           use             linux-server
           host_name       linux_client
           alias           Linux CentOS
           check_interval  1
           address         <IP_Linux_Client> # Replace with the client's IP without brackets <>
   }
   ```
   Finally, execute:
   ```bash
   nagioscheck # to verify everything is in order
   nagiosreload # to restart the Nagios service
   ```

3. **Linux Client Configuration:**
   Go to `/usr/local/nagios/etc/` and edit `nrpe.cfg`:
   ```bash
   nano nrpe.cfg
   ```
   Set allowed hosts:
   ```
   allowed_hosts=127.0.0.1,::1,<Nagios_Server_IP> # Replace with the Nagios server IP
   ```
   Finally, execute:
   ```bash
   systemctl restart nrpe
   systemctl status nrpe # and verify that everything is well configured
   ```
```

This README.md format is designed for easy copy-pasting into your repository, providing a comprehensive guide for using your Nagios lab setup.