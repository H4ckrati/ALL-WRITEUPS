##### Challenge Description 

Find a vulnerabilty in this service and exploit it.
You must manage to read index.php

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306125406.png)


Because of the Challenge name and Description, I tried an php command injection in Burpsuite by putting  ; then a command for example id :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306125109.png)

It worked so it is vulnerable ; now let's find the flag 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306125310.png)

We can see the flag .passwd which is very interesting let's explore it :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306125351.png)


Flag : S3rv1ceP1n9Sup3rS3cure

## WRONG CODE : 

>[!danger]
> ```
> $command = "ping -c 3 " . $target_ip;
> ```
> 	Here the ```$target_ip ``` is very dangerous because it allows you to execute any command by following the ip with ; (to execute another command after the first one)


## SOLUTION CODE :

This code wraps the IP in quotes, which means the `ls` command won't be executed as code, but will be treated as a string.

>[!type]
>``` 
>// SECURE VERSION
$safe_ip = escapeshellarg($target_ip);

