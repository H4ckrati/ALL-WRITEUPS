
### 1. L'IP de la Machine (Soccer : `10.129.1.84`)

C'est l'adresse du serveur que tu essaies de pirater.

- **Son rôle :** C'est la destination de tes attaques.
    
- **Utilisation :** Tu la tapes dans ton navigateur pour voir le site web, ou dans Nmap pour scanner les ports. (L'IP de la machine ne contient pas de terminal par défaut car c'est un serveur conçu pour envoyer des pages web (HTTP) et non pour donner un accès système, qu'il faut donc forcer à "sortir" vers toi via un **Reverse Shell**.)
    
- **Analogie :** C'est l'adresse de la banque que tu veux cambrioler.
    

### 2. Ton IP VPN (`10.10.14.110`)

C'est l'adresse que Hack The Box t'a attribuée quand tu as lancé ton fichier `.ovpn`.

- **Son rôle :** C'est ton "adresse de retour" à l'intérieur du réseau privé de HTB.
    
- **Utilisation :** Tu l'utilises dans tes **payloads** (ton code malveillant) pour dire au serveur où renvoyer la connexion (le Shell).
    
