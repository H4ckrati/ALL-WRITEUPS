
#### Challenge Description

Can you break into this super secure portal?
http://fickle-tempest.picoctf.net:58111


#### First and only step 

I just inspect the webpage and I see a easy javascript code :

```
|   |
|---|
|function verify() {|
|checkpass = document.getElementById("pass").value;|
|split = 4;|
|if (checkpass.substring(0, split) == 'pico') {|
|if (checkpass.substring(split*6, split*7) == 'eb02') {|
|if (checkpass.substring(split, split*2) == 'CTF{') {|
|if (checkpass.substring(split*4, split*5) == 'ts_p') {|
|if (checkpass.substring(split*3, split*4) == 'lien') {|
|if (checkpass.substring(split*5, split*6) == 'lz_2') {|
|if (checkpass.substring(split*2, split*3) == 'no_c') {|
|if (checkpass.substring(split*7, split*8) == 'b45}') {|
|alert("Password Verified")|
|}|
|}|
|}|
||
|}|
|}|
|}|
|}|
|}|
|else {|
|alert("Incorrect password");|
|}|
||
|}|
```

#### Second Step

Send it to AI and there you got the flag



Flag : 