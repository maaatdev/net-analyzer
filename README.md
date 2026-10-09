# net-analyzer

A tiny packet sniffer for the terminal. Point it at an interface, give it a BPF filter, watch the packets roll by.

Built in Python on top of [scapy](https://scapy.net/).

## Features

- Live capture on any interface, with standard BPF filters (`tcp port 443`, `host 10.0.0.1`, …)
- Per-layer decoding: Ethernet, IP, TCP (with flags), UDP, ICMP, ARP
- Color-coded output by protocol
- Stats at the end: protocols, top talkers, ports
- Optional `.pcap` export, ready to open in Wireshark

## Usage

```bash
pip install -r requirements.txt

# list interfaces
python src/main.py --list-interfaces

# 100 TCP packets on eth0, displayed live
sudo python src/main.py -i eth0 -f "tcp" -c 100 -l

# capture for 30s and save to a pcap
sudo python src/main.py -i eth0 -t 30 -o capture.pcap
```

| Flag | Description |
|---|---|
| `-i, --interface` | Interface to capture on |
| `-f, --filter` | BPF filter |
| `-c, --count` | Number of packets (0 = unlimited) |
| `-t, --timeout` | Capture duration in seconds (0 = unlimited) |
| `-o, --output` | Save packets to a `.pcap` file |
| `-l, --live` | Print packets as they arrive |
| `--list-interfaces` | Show available interfaces |

Packet capture needs root / admin privileges.

## Layout

```
src/
├── main.py              # CLI entry point
├── capture/             # sniffing + pcap export
├── analyzer/            # per-protocol decoding and stats
├── cli/                 # terminal display
└── utils/               # interfaces, filter validation, helpers
```
