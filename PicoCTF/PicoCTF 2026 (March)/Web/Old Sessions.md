
###### Challenge Description : 

Proper session timeout controls are critical for securing user accounts. If a user logs in on a public or shared computer but doesn’t explicitly log out (instead simply closing the browser tab), and session expiration dates are misconfigured, the session may remain active indefinitely.This then allows an attacker using the same browser later to access the user’s account without needing credentials, exploiting the fact that sessions never expire and remain authenticated.Your friend tells you to check out a new social media platform he built a few years ago. Although its still under development, he said the site is almost complete. He also mentioned that he hates constantly logging into sites, and so has made his page that 'once you login, you never have to log-out again'! Browse [here](http://dolphin-cove.picoctf.net:63399/), and find the flag!


##### Finding the admin account :

When I went to the mainpage, I saw this comment : 

"Hey I found a strange page at /sessions"

![](../../PicoCTF-assets/Pasted%20image%2020260309225122.png)


After visiting the /sessions pag
![](../../PicoCTF-assets/Pasted%20image%2020260309224941.png)



