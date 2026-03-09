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

*In fact, changing the alg is removing the signature so I could then forge the cookie only with a valid username (no need to get the signature)* ;

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142749.png)

Then go in inspect, change the cookie in the main page and here you go :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142848.png)


Flag : S1gn4tuR3_v3r1f1c4t10N_1S_1MP0Rt4n7


## Explanation of JWT : 

JWT is JWT contains 3 parts : 

>[!note]   See the image above 
Header , --> informations for example a username
Payload, --> the typ and alg
Signature --> This is very important for security
The signature = Header + Payload + Secret Key + Hash



It is in Json format.

  

  
>[!note] BurpSuite and JWT
Burpsuite scan identifies if there is any flaws with jwt token


>[!note] How to crack signatures ? 
>Basically hashcat tries to find the good combination to get the same result at the signature. So hashcat with every word of the wordlist do a mathematic calcul and combine them with the header and payload and tries to see if its = your signature 
>```
>hashcat -a 0 -m 16500 <jwt> <wordlist>
>```

