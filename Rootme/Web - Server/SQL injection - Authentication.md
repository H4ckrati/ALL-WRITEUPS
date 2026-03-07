#SQL #SQLi #OR1
#### Challenge Description

 Authentication v 0.01


Let's test a basic payload of SQL injection to see how the website reacts :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306214909.png)

>[!type]
> Explanation : This simple payload comments the rest of the input so the password will be commented ( not required anymore)


 This basic payload worked, now let's find the password in the source page (control + u)

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306214737.png)


Flag : t0_W34k!$


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

