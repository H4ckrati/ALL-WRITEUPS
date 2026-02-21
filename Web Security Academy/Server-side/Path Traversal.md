Path traversal (also called directory traversal) is a vulnerability that lets attackers access files outside the intended directory by manipulating file paths, potentially exposing sensitive data or system files. For example, if an app loads images using `filename=218.png`, an attacker could request `filename=../../../etc/passwd` to read the `/etc/passwd` file on a Unix server.

Lab1:
https://youtu.be/XhieEh9BlGc


## Common obstacles to exploiting path traversal vulnerabilities
(En sachant que ceci est le chemin des images `/var/www/images/`)

If an application blocks directory traversal sequences, an attacker may still bypass the defense by supplying an absolute path from the filesystem root, for example using `filename=/etc/passwd` to access the file directly. Because the website can block ../ for example, but as we said it can be easily bypassed.

Il peut être possible d’utiliser des séquences de traversée imbriquées, telles que `....//` ou `....\/`. Celles-ci se transforment en simples séquences de traversée lorsque la séquence interne est supprimée. Par exemple, Si l’utilisateur fournit `....//etc/passwd`, le filtre supprime la séquence `../` interne, ce qui transforme `....//` en `../` et donne finalement `../etc/passwd`.

Dans certains cas, les serveurs web suppriment automatiquement les séquences de traversée de répertoires avant de transmettre les données à l’application, mais ce filtrage peut parfois être contourné en utilisant différentes formes d’encodage (simple ou double), et des outils comme Burp Intruder proposent des listes de charges utiles encodées pour tester ces protections.

If an application requires the filename to begin with a specific base directory, this restriction can sometimes be bypassed by including that base path followed by directory traversal sequences, such as `filename=/var/www/images/../../../etc/passwd`. Pour remonter à la racine et aller à etc/passwd

An application may require the user-supplied filename to end with an expected file extension, such as `.png`. In this case, it might be possible to use a null byte to effectively terminate the file path before the required extension. For example: `filename=../../../etc/passwd%00.png`.



******
 Lab : File path traversal, traversal sequences blocked with absolute path bypass

Sur Burpsuite Repeater je prend la requête HTTP de open image in a new tab et tu remplace filename=27.jpg to filename=/etc/passwd to acces the sensitive information. (see 1st paragraphe)

----------------------------------------------------------------------
 Lab: File path traversal, traversal sequences stripped non-recursively
 
Par exemple, Si l’utilisateur fournit `....//etc/passwd`, le filtre supprime la séquence `../` interne, ce qui transforme `....//` en `../` et donne finalement `../etc/passwd`.
En sachant cela, on peut uniquement faire comme si on voulait faire cela ../../../etc/passwd mais vu qu'il ya un filtre qui enlève ../ alors on doit doubler tout donc sa donne cela :
....//....//....//etc/passwd (see deuxième paragraphe à partir du haut)

--------------------------------------------
Lab: File path traversal, traversal sequences stripped with superfluous URL-decode 

Ici le site, le site bloque les path traversal sequences like ../. And the site have a URL-decode. So I url encoded that ../../../etc/passwd. It did not work, so i read again and its says that its URL-Decode. But what if I URL-Encode it two times? Will it work? The Answer is yes because 2-1 = 1 donc = URL-Encode une seule fois, donc sa marche. (see 3eme paragraphe a partir du bas)

--------------------------------------------
Lab: File path traversal, validation of start of path

I just needed to go in Burp repeater, then find the open image in a new tab request, than i change the request to `filename=/var/www/images/../../../etc/passwd`. (see avant-dernier paragraphe) There you go !

--------------------------------------------

Lab: File path traversal, validation of file extension with null byte bypass

I just needed to go in Burp repeater, then find the open image in a new tab request, than i change the request to `filename=../../../etc/passwd%00.png`. (see dernier paragraphe) There you go ! %00 sert sert à **indiquer la fin d’une chaîne de caractères.

--------------------------------------------





## How to prevent a path traversal attack

La façon la plus sûre de prévenir les vulnérabilités de traversée de chemin est d’éviter complètement d’utiliser des données fournies par l’utilisateur pour accéder au système de fichiers, mais si cela n’est pas possible, il faut valider l’entrée puis s’assurer que le chemin résolu (canonique) reste à l’intérieur d’un répertoire de base prévu.  
Par exemple, une application peut autoriser uniquement des caractères sûrs dans le nom de fichier, puis vérifier que le chemin canonique commence toujours par `/var/www/images` avant de lire le fichier.

File file = new File("/var/www/images", userInput);
if (file.getCanonicalPath().startsWith("/var/www/images")) {
    // traiter le fichier en toute sécurité
}

### Ce que fait `getCanonicalPath()`

- Il prend un chemin avec `.` ou `..`
    
- Il **résout tous les `..` et `.`** pour donner le **chemin final réel**
    
- Il montre **où le fichier se trouve vraiment** dans le système

Donc au final le if n'est pas respecté