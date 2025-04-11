# Metasploit: Introduction

## Introduction

An introduction to the main components of the Metasploit Framework.

## Process

### Task 2: Main Components of Metasploit
This section focuses primarily on definitions:
- **Exploit**: A piece of code that takes advantage of a vulnerability on the target.
- **Vulnerability**: A flaw in a piece of software or hardware on the target system that an attacker can take advantage of.
- **Payload**: A piece of code that is designed to be run on the target system through use of an exploit.

#### Modules
- **Auxiliary**: Supporting modules that perform useful tasks such as scanning, gathering data, analysis, etc.
- **Encoders**: Used to encode the exploit and payload to attempt to avoid being detected by signature-based antivirus solutions. This does not mean they are working to avoid being scanned in the first place (my understanding is that the evasion modules are meant for that purpose).
- **Evasion**: Modules that aim to deliver the payload in a way that might avoid being scanned by antivirus, without having to install external tools.
- **Exploits**: Modules used to leverage vulnerabilities to execute the payload.
- **NOPs**: Often used as a buffer to achieve consistent payload sizes, NOPs (No OPeration) don't do anything. 
- **Payloads**: Codes to be executed on the target system. These can do many things depending on what your intention is, and are split into 4 types in the Metasploit Framework.
  - *Adapters*: Wraps single payloads to be in a different format, such as preparing a script to be run by a single powershell command.
  - *Singles*: Self contained payload that does not need to download an additional component to run.
  - *Stagers*: Designed to set up a connection between Metasploit and the target system to then send a Stage. This allows for larger payloads to be sent after a first initial Stager is set up.
  - *Stages*: A follow up payload to be downloaded by a Stager, allowing for larger payload sizes than might have been possible.
- **Post**: Modules meant for the final stage of exploitation, once a machine has been compromised.

#### Task 2 Questions:
**What is the name of the code taking advantage of a flaw on the target system?**\
> Exploit\
The exploit takes advantage of a vulnerability on the target system.

**What is the name of the code that runs on the target system to achieve the attacker's goal?**\
> Payload\
The payload is the code that is executed on the target system through the vulnerability.

**What are self-contained payloads called?**\
> Singles\
By self-contained, the question refers to payloads that act on their own, rather than setting up a connection to the target and continuing the attack that way. Singles can be as simple as adding a user to the system or running notepad.exe.

**Is "windows/x64/pingback_reverse_tcp" among singles or staged payload?**\
> Singles\
Googled this one, searching for "windows/x64/pingback_reverse_tcp" gave the github repository as the first result, where it was easy to see in the path (metasploit-framework/modules/payloads/**singles/windows**/x64/pingback_reverse_tcp.rb) that this file was under singles.

### Task 3:
Metasploit for the command line is launched by running ```msfconsole```. This interface supports most Linux commands, but does not support some things, such as output redirection. Tab autocomplete is helpful in this interface because it can autocomplete things that are unfamiliar to someone used to a Linux interface, such as Metasploit commands.

Running exploits is done using the ```use``` command followed by the exploit, or the number at the beginning of the search result line. This sets the context for our workspace. The ```show options``` command will show options related to the exploit you chose. The ```info``` command will show further information about the selected module. You can leave the context by using the ```back``` command.

The ```search``` command lets you check the Metasploit framework for relevant modules. You can search by CVE number, exploit names, or target system. You can specify type and platform of the search if you want like this: ```search type:auxiliary telnet```.

#### Task 3 Questions:
**How would you search for a module related to Apache?**\
> search apache\
All we have to do is make "Apache" our search query.

**Who provided the auxiliary/scanner/ssh/ssh_login module?**\
> todb\
I ran ```search ssh_login```, and the first result was the one in the question, so I then ran ```use 0```, and then ```info```, which brought up the information about the module.

### Task 4:
This task takes us through modifying parameters in a workspace, and using a module to establish a session.
Parameters are set using the ```set``` command, the ```setg``` command to set variables globally (keeps the variable set until you close Metasploit or clear it using ```unsetg```), and once all the parameters are set you can use the ```exploit``` command, or the ```run``` alias for the command.

#### Task 4 Questions:
**How would you set the LPORT value to 6666?**\
> set LPORT 6666

**How would you set the global value for RHOSTS  to 10.10.19.23 ?**\
> setg RHOSTS 10.10.19.23

**What command would you use to clear a set payload?**\
> unset payload

**What command do you use to proceed with the exploitation phase?**\
> exploit

These are pretty straight forward if you follow the task.

## Conclusion and Learning Outcomes
This room was a great introduction into how to use the Metasploit framework, and the questions were pretty straight-forward and mostly just asked for terms or commands described in the tasks. 
