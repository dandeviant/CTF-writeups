## Root Me CTF
https://tryhackme.com/room/rrootme

```bash
export IP=10.10.134.15
```
---
### Tasks
**Task 1: Deploy**

**Task2: Recon**

- Scan the machine, how many ports are open?
> ```bash
> nmap -sC -sV -oN 10.10.134.15
> ```
> Open Ports
> - 22 - ssh
> - 80 - Apache/2.4.29

- What version of Apache is running?
>  - Apache/2.4.29
- What service is running on port 22?
> SSH

- Find directories on the web server using the GoBuster tool.  

- What is the hidden directory?

**Task 3: Getting Shell**

**Task 4: Privesc**