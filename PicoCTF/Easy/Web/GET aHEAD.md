
#### Challenge Description 

Find the flag being held on this server to get ahead of the competitionhttp://wily-courier.picoctf.net:54196/

#### First Step

Access the given URL in browser and capture request/response using Burp Suite tool.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*UFY3fM5HO_s3eNcsOFBbJA.png)

We can notice only 2 buttons present in the application which changes color of the page on click.  
Click on the both buttons and analyze all captured traffics till now in the tool, however nothing looks interesting.  
Challenge name i.e. “**Get aHEAD**” seems to be a hint as **HEAD** is one of the known HTTP method which we can try in any of the captured request. Change method from GET to HEAD and generate response.

#### Second Step

![](https://miro.medium.com/v2/resize:fit:700/1*1DLnAcaVQQKIBNh-KSovnA.png)

It can be noticed that Flag value is revealed in the response.

![[Pasted image 20260220120310.png]]

Flag : picoCTF{r3j3ct_th3_du4l1ty_8b13f07}