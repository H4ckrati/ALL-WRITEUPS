#jwt #JWT #JWT-signature #JWT-SIGNATURE
##### Challenge Description 

To validate the challenge, connect as admin.


What is a JWT ? : 



First, to get a cookie let's login as guest,

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142947.png)


Let's see the cookie in this site : 

https://fusionauth.io/dev-tools/jwt-decoder

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309143110.png)

**Then to get into the account as admin, I would need to desactivate the algorithm (alg) and put none. Also to modify the username to admin to get into his account ;

*In fact, *

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142749.png)

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142848.png)









Flag : S1gn4tuR3_v3r1f1c4t10N_1S_1MP0Rt4n7