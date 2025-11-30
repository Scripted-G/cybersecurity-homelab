# Cybersecurity Homelab Setup

## Overview

This document outlines the setup of my cybersecurity homelab environment using KVM/QEMU virtualization on Arch Linux. The lab provides a safe, isolated environment for practicing penetration testing, system administration, and security operations.

## Host System

| Component | Details |
|-----------|---------|
| OS | Arch Linux |
| Virtualization | KVM/QEMU with virt-manager |
| CPU | AMD Ryzen 9 9900X |
| RAM | 32 GB |

## Virtual Machines

### Kali Linux
Primary penetration testing platform with security tools.

| Resource | Allocation |
|----------|------------|
| RAM | 4 GB |
| CPUs | 4 |
| Disk | 60 GB |
| Network | NAT (default) |

### Metasploitable 2
Intentionally vulnerable target system for exploitation practice.

| Resource | Allocation |
|----------|------------|
| RAM | 512 MB |
| CPUs | 1 |
| Disk | ~8 GB (pre-configured image) |
| Network | NAT (default) |
| OS | Ubuntu 8.04 (intentionally outdated) |

### Windows Server 2022
Domain controller and server administration practice.

| Resource | Allocation |
|----------|------------|
| RAM | 4 GB |
| CPUs | 2 |
| Disk | 50 GB |
| Network | NAT (default) |

### Windows 11 Pro
Windows client for domain joining and endpoint testing.

| Resource | Allocation |
|----------|------------|
| RAM | 4 GB |
| CPUs | 4 |
| Disk | 128 GB |
| Network | NAT (default) |

## Network Configuration

All VMs are connected to the default NAT network (`virbr0`) which provides:

- Isolation from the host's physical network
- Internet access for updates and tool downloads
- VM-to-VM communication on the same subnet (192.168.122.0/24)

## Setup Instructions

### Kali Linux

1. Download the official Kali Linux ISO from [kali.org](https://www.kali.org/get-kali/)
2. Open virt-manager and create a new VM
3. Select the downloaded ISO as installation media
4. Allocate resources (4 GB RAM, 4 CPUs, 60 GB disk)
5. Complete the standard Kali installation

### Metasploitable 2

Metasploitable 2 is distributed as a VMware image and needs to be converted for KVM.

**Download and convert:**

```bash
# Download Metasploitable 2
wget https://sourceforge.net/projects/metasploitable/files/Metasploitable2/metasploitable-linux-2.0.0.zip

# Extract the archive
unzip metasploitable-linux-2.0.0.zip
cd Metasploitable2-Linux

# Convert VMware disk to KVM format
qemu-img convert -f vmdk -O qcow2 Metasploitable.vmdk metasploitable2.qcow2

# Move to libvirt images directory
sudo mv metasploitable2.qcow2 /var/lib/libvirt/images/
```

**Create the VM:**

1. Open virt-manager
2. Select "Import existing disk image"
3. Browse to `/var/lib/libvirt/images/metasploitable2.qcow2`
4. Set OS type to "Ubuntu 8.04"
5. Allocate minimal resources (512 MB RAM, 1 CPU)
6. Connect to the default NAT network

**Default credentials:** `msfadmin` / `msfadmin`

### Windows Server 2022

1. Download the Windows Server 2022 ISO from Microsoft Evaluation Center
2. Open virt-manager and create a new VM
3. Select the downloaded ISO as installation media
4. Allocate resources (4 GB RAM, 2 CPUs, 50 GB disk)
5. Complete the Windows Server installation
6. Install VirtIO drivers for optimal performance (optional but recommended)

### Windows 11 Pro

1. Download the Windows 11 ISO from Microsoft
2. Open virt-manager and create a new VM
3. Select the downloaded ISO as installation media
4. Allocate resources (4 GB RAM, 4 CPUs, 128 GB disk)
5. Enable TPM 2.0 and Secure Boot in VM settings (required for Windows 11)
6. Complete the Windows 11 installation
7. Install VirtIO drivers for optimal performance (optional but recommended)

## SSH Access to Metasploitable 2

Metasploitable 2 uses older SSH algorithms that modern clients reject by default.

**One-time connection:**

```bash
ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa msfadmin@192.168.122.151
```

**Permanent configuration:**

Add the following to `~/.ssh/config`:

```
Host metasploitable
    HostName 192.168.122.151
    User msfadmin
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedKeyTypes +ssh-rsa
```

Then connect with:

```bash
ssh metasploitable
# Password: msfadmin
```

## Security Considerations

- **Isolation**: VMs are on an isolated NAT network, separate from the host's physical network
- **No Updates**: Metasploitable 2 is intentionally NOT updated to preserve vulnerabilities
- **Snapshots**: Take VM snapshots before major changes for easy recovery
- **Legal**: Only test on systems you own or have explicit permission to test

## Resources

- [Metasploitable 2 Download](https://sourceforge.net/projects/metasploitable/)
- [Kali Linux Documentation](https://www.kali.org/docs/)
- [Rapid7 Metasploitable Guide](https://docs.rapid7.com/metasploit/metasploitable-2/)
- [Windows Server Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)

---

*Last Updated: November 2025*
