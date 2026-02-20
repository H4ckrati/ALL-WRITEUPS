

![[Pasted image 20260218191251.png]]

# 1) Information Gathering

- **Information Gathering:** This essential phase serves as the cornerstone of a penetration test by collecting all available data regarding a company’s infrastructure, employees, and organizational structure to guide future exploitation.
    
- **Open-Source Intelligence (OSINT):** OSINT involves using publicly available tools and platforms like GitHub or social media to find sensitive data, such as leaked passwords or keys, that users often share unintentionally.
    
- **Infrastructure Enumeration:** This process maps out a company's digital footprint, including DNS records and cloud instances, to understand their network layout and identify active security measures like firewalls.
    
- **Service Enumeration:*(Software)* This step identifies specific applications running on a network and analyzes their version history to find outdated or unpatched software that administrators have failed to update. Example, You run `nmap -sV -p80 [IP_ADDRESS]`.
	the Nmap tells you the server is running **Apache httpd 2.4.49**.
    
- **Host Enumeration:**(Whole machine) This aspect focuses on individual targets to determine their operating systems, open ports, and internal services that may be poorly secured due to the assumption that they are unreachable from the internet.Example, You run a full port scan and OS detection (`nmap -A [IP_ADDRESS]`).You find that the machine is running **Windows 7** (an outdated OS) and has port **445 (SMB)** open.
    
- **Pillaging:** Performed during the post-exploitation phase, pillaging involves searching a compromised system for sensitive local files, credentials, and data to help escalate privileges or move laterally through the network

# 2) Vulnerability Assessment

- **Vulnerability Assessment :** C'est un processus analytique qui consiste à examiner les données recueillies précédemment pour comprendre l'origine et l'impact potentiel des failles découvertes.
    
- **Analysis Types :** L'analyse se divise en quatre catégories : **Descriptive** (caractéristiques des données), **Diagnostic** (causes et effets), **Predictive** (probabilités futures) et **Prescriptive** (actions pour prévenir les problèmes).
    
- **Logic and Conclusions :** Cette étape demande de poser des questions précises sur les résultats (ex: un port 2121) pour déduire la nature réelle des services, même si l'administrateur a tenté de les masquer.
    
- **Vulnerability Research :** On recherche activement des vulnérabilités connues (**CVE**) et des codes d'exploitation (**PoC**) dans des bases de données publiques comme _Exploit DB_ ou _NIST_.
    
- **Assessment of Attack Vectors :** Le pentester teste les services localement ou sur la cible pour valider les vecteurs d'attaque, tout en adaptant sa méthode selon les besoins de discrétion du client.
    
- **The Return (Iterative Process) :** Si aucune faille n'est trouvée, le pentester doit retourner à l'étape d'**Information Gathering** pour chercher des détails plus profonds, car ces deux phases se chevauchent constamment.

# 3) Exploitation

- **Exploitation :** Cette étape consiste à adapter les vulnérabilités découvertes pour obtenir un accès initial (foothold) ou élever ses privilèges, souvent en modifiant un code _Proof-of-Concept_ (PoC).
    
- **Prioritization of Attacks :** Avant d'agir, on classe les attaques selon leur **probabilité de succès**, leur **complexité** et le **risque de dommages** pour choisir la voie la plus efficace et sécuritaire.
    
- **Probability of Success :** On évalue les chances de réussite d'un exploit en s'aidant souvent du score **CVSS** pour déterminer si l'attaque vaut la peine d'être tentée.
    
- **Complexity :** Cet aspect estime l'effort et le temps de recherche nécessaires pour exécuter un exploit, une tâche qui varie selon l'expérience personnelle du testeur avec ladite faille.
    
- **Probability of Damage :** Il est crucial d'estimer si un exploit risque de faire planter un service (DoS) ou d'endommager le système, car la priorité reste la stabilité de l'environnement client.
    
- **Preparation for the Attack :** Pour les exploits risqués ou complexes, on recrée souvent le système cible dans une **machine virtuelle (VM)** locale pour tester et ajuster l'attaque avant de l'appliquer sur le vrai serveur.

# 4) Post-Exploitation

- **Post-Exploitation :** Cette phase consiste à exploiter l'accès obtenu pour naviguer dans le système, extraire des informations sensibles et tenter d'élever ses privilèges tout en restant idéalement indétectable.
    
- **Evasive Testing :** On adapte ses commandes et outils selon trois modes (**Évasif, Hybride ou Non-Évasif**) pour tester la capacité de détection de l'administrateur et identifier les "angles morts" de la surveillance.
    
- **Information Gathering (Local) :** Une fois à l'intérieur, le processus de collecte recommence pour énumérer les services internes, les partages réseau et les configurations accessibles uniquement depuis l'hôte compromis.
    
- **Pillaging :** Cette étape consiste à fouiller activement le système et le réseau pour voler des données critiques (mots de passe, clés SSH, fichiers clients) afin de prouver l'impact de l'attaque ou faciliter un mouvement latéral.
    
- **Persistence :** C'est l'action de mettre en place un accès permanent (backdoor) pour pouvoir revenir sur le système même si la connexion initiale est coupée ou si le service exploité redémarre.HYPER IMPORTANT!
    
- **Vulnerability Assessment (Internal) :** On analyse à nouveau les failles, mais cette fois-ci avec une perspective interne, en cherchant des erreurs de configuration ou des vulnérabilités locales pour monter en privilèges.
    
- **Privilege Escalation :** L'objectif est de passer d'un simple utilisateur à un compte à hauts privilèges (**Root** ou **Administrator**) afin de supprimer toutes les restrictions d'accès sur le réseau.
    
- **Data Exfiltration :** On teste si les systèmes de sécurité (**DLP/EDR**) détectent le transfert de données sensibles hors du réseau, souvent en utilisant des données factices (fictive data) pour éviter de manipuler de vraies informations confidentielles.

### Reminder : 

L'évaluation des vulnérabilités et la collecte d'informations ne s'arrêtent pas à la porte du serveur : une fois l'accès obtenu, l'analyse se déplace à l'intérieur du système pour révéler les failles invisibles de l'extérieur, car c'est dans l'intimité du réseau local que se cachent souvent les configurations les plus fragiles.

# 5)

