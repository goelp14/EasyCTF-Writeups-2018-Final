```
I got a letter in my email the other day... It makes me feel sad, but maybe it'll make you glad. :( file

Document:

EasyCTF University
February 10, 2018
Fips Bronsee
123 Fake Street
Bronseeland, AK 12304
USA

Dear Fip,
The Admissions Committee has carefully reviewed your application to the University of EasyCTF. After much consideration, I regret to inform you that we are unable to offer you a place in the Class of 2018. This year's applicant pool was the strongest in the university's history; in light of this, we were unable to offer admission to every worthy applicant.
I recognize that this message may come as a disappointment to you. Nevertheless, I encourage you to make your future educational plans with the same enthusiasm and initiative that led you to consider us.
We appreciate the interest you have shown in the University of EasyCTF. Best wishes as you pursue your educational goals.

Sincerely,
The Mods
The Mods
Office of Admissions
```

So what immediately stood out to me was that certain letters were bold. Spelling those letters out I get:

```
nevergonnagiveyouup
```

That was the answer.

Alas, no question would be that easy. Here is the real solution. I know that in most CTFs (from research) stuff that are word files generally are actually zip files. But, to be sure I opened it in a hex editor. The first 2 bits are:

```
50 4B
```

This translates to **PK**. With a little amount of research you would find that PK is indicative of a zip file so my hypothesis was correct.

So, I did some research and found that my terminal on mac can actually extract zip files. I actually found something similar from a different CTF [writeup](https://github.com/ctfs/write-ups-2015/tree/master/icectf-2015/forensics/document-troubles). I just followed what he did:

```
7z x <pathway to>/myletter.docx -oout
```
This gave me a folder called out. I opened it and found some suspicious stuff - mainly the picture named "template". I opened the image and voila it was the flag.

```
easyctf{r3j3ct3d_4nd_d3jected}
```


