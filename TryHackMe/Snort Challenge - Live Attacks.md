# Snort Challenge - Live Attacks

## Introduction

This scenario consists of two challenges. One concerning incoming traffic attempting to brute force the system, and one concerning outgoing traffic as a result of a reverse-shell on the system.

## Process

### Scenario 1: Brute-Force
In order to look at the traffic, I run ```sudo snort -v``` to run Snort in sniffer mode.
I see traffic incoming to port 22, which I know is commonly used for SSH.
Creating a ```local.rules``` file in the current working directory with the rule ```alert tcp any any -> any 22 (msg:"Port 22 activity detected"; sid:1;)```, and running ```sudo snort -A full -c local.rules``` stops the attack, and reveals the flag.

#### Scenario 1 Questions:
**Stop the attack and get the flag (which will appear on your Desktop)**
> THM{81b7fef657f8aaa6e4e200d616738254}

This flag is found by stopping the SSH attack for 1 min.

**What is the name of the service under attack?**
> SSH

Port 22 is commonly used for the SSH protocol.

**What is the used protocol/port in the attack?**
> TCP/22

### Scenario 2: Reverse-Shell
In order to look at the traffic, I run ```sudo snort -v``` again to run Snort in sniffer mode.
I see outgoing traffic from port 4444, which stands out.
Creating a ```local.rules``` file in the current working directory with the rule ```alert tcp any 4444 -> any any (msg:"Port 4444 activity detected"; sid:1;)```, and running ```sudo snort -A full -c local.rules``` stops the attack, and reveals the flag.

#### Scenario 2 Questions:
**Stop the attack and get the flag (which will appear on your Desktop)**
> THM{0ead8c494861079b1b74ec2380d2cd24}

This flag is found by stopping the outgoing reverse-shell transmission for 1 min.

**What is the used protocol/port in the attack?**
> tcp/4444

**Which tool is highly associated with this specific port number?**
> Metasploit

A google search of port 4444 common uses gives this information.

## Conclusion and Learning Outcomes
I enjoyed the problem solving of finding the port and protocols under attack in these scenarios and being able to put a stop to the attacks.
