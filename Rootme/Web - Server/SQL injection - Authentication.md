#SQL #SQLi #OR1
#### Challenge Description

 Authentication v 0.01


Let's test a basic payload of SQL injection to see how the website reacts :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306214909.png)

>[!type]
> Explanation : This simple payload comments the rest of the input so the password will be commented ( not required anymore)


 This basic payload worked, now let's find the password in the source page (control + u)

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306214737.png)


Flag : t0_W34k!$


## WRONG CODE : 

>[!note]
> ```
> $query = "SELECT * FROM users WHERE username = '";
> ```
> 	his is very dangerous. If a user enters `' OR '1'='1`, the final query becomes: `SELECT * FROM users WHERE username = '' OR '1'='1'` Since `'1'='1'` is always true, the database returns all users, bypassing the login


## SOLUTION CODE :

This code wraps the IP in quotes, which means the `ls` command won't be executed as code, but will be treated as a string.

>[!Note]
>``` 
>// 1. Send the TEMPLATE to the database first (no user data yet)
$stmt = $pdo->prepare('SELECT * FROM users WHERE username = :name');

// 2. Send the DATA separately
$stmt->execute(['name' => $user_input]);

