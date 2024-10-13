**Team GOTEN**

## Curtin CTF 2024

### Misc 1

**Category**  : Warmup

**Difficulty**  : Easy

**Points**  : 100 (If static)

**Attachments**  : misc_1

**Description**  :

**Solution**

Given the misc_1 file act like id_rsa. Given permission to misc_1 file. Connect ssh.
`ssh -i misc_1 username@ip`

Once successfully login, just `cat flag` to get out flag.

==CURTIN_CTF{H1dd3n_f1l3}==

### Misc 2

**Category**  : Miscellaneous

**Difficulty**  : Easy

**Points**  : 100 (If static)

**Attachments**  : misc_2

**Description**  :
SSH as `curtin` and enter the flag
Author: MetaSpoilt

**Solution**

Given the misc_2 file act like id_rsa. Given permission to misc_2 file. Connect ssh.
`ssh -i misc_2 username@ip`
Once successfully login, just notice on missing normal binary file which happen to be at `/usr/bin.` Looking into which `/bin` I manage to know that `/usr/bin == /bin`, then `/bin/cat flag` to get out flag.

==CURTIN_CTF{N0_P@TH}==

### Misc 3

**Category**  : Miscellaneous

**Difficulty**  : Easy

**Points**  : 250

**Attachments**  : misc_3

**Description**  :
SSH as `curtin2` and enter the flag
Author: MetaSpoilt

**Solution**

Given the misc_3 file act like id_rsa. Given permission to misc_3 file. Connect ssh.
`ssh -i misc_3 username@ip`
Once successfully login, notice on `.swp` which indicator as recover file in vim. Open the vim, and recover the file (i just enter), the flag will reveal on tops of it.

`vim -r flag`

==CURTIN_CTF{E$c@pe_C0d35}==

### Misc 4

**Category**  : Miscellaneous

**Difficulty**  : Easy

**Points**  : 250

**Attachments**  : misc_4

**Description**  :
SSH as `curtin3` and enter the flag
Author: MetaSpoilt

**Solution**

Given the misc_4 file act like id_rsa. Given permission to misc_4 file. Connect ssh.
`ssh -i misc_4 username@ip`
Once successfully login, notice on 2 file which one file have flag. The file a bit tricky since it name as `-`. To bypass this, using `./-` to get into directory and reveal the flag.

`cd ./-`

==CURTIN_CTF{d@$h3d_f0lder}==

### Misc 5

**Category**  : Miscellaneous

**Difficulty**  : Easy

**Points**  : 200

**Attachments**  : misc_5

**Description**  :
SSH as `ubuntu` and enter the flag
Author: MetaSpoilt

**Solution**

Given the misc_5 file act like id_rsa. Given permission to misc_5 file. Connect ssh.
`ssh -i misc_5 username@ip`
Once successfully login, notice the flag with owner and group of root. It remind me with LPE challenge. As starting, just try my luck to check sudo permission which normally needed password. But this challenge give us free sudo without password..

`sudo -l`

`/bin/more` require no password and have root permission.

`sudo more flag`

==`CURTIN_CTF{$ud0_N0_P@s$wd}`==

### Misc 7

**Category**  : Miscellaneous

**Difficulty**  : Easy

**Points**  : 500

**Attachments**  : misc_7

**Description**  :

`nc 52.221.246.50 1337` - get bash shell and enter the flag
Author: MetaSpoilt

**Solution**

Given the misc_7 file act like id_rsa. Given permission to misc_7 file. Connect ssh.
`ssh -i misc_7 username@ip`

But the server seem broken. Change into `nc ip port` to access as raw ssh.
Another LPE challenge but this time to read sudo permission we need password which we don't have.
Finding SUID to see any file can run in root permission

`find / -perm -4000 2>/dev/null` 

It show one interest file `/etc/security/print_file`

`/etc/security/print_file /home/ctf/flag`

==CURTIN_CTF{$3tU1D}==

### Extension

**Category**  : Warmup

**Difficulty**  : Easy

**Points**  : 50

**Attachments**  : flag.zip

**Description**  :

**Solution**

Given the file with zip extension. Further inspection show that the file not in archive (zip) but in RIFF format. I know RIFF format can be extracted using binwalk.

`binwalk -e flag.zip`

We get bunch of file but 2 file are interest more which flag.zip and password. The flag.zip needed password where we can get from password file. 
`7z x flag.zip ` and give correct password.

Decode the flag in  base64.

`strings flag | base64 -d`

==CURTIN_CTF{FLAG_EXTR4CTED_MiSS1ON_ACC0MPLISHED}==


