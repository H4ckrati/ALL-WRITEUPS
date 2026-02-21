Terms : Authentication is the process of verifying that a user is who they claim to be. Authorization involves verifying whether a user is allowed to do something.
2step authentification

In Burpsuite : 
What is the difference between Get Method and Post Method ?
- **GET Method**: Sends data in the URL. Used to **retrieve** information. Data is visible in the browser.  
    **Example:** `http://example.com/search?query=book`
    
- **POST Method**: Sends data in the **request body**. Used to **submit** or update data. Data is not visible in the URL.  
    **Example:** Submitting a login form with username and password.
    

In short: **GET = read, POST = write/submit**.

## Brute-force attacks, ## Brute-forcing usernames and ## Brute-forcing passwords

A brute-force attack is a trial-and-error method used by attackers to guess usernames and passwords, often using automated tools and wordlists. These attacks become more effective when attackers apply logic or publicly available information instead of random guessing. Usernames are often easy to predict because they follow common patterns, such as email formats or default names like “admin.” Passwords can also be vulnerable, even when password policies require complexity, because users tend to make predictable choices. People often modify simple passwords slightly to meet rules or when forced to change them regularly. As a result, websites without strong brute-force protection are highly exposed to these attacks.

## Username enumeration

Username enumeration is a technique where attackers identify valid usernames by observing differences in a website’s responses. For example, on a login page, entering a **valid username** with a wrong password might return the message _“Incorrect password”_, while an **invalid username** returns _“User does not exist.”_  This difference allows an attacker to determine which usernames are valid. This makes brute-force attacks easier by allowing attackers to focus only on confirmed usernames.


## Bypassing two-factor authentication

Bypassing two-factor authentication can happen when a website treats the user as logged in **before** the second verification step is fully checked, allowing access without completing 2FA. For example : After entering the correct username and password, the user is redirected to a page asking for a 2FA code. However, if the user manually changes the URL to `/my-account, the website grants access without verifying the code.


Lab1:
https://youtu.be/DEUCRYGt3TY

Lab2 :
https://youtu.be/2WpBVanEn3M

Lab3 :

En gros, il faut vérifier quand tu met mauvais password et mauvais username quel message d'erreur sa te met : Dans notre cas, ça me met Invalid password and/or username. So I need to go in Burpsuite and in intruder put the payload in intruder for the username than I go in settings and I put grep Match Invalid password and/or username. Than I snip attack, and I classify when its finished by the grep that i put. I will then see that I have one message that is different from the grep which is the original message but without a . in the end, which shows that the username is good because its a different message. From there, I can brute force normally the password. Comme quoi, une erreur de programmeur peut être dangereuse.