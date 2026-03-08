#LFI #RFI
##### Challenge Description 

Get in the admin section.
https://www.root-me.org/spip.php?article781

##### What is Local File Inclusion (LFI) ? : 

Local File Inclusion (LFI) occurs when an application allows a user to input a file path that the server then improperly includes or executes, leading to the disclosure of sensitive files like `/etc/passwd`. This vulnerability typically arises when input is not properly validated, allowing attackers to use directory traversal sequences like `../` to navigate the server's file system.

##### What is Remote File Inclusion (RFI) ? :

Remote File Inclusion (RFI) is a vulnerability that lets an attacker include a malicious file hosted on an external server into the target application's execution flow. By manipulating a script's include parameter to point to an external URL, the attacker can achieve remote code execution (RCE) on the vulnerable web server.

#### Get the Flag :

*Because of the Challenge Description and Name, I thought that I would need to access the page of the admin* :


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307215010.png)

So let's try to explore a little more :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307215118.png)

Go in the admin file :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307215145.png)

Open the content of index.php in a new tab :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307214919.png)

I got the creds :     admin / OpbNJ60xYpvAQU8

Flag : OpbNJ60xYpvAQU8