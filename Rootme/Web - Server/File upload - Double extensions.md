
##### Challenge Description 

Your goal is to hack this photo galery by uploading PHP code.  
Retrieve the validation password in the file .passwd at the root of the application.


When I upload my malicious php file : 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306224114.png)

Because of the Challenge Name, I tried to replace the file extension .php to .php.png :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306224328.png)

It worked !

Now let's find the flag in .passwd ! But where is it located ?

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306224703.png)

After some research I found the .passwd location 

>[!note]
>```
>```ls -la```  allows me to find every file even the ones that are hidden 

Because I know that the file is located in ../../../.passwd

Flag : Gg9LRz-hWSxqqUKd77-_q-6G8