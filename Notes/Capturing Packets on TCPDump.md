---
categories:
  - "[[Guides]]"
topics:
  - "[[Networking]]"
  - "[[Linux]]"
created:
---



### Command
```shell
$ tcpdump -nni <interface> udp port and <port> -c 100 -w /tmp/traffic.pcap
```

### 🧩 Command Breakdown

| Part                   | Meaning                                                                                                                                                                          |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-i`                   | Capture on the router. This sees all internal LAN traffic.                                                                                                                       |
| `udp port`             | Filter to capture only **UDP traffic on specified port**.                                                                                                                        |
| `-nn`                  | Prevent name resolution:  <br>- `-n`: don’t resolve IP addresses to hostnames  <br>- `-n`: don’t resolve port numbers to service names (e.g., shows `port` instead of `service`) |
| `-c 100`               | Capture **only 100 packets**, then stop automatically                                                                                                                            |
| `-w /tmp/traffic.pcap` | Write the captured packets to a **file** in **pcap format** (used by Wireshark and other tools) in `/tmp` (a writable directory on OpenWRT)                                      |

### 🧪 Result

- You’ll get a file named `/tmp/traffic.pcap` containing the **first 100 related packets** seen on the LAN.  
- You can then **analyze this in Wireshark**, or inspect which internal device sent traffic to `216.93.246.18:3478`.