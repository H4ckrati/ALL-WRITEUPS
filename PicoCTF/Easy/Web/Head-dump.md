
Welcome to the challenge! In this challenge, you will explore a web application and find an endpoint that exposes a file containing a hidden flag.The application is a simple blog website where you can read articles about various topics, including an article about API Documentation. Your goal is to explore the application and find the endpoint that generates files holding the server’s memory, where a secret flag is hidden.The website is running [picoCTF News](http://verbal-sleep.picoctf.net:57474/).


#### First Step 

I read the website and I saw a post that was talking about the backend developpement so I clicked on all the tag and all of a sudden the #API Documentation responded 


![[Pasted image 20260219183701.png]]

![[Pasted image 20260219183817.png]]

#### Second Step 

I then go into the url and added /heapdump at the end. It downloaded me a file which contained the flag 

![[Pasted image 20260219184025.png]]

Flag : picoCTF{Pat!3nt_15_Th3_K3y_dc0756a3}

