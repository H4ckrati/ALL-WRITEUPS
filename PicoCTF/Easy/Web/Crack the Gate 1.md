![](Pasted%20image%2020260219162428.png### Challenge Description : 

We’re in the middle of an investigation. One of our persons of interest, ctf player, is believed to be hiding sensitive data inside a restricted web portal. We’ve uncovered the email address he uses to log in: `ctf-player@picoctf.org`. Unfortunately, we don’t know the password, and the usual guessing techniques haven’t worked. But something feels off... it’s almost like the developer left a secret way in. Can you figure it out?

![](../../PicoCTF-assets/Pasted%20image%2020260219162428.png)
Additional details will be available after launching your challenge instance.

#### First Step

I opened the website, I go to inspect and I see a coded message in ROT 13 

![](Pasted%20image%2020260219162110.png)
I then decode it and it says :
NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"

#### Second Step

So to change the request in Burpsuite in the POST request in Repeater, I added this header :
X-Dev-Access: yes

![](Pasted%20image%2020260219162428.png)

#### Third Step 

I sent the modified request and I got this result :

![](Pasted%20image%2020260219162649.png)

Flag : picoCTF{brut4_f0rc4_1a386e6f}