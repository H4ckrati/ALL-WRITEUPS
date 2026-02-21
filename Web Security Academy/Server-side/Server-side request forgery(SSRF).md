## What is SSRF?

Server-side request forgery (SSRF) is a web vulnerability where an attacker tricks a server into making requests to unintended locations, such as internal services that are not publicly accessible.  
For example, if a web app fetches a user-supplied URL, an attacker could supply an internal address like an admin service, causing the server to expose sensitive data or credentials.

## SSRF attacks against the server

In this example, the application lets users **check the stock of a product** by sending a URL to the server, which then redirects the request to a back-end stock service.  
An attacker abuses this feature by changing the stock-check URL to `localhost/admin`, causing the server to redirect the request to its own internal **admin page** and return the protected content to the attacker.
## Some Real Cases :

- The access control check might be implemented in a different component that sits in front of the application server. When a connection is made back to the server, the check is bypassed.
- For disaster recovery purposes, the application might allow administrative access without logging in, to any user coming from the local machine. This provides a way for an administrator to recover the system if they lose their credentials. This assumes that only a fully trusted user would come directly from the server.
- The administrative interface might listen on a different port number to the main application, and might not be reachable directly by users.
### 1️⃣ Contrôle d’accès dans un composant séparé

👉 **Où est la sécurité ?**  
Dans un **élément placé devant** l’application (ex : proxy, pare-feu, reverse proxy).

👉 **Le problème :**  
Quand le serveur s’appelle lui‑même (`localhost`), la requête **ne passe pas par ce composant**, donc **aucune vérification n’est faite**.

🧠 _Exemple_ : la porte de sécurité est à l’entrée du bâtiment, mais si tu passes par une porte interne, personne ne te contrôle.

---

### 2️⃣ Accès admin autorisé depuis la machine locale (reprise après sinistre)

👉 **Pourquoi c’est fait ?**  
Pour permettre à un admin de réparer le système **sans se connecter** s’il a perdu ses identifiants.

👉 **Le problème :**  
Toute requête venant de `localhost` est considérée comme **100 % fiable**.

🧠 _Exemple_ : toute personne déjà à l’intérieur du bâtiment est automatiquement considérée comme un employé.

---

### 3️⃣ Interface admin sur un port différent

👉 **Comment ça marche ?**  
L’admin n’est pas sur le même port que le site :

`Site public : port 443 Admin : port 8080 (interne)`

👉 **Le problème :**  
Les utilisateurs ne peuvent pas accéder à ce port, **mais le serveur oui**.

🧠 _Exemple_ : une salle verrouillée sans porte extérieure, accessible seulement depuis l’intérieur.



Difference between accessing by SSRF localhost/admin and IP particular ?
SSRF to **`localhost`** accesses services on the **same server as the application**, while SSRF to a **specific internal IP** (e.g. `192.168.0.68`) accesses **other machines on the internal network**, potentially exposing different admin systems or services.

Lab1: 
https://youtu.be/lMxCQcktifs

Lab2:
https://youtu.be/t4Hrq7TCTPU