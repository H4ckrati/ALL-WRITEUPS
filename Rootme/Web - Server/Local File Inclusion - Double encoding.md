
##### Challenge Description : 

Find the validation password in the source files of the website.


#### LFI via double encoding

This lab is taking about local file inclusion by decode

the first thing we note that the url contains parameter page that request the page or show it’s content by getting it’s name

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*f74NLy-diRCCx_FCOlMqxA.png)

so if we try some thing like : ../ or .. or ….// all of this will not work with the page that says attack detected because it catch them as LFI attack so we should decode them but will not work with one decode.

it works only with double decode .

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*Ka9g8CZepyLhP3S0MedEzg.png)
but if we use double decode for ../ like this : %252e%252e%2f

it will works and will tell us that there are error in the included files

![](https://miro.medium.com/v2/resize:fit:1400/1*-1bGfeaNrAO1tYszEvZVTg.png)

So we can guess the name of the file that is **_conf_**.inc.php this is a famous file in the php configuration now we can request it by it’s name in the page parameter

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*NHRrrjDkBv0nuQx06GhI_g.png)

this empty page executes it’s code but we don’t see it so we need to encode it’s content before we get to prevent the code from execution so we write our payload using php wrappers.

php%253A%252F%252Ffilter%252Fconvert%252ebase64-encode%252Fresource%253Dconf

this code return the source code of the page with encoding it

so we will get it in the page

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:2000/1*glcD-Uuo7Po_XF_riaJGlg.png)

take the content copy and decode it in the burp for base64

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:2000/1*Eo2H2rI5tz-EEhAZJudCyQ.png)

then we catch the flag : Th1sIsTh3Fl4g!

then submit it .

Good Luck !

### Explanation: 


>[!note]



>[!mote]