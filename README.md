# tcpdump_basics

## Introduction
tcpdump is a powerful command-line packet analyzer tool that allows you to capture and inspect network traffic in real time. It is widely used for network troubleshooting, security analysis, and monitoring network activity.  

## Objective
* Learn how to install tcpdump on Linux systems.
* Capture network packets and understand their headers.
* Filter packets based on protocols, hosts, and ports.
* Analyze packet contents and save captures for later use.

## Requirements and Tools
* Linux system (Ubuntu/Debian recommended).
* tcpdump installed.
* Basic terminal/command-line knowledge.

## Step 1: Installation on Linux
1. Check whether tcpdump is installed on your system using:
```bash
which tcpdump
```
2. If it is not installed, install it using:
```bash
sudo apt install -y tcpdump
```

## Step 2: Capturing packet headers using tcpdump
1. Check all available interfaces:
```bash
sudo tcpdump -D
```
2. Capture all packets on an interface:
```bash
sudo tcpdump -i any
```
3. Stop packet capture using CTRL + C.
4. To limit the number of packets captured, use -c:
```bash
sudo tcpdump -i any -c 5
```
5. To disable name resolution for hosts, use -n.
6. To disable port resolution, use -nn

## Step 3: Understanding the output format
Example packet:
```
09:18:53.186955 eth0 In IP pnmaaa-an-in-f2.1e100.net.https > 192.168.0.128.35974: Flags [P.], seq 1:40, ack 39, win 1048, options [nop,nop,TS val 3951643436 ecr 157539099], length 39
```

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/packet_format.png'/>

Explanation:
* Timestamp (HH:MM:SS.microseconds): 09:18:53.186955
* Network interface: eth0
* Direction relative to capture point: In
* Packet type: IPv4 → IP
* Source host and port: pnmaaa-an-in-f2.1e100.net → Google server, .https → port 443 (HTTPS)
* Destination IP and port: 192.168.0.128 → local machine IP, .35974 → port
* Flags: Flags [P.] → PUSH flag, tells receiver to deliver buffered data immediately
| Flag	| Type | Symbol	| Description |
|-------|-------|--------|-------------|
| ACK	| Control	| .	| Acknowledges received data. |
| PSH	| Data	| P	| Push buffered data immediately. |
| RST	| Control	| R	| Reset the connection. |
| SYN	| Control	| S	| Initiates a connection. |
| FIN	| Control	| F	| No more data to send; gracefully close. |

* TCP sequence numbers: seq 1:40 → packet carries bytes 1 through 39
* TCP acknowledgment number: ack 39 → sender has received up to byte 38 and expects byte 39 next
* TCP window size: win 1048 → 1048 bytes sender is willing to receive
* TCP options: options [nop,nop,TS val 3951643436 ecr 157539099] → timestamp and padding
* TCP payload length: length 39 → 39 bytes

## Step 4: Filtering packets
1. Based on protocol: Capture ICMP packets (ping a website first):
```bash
sudo tcpdump -i any -c 50 icmp
```

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/protocol.png'/>

2. Based on host: Capture traffic related to a specific host:
```bash
sudo tcpdump -i any -c 50 -nn host <host IP>
```

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/host.png'/>

3. Based on port: Capture packets through a specific port:
```bash
sudo tcpdump -i any -c 50 -nn port 80
```

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/port.png'/>

4. Source IP address: Capture packets from a specific source IP:
```bash
sudo tcpdump -i any -c 50 -nn src <source IP>
```

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/source.png'/>

5. Complex expressions: Use and / or to combine conditions:
```bash
sudo tcpdump -i any -c 5 -nn src <source IP> and port 443
```

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/complex.png'/>

## Step 5: Checking packet content
To see packet content, tcpdump provides these flags:

| Option | Description |
|--------|-------------|
| -A  | ASCII payload only |
| -X | Hex + ASCII payload |
| -XX | Hex + ASCII + link-layer header |
| -s 0 | Capture entire packet (no truncation) |
| -vvv | Maximum verbosity, more header details |

Example:
```bash
sudo tcpdump -i any -c 10 -nn -A port 80
```

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/read%20packet%201.png'/>

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/read%20packet%202.png'/>

<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/read%20packet%203.png'/>

## Step 6: Saving captured packets
1. Save packets to a file with -w:
```bash
sudo tcpdump -i any -c 10 -nn -w webserver.pcap
```
2. .pcap stands for "packet capture" and is the convention for this file format.
3. Read saved packets:
```bash
tcpdump -nn -r webserver.pcap
```


<img src='https://github.com/TanunM/tcpdump_basics/blob/main/gallery/write_read.png'/>

4. You can also apply filters while reading to search for specific conditions.

## Troubleshooting
* Permission issues: Use sudo to run tcpdump.
* No packets captured: Check the correct interface (`-D`) and ensure there is network activity.  

## Key Learning
* Understanding the structure of TCP/IP packets.
* Using filters to capture specific traffic.
* Viewing packet payloads in ASCII and hexadecimal formats.
* Saving and reading ".pcap" files for offline analysis.  

## Reference
1. [An introduction to using tcpdump](https://opensource.com/article/18/10/introduction-tcpdump)  
2. [tcpdump official page](https://www.tcpdump.org/manpages/tcpdump.1.html#lbAG)
