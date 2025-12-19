# 🛡️ Azure Honeypot & Hybrid SOC (Wazuh)

![Status](https://img.shields.io/badge/Status-Completed-success) ![Platform](https://img.shields.io/badge/Platform-Azure%20%7C%20Linux-blue) ![Tools](https://img.shields.io/badge/Tools-Wazuh%20%7C%20Docker%20%7C%20Tailscale-green)

## 📖 Project Overview

In this project, I engineered a cloud-based **Honeypot** to attract real-world cyberattacks and monitor them in real-time using a **SIEM (Security Information and Event Management)** system. 

Unlike standard cloud deployments, this project utilizes a **Hybrid Architecture**:
1.  **The Honeypot:** An Azure Ubuntu VM exposed to the internet.
2.  **The SOC:** A Wazuh Manager running locally on my private network (Mac/Docker).
3.  **The Link:** A secure, encrypted mesh VPN (Tailscale) connecting the two without exposing management ports.

**Objective:** To analyze SSH Brute Force patterns, understand attacker behavior, and map attacks to the **MITRE ATT&CK** framework.

---

## 🏗️ Network Architecture

The architecture consists of an intentionally vulnerable cloud instance forwarding logs securely to a local analyst machine via a zero-trust tunnel.

```mermaid
graph TD
    %% Nodes
    Attacker((🛑 ATTACKER))
    
    subgraph Azure_Cloud [☁️ Microsoft Azure Cloud]
        NSG{🛡️ NSG Firewall}
        VM[🖥️ Ubuntu Honeypot VM]
    end
    
    subgraph Home_Lab [🏠 Local Network / SOC]
        Manager[📊 Wazuh SIEM Manager]
    end

    %% Connections
    Attacker -- "SSH Brute Force (Port 22)" --> NSG
    NSG -- "Allowed Traffic" --> VM
    VM == "Encrypted Log Tunnel (Tailscale)" ==> Manager

    %% Styling
    style Attacker fill:#ff4d4d,stroke:#333,stroke-width:4px,color:white
    style VM fill:#0078D4,stroke:#333,stroke-width:2px,color:white
    style Manager fill:#00cc66,stroke:#333,stroke-width:2px,color:white
    style NSG fill:#ffcc00,stroke:#333,stroke-width:2px
