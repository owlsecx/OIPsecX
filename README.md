# 🦉 OIPsecX

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux%20%2F%20Windows-informational?style=flat-square&logo=linux&logoColor=white&color=0a0c10"/>
  <img src="https://img.shields.io/badge/Category-ONetwork%20%2F%20VPN%20Security-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Protocol-IKEv1%20%2F%20IKEv2-cyan?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-2.0-cyan?style=flat-square"/>
</p>

> **OIPsecX** is an IKEv1/IKEv2 IPsec VPN scanner — discover VPN endpoints, fingerprint vendors, enumerate transform proposals, and optionally run Aggressive Mode to retrieve PSK identity data, for authorised network security audits.

---

> ⚠️ **AUTHORISED USE ONLY** — Use only on networks and systems you own or have explicit written permission to test. Unauthorised scanning is illegal. Aggressive Mode is detectable by IDS.

---

## 📌 Overview

OIPsecX sends crafted IKE packets over UDP to identify IPsec VPN endpoints, determine the IKE version, fingerprint the vendor from Vendor ID payloads, and enumerate supported encryption and authentication transform proposals. Aggressive Mode optionally elicits PSK identity and hash data from the server.

---

## 🖥️ Menu

| # | Module | Description |
|---|--------|-------------|
| **[1] Quick Scan** | Single host or IP — main mode probe only |
| **[2] Network Scan** | CIDR range or IP range — multi-threaded |
| **[3] Aggressive Scan** | Main mode + Aggressive mode — elicits PSK hash and Vendor ID |
| **[4] Scan From File** | Load targets from a `.txt` file (one per line, `#` lines skipped) |
| **[5] History** | View the last 25 scans from `~/.oipsecx_history.json` |

---

## 🎯 Target Formats

| Format | Example |
|--------|---------|
| Single IP | `192.168.1.1` |
| CIDR range | `192.168.1.0/24` |
| IP range | `192.168.1.1-254` |
| File | `targets.txt` (one target per line) |

---

## 🔍 Scan Modes

### Main Mode (default)
Sends an IKEv1 Main Mode SA packet and waits for a response. Determines if the host is running an IPsec VPN service, identifies the IKE version (v1 or v2), and parses returned transform proposals and Vendor ID payloads.

### Aggressive Mode
Sends an IKEv1 Aggressive Mode packet with a configurable identity string. If the server supports Aggressive Mode, it responds with its Vendor ID and may return a PSK hash — logged for the analyst to review.

> ⚠️ Aggressive Mode generates louder traffic and is commonly detected by IDS/IPS systems.

---

## 🏷️ Vendor Fingerprinting

OIPsecX identifies 15+ VPN implementations from Vendor ID payloads in the response:

| Vendor | Products Detected |
|--------|------------------|
| **Cisco** | VPN Concentrator, ASA/PIX, Unity, VPN 3000 |
| **Check Point** | Check Point, NGX |
| **Microsoft** | L2TP/IPsec, Windows 2000 VPN, Windows Server 2003 |
| **Nortel** | Contivity |
| **Netscreen** | Netscreen |
| **F5** | FirePass |
| **Zyxel** | ZyWALL |
| **Huawei** | Huawei VPN |
| **SSH** | SSH Sentinel |

---

## 🔧 Transform Enumeration

Per discovered host, OIPsecX parses and reports accepted transform proposals:

| Attribute | Supported Values |
|-----------|-----------------|
| **Encryption** | DES, 3DES, AES-128/192/256, Blowfish, CAST, Camellia |
| **Hash / PRF** | MD5, SHA-1, SHA-256, SHA-384, SHA-512 |
| **Auth Method** | PSK, RSA, DSS, ECDSA, ECDSA-SHA256/384/512, NULL |
| **DH Group** | MODP 768–8192, ECP 256/384/521, Brainpool 256/384/512 |

---

## ⚙️ Scan Options

| Option | Default | Description |
|--------|---------|-------------|
| Port | `500` | UDP port (IKE standard) |
| Timeout | `2.0s` | Per-host response timeout |
| Threads | `10` | Concurrent scan threads |
| Retries | `2` | Packet retransmissions per host |
| Aggressive | Off | Enable Aggressive Mode |
| ID data | `user` | Identity string sent in Aggressive Mode |
| Verbose | Off | Show per-packet errors |
| Output | — | Save results to JSON file |

---

## 📤 Output

Each scan produces:
- Colour-coded per-host results: IKE version, vendor, aggressive support, PSK ID, hash
- Final summary: packets sent/received, total targets, IKE servers found
- Optional JSON export: `<filename>.json` with all discovered hosts, vendor IDs, and PSK data
- Scan history appended to `~/.oipsecx_history.json`

---

## ⚙️ Requirements

- **Linux or Windows**
- **No Python installation needed** — runs as a standalone executable

---

## 🚀 Usage

```bash
./OIPsecX
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
