# picoCTF 2019
Second CTF training for 2024. 

    Starting : 15/2/2024
    End      : xxxxxx
    
Pending Task:

---

## General Skill
### Lets Warm Up
| 50 points
Author: Sanjay C/Danny Tunitis
**Description**
If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII? 

0x70 = p
==picoCTF{p}==

### Warmed Up
| 50 points
Author: Sanjay C/Danny Tunitis
Description**
What is 0x3D (base 16) in decimal (base 10)?

==picoCTF{61}==

### 2Warm
| 50 points
Author: Sanjay C/Danny Tunitis
**Description**
Can you convert the number 42 (base 10) to binary (base 2)? 

==picoCTF{101010}==

### what's a net cat?
| 100 points
Author: Sanjay C/Danny Tunitis
**Description**
Using netcat (nc) is going to be pretty important. Can you connect to jupiter.challenges.picoctf.org at port 41120 to get the flag?

`nc jupiter.challenges.picoctf.org 41120`

==picoCTF{nEtCat_Mast3ry_3214be47}==

### strings it
| 100 points
Author: Sanjay C/Danny Tunitis
**Description**
Can you find the flag in file without running it?

`strings file | grep "picoC"`

==picoCTF{5tRIng5_1T_7f766a23}==

### Bases
| 100 points
Author: Sanjay C/Danny T
**Description**
What does this `bDNhcm5fdGgzX3IwcDM1` mean? I think it has something to do with bases.

==picoCTF{l3arn_th3_r0p35}==

### First Grep
| 100 points
Author: Alex Fulton/Danny Tunitis
**Description**
Can you find the flag in file? This would be really tedious to look through manually, something tells me there is a better way.

`strings file | grep "picoC" `

==picoCTF{grep_is_good_to_find_things_5af9d829}==


### Based
| 200 points
Author: Alex Fulton/Daniel Tunitis
**Description**
To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program to prove you are on the way to becoming 1337? Connect with nc jupiter.challenges.picoctf.org 29956.

`nc jupiter.challenges.picoctf.org 29956 `


==picoCTF{learning_about_converting_values_b375bb16}==

### plumbing
| 200 points
Author: Alex Fulton/Danny Tunitis
**Description**
Sometimes you need to handle process data outside of a file. Can you find a way to keep the output from this program and search for the flag? Connect to jupiter.challenges.picoctf.org 4427.

`nc jupiter.challenges.picoctf.org 4427 | grep pico`

==picoCTF{digital_plumb3r_5ea1fbd7}==


### mus1c
| 300 points
Author: Danny
**Description**
I wrote you a song. Put it in the picoCTF{} flag format.

