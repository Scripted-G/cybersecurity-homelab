Cybersecurity Homelab Setup
Overview
This document outlines the setup of my cybersecurity homelab environment using KVM/QEMU virtualization on Linux. The lab consists of two virtual machines: Kali Linux (attacker) and Metasploitable 2 (vulnerable target), configured for hands-on penetration testing practice.
Environment Details
Host System:

OS: Arch Linux
Virtualization: KVM/QEMU with virt-manager
CPU: AMD Ryzen 9 9900X
RAM: 32 GB

Virtual Machines:

Kali Linux - Primary penetration testing platform

RAM: 4GB
CPUs: 4
Disk: 60GB
Network: NAT (default)


Metasploitable 2 - Intentionally vulnerable target system

RAM: 512MB
CPUs: 1
Disk: ~8GB (pre-configured image)
Network: NAT (default)
OS: Ubuntu 8.04 (intentionally outdated)

Windows Server 2022 - Domain controller and server administration practice

RAM: 4GB
CPUs: 2
Disk: 50GB
Network: NAT (default)

Windows 11 Pro - Windows client for domain joining and endpoint testing

RAM: 4GB
CPUs: 4
Disk: 128GB
Network: NAT (default)

Setup Process

1. Kali Linux Installation
Downloaded the official Kali Linux ISO from kali.org and created a new VM in virt-manager with standard installation settings.

2. Metasploitable 2 Setup
Download and Conversion:
bash# Download Metasploitable 2
wget https://sourceforge.net/projects/metasploitable/files/Metasploitable2/metasploitable-linux-2.0.0.zip

# Extract the archive
unzip metasploitable-linux-2.0.0.zip
cd Metasploitable2-Linux

# Convert VMware disk to KVM format
qemu-img convert -f vmdk -O qcow2 Metasploitable.vmdk metasploitable2.qcow2

# Move to libvirt images directory
sudo mv metasploitable2.qcow2 /var/lib/libvirt/images/

VM Creation:

Used virt-manager to import the existing disk image
Selected "Ubuntu 8.04" as the OS type
Allocated minimal resources (512MB RAM, 1 CPU)
Connected to default NAT network

3. Network Configuration
Both VMs are connected to the default NAT network (virbr0) which provides:

Isolation from the host network
Internet access for the Kali VM (for updates and tool downloads)
VM-to-VM communication on the same subnet

Network verification:
bash# From Metasploitable 2
ip addr show
# IP: 192.168.122.151

# From Kali
ping 192.168.122.151
# Successful response confirms connectivity
4. SSH Access Configuration
Initial Connection:
Metasploitable 2 uses older SSH key algorithms that modern SSH clients reject by default. Initial connection required compatibility flags:
bashssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa msfadmin@192.168.122.151
Permanent Configuration:
Created an SSH config file for easier access:
bashnano ~/.ssh/config
Added the following configuration:
Host metasploitable
    HostName 192.168.122.151
    User msfadmin
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedKeyTypes +ssh-rsa
Now can connect simply with:
bashssh metasploitable
# Password: msfadmin
5. Initial Reconnaissance
Basic port scan from Kali:
bashnmap -sV 192.168.122.151
Confirmed SSH service running on port 22 and multiple other services (to be explored in future exercises).
Security Considerations

Isolation: VMs are on an isolated NAT network, separate from the host's physical network
No Updates: Metasploitable 2 is intentionally NOT updated to preserve vulnerabilities for practice
Credentials: Default credentials (msfadmin/msfadmin) are used as this is a practice environment
Snapshots: VM snapshots should be taken before major exploitation attempts for easy recovery

Next Steps

Complete basic reconnaissance of all services on Metasploitable 2
Practice common vulnerability scanning techniques
Document exploitation attempts and findings
Expand lab with additional vulnerable VMs as skills progress

Resources

Metasploitable 2 Download
Kali Linux Documentation
Rapid7 Metasploitable Guide


Date: November 15, 2025
Status: Lab operational and ready for practice
