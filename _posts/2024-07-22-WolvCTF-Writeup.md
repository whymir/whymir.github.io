# WOLV CTF 2024
![image](https://hackmd.io/_uploads/Hk19-rQ06.png)


## Beginner
###  Web: The Gauntlet
**Description**
Can you survive the gauntlet?
10 mini web challenges are all that stand between you and the flag.

Note: Automated tools like sqlmap and dirbuster are not allowed (and will not be helpful anyway).
*https://gauntlet-okntin33tq-ul.a.run.app* 

**Solution**
```
Landing page and inspect the source code.
Have hidden path
```
![image](https://hackmd.io/_uploads/r1ReMHQAa.png)
The page now need to perform same task.
![image](https://hackmd.io/_uploads/Hy3GMSQ0a.png)


###  Pwn: babypwn
Just a little baby pwn.
nc babypwn.wolvctf.io 1337 

###  Forensics: Hidden Data
WOLPHV sent me this file. Not sure what to comment about it

![image](https://hackmd.io/_uploads/S1eEFBXCT.png)

![image](https://hackmd.io/_uploads/HJw8KrQCp.png)

==wctf{h1dd3n_d4t4_n0T_s0_h1dD3N}==

## Misc
###  Sanity Check

Join the Discord and read the #rules

Happy Hacking and Go Blue!
![image](https://hackmd.io/_uploads/ryOpVP7Ca.png)
==wctf{4_c0py_4nd_p4s+3_r4c3}==

###  Made Sense

i couldn't log in to my server so my friend kindly spun up a server to let me test makefiles. at least, they thought i couldn't log in :P
https://madesense-okntin33tq-ul.a.run.app 

## Forensics
###  Eternally Pwned: Infiltration

I recently had my passwords and other sensitive data leaked, but I have no idea how. Can you figure out how the attacker got in to my PC?

```
By reading all the action, we can see that this user being attack via smb which might be cause from eternal blue. 
```
![image](https://hackmd.io/_uploads/HkXiqHQ0T.png)
Decode all base64 and get the flag.
![image](https://hackmd.io/_uploads/rJAR5SQR6.png)
==wctf{l3tS_3teRn4lLy_g0_bLU3_7n9wm4iWnL}==

###  Eternally Pwned: Persistence

I get that the attackers were in my PC, but how did they achieve persistence?

MEMORY.DMP:

```
Looking into cmdline and pslist and notice that one suspicious file being execute. Actually im stuck here since my volatility cannot dump that file. But  one of friends said dont overthinking. I just put into decode and it appear to have new link. :X
```
![image](https://hackmd.io/_uploads/rJBXSIm0T.png)

![image](https://hackmd.io/_uploads/SJbVrUm0p.png)

==wctf{v0lAt1l3_m3m0ry_4qu1r3D_a3fe9fn3al}==

###  Eternally Pwned: Exfiltration

Ok yeah, they are definitely in the machine. But how did they manage to take my data?

You will likely find both the packet capture from Eternally Pwned: Infiltration and the memory dump from Eternally Pwned: Persistence to be useful

==wctf{ahk-5cr1pt5-4r3-d4ng3r0u5-dirmgaiwpd}==

###  Site Secret
There's been a secret flag on this website the whole time???

That's an interesting background...


---


## Web
###  Bean Cafe

This cool cafe offers a super secret menu! But it's being guarded by some AI detection systems. Hopefully we can get through it to try the coffee...

Note: Automated tools like sqlmap and dirbuster are not allowed (and will not be helpful anyway).
https://bean-cafe-okntin33tq-ul.a.run.app 

###  Upload Fun
I made a website where you can upload files.

What could go wrong?

Note: Automated tools like sqlmap and dirbuster are not allowed (and will not be helpful anyway).
https://upload-fun-okntin33tq-ul.a.run.app 

## Osint
###  WOLPHV I: Reconnaissance
100
dree_

A new ransomware group you may have heard about has emerged: WOLPHV

There's already been reports of their presence in articles and posts.

NOTE: Wolphv's twitter/X account and https://wolphv.chal.wolvsec.org/ are out of scope for all these challenges. Any flags found from these are not a part of these challenges

This is a start to a 5 part series of challenges. Solving this challenge will unlock WOLPHV II: Infiltrate

```
Long journey enough but their twitter group @wolphvgroup make some post that point to out of scope. Since this is osint challenge, reading through all comment and find one account @joeosint_ with noted need to investigate. The result was rot13 + base64.
```
![image](https://hackmd.io/_uploads/SJoI3wQRp.png)

==wctf{0k_1_d0nT_th1Nk_A1_w1ll_r3Pl4c3_Us_f0R_4_l0ng_t1me}==

###  WOLPHV II: Infiltrate

Since the WOLPHV twitter/x is out of comission now, I wonder where else the official WOLPHV group posts on social media. Maybe we can also infiltrate what they use to message each other

NOTE: Wolphv's twitter/X account and https://wolphv.chal.wolvsec.org/ are out of scope for all these challenges. Any flags found from these are not a part of these challenges

Solving this challege will unlock WOLPHV III, WOLPHV IV, and WOLPHV V


## Resources Tools
