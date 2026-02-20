Host: bandit.labs.overthewire.org

Port: 2220

Terms : 
ssh : connect to someone's computer

**Bandit Level 0**

[http://overthewire.org/wargames/bandit/bandit0.html](http://overthewire.org/wargames/bandit/bandit0.html)

In their website they give us the username and password for bandit0 and we have to find the password for bandit1

**Username:** bandit0

**Password:** bandit0

---

**Bandit Level 0 → Level 1**

[http://overthewire.org/wargames/bandit/bandit1.html](http://overthewire.org/wargames/bandit/bandit1.html)

While logged into the bandit0 user profile I ran the “ls” command to see if I find any useful files. The output showed that there is a file named “readme”. I read the file using “cat readme” and it contained 1 line with the following text: “boJ9jbbUNNfktd78OOpsqOltutMc3MY1”. I tried to use the string from the file to log into bandit1 and it worked.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/1.png)

**Username:** bandit1

**Password:** boJ9jbbUNNfktd78OOpsqOltutMc3MY1

---

**Bandit Level 1 → Level 2**

[http://overthewire.org/wargames/bandit/bandit2.html](http://overthewire.org/wargames/bandit/bandit2.html)

On the bandit website it says that the next password is saved in a file named -. When I logged into the account and ran "ls" I saw the file. I then ran "cat <-" and I saw that the password is: CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9. In order to read files that start with a dash, you have to redirect them to stdin with the < operator.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/2.png)

**Username:** bandit2

**Password:** CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

**Reference:** [https://unix.stackexchange.com/questions/16357/usage-of-dash-in-place-of-a-filename](https://unix.stackexchange.com/questions/16357/usage-of-dash-in-place-of-a-filename)

---

**Bandit Level 2 → Level 3**

[http://overthewire.org/wargames/bandit/bandit3.html](http://overthewire.org/wargames/bandit/bandit3.html)

The password for the next user is stored in a file called spaces in this filename. I ran " cat "spaces in this filename" " and received the next password: UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK. In order to read files with spaces in the name you have to put the file name in quotation marks.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/3.png)

**Username:** bandit3

**Password:** UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

---

**Bandit Level 3 → Level 4**

[http://overthewire.org/wargames/bandit/bandit4.html](http://overthewire.org/wargames/bandit/bandit4.html)

The password is stored in a hidden file in the inhere directory. I ran "ls" to see what files and directories there are and then I ran "cd" to move into the inhere folder. Then I ran "ls -a" to see all of the files including the hidden ones. All hidden files and folders in linux are stored with a dot in front of their name. The hidden file is named .hidden and after running "cat .hidden" I saw that the next password is pIwrPrtPN36QITSp3EQaw936yaFoFgAB.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/4.png)

**Username:** bandit4

**Password:** pIwrPrtPN36QITSp3EQaw936yaFoFgAB

---

**Bandit Level 4 → Level 5**

[http://overthewire.org/wargames/bandit/bandit5.html](http://overthewire.org/wargames/bandit/bandit5.html)

The password is stored in file inside the inhere folder, most of the files in the directory as written in binary and only one of them has human readable text. I "cd" into the folder and saw all of the files in the folder. When trying to read the binary files with "cat" I would get something I could not read. I could tried to read every file and see which one has the password but I can also have a loop that gives me information on the files to see which one is the human readable one. To do this I ran the command below:

_for x in {0..9}; do file ./-file0$x; done_

For loop will run from 0 through name and save the variable as x. With the help of the command "file" which gives you information about any file passed as a parameter. The files were names -file00 through -file09 and therefore we have to add the ./ in order for file to read the files. After the command finished I received the following output

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/5.png)

The password seems to be in the -file07 file and to read it I ran "cat <-file07" and the password is koReBOKuIDDepwhWk7jZC0RTdopnAYKh

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/6.png)

**Username:** bandit5

**Password:** koReBOKuIDDepwhWk7jZC0RTdopnAYKh

---

**Bandit Level 5 → Level 6**

[http://overthewire.org/wargames/bandit/bandit6.html](http://overthewire.org/wargames/bandit/bandit6.html)

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

_human-readable_

_1033 bytes in size_

_not executable_

To start I "cd" into the inhere folder and ran "ls" to see the files and folders and noticed that there were a bunch of folder and inside those folders a couple of files.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/7.png)

To make my life easier and not have to read every file I tried to "find" a way to look for the specific file with the properties given. I looked through the manual page of the find command and looked for ways of pointing out the properties of the file. I found the -size, -type, and -executable options.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/8.png) ![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/9.png) ![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/10.png)

I put them together with the properties given and I ran the "find" command below:

_find -type f -size 1033c ! -executable_

