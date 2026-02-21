SQL injection (SQLi) is a **web security vulnerability** where an attacker can interfere with an application’s database queries to access, modify, or delete data, and potentially compromise the server or cause denial-of-service attacks.

## How to detect SQL injection vulnerabilities

You can detect SQL injection vulnerabilities by testing each input of an application and checking whether unusual inputs cause errors, different results, or abnormal behavior.  
For example, entering a single quote (`'`), SQL logic like `OR 1=1` versus `OR 1=2`, or SQL expressions that should return different values can reveal changes in the application’s response.  
You can also look for delayed responses or external network interactions caused by SQL queries, or use automated tools like **Burp Scanner** to find most SQL injection issues quickly and reliably.

Important : It is important to put an apostrophe (`'`) before adding SQL logic because it **closes the original text value** in the SQL query, allowing anything that follows to be interpreted as **SQL code instead of plain text**.
## Retrieving hidden data

In this example 1, a shopping app uses the `category` URL parameter to build a SQL query like `SELECT * FROM products WHERE category='Gifts' AND released=1` to display only released products from the selected category while hiding unreleased ones. The restriction `released = 1` is being used to hide products that are not released. We could assume for unreleased products, `released = 0`.

**Example 2 (`Gifts'--`):**  
Because the application has no protection against SQL injection, an attacker can add `--` to comment out part of the SQL query.  
This removes the `AND released = 1` condition, causing the database to return **all products**, including unreleased ones.

**Example 3 (`Gifts' OR 1=1--`):**  
In this attack, the attacker adds a condition that is always true (`1=1`) and comments out the rest of the query.  
As a result, the database returns **every product in every category**, because the condition always evaluates to true.
Car meme si le produit n'est pas dans Gifts la condition 1=1 est toujours vraie donc produit toujours show up.

Lab 1: 
Firstly, by going on the filters, we can see category='Gifts' and to see if the website is vulnerable to SQL injection, i did category=' and yes it is vulnerable because adding a single apostrophe (`'`) breaks the normal behavior of the page and triggers an error or an unexpected response, which shows that the application is directly inserting the user input into a SQL query without proper protection.

So you can add SQL logic after I closed the text part with '. Then I readed the rules and i needed to show up the website to display one or more unreleased products, then to show every category i put '(initially added paragraph precedent) OR 1=1 it serves to show each category because the value is always true so categorie all show up. After I need to add -- so it forgets the other SQL logic after so for example if there is AND released = 1 it will erased so it will show all unreleased and released products.


https://youtu.be/alTceRdSxS0

## Subverting application logic

The application authenticates users by running a SQL query that checks whether the submitted username and password match a record in the database.  
Because it doesn’t protect against SQL injection, an attacker can use a SQL comment to remove the password check, causing the query to return the administrator’s account and log in without a valid password.


Lab 2 :

Sur Burpsuite, on peut faire click droit sur la requete de login avec faux credentials et on peut voir que si on fait ajouter au scan, c'est vulnerable a une injection SQL en commentant le password donc juste avec username il peut se login. Alors, on peut egalement remplacer username=wiener to username=' remarquant donc qu il ya un server error. Donc on peut exploiter cela en sachant qu'une injection SQL marcherait ici, donc on met le username=administrator ensuite on rajoute un ' pour ajouter la logique SQL (voir important en haut), on rajoute un -- pour commenter le reste. On send et le tour est joué.

2eme solution plus simple : 
Faire exactement la meme chose mais dans le input dans le site directement.
https://youtu.be/ML3aGaloczI
