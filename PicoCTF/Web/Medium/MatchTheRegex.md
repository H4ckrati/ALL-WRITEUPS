
#### Challenge Description 

How about trying to match a regular expressionThe website is running [here](http://saturn.picoctf.net:54097/).


### First Step 

I noticed that below the `let val` **statement**, there is a comment **showing** what is accepted as a `val`, which is `^p.....F!?`

![](ALL-WRITEUPS/PicoCTF/PicoCTF-assets/Pasted%20image%2020260222125257.png)
#### Second Step

This is regex and it means that :
- ^ → Start of the string
- p → The string must start with lowercase **p**
- ..... → Exactly **5 characters**
- F → Then uppercase **F**
- !? → Optional **!** 
#### Third Step 

Logically, because we want a string to access the flag. So let's try the string passwdF according to the regex and it worked 

![](ALL-WRITEUPS/PicoCTF/PicoCTF-assets/Pasted%20image%2020260222125133.png)

Flag : picoCTF{succ3ssfully_matchtheregex_c64c9546}