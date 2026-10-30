# NETWORKWALKS-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP

## Overview

This repository documents the installation and configuration of my cybersecurity lab environment using Oracle VirtualBox and Kali Linux on an Intel-based MacBook Pro. The purpose of this lab was to create a virtualized environment for practicing cybersecurity techniques, configuring virtual networks, troubleshooting connectivity issues, and managing Linux systems.

The lab included installing VirtualBox, importing Kali Linux, configuring a NAT Network using the `10.0.0.0/24` subnet, troubleshooting network connectivity, verifying internet access, and updating Kali Linux.

## 1. Installing Oracle VirtualBox

I began by installing Oracle VirtualBox on my Intel-based MacBook Pro.

### Installation Steps

1. Navigated to the official VirtualBox website: https://www.virtualbox.org/wiki/Downloads
2. Downloaded the VirtualBox installer compatible with Intel-based macOS systems.
3. Opened the downloaded `.dmg` file.
4. Followed the installation instructions to install VirtualBox.
5. Opened Oracle VirtualBox Manager to verify the installation was successful.

VirtualBox allowed me to run Kali Linux as a virtual machine without modifying my existing macOS installation.

## 2. Downloading and Installing Kali Linux

After successfully installing VirtualBox, I downloaded Kali Linux to use as my cybersecurity lab operating system.

### Installation Steps

1. Navigated to the official Kali Linux website: https://www.kali.org/get-kali/
2. Selected the prebuilt Kali Linux virtual machine for VirtualBox.
3. Downloaded the AMD64 version compatible with my Intel MacBook.
4. Extracted the downloaded `.7z` archive using 7-Zip.
5. Opened Oracle VirtualBox Manager.
6. Selected the option to open an existing virtual machine.
7. Located and selected the extracted Kali Linux `.vbox` configuration file.
8. Added the Kali Linux virtual machine to VirtualBox.
9. Started Kali Linux and logged in using the default credentials.

**Default credentials:**

- Username: `kali`
- Password: `kali`

After logging in, I verified that Kali Linux successfully booted and that I could access the desktop and Terminal.

### Virtual Machine Configuration

The virtual machine was configured to run Debian-based Kali Linux using VirtualBox's virtualized hardware environment.

I also adjusted the display settings and enabled automatic guest display resizing so that Kali Linux could properly use my MacBook's screen.

![Kali Linux Screenshot](Lab-1/NWW1SS2.png)

## 3. Configuring the NAT Network

The next objective was to configure a virtual network using the subnet `10.0.0.0/24`.

### Network Configuration

| Setting | Configuration |
|---|---|
| Network Name | NatNetwork |
| Network Type | NAT Network |
| Network Subnet | 10.0.0.0/24 |
| Subnet Mask | 255.255.255.0 |
| Network Interface | eth0 |
| Virtual Adapter | Intel PRO/1000 MT Desktop |
| Virtual Cable | Connected |

### Configuration Steps

1. Opened Oracle VirtualBox Manager.
2. Selected the Kali Linux virtual machine.
3. Navigated to **Settings → Network → Adapter 1**.
4. Enabled the network adapter.
5. Selected **NAT Network** under the "Attached to" dropdown menu.
6. Selected the virtual network named `NatNetwork`.
7. Confirmed that the virtual cable was connected.
8. Saved the configuration and started Kali Linux.

The objective of this configuration was to allow the Kali Linux virtual machine to communicate through a virtual network while using the host computer's internet connection.

### Verifying the Network Interface

Inside Kali Linux, I opened Terminal and executed:

```bash
ip a
```

This command displayed the available network interfaces and their assigned IP addresses.

I also used:

```bash
ip -4 addr show eth0
```

During troubleshooting, I confirmed that the Ethernet interface was active and had received the IPv4 address:

```text
10.0.0.2/24
```

This confirmed that the interface had an IPv4 configuration within the intended subnet, although internet connectivity had not yet been established.

## 4. Troubleshooting Network Connectivity

After configuring the NAT Network, I encountered connectivity issues when attempting to access the internet from Kali Linux.

### Initial Connectivity Tests

I first tested external connectivity using:

```bash
ping -c 4 8.8.8.8
```

The test returned 100% packet loss, indicating that the virtual machine could not successfully reach the external IP address.

I then tested domain-name resolution:

```bash
ping -c 4 google.com
```

The command returned:

```text
Temporary failure in name resolution
```

This indicated that Kali Linux could not resolve the domain name.

### Troubleshooting Steps

To investigate the issue, I checked the network interfaces:

```bash
ip a
```

I then inspected the routing table:

```bash
ip route
```

I also restarted NetworkManager:

```bash
sudo systemctl restart NetworkManager
```

And attempted to reconnect the Ethernet interface:

```bash
sudo nmcli device connect eth0
```

These commands helped me identify whether the network interface was active, whether an IP address was assigned, and whether Kali Linux had a usable network connection.

### Changing the VirtualBox Network Mode

