#### Challenge Description :

Get an administrator access to the webpage.


According to the challenge name and to the webpage, I thought that I needed to change an HTTP Header to get the flag :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306120326.png)

I went in BurpSuite and I saw that this HTTP Header which was suspicious :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306120432.png)


I then decided to Modify it in my request and I got this result :


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306120509.png)

Flag : HeadersMayBeUseful
