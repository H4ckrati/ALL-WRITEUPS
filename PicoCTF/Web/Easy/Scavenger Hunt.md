#Index
#### Challenge Description

There is some interesting information hidden around this site. Can you find it?http://wily-courier.picoctf.net:57860/


### Step by Step

Website is written in HTML but when I click in What it gives me information

![](https://miro.medium.com/v2/resize:fit:618/1*GBuGWrUaeALdvZubhRUSqg.png)


#### Part 1 : 

Open Inspect Element and here’s my first part of the flag. Wrote it down on my notepad just in case I find another set.

![](https://miro.medium.com/v2/resize:fit:858/1*YRu9LbtRDfrIA0xE4PFXyA.png)

#### Part 2 : 

went to source, on CSS another piece of the flag is in the last

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*nyWH2hKWrrUBVU4Rm-1daQ.png)

#### Part 3

went to JS and the comment didn’t provide a piece of flag but it gives me a hint of question:

![](https://miro.medium.com/v2/resize:fit:856/1*XF_owO4PUTRI9bI0geH2Bw.png)

So typically in order to prevent Google from indexing your website we usually use robots.txt. It is a file used by website to tell search engines of which can be index or cannot be index. We need to specify the path to robots.txt

En effet, 'En règle générale, pour empêcher Google d'indexer votre site web, on utilise le fichier **robots.txt**. C'est un fichier utilisé par les sites pour indiquer aux moteurs de recherche quelles pages peuvent être indexées ou non. Il est nécessaire de spécifier le chemin d'accès vers ce fichier **robots.txt**.'

Indexer :  **Indexer**, c'est l'action pour un moteur de recherche (comme Google) d'ajouter une page web à son immense **catalogue** (son "index"). 


On the URL just add /robots.txt then it will redirect you to this page:

![](https://miro.medium.com/v2/resize:fit:1110/1*-78vxuBvz1YurkNNEuPzhw.png)

#### Part 4 :

There’s the part 3 of the flag the website tells me that it has an apache server technically apache server has some hidden files like /.htaccess /.htpasswd and other hidden files. I pivot on the /.htaccess and that’s where I got the part 4

![](https://miro.medium.com/v2/resize:fit:1190/1*lKo00igH0tM0cTIL2DufaQ.png)

#### Part 5 : 

Then another hint where it says “I love making websites on my Mac, I can store a lot of information there.”

It is referring to MAC OS proprietary file called DS_Store that holds attributes/meta-data about the folder it resides in. So specifying the path as /.DS_Store then the last part of the flag is revealed

###### A **.DS_Store** file is a hidden macOS metadata file created by the Finder to store a folder's custom viewing preferences, such as icon positions and background colors, which can accidentally reveal a list of other files if uploaded to a web server.

![[../../../ALL-assets/Pasted image 20260219203542.png]]
#### Final Part : 

First Part : picoCTF{t
Second Part : h4ts_4_l0
Third Part : t_0f_pl4c
Fourth Part : 3s_2_lO0k
Fifth Part : _9588550}

Full flag : picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_9588550}