Because the original NAT Network configuration was not providing internet access, I changed the virtual machine's network mode for troubleshooting.

1. Shut down Kali Linux.
2. Opened VirtualBox Manager.
3. Navigated to **Settings → Network → Adapter 1**.
4. Changed the network mode from **NAT Network** to **NAT**.
5. Confirmed that the virtual cable was connected.
6. Saved the settings and restarted Kali Linux.

This allowed VirtualBox to use its standard NAT configuration rather than the separately configured NAT Network.

## 5. Successfully Establishing Internet Connectivity

After changing the network configuration and restarting Kali Linux, I verified that the virtual machine had obtained a working IP address.

I ran:

```bash
ip route
```

The output showed:

```text
default via 10.0.2.2 dev eth0
10.0.2.0/24 dev eth0
```

The Kali Linux virtual machine was using the IPv4 address:

```text
10.0.2.15
```

This confirmed that the virtual machine was connected to VirtualBox's default NAT network.

### Testing External Connectivity

I ran:

```bash
ping -c 4 8.8.8.8
```

The results showed:

```text
4 packets transmitted, 4 received, 0% packet loss
```

Next, I tested DNS resolution:

```bash
ping -c 4 google.com
```

The results also showed:

```text
4 packets transmitted, 4 received, 0% packet loss
```

Both tests were successful.

These results confirmed that Kali Linux could communicate with external IP addresses and successfully resolve domain names.

**Result:** Internet connectivity was successfully established using VirtualBox's standard NAT mode.

## 6. Updating Kali Linux

After establishing internet connectivity, the next step was to update the Kali Linux operating system.

Keeping Kali Linux updated is important because updates provide security patches, bug fixes, software improvements, and newer versions of cybersecurity tools.

### Step 1: Update the Package Repository

I opened Terminal and entered:

```bash
sudo apt update
```

This command retrieves the latest package information from Kali Linux's configured software repositories.

### Step 2: Upgrade Installed Packages

Next, I ran:

```bash
sudo apt full-upgrade -y
```

This command upgrades installed packages and allows package dependency changes when necessary.

The `-y` option automatically accepts applicable confirmation prompts.

### Step 3: Restart Kali Linux

After the update process, I used:

```bash
sudo reboot
```

Restarting the virtual machine allows system updates that require a reboot to take effect.

### Step 4: Verify System Information

To check the operating system information, I used:

```bash
cat /etc/os-release
```

To check the Linux kernel version, I used:

```bash
uname -r
```

These commands provide information about the installed Kali Linux operating system and kernel.

## 7. Commands Used During the Lab

| Command | Purpose |
|---|---|
| `ip a` | Display network interfaces and IP addresses |
| `ip -4 addr show eth0` | Display the IPv4 address of eth0 |
| `ip route` | Display the routing table and default gateway |
| `nmcli device status` | Check network device status |
| `nmcli connection show` | Display NetworkManager connections |
| `sudo systemctl restart NetworkManager` | Restart the network management service |
| `sudo nmcli device connect eth0` | Connect the Ethernet interface |
| `ping -c 4 8.8.8.8` | Test external IP connectivity |
| `ping -c 4 google.com` | Test internet connectivity and DNS |
| `sudo apt update` | Refresh package repository information |
| `sudo apt full-upgrade -y` | Upgrade installed software packages |
| `sudo reboot` | Restart Kali Linux |
| `cat /etc/os-release` | Display operating system information |
| `uname -r` | Display the Linux kernel version |

## 8. Lab Results

Through this lab, I successfully installed Oracle VirtualBox and deployed a Kali Linux virtual machine on my Intel-based MacBook Pro.

I configured a NAT Network using the `10.0.0.0/24` subnet and examined the virtual machine's network interface and IP addressing.

During testing, I encountered connectivity problems involving IP configuration, routing, and DNS resolution. To troubleshoot these issues, I inspected the network interfaces, restarted NetworkManager, and adjusted the VirtualBox network configuration.

After switching to standard NAT mode, I successfully established internet connectivity and confirmed that Kali Linux could reach external IP addresses and resolve domain names.

I also documented the commands used to update Kali Linux and verify its operating system information.

## 9. Conclusion

This lab provided practical experience with virtualization, Linux installation, virtual network configuration, IP addressing, and network troubleshooting.

I learned how to configure network adapters in VirtualBox, examine Linux network interfaces, troubleshoot connectivity problems, and verify successful internet access using command-line tools.

The lab also demonstrated the differences between VirtualBox's NAT Network and standard NAT configurations.

The resulting Kali Linux virtual machine provides a foundation for future cybersecurity labs involving network analysis, security testing, Linux administration, and cybersecurity tools.

## References

- Oracle VirtualBox: https://www.virtualbox.org/
- Kali Linux: https://www.kali.org/
- Kali Linux Documentation: https://www.kali.org/docs/
- VirtualBox Documentation: https://www.virtualbox.org/manual/
