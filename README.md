# Port Scanning My Local Network (Nmap + Wireshark)

## what this is about
basically, I used `nmap` to scan all the devices on my local Wi-Fi to see which ports are open (aka which services are running), and then used `Wireshark` to actually look at the packets being sent during the scan.

pretty cool to actually see what a port scan *looks like* in Wireshark.

---

## tools I used
- Nmap (for scanning)
- Wireshark (for analyzing packets)
- Windows 11

---

## what I did

### 1. figured out my IP range
- ran `ipconfig` to see my IP
- scanned the full range with:
nmap -sS 192.168.x.0/24


> `-sS` does a SYN scan = super fast and doesn't fully connect, just checks if ports respond.

---

### 2. made the scan report safe to share
I removed/redacted all private info like:
- IP addresses
- MAC addresses
- Device names

and saved the clean version as:
[`nmap-scan-sanitized.txt`](nmap-scan-sanitized.txt)

---

### 3. captured packets in Wireshark
While the scan was running, I captured traffic in Wireshark.

Then used this filter:
tcp.flags.syn == 1 and tcp.flags.ack == 0

That shows just the SYN packets (when nmap is "knocking" on ports).

Took a screenshot — blurred anything sensitive:
[`wireshark-scan-screenshot.png`](wireshark-scan-screenshot.png)

---

## what I noticed / learned

- Ports like 22 (SSH), 80 (HTTP), 443 (HTTPS), and 445 (SMB) were open on some devices
- TCP SYN scan is basically just the first part of a handshake (sends SYN, doesn’t complete the rest)
- If a port is open → you get SYN-ACK
- If it’s closed → you get RST
- If it’s filtered → you get nothing (or retransmissions)

also, saw some `[TCP Retransmission]` packets which probably means the device/firewall didn’t reply.

---

## files in this repo

| file                           | what it is                              |
|--------------------------------|------------------------------------------|
| `nmap-scan-sanitized.txt`      | cleaned up scan result                  |
| `wireshark-scan-screenshot.png`| screenshot from Wireshark (SYN filter)  |
| `README.md`                    | this writeup                           |

---

## note to whoever's reading

this was done on my **own network**, purely for learning.  
don’t run scans on networks you don’t have permission for — that’s illegal.
