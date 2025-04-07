# pfsense-remote-deployment
# Remote Firewall Deployment with pfSense Using Ubuntu Server, KVM, and Tailscale

## Project Overview

This project involved remotely deploying and configuring an enterprise-grade firewall (pfSense) on a headless Ubuntu server without physical access to the machine.

Secure access was established using Tailscale VPN. A virtualization environment was installed on Ubuntu Server using KVM and QEMU, and pfSense was deployed inside a virtual machine. Installation and configuration were performed over a VNC connection, and access to the pfSense WebGUI was secured through an SSH tunnel. The objective was to create a fully operational, remotely managed firewall lab setup, simulating real-world infrastructure deployment scenarios.

---

## Tools and Technologies Used

- **Ubuntu Server 24.04** – Host operating system
- **Tailscale VPN** – Secure remote access
- **KVM/QEMU** – Virtualization platform
- **pfSense 2.7.2** – Firewall and router appliance
- **VNC Viewer** – Remote graphical console access
- **SSH** – Secure command-line access and tunneling

---

## Project Steps

### 1. Dual Boot Setup

Installed Ubuntu Server 24.04 alongside Windows in a dual boot configuration, preparing the server for headless operation and remote management.

### 2. Secure Remote Access with Tailscale

Configured Tailscale VPN on the server to establish a secure and encrypted private network. Enabled MagicDNS to allow name-based access instead of relying on dynamic IP addresses.

### 3. Preparing the Virtualization Environment

Installed KVM, QEMU, and related management tools. Verified hardware virtualization support and set up default NAT networking (via `virbr0`) for virtual machine communication.

### 4. Deploying pfSense in a Virtual Machine

Downloaded the official pfSense ISO installer. Created a new virtual machine with:
- 2 vCPUs
- 2 GB RAM
- 8 GB disk
- Network attached to the NAT bridge

Configured the VM to use VNC for remote installation access.

### 5. Installing and Configuring pfSense

Completed the pfSense installation over a VNC session. Key initial setup tasks included:
- Setting the WAN interface to obtain an IP address via DHCP
- Setting Cloudflare (1.1.1.1) and Google (8.8.8.8) as DNS servers
- Configuring system timezone
- Changing the default admin password

### 6. Secure Access to pfSense WebGUI

Created an SSH tunnel to securely access the pfSense WebGUI without exposing management interfaces to the public internet:

```bash
ssh -L 8443:192.168.122.180:443 user@my-server-name
```

Accessed the pfSense WebGUI locally at `https://localhost:8443`.

### 7. Verifying Firewall Status

Confirmed that the pfSense firewall was active and running by checking firewall status:

```bash
pfctl -s info
```

---

## Repository Structure

```
pfsense-remote-deployment/
├── README.md
├── commands.txt
├── screenshots/
│   ├── tailscale-dashboard.png
│   ├── vnc-pfsense-installation.png
│   ├── pfsense-console.png
│   ├── pfsense-webgui-login.png
│   └── pfsense-dashboard.png
└── network_diagram.png
```

- **README.md** – Main project documentation
- **commands.txt** – Important terminal commands used during setup
- **screenshots/** – Visuals documenting major steps
- **network_diagram.png** – Visual overview of network setup

---

## Challenges Encountered

### Missing Serial ISO

The official pfSense serial console installer was only available as a `.img` file, not an `.iso`, which led to boot errors when attempting to install on a headless server. This was resolved by downloading the standard graphical ISO and connecting via VNC.

### Permission Denied for ISO Access

Initially, the virtualization system (libvirt/QEMU) was unable to access the ISO file due to user permission issues. Adjusted permissions and ensured the ISO was placed in an accessible location for the hypervisor.

### Temporary Firewall Disablement

During initial configuration, the pfSense firewall was temporarily disabled to allow WebGUI access. Verified that pfSense automatically re-enabled the firewall service after reboot to maintain security.

### SSH Tunnel for WebGUI Access

To avoid exposing pfSense's administrative interface directly to the internet, a secure SSH tunnel was created, allowing safe and encrypted access to the WebGUI from the local machine.

---

## Key Skills Demonstrated

- Secure remote server management over VPN
- Virtual machine deployment and headless server operations
- Firewall installation, configuration, and verification
- SSH tunneling and port forwarding
- Basic network configuration and troubleshooting

---
