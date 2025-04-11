# Hydra

## Introduction

Hydra is a tool used to bruteforce online passwords.\
This room walks us through using Hydra to bruteforce a POST login form, and also asks us to bruteforce SSH credentials.

## Process

### Task 2: Using Hydra
This task shows us examples of how the hydra command is used on the command line.\
We are given a target machine to attack, and when I go to the given IP there is a login form.\
This is the command given to demonstrate how to bruteforce a POST login form: ```hydra -l <username> -P <wordlist> [target IP] http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V```

![image](https://github.com/user-attachments/assets/c6d49e13-2001-4f28-ac23-9549e96e58c9)

I submit a test login to see the request details and how they relate to the variables that I must pass into the Hydra command.\
The request sent has 2 fields, username and password, which logically correlate to the ```username=^USER^&password=^PASS^``` part of the command. It is explained that the ```F=incorrect``` portion of the command is a string that appears in the server reply when the login fails.\
It is also explained that the first part of the ```"/:username=^USER^&password=^PASS^:F=incorrect"``` portion of the command (the ```/``` before the first ```:```) refers to the login page URL. In the case of our example, I had to change this to ```/login```.

#### Task 2 Questions:
**Use Hydra to bruteforce molly's web password. What is flag 1?**
> THM{2673a7dd116de68e85c48ec0b1f2612e}

Running ```sudo hydra -l molly -P /usr/share/wordlists/rockyou.txt [target IP] http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V``` gives the password "sunshine" in 40 attempts. Loggin in with this showed the flag.

**Use Hydra to bruteforce molly's SSH password. What is flag 2?**
> THM{c8eeb0468febbadea859baeb33b2541b}

Running ```sudo hydra -l molly -P /usr/share/wordlists/rockyou.txt [target IP] -t 4 ssh``` gives the password "butterfly". Logging in through SSH with this password puts us in a directory with flag2.txt in it.

## Conclusion and Learning Outcomes
Hydra is an easy to use bruteforce tool. It is cool to know how to use it but there isn't a lot of deep learning to reflect on in this room. I am looking forward to exercises where I may need to use Hydra for more realistic scenarios.
