
#### Challenge Description 

Who doesn't love cookies? Try to figure out the best one.http://wily-courier.picoctf.net:54494/

### Step by Step 

Access the given URL in browser and capture request/response using Burp Suite tool.  
Provide any random text i.e. “kamal” and click on the **Search** button. Application will show a message as “That doesn’t appear to be a valid cookie.” and [http://mercury.picoctf.net:17781/search](http://mercury.picoctf.net:17781/search) endpoint as POST method is captured in burp which will generate 302 FOUND response and will redirect to home page.  
Analyze the generated cookies as challenge description pointing to it. It can be noticed that cookie parameter “name” is set to -1.

Press enter or click to view image in full size


![](https://miro.medium.com/v2/resize:fit:1400/1*_gahDAeOQAw-i7ZY0-Dpnw.png)


Now provide the same value “**snickerdoodle**” which is reflecting in Search input field and analyze traffic. Application will show a message as “That is a cookie! Not very special though…” and [http://mercury.picoctf.net:17781/search](http://mercury.picoctf.net:17781/search) endpoint as POST method is captured in burp which will generate 302 FOUND response and will redirect to **/check** endpoint. Also, this time **cookie parameter “name” is set to 0**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*liwuFvi3R_ZkMj4bK0vqww.png)

Send [http://mercury.picoctf.net:17781/check](http://mercury.picoctf.net:17781/check) request in Repeater and change name value to 1 then 2. Generate responses and observe different response is generated as per the Cookie value.  
Send this request to Intruder and add payload in **name** parameter value to attack as Numbers payload type. Observe a different response length for the **Cookie: name=18**. It can be observed that Flag value is revealed in the response for this request.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*HykYQR1nV5h9rKm5ZkbLOw.png)

Challenge Solved. Thanks..

Flag : picoCTF{3v3ry1_l0v3s_c00k135_a4dadb49}