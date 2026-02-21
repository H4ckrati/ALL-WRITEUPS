File upload vulnerabilities occur when a server doesn’t properly validate uploaded files, allowing attackers to upload and sometimes execute malicious files, potentially leading to remote code execution.

## How do file upload vulnerabilities arise?

File upload vulnerabilities arise when developers rely on defectueux, bypassable, or inconsistently applied validation checks (such as weak blacklists or manipulable file properties), allowing attackers to upload dangerous files.

## Exploiting unrestricted file uploads to deploy a web shell

Unrestricted file upload vulnerabilities allow attackers to upload and execute server‑side scripts, letting them deploy a **web shell** that gives full control over the server.  
**Example:** an attacker uploads a malicious PHP file and then sends an HTTP request to it to run system commands or read sensitive files from the server.

Lab 1: 

 I needed to create a php script that allows me to file_get_contents('/home/carlos/secret');
 to get the password to submit the solution. Then i need to upload it and click droit sur l avatar faire open in a new page et la reponse serait la. 
 Ou
 Sur burpsuite, allez et mettre les deux requetes http my-account/avatar et files/avatars/myexploit.php et voir le render et le tour est jouer.
 
 https://youtu.be/rPdn88pO7x0


## Exploiting flawed validation of file uploads

In real-world websites, file upload protections usually exist but can still be flawed enough to exploit for remote code execution.


## Flawed file type validation

*Les formulaires simples envoient du texte avec application/x-www-form-urlencoded(ex. `nom=Anas&age=20`), tandis que l’envoi de fichiers comme une image JPG ou un PDF utilise multipart/form-data.  
⚠️ La faille de sécurité apparaît quand le serveur vérifie mal le type de fichier envoyé, ce qui permet à un attaquant d’uploader un fichier malveillant (ex. un script) déguisé en image.

Par exemple, le serveur vérifie uniquement l’extension du fichier et accepte `image.jpg`, alors que le contenu réel est un script malveillant.  
Un attaquant peut aussi modifier manuellement le `Content-Type` en `image/jpeg` même si le fichier n’est pas une vraie image, et le serveur l’accepte sans analyser le contenu réel.

## Other Flawed file type validation

For example, an attacker can upload a malicious script while setting the header to `Content-Type: image/jpeg`using Burp repeater, causing the server to accept it as a valid image even though it is not.

Lab 2: 
Un peu plus complexe, mais toujours aussi simple, le site a bloquer le téléchargement de file autre que png et jpeg, ce qui fait en sorte qu'il faut bypass ce petit système la.
Donc tu dois mettre une image valide sur le site ensuite tu vas sur burpsuite tu change la requete afin d y mettre le exploit.php et tu peux mettre le script en bas tu met le echo file_get_contents('/home/carlos/secret'); to get the password then you go to the other page (on burpsuite repeater) which is the /files/avatars/...jpg so you change it to /files/avatars/exploit.php.
You click send and there you go the answer is right there
