
Challenge description :

Use burpsuite !

#### First Step :

So firstly in the website I fill all the input then I analyze all the request with burpsuite then I see that I need to bypass to OTP because otherwise it will say this : Invalid OTP



#### Second Step : 

In the POST request where there is the otp I tried to delete the OTP then send the request again and here we go I got the Flag 

![](../../PicoCTF-assets/Pasted%20image%2020260221014743.png)


Initially it was like this :

![](../../PicoCTF-assets/Pasted%20image%2020260221014756.png)







Flag : picoCTF{#0TP_Bypvss_SuCc3$S_2e80f1fd}
