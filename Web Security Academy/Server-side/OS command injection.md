## What is OS command injection?

OS command injection allows an attacker to execute system commands on a server, potentially fully compromising the application and other connected systems.

## Useful commands

| Purpose of command    | Linux         | Windows         |
| --------------------- | ------------- | --------------- |
| Name of current user  | `whoami`      | `whoami`        |
| Operating system      | `uname -a`    | `ver`           |
| Network configuration | `ifconfig`    | `ipconfig /all` |
| Network connections   | `netstat -an` | `netstat -an`   |
| Running processes     | `ps -ef`      | `tasklist`      |

## Injecting OS commands

A shopping app shows stock status by taking user-supplied product and store IDs from a URL and passing them directly to a shell command, which can create a risk of OS command injection.
For example, if an attacker sets `storeID=29; & whoami &`, the application may run `stockreport.pl 381 29; whoami`, unintentionally executing an extra OS command.



Lab 1:
On peut voir que lorsque qu'on analyse sur Burpsuite lorsque le site change de stock, on peut voir que tout en bas de la request HTTP il y a productId=3&storeId=2; . On essaie d ajouter whoami a la toute fin et le tour est joué

C’est le `;` qui dit au shell : “exécute autre chose”, voici la nouvelle commande a éxecuter.

Alors :

First Solution:
productId=2&storeId=2 whoami = Not good
productId=2&storeId=2;whoami = Good

Second Solution : 
productId=2 & whoami #&storeId=2 
productId=2+%26+whoami+%23&storeId=2 (same as line before but url-encoded for burpsuite
) 

Pourquoi la deuxième solution marche ?
### 1️⃣ Côté **serveur web (URL parsing)**

- `&` sépare les paramètres HTTP
    
- Le serveur voit :
    
    - `productId = 2 & whoami #`
        
    - `storeId = 2`
        

Donc **`storeId` existe bien** au niveau HTTP.

`storeId` est vulnérable, car il est utilisé pour appeler un programme du système afin de récupérer l’état du stock. alors que `productId` reste dans l’application et n’est jamais exécuté par le shell.

https://youtu.be/GDUadTiXXVk