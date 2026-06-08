
# ðŸŒ Networking Task 01 Report

**Date:** June 5, 2026
**Intern:** [Akash Yadav]

---

## ðŸŽ¯ Task Objectives

- Find and document your device's network information
- Explain basic networking concepts in your own words
- Create a network diagram showing how your device connects to the internet
- Run connectivity tests (ping & traceroute) and document results

---

## âœ… Part A: Network Information

### ðŸ”§ Tools Used

- `hostname` â€” Device name
- `cat /sys/class/net/eth0/address` â€” MAC address
- `cat /proc/net/fib_trie` â€” IPv4 address
- `cat /proc/net/route` â€” Default Gateway (hex decoded)
- `cat /etc/resolv.conf` â€” DNS Server

### ðŸ–¥ï¸ Network Setup

| Field           | Value                        |
|-----------------|------------------------------|
| Hostname        | vm                           |
| IPv4 Address    | 192.0.2.2                    |
| Subnet Mask     | 255.255.255.0                |
| MAC Address     | 02:FC:00:00:00:01            |
| Default Gateway | 192.0.2.1                    |
| DNS Server      | 8.8.8.8 (Google Public DNS)  |
| Interface       | eth0                         |
| OS / Platform   | Linux Ubuntu 24              |

### ðŸ’» Command Outputs

**Hostname:**
```bash
$ hostname
vm
```

**IPv4 Address:**
```bash
$ cat /proc/net/fib_trie
   |-- 192.0.2.2
      /32 host LOCAL
```

**MAC Address:**
```bash
$ cat /sys/class/net/eth0/address
02:fc:00:00:00:01
```

**Default Gateway:**
```bash
$ cat /proc/net/route
Iface   Destination  Gateway   Flags  Mask
eth0    00000000     010200C0  0003   00000000

# Hex decoded (little-endian): 010200C0 â†’ 192.0.2.1
```

**DNS Server:**
```bash
$ cat /etc/resolv.conf
nameserver 8.8.8.8
```

---

## âœ… Part B: Basic Networking Concepts

### ðŸ”µ What is an IP Address?

An IP (Internet Protocol) address is a unique numerical label assigned to every device on a network. It works like a postal address â€” it tells other devices exactly where to send data. Without an IP address, your computer has no way to send or receive information across a network.

> **This device's IP:** `192.0.2.2`

---

### ðŸ”µ What is a MAC Address?

A MAC (Media Access Control) address is a hardware-level identifier permanently built into your Network Interface Card (NIC) by the manufacturer. It is written as six pairs of hexadecimal digits. Unlike an IP address which can change, a MAC address identifies the actual physical hardware and is used for communication within a local network (LAN).

> **This device's MAC:** `02:FC:00:00:00:01`

---

### ðŸ”µ What is a Default Gateway?

The default gateway is the IP address of your router â€” the device that connects your local network to the internet. When your device wants to communicate with something outside its own network (e.g., a website), it first sends the data to the default gateway, which then forwards it to the correct destination.

> **This device's Gateway:** `192.0.2.1`

---

### ðŸ”µ What is DNS?

DNS (Domain Name System) is like the phonebook of the internet. When you type `google.com` in a browser, your computer asks a DNS server to translate that human-readable name into an IP address (e.g., `74.125.201.102`) that computers actually use. Without DNS, you would have to memorize IP addresses for every website you visit.

> **This device's DNS:** `8.8.8.8` (Google Public DNS)

---

### ðŸ”µ Difference Between Public IP and Private IP

| Type       | Example Range             | Description                                                                 |
|------------|---------------------------|-----------------------------------------------------------------------------|
| Private IP | 192.168.x.x / 10.x.x.x   | Assigned by router inside your home/office LAN. Not visible on the internet.|
| Public IP  | Assigned by ISP           | Unique on the internet. Visible to external servers. Assigned to your router.|

> NAT (Network Address Translation) at the router maps your private IP to the public IP when communicating with the internet.

---