It looks for a file that is size 1033 bytes and it is not executable. The output was ./maybehere07/.file2 and then I read the file I see that the password is: DXjZPULLxYr17uwoI01bNLQbtFemEgo7.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/11.png)

I also tried to find the file with the ls and grep command below but I could not figure out how to display the full path of the file.

_ls -laR | grep rw-r | grep 1033_

This commands looks for all files recursively and displays them the log way. Since it is being display the long way, I grep every file that has the rw-r permissions meaning is not executable and then grep the 1033 for the size and it found the file.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/12.png)

**Username:** bandit6

**Password:** DXjZPULLxYr17uwoI01bNLQbtFemEgo7

---

**Bandit Level 6 → Level 7**

[http://overthewire.org/wargames/bandit/bandit7.html](http://overthewire.org/wargames/bandit/bandit7.html)

The password for the next level is stored somewhere on the server and has all of the following properties:

_owned by user bandit7_

_owned by group bandit6_

_33 bytes in size_

For this part I used the find command below to find a file owned by user bandit7, owned by group bandit6 and 33 bytes of size.

_find / -user bandit7 -group bandit6 -size 33c 2>/dev/null_

_/ from root folder_

_-user the owner of the file._

_-group the group owner of the file._

_-size the size of the file._

_2>/dev/null redirects error messages to null so that they do not show on stdout._

With that command we found the file in the following directory: /var/lib/dpkg/info/bandit7.password. When I read the file I found that the password is HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/13.png)

**Username:** bandit7

**Password:** HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

---

**Bandit Level 7 → Level 8**

[http://overthewire.org/wargames/bandit/bandit8.html](http://overthewire.org/wargames/bandit/bandit8.html)

The password for the next level is stored in the file data.txt next to the word millionth.

When I logged into the profile I saw the data.txt file and when I read it a bunch of lines came up and it looked like all of the lines had the same structure. It was a word followed by some spaces and then a possible password. The hint was that the password is next to the word millionth, so I used the command below to read the file and then grep the word millionth.

_cat data.txt | grep millionth_

The command only return 1 line and it contained the password:

_millionth cvX2JJa4CFALtqS87jk27qwqGhBM9plV_

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/14.png)

**Username:** bandit8

**Password:** cvX2JJa4CFALtqS87jk27qwqGhBM9plV

---

**Bandit Level 8 → Level 9**

[http://overthewire.org/wargames/bandit/bandit9.html](http://overthewire.org/wargames/bandit/bandit9.html)

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

To do this I ran the command below in order to sort the lines alphabetically and then remove all duplicates from the output.

_sort data.txt | uniq -u_

The password is: UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/15.png)

**Username:** bandit9

**Password:** UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

**Reference:**

[http://www.liamdelahunty.com/tips/linux_remove_duplicate_lines_with_uniq.php](http://www.liamdelahunty.com/tips/linux_remove_duplicate_lines_with_uniq.php)

---

**Bandit Level 9 → Level 10**

[http://overthewire.org/wargames/bandit/bandit10.html](http://overthewire.org/wargames/bandit/bandit10.html)

The password for the next level is stored in the file data.txt in one of the few human-readable strings, beginning with several �=� characters.

According to the hint, the file contains both strings and binary data which can make it difficult to read. In order to sort out the plain text I ran "cat data.txt | string". The next part is to grep the lines that start with the = sign. To do everything in 1 line I used the command below:

_cat data.txt | strings | grep ^=_

This returned 3 strings:

_========== password_

_========== ism_

_========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk_

They all start with several = signs and if all of the passwords follow the same format the password should be the last line: truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk.

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/16.png)

**Username:** bandit10

**Password:** truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

---

**Bandit Level 10 → Level 11**

[http://overthewire.org/wargames/bandit/bandit11.html](http://overthewire.org/wargames/bandit/bandit11.html)

The password for the next level is stored in the file data.txt, which contains base64 encoded data

The data.txt contains 1 line that was encoded in base64. In order to decode the file I ran the command below:

_cat data.txt | base64 --decode_

The output was the following: The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/17.1.png)

**Username:** bandit11

**Password:** IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

---

**Bandit Level 11 → Level 12**

[http://overthewire.org/wargames/bandit/bandit12.html](http://overthewire.org/wargames/bandit/bandit12.html)

The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

The data.txt file contains 1 line that was encrypted with the ROT13 algorithm. In order to decrypt it, I have to replace every letter by the letter 13 positions ahead. For example, with this encryption the letter a would be replaced with n. The word banana would encrypt to onanan. To decrypt the line I ran the command below:

_cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'_

The command above send the line to stdout where tr shifts every letter 13 positions. After running that command the output was:

_The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu_

![](https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/pictures/17.png)

**Username:** bandit12

**Password:** 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu