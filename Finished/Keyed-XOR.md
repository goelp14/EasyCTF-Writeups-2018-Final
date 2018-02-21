## Keyed XOR
By PGODULTIMATE

This problem was one that I truly used team work on. My teammate was the first one to take a look at this one and set up:

```from binascii import unhexlify
from binascii import hexlify

x = []
for i in '	 \
	 \
				 \
		 \
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

This gave us **easyctf{** but nothing else. He said he got this first word by reverse engineering the code based on the fact that the format is going to be **easyctf{}**. Nonetheless, while looking at this code I was thinking, "Whats with all those spaces?" After asking my teammate he told me that this is what was printed when he opened the file. This struck me as odd because I know the XOR format is like **\x<hex>**. That means that our text editor is not able to view the characters. So, I wrote another program to work on this. 

```
f = open("/Users/PGoel/Desktop/xortext.txt","rb")
file_contents = f.read()
print (file_contents)
f.close()
```

Make sure that this code is on **python3** or this would not work. FYI I am reading the XOR File given. The result I get is:

```
\x16\x14\x03\x1c\x11\x1c\x13\x16\x07\x02\x02\x08\x0b\x1c\x04\t\x15\r\x15\x02\x15\x19\x11\x02\x1b\x1b\x1d\x1a\r\t\x14\x07\x0c\x01\x16\x02\x06\t\x0e\x11\x02\x1d\t\x15\x1b\n\x11\t\x19\x1a\x15\x03\x05\x02\x0e\x04\n\x10\x06\x11\x03\x16\x19\x0c\x1a\r\x03\x0e\x11\x19\x11\x07\x15\x17\x18
```

I then discussed what this means and how to actually use it in the code we had because I was too lazy to rewrite it. My teammate then suggested to convert it into an array of individual numbers because that's what the **ord()** function did anyway. So we used this code:

```
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

```
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

The result printed ***Superhuman*** so that was correct. Now I had to just find out what the second key word was. Essentially I would do the same thing I did to determine if Superhuman was the right word for the second word. This basically just means that **key = 'superhuman' + f** instead of just **f**. So, I just replaced the code with this:

```
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

I basically just filtered the results so that it printed out all the flags that were in the format **easyctf{}**. I got:

```
easyctf{flrgxoazfdzlflagishwlgdhrsqu`aqhypibddxtelvqkwyyipciihevcpwahfd}
possession

easyctf{flrixoagrhgqflagishwlgdfrslal|bqhypibddxtebvqkjmutbpciihevcpwaffd}
passengers

easyctf{flrzdzazfdzlflagishwlgducgsqu`aqhypibddxteqjdkwyyipciihevcpwauzq}
profession

easyctf{flrzdhajadzlflagishwlgducusar`aqhypibddxteqjvkg~yipciihevcpwauzc}
protection

easyctf{flvzjjaeyhgqflagishwlg`umwsnjl|bqhypibddxtaqdtkhfutbpciihevcpweuta}
travellers

easyctf{flrzdazfdzlflagishwlgducbsqu`aqhypibddxteqjakwyyipciihevcpwauzt}
procession

easyctf{flrmyayadzlflagishwlgdb~bsrr`aqhypibddxtefwakt~yipciihevcpwabgt}
perception

easyctf{flrmyzajadzlflagishwlgdb~gsar`aqhypibddxtefwdkg~yipciihevcpwabgq}
perfection

easyctf{flgfnahadpqflagishwlgqixsscr`kbqhypibddxtpmqpke~ycbpciihevcpwtiae}
entreaties

easyctf{floiyoa`yapqflagishwlgyf~rskjekbqhypibddxtxbwqkmf|cbpciihevcpw|fgd}
marseilles

easyctf{flce~oadpcaqflagishwlgujyrsocgzbqhypibddxttnpqkio~rbpciihevcpwpj`d}
amusements

easyctf{flagynajadzlflagishwlgwh~ssar`aqhypibddxtvlwpkg~yipciihevcpwrhge}
correction

easyctf{flq|jhadpcaqflagishwlggsmusocgzbqhypibddxtfwdvkio~rbpciihevcpwbstc}
statements

easyctf{flomxoagrhgqflagishwlgybrslal|bqhypibddxtxfvqkjmutbpciihevcpw|bfd}
messengers

easyctf{fl`zbxangbzoflagishwlgvueesetfa|qhypibddxtwqlfkcxi|pciihevcpwsu|s}
bridegroom

easyctf{flq|jnaepdrjflagishwlggsmssnc`iyqhypibddxtfwdpkhoyaypciihevcpwbste}
stareleigh

easyctf{flpmxlajadcgflagishwlgfbqsar`xtqhypibddxtgfvrkg~yptpciihevcpwcbfg}
respective

easyctf{flrznampcaqflagishwlgduibsfcgzbqhypibddxteq`ak`o~rbpciihevcpwaupt}
precedents

easyctf{flv`yyaypcvgflagishwlg`o~dsrcgmtqhypibddxtakwgkto~etpciihevcpweogr}
threepence

easyctf{flpmhnahadzlflagishwlgfbosscr`aqhypibddxtgffpke~yipciihevcpwcbve}
recreation

easyctf{flrzdzazfbgqflagishwlgducgsquf|bqhypibddxteqjdkwytbpciihevcpwauzq}
professors

easyctf{flrmyagalrgflagishwlgdb~bslrhitqhypibddxtefwakj~qatpciihevcpwabgt}
percentage

easyctf{fl`zbxazxl|fflagishwlgvueesqkhguqhypibddxtwqlfkwgqoupciihevcpwsu|s}
bridesmaid

easyctf{fldixhag|crqflagishwlgrfuslogibqhypibddxtsbvvkjc~abpciihevcpwwffc}
fastenings

easyctf{flfaxnazehvvflagishwlgpnssqvlmeqhypibddxtqjvpkwzueepciihevcpwunfe}
disrespect

easyctf{flfalnazfdzlflagishwlgpnkssqu`aqhypibddxtqjbpkwyyipciihevcpwunre}
digression

easyctf{flaiylagahgqflagishwlgwf~qslrl|bqhypibddxtvbwrkj~utbpciihevcpwrfgg}
carpenters

easyctf{flfaxoagahgqflagishwlgpnrslrl|bqhypibddxtqjvqkj~utbpciihevcpwunfd}
dissenters

easyctf{flrznampcvgflagishwlgduibsfcgmtqhypibddxteq`ak`o~etpciihevcpwaupt}
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

easyctf{flumorazqllqflagishwlgcbhosqbhwbqhypibddxtbfalkwnqbpciihevcpwfbqy}
wednesdays

easyctf{flrzdzazfbggflagishwlgducgsquf|tqhypibddxteqjdkwyttpciihevcpwauzq}
professore

easyctf{flrznhagfdzlflagishwlgduiuslu`aqhypibddxteq`vkjyyipciihevcpwaupc}
pretension

easyctf{flrznjagadcgflagishwlgduiwslr`xtqhypibddxteq`tkj~yptpciihevcpwaupa}
preventive

easyctf{fldgyza`axggflagishwlgrh~gskr||tqhypibddxtslwdkm~ettpciihevcpwwhgq}
forfeiture

easyctf{flv`yyazvbggflagishwlg`o~dsqef|tqhypibddxtakwgkwittpciihevcpweogr}
threescore
reached end
```
Immediately the result for ***compensate*** stood out because the start was just a bunch of "flagflagflag" in the beginning. I tested that out and it was correct:

```
easyctf{flagflagflagflagishwlgwhaqsluhztqhypibddxtvlhrkjyqrtpciihevcpwrhxg}
```
