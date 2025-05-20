# Friday Overtime

## Introduction

This scenario involves the analysis of malware samples. As stated in the problem these artefacts were retrieved from a real-world cyber-attack, so the analysis is all done in a VM.

## Process

### Task 1: Challenge Scenario
This task takes us through analyzing the artefacts and finding information regarding the attack details.

#### Task 2 Questions:
**Who shared the malware samples?**
> Oliver Bennett

The message containing the samples and the information regarding the event was sent by Oliver Bennett.

**What is the SHA1 hash of the file "pRsm.dll" inside samples.zip?**
> 9d1ecbbe8637fed0d89fca1af35ea821277ad2e8

Running ```sha1sum pRsm.dll``` after extracting the files and navigating into the sample folder gives the hash we are asked for.

**Which malware framework utilizes these DLLs as add-on modules?**
> MgBot

Searching for the filenames brings up an [article](https://www.welivesecurity.com/2023/04/26/evasive-panda-apt-group-malware-updates-popular-chinese-software/) referencing a Chinese-speaking APT group and their custom malware framework, MgBot.

**Which MITRE ATT&CK Technique is linked to using pRsm.dll in this malware framework?**
> T1123

The same article used in the last question lists the plugin DLLs used by the MgBot framework, and lists pRsm.dll as a plugin used to capture input and output audio streams.\
Looking through the techniques listed on the [MgBot MITRE ATT&CK page](https://attack.mitre.org/software/S1146/), the technique most relevant to the pRsm.dll plugin is Audio Capture, with ID T1123.

**What is the CyberChef defanged URL of the malicious download location first seen on 2020-11-02?**
> hxxp[://]update[.]browser[.]qq[.]com/qmbs/QQ/QQUrlMgr_QQ88_4296[.]exe

The same article lists malicious download locations associated with the threat.

**What is the CyberChef defanged IP address of the C&C server first detected on 2020-09-14 using these modules?**
> 122[.]10[.]90[.]12

The same article also lists the MgBot C&C server IPs.

**What is the SHA1 hash of the spyagent family spyware hosted on the same IP targeting Android devices on November 16, 2022?**
> 1c1fe906e822012f6235fcc53f601d006d15d7be

Searching for the malicious IP found in the previous question on VirusTotal shows the hash we are looking for under the Android Spyware entry in the Relations tab.

## Conclusion and Learning Outcomes
This was a great walkthrough of the analysis of malware and the process of finding information on the threat.
