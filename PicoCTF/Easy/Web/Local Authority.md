
#### Challenge Description 

Can you get the flag?Go to this [website](http://saturn.picoctf.net:58277/) and see what you can discover.


#### First Step 

I logged in with random credentials and I then inspect the page with the invalid login page which allows me to analyze how to the website checkthepassword

![[../../../ALL-assets/Pasted image 20260219194302.png]]

But where is the function checkPassword ?

#### Second Step 

I then explore the file secure.js and I found the credentials 

![[../../../ALL-assets/Pasted image 20260219194403.png]]

username = admin                 &       password= strongPassword098765

#### Third Step

Login with these credentials and find the flag

![[../../../ALL-assets/Pasted image 20260219194512.png]]

Flag : picoCTF{j5_15_7r4n5p4r3n7_b0c2c9cb}