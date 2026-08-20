# CyberDefenders-Web-Investigation-Writeup

[![Challenge Link](https://shields.io)](https://cyberdefenders.org)
[![Category: Network Forensics](https://shields.io)](#)
[![Difficulty: Medium](https://shields.io)](#)

A comprehensive walkthrough and analysis of the **Web Investigation** network forensics challenge from CyberDefenders. This repository details the step-by-step investigation of a database compromise targeting an online bookstore.

## 📖 Scenario Overview
An automated alert triggered an alarm at *BookWorld* due to an unusual spike in database queries and server resource usage. As the lead SOC analyst, this investigation analyzes the captured network traffic (`.pcap`) to uncover the attack vector, identify the scope of the data breach, trace user compromise, and determine how the attacker achieved persistence.

## 🛠️ Tools & Skills Demonstrated
* **Network Analysis:** Wireshark, NetworkMiner
* **Data Decoding:** CyberChef (URL & Hex Decoding)
* **Threat Hunting Tactics:** Initial Access tracking, Directory Enumeration detection, SQL Injection (SQLi) payload analysis, Web Shell identification.

## 📁 Repository Structure
* 📄 `writeup.md` — The complete step-by-step breakdown of all 9 investigation questions with methodology and logical reasoning.
* 📦 `network-traffic/` *(Optional)* — Notes or placeholder references to the PCAP file.

---

## 🚀 View the Full Walkthrough

The detailed answers and deep-dive technical methodology can be found directly in the writeup file:

👉 **[Read the Full Investigation Writeup Here (writeup.md)](./writeup.md)** 👈

---

## 🔍 Key Findings Summary
1. **Attacker Location:** The attack originated from an IP geolocated in Shijiazhuang, executing rapid automated scans.
2. **Vulnerability Exploited:** A severe SQL Injection flaw within the `search.php` script allowed total database enumeration.
3. **Data Exfiltrated:** Customer directories and structural tables (`customers`) were completely mapped and targeted.
4. **Persistence Achieved:** The attacker successfully logged into `/admin/` via brute-forced credentials and uploaded a malicious PHP web shell (`NVri2vhp.php`).
