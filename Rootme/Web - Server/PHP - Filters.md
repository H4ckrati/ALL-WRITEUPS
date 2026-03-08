
##### Challenge Description

Retrieve the administrator password of this application.

https://repository.root-me.org/Programmation/PHP/EN%20-%20Using%20and%20understanding%20PHP%20streams%20and%20filters.pdf


```
<?php
$inc="accueil.php";
if (isset($_GET["inc"])) {
    $inc=$_GET['inc'];
    if (file_exists($inc)){
        $f=basename(realpath($inc));
        if ($f == "index.php" || $f == "ch12.php"){
            $inc="accueil.php";
        }
    }
}
include("config.php");
// ... (le reste c'est l'affichage HTML)
include($inc);
?>

```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308140722.png)

When I enter the config.php file and I see at the decoded version :

```
<?php
$username="admin";
$password="DAPt9D2mky0APAF";
```


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308140706.png)

Flag : DAPt9D2mky0APAF