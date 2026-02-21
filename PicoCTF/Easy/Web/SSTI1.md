
### Description of the Challenge

I made a cool website where you can announce whatever you want! Try it out!I heard templating is a cool and modular way to build web apps! Check out my website [here](http://rescued-float.picoctf.net:50870/)!


#### First Step

I go into the website and I see that the Title of the Challenge is SSTI1, so Im thinking that the website is vulnerable to SSTI. I try it by entering this in the input of the website: 

![[../../../ALL-assets/Pasted image 20260219171259.png]]

It is vulnerable to SSTI because it render 49.
#### Second Step

I then modify the POST request in burpsuite repeater allowing me to search and craft an SSTI which will allow me to read the content of the flag

![[../../../ALL-assets/Pasted image 20260219171526.png]]

I then see that there is a file named flag --> I need to read it


#### Third Step

I then replace ls by cat flag --> Look at the result : 

![[../../../ALL-assets/Pasted image 20260219171630.png]]

Flag : picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_f5438664}