```
Pico's a CTFFFFFFF
my mind is waitin
It's waitin

Put my mind of Pico into This
my flag is not found
put This into my flag
put my flag into Pico


shout Pico
shout Pico
shout Pico
--- snippet---
```
Rockstar esolag. Decode it [online](https://codewithrockstar.com/online).

==picoCTF{rrrocknrn0113r}==

### flag_shop
| 300 points
Author: Danny
**Description**
There's a flag shop selling stuff, can you buy a flag? Source. Connect with nc jupiter.challenges.picoctf.org 9745.

```
#include <stdio.h>
#include <stdlib.h>
int main()
{
    setbuf(stdout, NULL);
    int con;
    con = 0;
    int account_balance = 1100;
    
                if(number_flags > 0){
                    int total_cost = 0;
                    total_cost = 900*number_flags;
                    printf("\nThe final cost is: %d\n", total_cost);
                    if(total_cost <= account_balance){
                        account_balance = account_balance - total_cost;
                        printf("\nYour current balance after transaction: %d\n\n", account_balance);
                    }
                    else{
                        printf("Not enough funds to complete purchase\n");
                    }                           
          else if(auction_choice == 2){
                printf("1337 flags cost 100000 dollars, and we only have 1 in stock\n");
                printf("Enter 1 to buy one");
                int bid = 0;
                fflush(stdin);
                scanf("%d", &bid);
```
Initial value in account 1100 and the flag price 100000. It so much different here. But the vuln can be found if we manage to get total cost into negative value? But how? Here the problem since we cannot enter negative value. How about interger overflow?

`3000000`

==picoCTF{m0n3y_bag5_65d67a74}==

### 1_wanna_b3_a_r0ck5tar
| 350 points
Author: Alex Bushkin
:warning: 
The Rockstar language has changed since this problem was released! Use this Wayback Machine URL to use an older version of Rockstar, here.

**Description**
I wrote you another song. Put the flag in the picoCTF{} flag format

==picoCTF{BONJOVI}==

---


## Forensics

### Glory of the Garden
| 50 points
Tags: 

Author: jedavis/Danny
Description
This garden contains more than it seems.

`strings file`

==picoCTF{more_than_m33ts_the_3y33dd2eEF5}==
### So Meta
| 150 points
Tags: 

Author: Kevin Cooper/Danny
Description
Find the flag in this picture.

```
strings filename
exiftool filename
```

==picoCTF{s0_m3ta_d8944929}==

### Shark on wire 1
| 150 points
Tags: 

Author: Danny
Description
We found this packet capture. Recover the flag.

![image](https://hackmd.io/_uploads/ByvN8Kjj6.png)

The file stream in UDP, follow the UDP and get the flag.


==picoCTF{StaT31355_636f6e6e}==


### extensions
| 150 points
Tags: 

Author: Sanjay C/Danny
Description
This is a really weird text file TXT? Can you find the flag?

![image](https://hackmd.io/_uploads/SJV0IKija.png)
`tesseract f.png outputbase`

==picoCTF{now_you_know_about_extensions}==

### What Lies Within
| 150 points
Tags: 

Author: Julio/Danny
Description
There's something in the building. Can you retrieve the flag?
`zsteg imge`
==picoCTF{h1d1ng_1n_th3_b1t5}==

### m00nwalk
| 250 points
Tags: 

Author: Joon
Description
Decode this message from the moon.

Using qsstv to intercept the sound and convert into image.

==picoCTF{beep_boop_im_in_space}==

### WhitePages
| 250 points
Tags: 

Author: John Hammond
Description
I stopped using YellowPages and moved onto WhitePages... but the page they gave me is all blank!

Reading the hex, it appear to have same pattern. 
```
E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 20 20 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 20 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 20 20 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 20 20 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 20 E2 80 83 20 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 20 20 20 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 20 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 20 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 20 E2 80 83 E2 80 83 20 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 20 20 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 E2 80 83 E2 80 83 20 20 E2 80 83 20 20 20 20 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 E2 80 83 E2 80 83 20 E2 80 83 E2 80 83 20 

```

```
from pwn import *

with open("whitepages.txt", "rb") as bin_file:
    data = bytearray(bin_file.read()) 
    data = data.replace(b'\xe2\x80\x83', b'0')
    data = data.replace(b'\x20', b'1')
    data = data.decode("ascii")
    print unbits(data)
```
E2 80 83 = 0
20 = 1
Convert all and change to ascii.

==picoCTF{not_all_spaces_are_created_equal_7100860b0fa779a5bd8ce29f24f586dc}==

### c0rrupt
| 250 points
Tags: 

Author: Danny
Description
We found this file. Recover the flag.


==picoCTF{c0rrupt10n_1847995}==

### like1000
| 250 points
Tags: 

Author: Danny
Description
This .tar file got tarred a lot.



---


## Reverse Engineering
## PWN
### seed-sPRiNG
| 350 points
Tags: 

Author: John Hammond
Description
The most revolutionary game is finally available: seed sPRiNG is open right now! seed_spring. Connect to it with nc jupiter.challenges.picoctf.org 34558.
 
The seed will taken by current time. So we need to create random_seed generator and send it to service.

```
#include <stdio.h> 
#include <time.h>
#include <stdlib.h> 
  
int main () 
{ 
    int i;
      
    srand(time(0)); 
    
    for (i = 0; i < 30; i++)
    {
        printf("%d\n", rand() & 0xf); 
    }
      
    return 0; 
} 

```

==picoCTF{pseudo_random_number_generator_not_so_random_81b0dd7e}==

### 

## Web

### Insp3ct0r
| 50 points
Author: zaratec/danny
**Description**
Kishor Balan tipped us off that the following code may need inspection:

It mention about inspect. Open the browser and check the source.

Part 1 = picoCTF{tru3_d3 //found in main.
Part 2 = t3ct1ve_0r_ju5t // found in css
Part 3 = _lucky?2e7b23e3} // ound in js

Combine all and submit the flag. 

==picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?2e7b23e3}==

### where are the robots
| 100 points

Author: zaratec/Danny
**Description**
Can you find the robots? 

Put directory /robots.txt and follow the directory given .

```
/1bb4c.html

```
==picoCTF{ca1cu1at1ng_Mach1n3s_1bb4c}==

### logon
| 100 points
Tags: 

Author: bobson
**Description**
The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? 


Briefly description that we need to access as Joe and the password can be anything. But the trick is, hiding from all user which mean need to be admin
```
Joe : Joe
Cookie: admin : false - true

```

==picoCTF{th3_c0nsp1r4cy_l1v3s_0c98aacc}==

### dont-use-client-side
| 100 points
Tags: 

Author: Alex Fulton/Danny
**Description**
Can you break into this super secure portal? 

```
function verify() {
    checkpass = document.getElementById("pass").value;
    split = 4;
    if (checkpass.substring(0, split) == 'pico') {
      if (checkpass.substring(split*6, split*7) == '706c') {
        if (checkpass.substring(split, split*2) == 'CTF{') {
         if (checkpass.substring(split*4, split*5) == 'ts_p') {
          if (checkpass.substring(split*3, split*4) == 'lien') {
            if (checkpass.substring(split*5, split*6) == 'lz_b') {
              if (checkpass.substring(split*2, split*3) == 'no_c') {
                if (checkpass.substring(split*7, split*8) == '5}') {
                  alert("Password Verified")
```
Clear text password but in order. Reorder it and submit the password

==picoCTF{no_clients_plz_b706c5}==

### picobrowser
| 200 points
Tags: 

Author: Archit
Description
This website can be rendered only by picobrowser, go and catch the flag! 

Open the browser and try to get flag. 
:interrobang: You're not picobrowser! Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0 


Well said that we need to change agent to picobrowser.


1. Using burpsuite
![image](https://hackmd.io/_uploads/Sk1uskjia.png)
Change the user-agent to picobrowser
![image](https://hackmd.io/_uploads/ByTioyio6.png)


```
2. Curl

curl -A "picobrowser" IP
```
==picoCTF{p1c0_s3cr3t_ag3nt_84f9c865}==

### Client-side-again
| 200 points
Tags: 

Author: Danny
Description
Can you break into this super secure portal? 

![image](https://hackmd.io/_uploads/r1eknyiop.png)


```
 */
function verify() {
  checkpass = document[_0x4b5b("0x0")]("pass")[_0x4b5b("0x1")];
  /** @type {number} */
  split = 4;
  if (checkpass[substring](0, split * 2) == _0x4b5b("0x3")) // "picoCTF{" 
  {
    if (checkpass[substring](7, 9) == "{n") {
      if (checkpass[substring](split * 2, split * 2 * 2) == _0x4b5b("0x4"))//not_this" {
        if (checkpass[substring](3, 6) == "oCT") {
          if (checkpass[substring](split * 3 * 2, split * 4 * 2) == _0x4b5b("0x5")) //f49bf} {
            if (checkpass["substring"](6, 11) == "F{not") {
              if (checkpass[substring](split * 2 * 2, split * 3 * 2) == _0x4b5b("0x6")) //_again_e{
                if (checkpass[substring](12, 16) == _0x4b5b("0x7")) //this {
                  alert(_0x4b5b("0x8"));
                }
              }
            }
          }
        }
      }
    }
  } else {
    alert(_0x4b5b("0x9"));
  }
}
```

[deobfuscate](https://deobfuscate.relative.im/) the js and reorder the flag.

==picoCTF{not_this_again_ef49bf}==

### Irish-Name-Repo 1
| 300 points
Tags: 

Author: Chris Hensler
**Description**

Do you think you can log us in? Try to see if you can login!

![image](https://hackmd.io/_uploads/Bk521xii6.png)

```
/support.html
Hi. I tried adding my favorite Irish person, Conan O'Brien. But I keep getting something called a SQL Error

Can you help me find my parents. I think they were Irish.
```

Basically SQL error point to SQL Injection. Let try inject in password form.

`' or '1'='1' -- `

==picoCTF{s0m3_SQL_c218b685}==


### Irish-Name-Repo 2
| 350 points
Tags: 

Author: Xingyang Pan
**Description**

Someone has bypassed the login before, and now it's being strengthened. Try to see if you can still login! 

`admin'--`
It just filter password not username. Inject still can be happen.

==picoCTF{m0R3_SQL_plz_fa983901}==

### Irish-Name-Repo 3
| 400 points
Tags: 

Author: Xingyang Pan
Description
Try to see if you can login as admin!

using curl to debug the operation happen.

`
curl IP --data "password=test&debug=1"
`
![image](https://hackmd.io/_uploads/HJIBQlsia.png)

Somehow it happen to have some encryption with rot13. Let put our payload

`' or 1=1'-- to ' be 1=1--'

==picoCTF{3v3n_m0r3_SQL_06a9db19}==

### JaWT Scratchpad
| 400 points
Tags: 

Author: John Hammond
Description
Check the admin scratchpad! 

We cannot login as admin, but can be anybody.
![image](https://hackmd.io/_uploads/BJ7GHeoj6.png)

Change the jwt cookies from our name to admin.
```
using JWT.io to decode. 
Using JTR to crack sha256.
Change user to admin
Weak credential - ilovepico
```

==picoCTF{jawt_was_just_what_you_thought_1ca14548}==
    
### Java Script Kiddie
| 400 points
Tags: 

Author: John Johnson
Description
The image link appears broken..


### Java Script Kiddie 2
| 450 points
Tags: 

Author: John Johnson
Description
The image link appears broken... twice as badly... 




## Crypto

## Resources Tools
1. https://codewithrockstar.com/online
2. https://deobfuscate.relative.im/