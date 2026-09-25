<<<<<<< HEAD
# SOC homelab — lab 1: SOC monitoring foundation

Personal homelab project built to demonstrate CCNA and Security+ level skills for a SOC analyst role. This is lab 1 of a planned 3-lab portfolio (see roadmap below).

## Goal

Build a small, isolated virtual network with a Windows endpoint, a SIEM (Wazuh), and a Kali attacker VM. Simulate real attack techniques against the endpoint and detect/triage them in the SIEM dashboard.

## Environment

- Host: Windows 11, 16 GB RAM, Oracle VirtualBox 7.x
- Network: VirtualBox NAT Network (isolated from the home LAN, internet access via NAT for updates)
- SIEM: Wazuh v4.14.8 (official OVA appliance)
- Endpoint: Windows 11 Enterprise (90-day evaluation), Guest Additions
- Attacker: Kali Linux (planned)

## Progress log

**Stage 1 — VirtualBox installed**
Installed VirtualBox + Extension Pack on the host, confirmed CPU virtualization support.

**Stage 2 — Isolated lab network created**
Created a VirtualBox NAT Network (`10.0.2.0/24`) so lab VMs can reach each other and the internet without being exposed to the home network.

**Stage 3 — Wazuh manager deployed**
Imported the official Wazuh 4.14.8 OVA, scaled down to 2 CPU / 4 GB RAM for lab use, attached to the NAT Network.

- *Issue:* Dashboard was unreachable from the host browser at first.
  - *Cause 1:* Used the subnet's broadcast address (`10.0.2.255`) instead of the manager's actual address (`10.0.2.3`, found via `ip a`).
  - *Cause 2:* A VirtualBox NAT Network isolates the host from the guest VMs by design (similar to being outside a router) — the host can't dial in without port forwarding. Verified dashboard access from inside the Windows endpoint instead, since guest-to-guest traffic on the NAT Network works without extra configuration.
- *Issue:* Default `admin` password couldn't be changed via the dashboard's self-service "forgot password" flow (`status: admin is reserved`) — that flow only applies to custom users, not Wazuh's built-in accounts.
  - *Fix:* Changed it from the manager's console using the bundled CLI tool:
    `sudo bash /usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh -u admin -p '<new-password>'`

**Stage 4 — Windows 11 endpoint (in progress)**
Downloaded the Windows 11 Enterprise evaluation ISO from the Microsoft Evaluation Center, created the VM (2 CPU / 4 GB RAM / 60 GB dynamic disk), enabled EFI + TPM 2.0 emulation in VirtualBox to satisfy Windows 11's install requirements, installed with a local (offline) account, attached to the NAT Network.

- *Issue:* Guest Additions installer didn't appear to do anything after Devices → Insert Guest Additions CD image.
  - *Cause:* The virtual optical drive still held the original Windows install ISO — the swap hadn't actually taken effect.
  - *Fix:* Re-triggered the swap and force-unmounted the locked medium; confirmed via File Explorer that the CD drive's label changed before running `VBoxWindowsAdditions.exe` as administrator.

## Next steps

- Finish Guest Additions install and reboot the endpoint.
- Install Sysmon (SwiftOnSecurity config) + the Wazuh agent on the endpoint, confirm logs flow into the dashboard.
- Set up the Kali Linux attacker VM.
- Run first attack simulation (Atomic Red Team) and document detection in Wazuh, mapped to MITRE ATT&CK.

## Roadmap

This lab is the foundation for a 3-lab portfolio:

1. **SOC monitoring foundation** (this lab) — Wazuh + Sysmon + simulated attacks
2. **Network defense layer** — pfSense/OPNsense, VLAN segmentation, Suricata IDS/IPS feeding into this SIEM
3. **Incident response & threat hunting** — multi-stage simulated breach, full IR report
=======
# soc-homelab1
A homelab showcasing CCNA and Sec+ knowledge
>>>>>>> e5629d307f2526844b6a8fe01fb2cba0d5e4b500
