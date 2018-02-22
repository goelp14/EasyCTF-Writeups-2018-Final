# Taking Input

By PGODULTIMATE

```
OK, OK, you got Hello, world down, but can you greet specific people?

You'll be given the input of a certain name. Please greet that person using the same format. For example, if the given input is Michael, print Hello, Michael!.

For Python, consider the input() function.
For Java, consider System.in.
For C, consider including stdio.h and reading input using read.
For C++, consider including iostream and reading input using c in.
```

This is basically doing a pretty simple Hello World but with interchangeable names in front.

```py
#!/usr/bin/env python3

def printName(name): #take in the name as a parameter
    nameFull = "Hello, " + name + "!" #just add the name
    print (nameFull) #this is not hard

printName(input())
```



