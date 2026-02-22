#SSTI #Sanitization
#### Challenge Description 

I made a cool website where you can announce whatever you want! I read about input sanitization, so now I remove any kind of characters that could be a problem :)I heard templating is a cool and modular way to build web apps! Check out my website [here](http://shape-facility.picoctf.net:49888/)!


#### First Step 

We know that it is a Server-side Template Injection vulnerability and as you can see, there will be input sanitization according to the description.

So I tried to type {{7fois7}} to see if it is vulnerable 

![](../../PicoCTF-assets/Pasted%20image%2020260222115046.png)

And boom it rendered 49 so it is vulnerable to SSTI 


#### Second Step 

I need to detect which characters is sanitized in the website and I found 