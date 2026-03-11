##### Challenge Description 

Retrieve the administrator password.

>[!note] What is RSS ?
>RSS (Really Simple Syndication) is a standardized XML-based web feed that allows users and applications to receive automatic, real-time updates from their favorite websites in a simplified, text-based format.


Consequently, I visited [https://pastebin.com](https://pastebin.com/) and uploaded a payload that returned the backend source code of index.php, encoded using base64.

You can access the payload here: [https://pastebin.com/raw/Kyr9Rpu0](https://pastebin.com/raw/Kyr9Rpu0)

![](https://miro.medium.com/v2/resize:fit:1400/1*J5BHsHJJY0r7ZfRlow1IlQ.png)


Then I copied the raw link from Pastebin and injected it into the URL input.


![](https://miro.medium.com/v2/resize:fit:1400/1*cxQklji22tKWARbr2dS5ww.png)


Now that I’ve obtained the encoded Base64 source code, I opened the decoder in Burp Suite and decoded it, resulting in the PHP source code.

```
 <?php  
  
echo '<html>';  
echo '<header><title>XXE</title></header>';  
echo '<body>';  
echo '<h3><a href="?action=checker">checker</a>&nbsp;|&nbsp;<a href="?action=auth">login</a></h3><hr />';  
  
if ( ! isset($_GET['action']) ) $_GET['action']="checker";  
  
if($_GET['action'] == "checker"){  
  
   libxml_disable_entity_loader(false);  
   libxml_use_internal_errors(true);  
  
   echo '<h2>RSS Validity Checker</h2>  
   <form method="post" action="index.php">  
   <input type="text" name="url" placeholder="http://host.tld/rss" />  
   <input type="submit" />  
   </form>';  
  
  
    if(isset($_POST["url"]) && !(empty($_POST["url"]))) {  
        $url = $_POST["url"];  
        echo "<p>URL : ".htmlentities($url)."</p>";  
        try {  
            $ch = curl_init("$url");  
            curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);  
            curl_setopt($ch, CURLOPT_TIMEOUT, 3);  
            curl_setopt($ch, CURLOPT_CONNECTTIMEOUT ,0);   
            $inject = curl_exec( $ch );  
            curl_close($ch);  
            $string = simplexml_load_string($inject, null, LIBXML_NOENT);  
            if ( ! is_object($string) || !$string || !($string->channel) || !($string->channel->item)) throw new Exception("error");   
  
            foreach($string->channel->item as $row){  
                print "<br />";  
                print "===================================================";  
                print "<br />";  
                print htmlentities($row->title);  
                print "<br />";  
                print "===================================================";  
                print "<br />";  
                print "<h4 style='color: green;'>XML document is valid</h4>";  
            }  
        } catch (Exception $e) {  
            print "<h4 style='color: red;'>XML document is not valid</h4>";  
        }  
  
    }  
}  
  
if($_GET['action'] == "auth"){  
    echo '<strong>Login</strong><br /><form METHOD="POST">  
    <input type="text" name="username" />  
    <br />  
    <input type="password" name="password" />  
    <br />  
    <input type="submit" />  
    </form>  
    ';  
    if(isset($_POST['username'], $_POST['password']) && !empty($_POST['username']) && !empty($_POST['password']))  
    {  
        $user=$_POST["username"];  
        $pass=$_POST["password"];  
        if($user === "admin" && $pass === "".file_get_contents(".passwd").""){  
            print "Flag: ".file_get_contents(".passwd")."<br />";  
        }  
  
    }  
  
}  
  
  
echo '</body></html>';

print "Flag: ".file_get_contents(".passwd")."<br />";

```

So, to get the flag, I must retrieve the content of the `.passwd` file. Then, I created a new Pastebin link by simply changing `index.php` to `.passwd` in order to obtain the password.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*-cXkCvvKWXt_q5KslaitBA.png)


So, I copied the Pastebin link ([https://pastebin.com/raw/tbCiMmPf](https://pastebin.com/raw/tbCiMmPf)) and pasted it into the vulnerable function, and the XML document was successfully validated.


![](https://miro.medium.com/v2/resize:fit:1400/1*XCEQ2qIQvmqJO7CELMo1Lw.png)

Now, I have successfully obtained the encoded base64 flag.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*rUYzq9rLswglSrdpvX3vQw.png)


