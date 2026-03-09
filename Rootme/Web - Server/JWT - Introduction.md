#jwt #JWT #JWT-signature #JWT-SIGNATURE
##### Challenge Description 

To validate the challenge, connect as admin.

https://portswigger.net/web-security/jwt

#### What is a JWT ? : 

A JWT (JSON Web Token) is a compact, URL-safe way to represent information between two parties as a JSON object. It is most commonly used for authentication because it is digitally signed, ensuring that the data inside hasn't been tampered with after being issued.


#### First, to get a cookie let's login as guest,

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142947.png)


Let's see the cookie in this site : 

https://fusionauth.io/dev-tools/jwt-decoder

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309143110.png)


#### Get the flag by changing the JWT :

**Then to get into the account as admin, I would need to desactivate the algorithm (alg) and put none. Also to modify the username to admin to get into his account ;

*In fact, changing the alg is removing the signature so I could then forge the cookie only with a valid username (no need to get the signature)* ;

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142749.png)

Then go in inspect, change the cookie in the main page and here you go :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260309142848.png)


Flag : S1gn4tuR3_v3r1f1c4t10N_1S_1MP0Rt4n7


## Explanation of JWT : 

JWT is in JSON format and it contains 3 parts : 

>[!note]   See the image above 
Header , --> informations for example a username
Payload, --> the typ and alg
Signature --> This is very important for security
The signature = Header + Payload + Secret Key + Hash

  
>[!note] BurpSuite and JWT
Burpsuite scan identifies if there is any flaws with jwt token


>[!note] How to crack signatures ? 
>Basically hashcat tries to find the good combination to get the same result at the signature. So hashcat with every word of the wordlist do a mathematic calcul and combine them with the header and payload and tries to see if its = your signature 
>```
>hashcat -a 0 -m 16500 <jwt> <wordlist>
>```

