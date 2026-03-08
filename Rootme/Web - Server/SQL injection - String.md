#SQL #SQlite #sql #sql-strings #SQL-strings
###### Challenge Description
https://www.root-me.org/fr/Documentation/Web/Injection-SQL


### Find the attack Vector

I tried a basic SQL injection in the login like this but it did not work :

```
login=administrator' OR '1'='1#&password=AD
```

I tried a basic SQL injection in the research section and it worked ! :


All info about SQL injection - String :

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md#sqlite-string

###### First I discovered that,

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308100151.png)

The Select doesn't worked like showed in the github :
##### Explanation 

To sum it up, `UNION SELECT` acts as a connector that attaches your own data request to the end of the website's original query, allowing you to extract secret information like passwords. A simple `SELECT` fails because it creates two separate, conflicting commands that the database cannot process simultaneously, leading to a syntax error.


### Find the password :

The utility of that command was to force the database to reveal its own internal "blueprint" (schema), allowing you to see exactly how tables like `users` are structured and which columns contain the passwords you need to extract.

```
' UNION SELECT 1,sql FROM sqlite_master--
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308095912.png)


Payload to find the admin password :

```
`' UNION SELECT username, password FROM users--`
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308095701.png)

Flag : c4K04dtIaJsuWdi