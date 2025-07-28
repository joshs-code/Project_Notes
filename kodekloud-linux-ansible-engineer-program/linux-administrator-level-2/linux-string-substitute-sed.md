# Linux String Substitute (sed)

There is some data on `Nautilus App Server 3` in `Stratos DC`. Data needs to be altered in several of the files. On `Nautilus App Server 3`, alter the `/home/BSD.txt` file as per details given below:

a. Delete all lines containing word `copyright` and save results in `/home/BSD_DELETE.txt` file. (Please be aware of case sensitivity)\
\
b. Replace all occurrence of word `the` to `their` and save results in `/home/BSD_REPLACE.txt` file.\
\
`Note:` Let's say you are asked to replace word `to` with `from`. In that case, make sure not to alter any words containing the string itself; for example upto, contributor etc.



My Solution:

```
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:73HqUG3EBIpbHIclJ/IKpUq10oI9cAMHQiItIwSADVo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
[banner@stapp03 ~]$ sed 's/copyright/d' /home/BSD.txt > /home/BSD_DELETE.txt
-bash: /home/BSD_DELETE.txt: Permission denied
[banner@stapp03 ~]$ sudo sed 's/copyright/d' /home/BSD.txt > /home/BSD_DELETE.txt
-bash: /home/BSD_DELETE.txt: Permission denied
[banner@stapp03 ~]$ sudo sed 's/copyright/d' /home/BSD.txt > /home/BSD_DELETE.txt
-bash: /home/BSD_DELETE.txt: Permission denied
[banner@stapp03 ~]$ sudo sed 's/copyright/d' /home/BSD.txt >> /home/BSD_DELETE.txt
-bash: /home/BSD_DELETE.txt: Permission denied
[banner@stapp03 ~]$ sudo su

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
Sorry, try again.
[sudo] password for banner: 
[root@stapp03 banner]# sudo sed 's/copyright/d' /home/BSD.txt > /home/BSD_DELETE.txt
sed: -e expression #1, char 13: unterminated `s' command
[root@stapp03 banner]# sudo sed '/copyright/d' /home/BSD.txt > /home/BSD_DELETE.txt
[root@stapp03 banner]# sed 's/the/their/g' /home/BSD.txt > /home/BSD_REPLACE.txt
```
