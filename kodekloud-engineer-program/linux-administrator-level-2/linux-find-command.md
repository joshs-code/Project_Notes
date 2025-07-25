# Linux Find Command

During a routine security audit, the team identified an issue on the Nautilus App Server. Some malicious content was identified within the website code. After digging into the issue they found that there might be more infected files. Before doing a cleanup they would like to find all similar files and copy them to a safe location for further investigation. Accomplish the task as per the following requirements:

a. On `App Server 1` at location `/var/www/html/official` find out all files (not directories) having `.php` extension.

b. Copy all those files along with their `parent directory structure` to location `/official` on same server.

c. Please make sure not to copy the entire `/var/www/html/official` directory content.



My Solution:

```
[tony@stapp01 ~]$ sudo find /var/www/html/official -type f -name *.php -exec cp --parents {} /official \;

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ 
```
