# 🛡️ End-to-End SOC Analysis & Active Incident Response Pipeline Lab
[![Splunk](https://shields.io)](https://www.splunk.com)
[![OS](https://shields.io)](https://ubuntu.com)
[![Virtualization](https://shields.io)](https://virtualbox.org)
[![Framework](https://shields.io)](https://attack.mitre.org)
[![NIST](https://shields.io)](https://csrc.nist.gov)

## 📋 Executive Summary
This repository documents the architectural deployment and live execution of a **NIST SP 800-61 r2-aligned Incident Response lifecycle**. The simulation demonstrates a Tier 1/2 SOC Analyst workflow triaging, investigating, and autonomously containing a phishing-driven corporate VPN credential compromise.

*   **The Scenario:** A threat actor harvested valid active directory credentials of user `Zez.Boy` via a deceptive, multi-redirect lookalike PayPal phishing link. The adversary then attempted persistent remote entry via a perimeter OpenVPN gateway.
*   **The Mitigation:** The attack was completely neutralized at the MFA barrier. Security automation configurations dynamically identified the threat across a 24-hour findtime window, dropping the network session and enforcing an active firewall ban against the attacker's IP (`68.125.245.23`) after 3 failed OTP challenges.

---

## 💻 Lab Environment Setup & Virtualization
To ensure realistic log generation and threat behavior replication, the entire target environment was isolated inside a hypervisor structure using **Oracle VM VirtualBox**:

### 1. Guest Operating System Config
*   **OS Platform:** Ubuntu Linux 64-bit Server
*   **Security Context:** Hardened kernel instance serving as the primary corporate network resource plane. Holds the destination paths for Splunk, OpenVPN endpoints, and the security configuration files (`/etc/fstab` storage parameters).

### 2. Dual-Adapter Network Topology Engineering
To simulate external attack vectors crossing corporate boundaries into a target network environment, a segregated dual-homed networking landscape was provisioned in VirtualBox:
*   **Adapter 1 (NAT):** Configured to route external network egress, enabling updates, patch management, and outbound Splunk registration tracks.
*   **Adapter 2 (Host-Only Interface - `enp0s8`):** Serves as the private host-to-guest perimeter interface corridor. It maps the local virtual interface pool (`192.168.56.X`), forcing the Windows host PC to act as an unmanaged external threat node attempting boundary penetration.

---

## 🛠️ Infrastructure Architecture & Engineering Components
Instead of just analyzing data, this lab showcases full blue-team engineering. The underlying topology was custom-built inside the sandboxed Linux environment using:
*   **SIEM Analytics Core:** Splunk Enterprise & Splunk Universal Forwarder pipeline tracking data paths (`inputs.conf`).
*   **Network Infrastructure:** OpenVPN Server running over a hardened dual-adapter virtual network topology (NAT + Host-Only).
*   **Identity & MFA Layer:** Pluggable Authentication Modules (PAM) chained with a Google Authenticator TOTP dynamic core.
*   **Intrusion Mitigation Engine:** Fail2Ban daemon configured with target regular expression match definitions (`failregex`).

---

## 🗺️ MITRE ATT&CK® Technique Mapping

| Lifecycle Phase | MITRE ID | Technique Name | Lab Simulation Context / Artifacts |
| :--- | :--- | :--- | :--- |
| **Initial Access** | **T1566.002** | Phishing: Malicious Link | Spoofed PayPal email containing credential harvesting URL sent from IP `185.31.27.181`. |
| **Execution** | **T1204.001** | User Execution: Malicious Link | Target user `Zez.Boy` fell victim to the lure and clicked the phishing link. |
| **Credential Access** | **T1539** | Steal Web Session Credentials | Attacker captured valid credentials via a rogue redirection site (`52489602342096554.sbs`). |
| **Defense Evasion** | **T1564** | Hide Artifacts (URL Redirection) | Attacker routed traffic through a multi-redirect chain in Poland to obscure infrastructure. |
| **Persistence** | **T1133** | External Remote Services | Attacker attempted to establish remote access via OpenVPN gateway (`UZEZI-VPN-SRV`). |
| **Lateral Movement** | **T1078.002** | Valid Accounts: Domain Accounts | Attacker successfully bypassed primary authentication using stolen password. |
| **Defensive Action** | **T1531** | Account Access Removal | OpenVPN/Fail2Ban triggered a Security Lockout Rule, severing the connection. |

---

## 🛠️ Advanced Engineering: Troubleshooting & Diagnostics
Recruiters look for analysts who can debug under pressure. The project involved solving two critical silent failure points during installation:

### 1. The Local PAM Test Defect
*   **The Issue:** Running `sudo login zez.boy` completely skipped the Google Authenticator 2FA challenge, suggesting broken system rules.
*   **The Discovery:** The default `login` binary explicitly invokes `/etc/pam.d/login` rules, ignoring the newly engineered custom OpenVPN security stack.
*   **The Resolution:** Installed and configured the lightweight `pamtester` micro-utility to safely isolate and validate the rule-file directly:
    ```bash
    sudo pamtester openvpn zez.boy authenticate
    ```
    This verified 100% execution integrity without modifying global OS login screens.

### 2. Silent Fail2Ban Match Failures
*   **The Issue:** The test harness execution command `fail2ban-regex` initially reported `Matches: 0` despite valid OpenVPN error streams.
*   **The Discovery:** A syntax parameter typo (`failregrex` instead of `failregex`) caused the engine to silently ignore regex logic.
*   **The Resolution:** Corrected configuration files in `/etc/fail2ban/filter.d/openvpn-mfa.conf`, enabling instantaneous active network banning.

---

## 🚀 Navigation & Blueprint
*   👉 **[Click Here to Read the Complete Incident Response Playbook (PLAYBOOK.md)](./PLAYBOOK.md)** — Step-by-step SOC execution with full Splunk queries and screenshots.
*   👉 **[Explore Investigation Screenshots Folder](./screenshots/)** — Direct path to evidence images.
