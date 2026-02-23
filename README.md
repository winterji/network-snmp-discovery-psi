# kiv-psi-2

Semestral project for the **Computer Networks (KIV/PSI)** course.

## Overview

This project implements automated **network topology discovery**. It identifies the default gateway via DHCP and then recursively maps the network by querying routers over **SNMP** (Simple Network Management Protocol).

## How It Works

1. **DHCP Discovery** – A DHCP Discover packet is broadcast on the network. The response is captured and the default gateway IP address is extracted from it.
2. **SNMP Routing Table Walk** – Starting from the default gateway, the application queries each router's IP routing table (OID `1.3.6.1.2.1.4.21.1.1`) using SNMP `WALK` to find adjacent network addresses.
3. **SNMP ARP Table Walk** – For each discovered router, the ARP table (OID `1.3.6.1.2.1.4.22`) is queried to list devices on the directly connected subnet.
4. **Recursive Exploration** – The process repeats for every newly discovered router until the entire reachable topology has been mapped.
5. **Topology Output** – The final result is printed as a list of routers with their directly connected neighbours.

## Files

| File | Description |
|------|-------------|
| `test.py` | Main entry point – sends a DHCP Discover, sniffs the reply, and runs the topology discovery. |
| `main.py` | Earlier prototype with basic SNMP GET / routing-table discovery logic. |
| `sniffer.py` | Standalone DHCP sniffer utility. |

## Requirements

- Python 3.9+
- [Scapy](https://scapy.net/) (`pip install scapy`)
- [pysnmp](https://pysnmp.readthedocs.io/) (`pip install pysnmp`)

The application must be run with sufficient privileges to send and capture raw network packets (e.g. `sudo` on Linux).

## Usage

```bash
sudo python test.py
```

The program will:
1. Broadcast a DHCP Discover packet and wait for a reply (up to 2 seconds).
2. Extract the default gateway from the DHCP response.
3. Recursively discover routers and their connected devices via SNMP (the community string is `PSIPUB` by default and can be changed in the source code).
4. Print the resulting network topology to standard output.

## License

This project is licensed under the [MIT License](LICENSE) – Copyright © 2025 Jiří Winter.
