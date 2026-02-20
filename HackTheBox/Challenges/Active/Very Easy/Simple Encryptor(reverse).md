
Hi! Today I will write about a reverse engineering very easy challenge that you can do without a internet conection. You just need to have the files provided by HTB.

After I downloaded the zip archive from HTB, I saw two archives in the directory. So, I used the ‘file’ command to learn more about the files.

![](https://miro.medium.com/v2/resize:fit:571/1*UH_8_V_-rZgZizsEItOZUQ.png)

One of the archives is an executable file, and another is a data file. So, I use the site [dogbolt.org](http://dogbolt.org/) to decompile the archive. I use the Ghidra version.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*6iIpBEPdNrQK33UsMvIJxQ.png)

I go to the ‘main’ function to understand what’s happening here.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*Pdx1UVZYrlRTOfSLWnwTcA.png)

When you disassemble a binary archive, it is usual for the code to not be very clear. But it basically does the following:

- `srand` sets a random value that is used to encrypt the flag;
- The `local_30` variable opens the flag;
- The `local_28` variable tells us the size of the flag;
- The `local_20` variable allocate the necessary memory for the flag.

Cool. Now we can understand the code. The only problem is the `rand()` function.

Well, everyone knows there are no real randoms in computer science, right? So, I went to the C official documentation to understand that function. And, according to [this](https://devdocs.io/c/numeric/random/rand):

'''`[srand()](https://devdocs.io/c/numeric/random/srand)` seeds the pseudo-random number generator used by `rand()`. If `rand()` is used before any calls to `srand()`, `rand()` behaves as if it was seeded with `[srand](http://en.cppreference.com/w/c/numeric/random/srand)(1)`. Each time `rand()` is seeded with `srand()`, it must produce the same sequence of values.'''

To summarize: if I use the same seed value, I will get the same sequence of numbers. Very good. Now, what I have to do is find the seed number and set it in the encryption process.

The encryption process looks like this:

![](https://miro.medium.com/v2/resize:fit:344/1*pT6orV6mcQO2P8rwEIIOnw.png)

So, to reverse the encryption process, the logic would be like this:

![](https://miro.medium.com/v2/resize:fit:379/1*hT73dtkU-CAxPi0FdZAiyQ.png)

The code shows us that the seed is a four-digit number. So, in a regular sort algorithm, I just need to try 9,999 times, and one of these will be the seed. Let’s do a loop!

![](https://miro.medium.com/v2/resize:fit:665/1*5RQ7O-CXLgLyBvyJ4J9V4A.png)

This is the exploit code I built for it. I used half an hour to write this because I forgot a lot about C syntax. But I won! By executing the exploit archive, I can find the flag.

![](https://miro.medium.com/v2/resize:fit:437/1*IvfT1o-WQ1oROlqeThPwXQ.png)

This is the most difficult challenge I have done so far, and I had a lot of fun solving it. Thank you for reading this, and see you later!