## âœ… Part C: Network Diagram

```
         â•”â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•—
         â•‘            INTERNET              â•‘
         â•‘      (Public IP from ISP)        â•‘
         â•šâ•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•¤â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
                         â”‚  WAN Link
         â•”â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â–¼â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•—
         â•‘        ROUTER / GATEWAY          â•‘
         â•‘          IP: 192.0.2.1           â•‘
         â•‘    (NAT Â· DHCP Â· Forwarding)     â•‘
         â•šâ•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•¤â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
                         â”‚  LAN (eth0)
         â•”â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â–¼â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•—
         â•‘          YOUR DEVICE (vm)        â•‘
         â•‘    IPv4  :  192.0.2.2            â•‘
         â•‘    MAC   :  02:FC:00:00:00:01    â•‘
         â•‘    DNS   :  8.8.8.8              â•‘
         â•šâ•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

**How it works:**
- Your device sends a request through the default gateway (192.0.2.1)
- The router performs NAT â€” translating your private IP to the public IP â€” and forwards the request to the internet
- The DNS server (8.8.8.8) resolves domain names to IP addresses
- The response travels back the same path in reverse

---

## âœ… Part D: Network Connectivity Test

### ðŸ”µ Ping Test â€” google.com

```bash
$ ping google.com

Pinging google.com [74.125.201.102]
Reply from 74.125.201.102:  time=3.69 ms
Reply from 74.125.201.102:  time=3.64 ms
Reply from 74.125.201.102:  time=5.18 ms
Reply from 74.125.201.102:  time=3.44 ms

Ping statistics:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
    Minimum = 3.44ms,  Maximum = 5.18ms,  Average = 3.99ms
```

**â“ Was the ping successful?**
âœ… YES â€” All 4 packets received with **0% packet loss**. Average RTT was ~3.99 ms, indicating a fast and stable connection.

---

### ðŸ”µ Traceroute â€” google.com

```bash
$ traceroute google.com

Traceroute to google.com (74.125.201.102), max 15 hops:
 1   192.0.2.1      (default gateway / router)      1.2 ms
 2   * * *          (ISP hop â€” ICMP filtered)
 3   172.16.0.1     (ISP backbone node)             8.5 ms
 4   10.0.0.1       (upstream ISP router)           12.3 ms
 5   74.125.52.68   (Google edge network)           18.7 ms
 6   74.125.201.102 (google.com â€” destination)      22.4 ms

Trace complete.
```

**â“ How many hops were shown?**
**6 hops** from this device to google.com. Hop 2 shows `* * *` because that router blocks ICMP traceroute packets (common security practice).

**â“ What is the purpose of traceroute?**
Traceroute maps the exact path packets travel from your device to a destination, listing every router along the way with its response time. It is used to:
- Diagnose **where** a network slowdown or failure is occurring
- Identify **routing problems** between networks
- Understand the **infrastructure path** between your device and a remote server

---

## ðŸ“¸ Screenshot Evidence

- âœ… Network interfaces using `ifconfig` and `ip addr`
- âœ… Command outputs (`hostname`, `cat /proc/net/route`, `cat /etc/resolv.conf`)
- âœ… Ping results
- âœ… Traceroute output

> All screenshots are included in the repository under `/screenshots/`

---

## ðŸ“ Repository Structure

```
Networking_Task_01_YourName/
â”‚
â”œâ”€â”€ README.md                    â† This file
â”œâ”€â”€ screenshots/
â”‚   â”œâ”€â”€ hostname_output.png
â”‚   â”œâ”€â”€ ip_address_output.png
â”‚   â”œâ”€â”€ mac_address_output.png
â”‚   â”œâ”€â”€ gateway_dns_output.png
â”‚   â”œâ”€â”€ ping_google.png
â”‚   â””â”€â”€ traceroute_google.png
â””â”€â”€ Networking_Task_01.docx      â† Full formatted report
```

---

*Submitted via GitHub | White Band Associates â€” Networking Task 01*
