## Keyed XOR

By PGODULTIMATE

This problem was one that I truly used team work on. A XOR problem is generally defined as an encryption where there is a repeating key that can be used to both encrypt and decrypt a message. A great mathematical sample was provided to me by **@Ptomerty **where `a XOR b XOR a` is just equal to `b`. My teammate was the first one to take a look at this problem and set up but keeping this concept in mind.

```py
from binascii import unhexlify
from binascii import hexlify 

x = []
for i in '     \
     \
                 \
         \
     \
    ':
      x.append(ord(i))
i = 0
while i < 20538:
    key = 'superhuman'
    flag = ''
    for i in range(len(x)):
        flag += chr(x[i] ^ ord(key[i % len(key)]))
    print("\n" + flag)
    i += 1
```

Basically he knew he was being given an encrypted message and needed to use the common key to break the **XOR**. However, his code gave us **easyctf{** but nothing else. He said he got this first word by reverse engineering the code based on the fact that the format is going to be **easyctf{}**. Nonetheless, while looking at this code I was thinking, "Whats with all those spaces?" After asking my teammate he told me that this is what was printed when he opened the file. This struck me as odd because I know the XOR format is like `\x<hex>`. That means that our text editor is not able to view the characters. So, I wrote another program to work on this.

```py
f = open("/Users/PGoel/Desktop/xortext.txt","rb")
file_contents = f.read()
print (file_contents)
f.close()
```

Make sure that this code is on **python3** or this would not work. FYI I am reading the XOR File given. The result I get is:

```
\x16\x14\x03\x1c\x11\x1c\x13\x16\x07\x02\x02\x08\x0b\x1c\x04\t\x15\r\x15\x02\x15\x19\x11\x02\x1b\x1b\x1d\x1a\r\t\x14\x07\x0c\x01\x16\x02\x06\t\x0e\x11\x02\x1d\t\x15\x1b\n\x11\t\x19\x1a\x15\x03\x05\x02\x0e\x04\n\x10\x06\x11\x03\x16\x19\x0c\x1a\r\x03\x0e\x11\x19\x11\x07\x15\x17\x18
```

I then discussed with my teammate what this means and how to actually use it in the code we had because I was too lazy to rewrite said code. My teammate then suggested to convert it into an array of individual numbers because that's what the `ord()` function did anyway. So we used this code:

```py
xored = "\x16\x14\x03\x1c\x11\x1c\x13\x16\x07\x02\x02\x08\x0b\x1c\x04\t\x15\r\x15\x02\x15\x19\x11\x02\x1b\x1b\x1d\x1a\r\t\x14\x07\x0c\x01\x16\x02\x06\t\x0e\x11\x02\x1d\t\x15\x1b\n\x11\t\x19\x1a\x15\x03\x05\x02\x0e\x04\n\x10\x06\x11\x03\x16\x19\x0c\x1a\r\x03\x0e\x11\x19\x11\x07\x15\x17\x18"
x = []
for i in xored:
  x.append(ord(i))
print(x)
```

The result is:

```
[22, 20, 3, 28, 17, 28, 19, 22, 7, 2, 2, 8, 11, 28, 4, 9, 21, 13, 21, 2, 21, 25, 17, 2, 27, 27, 29, 26, 13, 9, 20, 7, 12, 1, 22, 2, 6, 9, 14, 17, 2, 29, 9, 21, 27, 10, 17, 9, 25, 26, 21, 3, 5, 2, 14, 4, 10, 16, 6, 17, 3, 22, 25, 12, 26, 13, 3, 14, 17, 25, 17, 7, 21, 23, 24]
```

So I put this in the previous code and the results were a little better. I still had a major problem: I only know one of the flags. I wasn't even sure if **Superhuman** was the only word that resulted in **easyctf{**! So I decided to just feed in every single key until I figure it out. I first decided to see if Superhuman was indeed the right first key.

```py
from binascii import unhexlify
from binascii import hexlify
x = [22, 20, 3, 28, 17, 28, 19, 22, 7, 2, 2, 8, 11, 28, 4, 9, 21, 13, 21, 2, 21, 25, 17, 2, 27, 27, 29, 26, 13, 9, 20, 7, 12, 1, 22, 2, 6, 9, 14, 17, 2, 29, 9, 21, 27, 10, 17, 9, 25, 26, 21, 3, 5, 2, 14, 4, 10, 16, 6, 17, 3, 22, 25, 12, 26, 13, 3, 14, 17, 25, 17, 7, 21, 23, 24]
f = ""
i = 0
while i < 20538:
      f = raw_input()
    if (f == "contends"):
        print("reached end")
    key = f
    flag = ''
    for i in range(len(x)):
        flag += chr(x[i] ^ ord(key[i % len(key)]))
    if (flag.find("easyctf{") != -1):
        print("\n" + flag)
        print(f)
    i += 1
```

The result printed _**Superhuman**_ so that was correct. Now I had to just find out what the second key word was. Essentially I would do the same thing I did to determine if Superhuman was the right word for the second word. This basically just means that `key = 'superhuman' + f` instead of just **f**. So, I just replaced the code with this:

```py
from binascii import unhexlify
from binascii import hexlify
x = [22, 20, 3, 28, 17, 28, 19, 22, 7, 2, 2, 8, 11, 28, 4, 9, 21, 13, 21, 2, 21, 25, 17, 2, 27, 27, 29, 26, 13, 9, 20, 7, 12, 1, 22, 2, 6, 9, 14, 17, 2, 29, 9, 21, 27, 10, 17, 9, 25, 26, 21, 3, 5, 2, 14, 4, 10, 16, 6, 17, 3, 22, 25, 12, 26, 13, 3, 14, 17, 25, 17, 7, 21, 23, 24]
f = ""
i = 0
while i < 20538:
      f = raw_input()
    if (f == "contends"):
        print("reached end")
    key = 'superhuman' + f
    flag = ''
    for i in range(len(x)):
        flag += chr(x[i] ^ ord(key[i % len(key)]))
    if (flag.count("{") == 1 and flag.count("}") == 1 and flag.endswith("}")):
        print("\n" + flag)
        print(f)
    i += 1
```

I basically just filtered the results so that it printed out all the flags that were in the format **easyctf{}**. I got a bunch of different results, but there was an area that immediately stuck out to me:

    easyctf{flrznampcvgflagishwlgduibsfcgmtqhypibddxteqako~etpciihevcpwaupt}
    precedence

    easyctf{flqkjlanglvgflagishwlggdmqsethmtqhypibddxtf`drkcxqetpciihevcpwbdtg}
    scapegrace

    easyctf{flagflagflagflagishwlgwhaqsluhztqhypibddxtvlhrkjyqrtpciihevcpwrhxg}
    compensate

    easyctf{flrgxoazfbgqflagishwlgdhrsquf|bqhypibddxtelvqkwytbpciihevcpwahfd}
    possessors

    easyctf{flaitamglyqflagishwlgwfxisfthbbqhypibddxtvbqjk`xqjbpciihevcpwrfa}
    cathedrals

    easyctf{flfmxlagfhgqflagishwlgpbqslul|bqhypibddxtqfvrkjyutbpciihevcpwubfg}
    despensers

Mainly, it was the result for **compensate** because the start was just a bunch of "flagflagflag" in the beginning. I tested that out and it was correct:

```
easyctf{flagflagflagflagishwlgwhaqsluhztqhypibddxtvlhrkjyqrtpciihevcpwrhxg}
```



