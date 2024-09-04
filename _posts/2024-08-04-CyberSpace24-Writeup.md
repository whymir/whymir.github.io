# Beginner
## Encryptor
File : [encryptor.apk](https://pixeldrain.com/u/etV6XCqR)

Given android file (.apk). To start, read the source code by extracting the apk using MobSF tools or apktool. 

### Solve by Tools
![image](https://hackmd.io/_uploads/Skg1uKB20.png)

Above image give the overview on apk being uploaded. Our main focus here is Source code. The dashboard also show interesting part on API where it use base64 encoded and decoded and also crypto.

![image](https://hackmd.io/_uploads/r1NEuYSnR.png)

Moving into source code name as `*MainActivity.java*` or located at `com.example.encryptor.MainActivity`

![image](https://hackmd.io/_uploads/S1NOdtShR.png)
![image](https://hackmd.io/_uploads/HJTWcFrh0.png)

:::info
The snippet code show the encryption part using Blowfish. 
The key being encoded using base64. 
`(ZW5jcnlwdG9yZW5jcnlwdG9y)`
The result will be save in asset name as `enc.txt`
:::


### Solve by apktool
Run below script to decode the apk.
`apktool d -p . encryptor.apk `

The decode file in smali. To read it, use `jadx-gui` where it convert smali to java. 

![image](https://hackmd.io/_uploads/BkkRitS30.png)

We know that the encryption operation happen here. The encryption using blowfish with key being encoded using base64. 

![image](https://hackmd.io/_uploads/HJuyhtS2R.png)
The encryption then being save to asset file name as `enc.txt`.

Locate the directory `/asset` and file `enc.txt` to get the encryption text. 

![image](https://hackmd.io/_uploads/BkbEhYr2R.png)

Using any online tools to decode blowfish encryption.

`key = encryptorencryptor`

![image](https://hackmd.io/_uploads/ryCMoFr3C.png)

==CSCTF{3ncrypt0r_15nt_s4Fe_w1th_4n_h4Rdc0d3D_k3y!}==