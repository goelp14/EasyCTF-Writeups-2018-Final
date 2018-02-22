# Exclusive

By PGODULTIMATE

```
Given two integers a and b, return a xor b. Remember, the xor operator is a bitwise operator that's usually represented by the^character.

For example, if your input was 5 7, then you should print 2.
```

This challenge is pretty straight forward. You take in two integers and then XOR them.

```py
#!/usr/bin/env python3

def getXored(acid):
  a,b = acid.split() #stores input into two separate variables
  print (int(a) ^ int(b)) #prints the XOR of those variables
  
getXored(input()) #takes in input
```



