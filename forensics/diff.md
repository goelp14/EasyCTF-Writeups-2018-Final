# Diff

### By PGODULTIMATE

```
Sometimes, the differences matter. Especially between the files in this archive.

Hint: This is a TAR archive file. You can extract the files inside this tar by navigating to the directory where you downloaded it and running tar xf file.tar! If you don't have tar on your personal computer, you could try doing it from the Shell server. Once you extract the files, try comparing the hex encodings of the files against the first file.

Hint: Check the man page for diff by typing "man diff".
```

So the first thing I did was obviously download the files. There were four files and it was pretty clear that I would have to find the difference between them. Unfortunately there were exec files so I wasn't so sure how to approach that initially \(The hint was added later\). After some thinking and eventually the release of the hint, I thought that hexdumping it should at least give me something - and it did. However, I realized that it would take me way too long to figure out. I knew there had to be a way to do this pretty quickly so I continued my research. I eventually found [this website](https://stackoverflow.com/questions/12118403/how-to-compare-binary-files-to-check-if-they-are-the-same) on Stack Overflow. The one that talked about **vbindiff** stuck out to me the most so I decided to try that. In terminal I typed:

```
$ brew install vbindiff
```

This installed vbindiff \(I use a macbook\). Then I actually used it:

```
$ vbindiff path/to/file /path/to/file2
```

This gave me a really nice side by side comparison of the two files while highlighting the differences.

![](/assets/Screen Shot 2018-02-21 at 10.04.24 PM.png)

I showed the results of the comparison between **file** \(top\) and **file2** \(bottom\) because I wanted give you a perspective on how it looks but I would not be doing this for the rest of the files. The bits that were highlighted were:

```
0000 0000: 65
0000 0070: 61
0000 00E0: 73
0000 0110: 79 63 74
0000 0120: 66
0000 0180: 7B
0000 01E0: 64
0000 03A0: 69
```

This translates to **easyctf{di** which shows that I am doing this correctly. I know that the **file2** has priority over **file** because the hint mentioned to compare all the files to **file** and look for the differences.

I did the same processes for **file3** and got:

```
0000 00B0: 66
0000 0100: 66
0000 01F0: 69
0000 0310: 6E 69
0000 03C0: 74 6C
```

This is still part of the **file3** comparison but at this point I found one area where both files had a difference:

```
File:
0000 1100: 6C 61

File2:
0000 1100: 79 5F
```

This messed with me for a while because I wasn't sure if I should include that difference. However, after a little too long of time, I remembered that the hint said to compare against **file** so I thought I should give priority to **file3**. This ended up being the right approach.

_Differences continued:_

```
0000 11A0: 61 6E 5F
```

This yields **easyctf{diffinitly\_an\_** - I'm getting closer. I finally compare **file** to **file4** and find:

```
0000 04E0: 65 7A 5F 70 72 6F  62 6C 65 6D 21 7D
```

The problem writer must have gotten tired of hiding everything because this last one was all in one line. Putting everything together I get:

**easyctf{diffinitly\_an\_ez\_problem!}**

