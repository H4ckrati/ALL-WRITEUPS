
#### Challenge Description 

Can you get the flag?Go to this [website](http://saturn.picoctf.net:58277/) and see what you can discover.


#### First Step 

I logged in with random credentials and I then inspect the page with the invalid login page which allows me to analyze how to the website checkthepassword

![](../../PicoCTF-assets/Pasted%20image%2020260221014811.png)

But where is the function checkPassword ?

#### Second Step 

I then explore the file secure.js and I found the credentials 

![](../../PicoCTF-assets/Pasted%20image%2020260221014824.png)

username = admin                 &       password= strongPassword098765

#### Third Step

Login with these credentials and find the flag

![](../../PicoCTF-assets/Pasted%20image%2020260221014840.png)

Flag : picoCTF{j5_15_7r4n5p4r3n7_b0c2c9cb}