### Warmup-Pwn

**Category**  : Warmup

**Difficulty**  : Easy

**Points**  : 100

**Attachments**  : warmup

**Description**  :

**Solution**

Given the file in elf format and it mention about pwn. Skip the first part which checksec. Understand what this pwn want by running it. It show that we need to supply with magic byte for win function. 

To archive that, im using gdb and `info function | grep win`
Send SS to admin to receive flag.

![](https://cdn.discordapp.com/attachments/1294498819282698363/1294498958747373668/image.png?ex=670be45a&is=670a92da&hm=6182d8059284ec66b05cdc37d1dd46df9f11a9d137479ac9d52fa3986ba00508&=)

==CURTIN_CTF{Cyb3rW4rri0rPr0t3ctTh3N3tw0rk}==

### Easy Login

**Category**  : Web Exploitation

**Difficulty**  : Easy

**Points**  : 100

**Attachments**  : NA

**Description**  :

**Solution**

Provided with url with port `8001`. It show a login page. Using simple payload to bypass login page.

`admin' or '1'='1'-- # `

==CURTIN_CTF{sql_1nj3ct1on_v1ct0ry}==


### Crumb Trails

**Category**  : Web Exploitation

**Difficulty**  : Easy

**Points**  : 100

**Attachments**  : NA

**Description**  :

**Solution**

Provided with url with port `8002`. It show an images of cookies and mention about `chocolate`. Looking into cookies show that it was `vanilla`. Change the value `vanilla to chocolate` and get the flag.

==CURTIN_CTF{c00kies_cutt3r_is_c00l}==

### Valuable Feedback

**Category**  : Web Exploitation

**Difficulty**  : Medium

**Points**  : 350

**Attachments**  : NA

**Description**  :

**Solution**

Provided with URL. It have feedback form. At first i thought it was XSS but there is no bot visiting this page make XSS useless. Next, i notice on webpage being rendered on whatever we send.

It might use render template at back and might be injected with SSTI.
Trying simple payload to proof it `{{2*2}}` or `{{7*7}}`

and it return `49` proof that it vulnerable. 
using this payload to get the flag.

`{{ self.__init__.__globals__.__builtins__.__import__('os').popen('/fag.txt').read() }}`

==CURTIN_CTF{t3mpl4t3s_ar3_e4sy_t0_cr4ck}==

### Sneaky source

**Category**  : Web Exploitation

**Difficulty**  : Medium

**Points**  : 300

**Attachments**  : NA

**Description**  :

**Solution**

Provided with URL with submit form for any URL in format `http or https`. 

Using burpsuite to play around and notice that we can send parameter `file:///`. 

using `/etc/passwd/` to proof the concept.

Once the concept and proven. We can read the file using 
`/proc/self/cwd/app.py` Just try my luck to read flag in same directory and got the flag.

`/proc/self/cwd/flag.txt`

==CURTIN_CTF{S3rver_S1de_Surf1ng}==

### Internal View

**Category**  : Web Exploitation

**Difficulty**  : Medium

**Points**  : 400

**Attachments**  : NA

**Description**  :

**Solution**

Provided with URL with submit form for any URL as as one challenge before. But the author mention flag at `/internal/flag` with port 5000. 
SSRF challenge and `localhost` or `127.0.0.1` are blocked. But we can bypass it using shortform URL.

`http://127.0.1:5000/internal/flag`

==CURTIN_CTF{S3rver_S1de_Surf1ng}==

### Just Ring

**Category**  : Reverse Engineering
**Difficulty**  : Easy
**Points**  : 100
**Attachments**  : just_ring

**Description**  :

**Solution**

Provided with elf file. Running it and it will return `encoded_secret.txt`. 
`strings encoded_secret.txt | base64-d ` and wrap it with flag format.

==CURTIN_CTF{3l1t3H4ck3rSk1llzF0rTh3W1n2024!}==

### PinValidator

**Category**  : Reverse Engineering

**Difficulty**  : Easy

**Points**  : 250

**Attachments**  : pinvalidator.zip

**Description**  :

**Solution**

Provided with zip file that contain part of apk file. Open using `jadx-gui` to have better read. 
Always check for `/com/applicationname/MainActivity`where it happen to have the main function. 

```
private  static  final  String  HARDCODED_PIN  =  "7331";
private  static  final  String  HARDCODED_STRING  =  "^\"u}B~%F'\"x%bU&r%dZ'p%P&d%`%d";
  ```

Focus on what operation it does in line 26 and 69.
![screebnshot](https://ibb.co/MNCrzZz)
Write a script to return flag.

```
def xor_string(input_str, xor_value):
    result = []
    for char in input_str:
        result.append(chr(ord(char) ^ xor_value))
    return ''.join(result)

if __name__ == "__main__":
    # Your specific input string
    input_str = '^"u}B~%F\'"x%bU&r%dZ\'p%P&d%`%d'
    xor_value = 22  # Example XOR value

    # Call the xor_string function
    result = xor_string(input_str, xor_value)

    # Print the original and XORed results
    print("Original String:", input_str)
    print("XOR Value:", xor_value)
    print("XOR Result:", result)
```

==CURTIN_CTF{H4ckTh3P14n3tC0d3rL1f3F0r3v3r}==

### Divulger Part 1

**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 125

**Attachments**  :NA

**Description**  :
A person with the online alias 'greekguy' had leaked details of Turkish Phone Number IDs.
What e-mail was the person using?
Author: RDxR10

**Solution**

Refer to `x.com` Darkwebmonitoring the 'greekguy` have been mention as threat actor and leaked the information in breachforum.

Unfortunately,  this guys was banned right after posting about leaks data. 

In 2024, one guys name as Emo had post into their telegram about leaked database breachforum V1.

Move towards `breachforum/user-emo` lead to telegram where the database leak can be found. Download the file and using `grep` to ease job.

![](https://cdn.discordapp.com/attachments/1159746540525600859/1294843403586240594/image.png?ex=670c7c64&is=670b2ae4&hm=0aea1bd1068425d8f7509f0a80ff70b10b77ae9eff26c2e907e544dc6391f510&=)

==CURTIN_CTF{kpopayda@gmail.com}==

### Divulger Part 2 

**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 125

**Attachments**  :NA

**Description**  :

We have an intel that the same person had "Self-banned" himself.
When did this happen?
Format: CURTIN_CTF{DD_MM_YYYY}
Author: RDxR10

**Solution**
Since we found the article said the greekguys were banning. Looking at the date and try to put it. 
This posting on 29/11/2024 and we try 28 and flag accepted.

![](https://cdn.discordapp.com/attachments/1287649352881405986/1294569421737689108/image.png?ex=670cceba&is=670b7d3a&hm=59861fde29b76803f1b2438970c03780147031adb9ab93361fc8a6a75966569a&=)

==CURTIN_CTF{28_11_2024}==

### Divulger Part 3


**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 125

**Attachments**  :NA

**Description**  :

It seems greekguy was communicating with Stranger020.
What email was Stranger020 using? When did the person register on the forum?
Format: CURTIN_CTF{email_dd_mm_yyyy}
Author: RDxR10

**Solution**

Since we found the database, using `grep` to get the user.
`grep stranger020`

==CURTIN_CTF{sergeyevic@protonmail.com_12_11_2022}==

### Divulger Part 4

**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 471

**Attachments**  :NA

**Description**  :

There were talks of dumping the entire Health Management database.
Which site were the leakers chatting about?
What was the Storage Engine of the database?
Flag Format: CURTIN_CTF{site_storageengine}
Author: RDxR10

**Solution**
Since we found the database, using `grep` to get the name of mysql.

`grep mysql`

![](https://cdn.discordapp.com/attachments/1287649352881405986/1294650095572221952/image.png?ex=670bc85c&is=670a76dc&hm=20ebae80915354f426a0896f64fe4ea3a81d4899473536370d3d743494032587&=)

==CURTIN_CTF{hsys.saglik.gov.tr_MyISAM}==

### Crash at the Airdrome Part 1

**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 200

**Attachments**  : crash_at_the_airdrome_challenge.png

**Description**  :

A: What was the engine model of the aircraft? 
B: When was the aircraft manufactured? (DD-MM-YYYY)
Flag Format: CURTIN_CTF{A_B}
Author: RDxR10

**Solution**

This images related to incident that happen to Lion Air 904. [Link](https://en.wikipedia.org/wiki/Lion_Air_Flight_904) 
All information about engine and manufactured mention in this Wikipedia.

==CURTIN_CTF{CFM56-7B24E_19-02-2013}==

### Crash at the Airdrome Part 2

**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 298

**Attachments**  : crash_at_the_airdrome_challenge.png

**Description**  :

When was the first flight of this aircraft model?
Flag Format: CURTIN_CTF{DD_MM_YYYY}
Author: RDxR10

**Solution**

Refer to this model, which Boeing 737-800 first flew return the date of first flight. 

==CURTIN_CTF{31_07_1997}}==


### The Package

**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 100

**Attachments**  : NA

**Description**  :

Can you get the details please? All I have is this:
`278154251425`
Where was the package picked up? When? Add the dimensions too (in cms).
Format: CURTIN_CTF{Where_DD_MM_YYYY_l_b_h} 
Author: RDxR10

**Solution**

Using google dork to get which courier and it will automatically show which tracking number belongs to. 

`number + tracking number`
Then track using FedEx

==CURTIN_CTF{Beckton_12_08_2024_60_31_20}==

### Time Travel

**Category**  : OSINT

**Difficulty**  : Easy

**Points**  : 500

**Attachments**  : NA

**Description**  :

I wrote something on my blog page but accidentally deleted it. Can’t recall when. 
Maybe you can travel back in time and find out?

Author: RDxR10

**Solution**
Since this is part of OSINT challenge, and after a few of dead end. I remember previous challenge where we can find user from telegram. 

Moving to user telegram, in bio it show `rdxr10.bearblog.dev`

Another hint say not regular web archive which mean my waybackmachine are useless.

Using google to list all web archive and try it one by one narrow to ghostarchieve website.
[image](https://ibb.co/Xt62wMX)

==CURTIN_CTF{oh_so_this_machine_actually_works_479412}==

### Cracked PDF

**Category**  : Forensics and Stego

**Difficulty**  : Easy

**Points**  : 250

**Attachments**  : cracked.zip

**Description**  :

In this file lies a secret that’s been locked away, protected by a rock solid password. But once you gain access, be careful not to let appearances deceive you. Can you see through the illusion and uncover the real message?

md5sum: ed611d39b0c6c1b63df2ecd9ef40e533 
sha1sum: 1d3e1797ae53bdd8c53796ef3efe2c828a6e22ec
Author: @flabby

**Solution**

Given the zip file and inside it have protected pdf file.
Using JohnTheRipper to get password.

`1. pdf2john cracked.pdf > hash`

`2. john hash --wordlist=/path/to/rockyou/`

Once done, it show `PRETTY` as password. Inside the pdf have something looks like morse code.

Since this is forensics challenge and regarding to pdf file. I always change into text or html using `pdftotext` or `pdftohtml`
This is not morse code but binary where `.` equal to 0 and `-` is 1. Write a simple script to change it and get binary. Using cyberchef to decode it and reveal the flag.

==CURTIN_CTF{UNDERST4NDING_WH1TE_SP4CES_IS_IMP0RT4NT}==

### CrazySignal

**Category**  : Forensics and Stego

**Difficulty**  : Easy

**Points**  : 450

**Attachments**  : crazysignal.zip

**Description**  :
Lost in transmission, a familiar code awaits discovery. But this one wasn't sent the usual way—it’s hidden in a broadcast, distorted by a robotic signal. To recover the message, you’ll need to tune in carefully and decode what’s hidden in plain sight. Can you crack the transmission and reveal the secret?

md5sum: c648727fd299476d220848ac6367e95a
sha1sum: 95a13b689f1e2eb9c90df646d146eabd6bcb6755
Author: @flabby

**Solution**

Given the file in zip and inside it have flag.mpeg. Open the file and the sound quite familiar to me which refer to sstv encoder.
We just need to supply the right format of it either in wav or ogg.
But the issues is my machine cannot read wav file. I need to change mpeg to ogg using online converter. :X

`sstv -d flag.ogg -o flag.png`

It then reveal qr-code. using zbarimg to get the flag.

`zbarimg flag.png`'

==CURTIN_CTF{SP4C3_EXPL0RERS_MU5T_KNOW_AB0U7_SSTV}==

### Inziption

**Category**  : Forensics and Stego

**Difficulty**  : Easy

**Points**  : 150

**Attachments**  : file100.zip

**Description**  :

Defense in Depth is the foundation for robust security, layering protection to deter even the most persistent threats. But what happens when there’s a leak in the armor? Can the sheer depth of protection save the day?

md5sum: c19da4010ecc2f0f734fca5f5c021ec0
sha1sum: a5a3b3a31b7a5b7ecc77dd588daef9594826c42d
Author: @flabby

**Solution**

Given the file in zip and inside it have another zip file that being protected.  It similar to Musyoka dolls.  The tricks is, previous file comment are password for current file. Example : file100 comment is password for file99. It keep loop until 0 where flag located in comment. Unfortunately my script not working well. So i need to do it manually. XDS

==CURTIN_CTF{1_H0P3_YOU_AUT0M4TED_TH1S_Z1P_B0MB}==

