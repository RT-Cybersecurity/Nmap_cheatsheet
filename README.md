# Nmap_cheatsheet

_Nmap ("Network Mapper") is an open-source tool for network exploration and security auditing. It was designed to rapidly scan large networks, although it works fine against single hosts. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics. While Nmap is commonly used for security audits, many systems and network administrators find it useful for routine tasks such as network inventory, managing service upgrade schedules, and monitoring host or service uptime._

  # TARGET SPECIFICATION
<details>
  <summary>Target Specification</summary>

  * Scan Specific or single IP 
  * Scan range of IP
  * Scan CIDR notation
  * Scan Domain

```console
nmap 192.168.1.1
nmap 192.168.1.1-200
nmap 192.168.1.1/24
nmap scanme.nmap.org
```
  
  * -iL for scan target from file

```console
nmap -iL target_list.txt
```
  
  * -iR for scan random 100 hosts 

```console
nmap -iR 100
```

  * --exclude for Exclude listed hosts

```console
nmap --exclude 192.168.1.120
```
</details>

# HOST DISCOVERY

<details>
  <summary>Host Discovery</summary>

  *  -sL for no scan just list of traget host

```console
nmap -sL 192.168.1.1/24
```

  *  -sn for Disable port scan. host discovery Only 

```console
nmap -sn 192.168.1.1/24
```

  *  -Pn for Disable host discovery. Port scan Only 

```console
nmap -Pn 192.168.1.1/24
```

  *  -PS, -PA, -PU, -PR for TCP SYN, TCP ACK, UDP, ARP discovery
    
note: -PS, -PA default Port is 80, and -PU default port is 40125, and ARP discovery for local network

```console
nmap -PS22-25,80 192.168.1.1-10 
nmap -PA22-25,80 192.168.1.1-10
nmap -PU53 192.168.1.1-10
nmap -PR 192.168.1.1/24
```

  *  -n for Nerver do DNS resolution

```console
nmap -n 192.168.1.1-10
```

</details>

# SCAN TECHNIQUES

<details>
  <summary>Scan Techniques</summary>

* -sS TCP SYN port scan (Default)  

```console
nmap -sS 192.168.1.1
```

* -sA TCP ACK port scan  

```console
nmap -sA 192.168.1.1
```

* -sU UDP port scan  

```console
nmap -sU 192.168.1.1
```

* -sX Xmas scan  

```console
nmap -sX 192.168.1.1
```

* -sM TCP Maimon port scan - This technique is exactly the same as NULL, FIN, and Xmas scan, except that the probe is FIN/ACK.  

```console
nmap -sX 192.168.1.1
```

* -sW Xmas TCP Window port scan   

```console
nmap -sW 192.168.1.1
```

</details>

# PORT SPECIFICATION AND SCAN ORDER

<details>
  <summary>Port Specification And Scan Order</summary>

* -p <port ranges>: Only scan specified ports

```console
nmap -p21 192.168.1.1
nmap -p21-25 192.168.1.1
nmap -p U:53,111,137,T:21-25,80,139,8080,S:9 192.168.1.1
nmap -p http,https 192.168.1.1 
```

* -p- scan all ports   

```console
nmap -p- 192.168.1.1
```

* -F Fast port scan (100 ports)    

```console
nmap -F 192.168.1.1
```

* --top-ports Port scan the top x ports    

```console
nmap --top-ports 2000 192.168.1.1
```

</details>

# SERVICE/VERSION DETECTION

<details>
  <summary>Service/Version Detection</summary>

* -sV open ports to determine service/version info

```console
nmap -sV -p22 192.168.1.1
```

* -sV --version-intensity Intensity level 0 to 9. A Higher number increases possibility of correctness 

```console
nmap -sV --version-intensity 8 -p22 192.168.1.1
```

* -sV --version-light Enable light mode. Lower possibility of correctness. Faster 

```console
nmap -sV --version-light -p22 192.168.1.1
```

* -sV --version-all Enable intensity level 9. Higher possibility of correctness. Slower  

```console
nmap -sV --version-all -p22 192.168.1.1
```

* -A Enables OS detection, version detection, script scanning, and traceroute
  
```console
nmap -A 192.168.1.1
```

</details>

# SCRIPT SCAN

<details>
  <summary>Script Scan</summary>

* -sC Scan with default NSE scripts. Considered useful for discovery and safe
  
```console
nmap -sC 192.168.1.1
```

* --script default Scan with default NSE scripts. Considered useful for discovery and safe
  
```console
nmap --script default 192.168.1.1

nmap --script=banner 192.168.1.1
nmap --script=banner,http 192.168.1.1
nmap --script=http* 192.168.1.1
nmap --script "not intrusive" 192.168.1.1
```

* --script-args NSE script with arguments

```console
nmap --script snmp-sysdescr --script-args snmpcommunity=admin 192.168.1.1 
```

</details>

# OS DETECTION

<details>
  <summary>OS Detection</summary>

* -O Enable OS detection
  
```console
nmap -O 192.168.1.1
```

* -O --osscan-limit  If at least one open and one closed TCP port are not found it will not try OS detection against host 
  
```console
nmap -O --osscan-limit 192.168.1.1
```

* -O --osscan-guess  Makes Nmap guess more aggressively 
  
```console
nmap -O --osscan-guess 192.168.1.1
```

