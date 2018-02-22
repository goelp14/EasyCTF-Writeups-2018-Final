# Over and Over

By PGODULTIMATE

```
over and over and over and over and over and ...

Given a number N, print the string "over [and over]" such that the string contains N "over"s. There should not be newlines in the string.

For example:

For N = 1, print "over".
For N = 5, print "over and over and over and over and over".

For Python, consider using for and range.

For Java/CXX, consider using a for loop.
Try doing it with while too for practice!
```

Again, another straight forward question with another straight forward solution.

```py
#!/usr/bin/env python3

def overAndOverAgain(N):
    output = "over" #initial
    addOn = " and over" 
    num = int(N) #make sure number is an integer
    for _ in range(1,num): #cycle through number of times needed for add on
            output += addOn
    if (num > 0):
        print (output)
    else:
        print ("") #contingency plan in case problem tries to trick you
                
overAndOverAgain(input()) #you should know by now
```



