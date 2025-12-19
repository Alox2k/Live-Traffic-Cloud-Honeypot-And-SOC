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
graph LR
    %% Definitions of the main nodes
    subgraph Internet [The Internet / Attackers]
        Hacker(Attacker Bots & Scanners)
    end

    subgraph Azure_Cloud [Azure Cloud Platform]
        NSG{NSG Firewall}
        VM[Ubuntu Honeypot VM]
    end

    subgraph Home_Lab [Local SOC / Mac]
        Wazuh[Wazuh SIEM Manager]
    end

    %% Connections and Traffic Flow
    Hacker -- "SSH Brute Force Traffic (Port 22)" --> NSG
    NSG -- "Allowed Traffic" --> VM
    VM -- "Encrypted Log Shipping (Tailscale VPN)" --> Wazuh

    %% Styling for a professional look
    classDef attacker fill:#ffcccb,stroke:#ff0000,stroke-width:2px;
    classDef cloud fill:#cce5ff,stroke:#0066cc,stroke-width:2px;
    classDef soc fill:#d4edda,stroke:#28a745,stroke-width:2px;
    classDef firewall fill:#fff3cd,stroke:#ffc107,stroke-width:2px,stroke-dasharray: 5 5;

    class Hacker attacker;
    class VM,Azure_Cloud cloud;
    class Wazuh,Home_Lab soc;
    class NSG firewall;