* -O --max-os-tries  Set the maximum number x of OS detection tries against a target 
  
```console
nmap -O --max-os-tries 1 192.168.1.1
```

* -A Enables OS detection, version detection, script scanning, and traceroute
  
```console
nmap -A 192.168.1.1
```

</details>

# TIMING AND PERFORMANCE

<details>
  <summary>Timing And Performance</summary>

*  -T<0-5>: Set timing template (higher is faster)
  
```console
nmap -T4 -v -sS -p- 192.168.1.1
```

Note: _-T0 Paranoid (0) Intrusion Detection System evasion, -P1 Sneaky (1) Intrusion Detection System evasion, -P2 Polite (2) slows down the scan to use less bandwidth and use less target machine resources, -P3 Normal (3) which is default speed, -P4 Aggressive (4) speeds scans; assumes you are on a reasonably fast and reliable network, -P5 Insane (5) speeds scan; assumes you are on an extraordinarily fast network._

</details>

# FIREWALL/IDS EVASION AND SPOOFING

<details>
  <summary>Firewall/IDS Evasion And Spoofing</summary>

* -f Requested scan (including ping scans) use tiny fragmented IP packets. Harder for packet filters 
  
```console
nmap -f 192.168.1.1

# -mtu Set your own offset size 
nmap --mtu 32 192.168.1.1
```

* -D Send scans from spoofed IPs 
  
```console
nmap -D 192.168.1.101,192.168.1.102, 192.168.1.103,192.168.1.23 192.168.1.1 
```

* -S Scan Facebook from Microsoft (-e eth0 -Pn may be required) 
  
```console
nmap -S www.microsoft.com www.facebook.com
```

* -g Use given source port number 
  
```console
nmap -g 53 192.168.1.1
```

* --proxies  Relay connections through HTTP/SOCKS4 proxies 
  
```console
nmap --proxies http://192.168.1.1:8080, http://192.168.1.2:8080 192.168.1.1 
```

* --data-length  Appends random data to sent packets 
  
```console
nmap --data-length 200 192.168.1.1 
```

```console
# Example IDS Evasion command 

sudo nmap -f -T0 -n -Pn –data-length 200 -D 192.168.1.101,192.168.1.102,192.168.1.103,192.168.1.23 192.168.1.1 
```

</details>

# OUTPUT

<details>
  <summary>Output</summary>

* -oN Normal Output
  
```console
nmap 192.168.1.1 -oN filename
```

* -oX XML Output
  
```console
nmap 192.168.1.1 -oX filename
```

* -oG Grepable Output

```console
nmap 192.168.1.1 -oG filename
```

* -oA Output in the three major formats at once 
  
```console
nmap 192.168.1.1 -oA filename
```

* -v Increase the verbosity level (use -vv or more for greater effect) 

```console
nmap -v 192.168.1.1

nmap -vv 192.168.1.1
```

* -d Increase debugging level (use -dd or more for greater effect)
  
```console
nmap -d 192.168.1.1

nmap -dd 192.168.1.1
```

* --reason 	Display the reason a port is in a particular state, same output as -vv

```console
nmap 192.168.1.1 --reason 	
```

* --open Only show open (or possibly open) ports

```console
nmap 192.168.1.1 --open 
```

* --packet-trace 	Show all packets sent and received

```console
nmap 192.168.1.1 -T4 --packet-trace 	
```

* --iflist Shows the host interfaces and routes

```console
nmap --iflist 
```

* --resume Resume a scan

```console
nmap --resume results.file 
```

</details>

# MISC

<details>
  <summary>Miscellaneous Options</summary>

* -h Print this help summary page.
  
```console
nmap -h
```

* -6 IPv6 scanning
  
```console
nmap -6 2607:f0d0:1002:51::4 
```

* -V Print version number
  
```console
nmap -V
```

</details>

# OTHER USEFUL COMMANDS

<details>
  <summary>Other Useful Commandss</summary>
  
* CTF Commands

```console
# Quick Scan with Scripts Check (Default Ports - Top 1k)
nmap -sV -sC -oA std 192.168.1.1

# Run Again with all ports
nmap -p- -sV -sC -oA std_port 192.168.1.1

# Aggressive Scan (All Enabled - if needed)
nmap -p0- -v -A -T4 -oA aggro 192.168.1.1

# Convert nmap xml files to html files
xsltproc aggro.xml -o nmap.html
```

* Useful Nmap Scripts

```console
# View scripts
ls /usr/share/nmap/scripts/

# HTTP
nmap --script http-enum -v 192.168.1.1 -p80 -oA http_enum

# HTTP/DNS
nmap --script dns-brute -v 192.168.1.1 -p80,443 -oA dns_brute

# SMB
nmap --script smb-enum-users.nse -p445 192.168.1.1 -oA smb-enum-users
nmap --script smb-brute.nse -p445 192.168.1.1 -oA smb_brute

# Vulnerability (Downloads required — vulners, vulscan)
nmap --script vulners,vulscan/vulscan.nse --script-args vulscandb=scipvuldb.csv -sV -p<Ports> 192.168.1.1 -oA vuln

```
* CEH Exam and other exam

```console
nmap -Pn -sS -A -oA <Filename> 192.168.1.1/24 -vv
nmap -sV -sC -p21-25,80,444,53 -oA <filename> 192.168.1.2 -vv
```

_Resource Reference Links : https://nmap.org/_

</details>


