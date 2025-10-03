# Advanced Cybersecurity Home Lab with Suricata & Wazuh

![Cybersecurity](https://img.shields.io/badge/Domain-Cybersecurity-blue) ![NIDS](https://img.shields.io/badge/Technology-Suricata-orange) ![SIEM](https://img.shields.io/badge/Technology-Wazuh-brightgreen)

This repository documents the architecture and function of a multi-layered cybersecurity home lab. The environment is designed to simulate, detect, and analyze cyber threats using a combination of a Network Intrusion Detection System (**NIDS**) and a Host-based Intrusion Detection System (**HIDS**) integrated with a centralized Security Information and Event Management (**SIEM**) platform. 🛡️

---

## Lab Architecture Diagram

The diagram below illustrates the flow of attack traffic and security data within the virtualized network.

```mermaid
graph TD
    subgraph "VirtualBox Host-Only Network"
        direction LR
        
        subgraph "Monitoring & SIEM"
            direction TB
            sensor["<div style='font-weight:bold'>Ubuntu Sensor</div>- Suricata (NIDS)<br>- Wazuh Agent (HIDS)"]
            wazuh_server["<div style='font-weight:bold'>Wazuh Server</div>(SIEM)"]
            sensor -.->|"Log & Event Data"| wazuh_server
        end

        subgraph "Target Systems"
            direction TB
            victim1[Metasploitable 2]
            victim2[DVWA Server]
        end

    end

    attacker["<div style='font-weight:bold'>Kali Linux</div>(Attacker)"]

    attacker -- "Simulated Attack Traffic" --> sensor
    attacker -- " " --> victim1
    attacker -- " " --> victim2

    classDef default fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef highlight fill:#e6f2ff,stroke:#005cc5,stroke-width:2px;

    class attacker highlight;
```

---

## Core Components

* **Kali Linux (Attacker):** An offensive security distribution used to launch simulated attacks, including network reconnaissance, vulnerability scanning, and exploitation.
* **Ubuntu Server (Sensor):** The network's watchdog, this server is configured in promiscuous mode to monitor all traffic.
    * **Suricata (NIDS):** Inspects network packets in real-time, using signature-based rules to detect known threats, scans, and malicious payloads.
    * **Wazuh Agent (HIDS):** Monitors the sensor host itself, collecting logs, checking for rootkits, and monitoring file integrity.
* **Wazuh Server (SIEM):** The central brain of the lab. It ingests, analyzes, and correlates security data from the Wazuh agent, providing a unified dashboard for alert triage and incident investigation.
* **Target Systems (Victims):** Purposefully vulnerable servers that act as the targets for simulated attacks.
    * **Metasploitable 2:** A well-known vulnerable Linux VM designed for security training.
    * **DVWA Server:** A server hosting the Damn Vulnerable Web Application for practicing web-based exploits.

---

## Technologies Used

* **Virtualization:** Oracle VirtualBox
* **NIDS Engine:** Suricata
* **HIDS & SIEM:** Wazuh
* **Sensor OS:** Ubuntu Server
* **Attacker OS:** Kali Linux
* **Vulnerable Targets:** Metasploitable 2, DVWA
* **Pentesting Tools:** Nmap, Metasploit Framework

---

## Attack Scenarios & Detection

The following are examples of attacks performed in the lab to test its detection capabilities.

### Scenario 1: Network Reconnaissance 📡

A TCP SYN "stealth" scan was launched from the Kali machine to identify open ports on the Metasploitable 2 server.
![Nmap Scan Alert](images/nmap-alert.png)

* **Detection:** Suricata successfully flagged the scan, triggering the **"ET SCAN NMAP OS Detection Probe"** alert, which was forwarded to the Wazuh server for analysis.

### Scenario 2: Web Server Exploitation 💥

A known command injection vulnerability in the Damn Vulnerable Web Application (DVWA) was exploited using tools on the Kali machine to gain shell access.

![Exploit Alert](images/exploit-alert.png)

* **Detection:** The malicious HTTP POST request containing the payload was identified by Suricata's rule set, generating a high-severity alert for a web-based attack.

---

## Skills Demonstrated

This project provides hands-on experience with tools and procedures used daily in a Security Operations Center (SOC).

* **Security Tool Integration:** Deployed and configured Suricata (NIDS) and Wazuh (HIDS/SIEM) to create a multi-layered defense system.
* **Threat Detection & Analysis:** Utilized Suricata for signature-based network threat detection and Wazuh for host-level log analysis and threat intelligence.
* **Incident Simulation:** Executed penetration testing techniques (reconnaissance, exploitation) to generate realistic security alerts and test defensive postures.
* **SIEM Operations:** Gained practical experience using a SIEM dashboard to investigate, correlate, and triage security alerts from disparate sources.
