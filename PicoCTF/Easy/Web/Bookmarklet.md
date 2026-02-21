
###  Challenge description

Why search for the flag when I can make a bookmarklet to print it for me?Browse [here](http://titan.picoctf.net:65211/), and find the flag


#### FIrst Step

I go on the website and I copied the bookmarklet : 

        javascript:(function() {
            var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓ¨ÍÕÄ¦í";
            var key = "picoctf";
            var decryptedFlag = "";
            for (var i = 0; i < encryptedFlag.length; i++) {
                decryptedFlag += String.fromCharCode((encryptedFlag.charCodeAt(i) - key.charCodeAt(i % key.length) + 256) % 256);
            }
            alert(decryptedFlag);
        })();


#### Second Step 

I then decided to go into Javascript compiler and run the same code but instead of alert console.log

![](../../PicoCtF-assets/Pasted image 20260219193701.png)


Flag : picoCTF{p@g3_turn3r_18d2fa20} 