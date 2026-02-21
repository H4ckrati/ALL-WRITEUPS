
Challenge description :

Use burpsuite !

#### First Step :

So firstly in the website I fill all the input then I analyze all the request with burpsuite then I see that I need to bypass to OTP because otherwise it will say this : Invalid OTP



#### Second Step : 

In the POST request where there is the otp I tried to delete the OTP then send the request again and here we go I got the Flag 

![[../../../ALL-assets/Pasted image 20260219191303.png]]


Initially it was like this :

![[../../../ALL-assets/Pasted image 20260219191353.png]]







Flag : picoCTF{#0TP_Bypvss_SuCc3$S_2e80f1fd}
