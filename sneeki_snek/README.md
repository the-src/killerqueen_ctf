## Sneeki Snek

We have a file and its content is like

```
  4           0 LOAD_CONST               1 ('')
              2 STORE_FAST               0 (f)

  5           4 LOAD_CONST               2 ('rwhxi}eomr\\^`Y')
              6 STORE_FAST               1 (a)

  6           8 LOAD_CONST               3 ('f]XdThbQd^TYL&\x13g')
             10 STORE_FAST               2 (z)

  7          12 LOAD_FAST                1 (a)
             14 LOAD_FAST                2 (z)
```
So first we need to understand what this is. With a bit of quick research we found our answer.

### It's (Python bytecode)[https://opensource.com/article/18/4/introduction-python-bytecode] (assembly).

So let's explain some basic details.

- The number in the start  represents the line number in our actual python code.
- Definitions such as "LOAD_CONST" or "LOAD_FAST" are represents instructions.
- Last part is the actual python code, our variables, our loops and etc.

For example when we saw 4 and 5. line theirs python code is something like:

```py
f = ''
a = 'rwhxi}eomr\\^`Y'
```
You can think of we are loading the f character and storing it as empty string; loading a, storing it as "rwhxi}eomr\\^`Y".

SO IN ORDER TO FIND THE ANSWER WE NEED TO EVALUATE ALL CODE TO PYTHON.

### But there is a simple module for this to get this easier. The "dis" module.

Simply write your code into a function, import the dis module andddd..

```py
import dis

def myfunc():
  f = ''
  a = 'rwhxi}eomr\\^`Y'
dis.dis(myfunc)
```
Then check whether is output similar to our bytecode.

#### Try and write!

Our actual python code looks like this:

```py
f = ''
a = 'rwhxi}eomr\\^`Y'
z = 'f]XdThbQd^TYL&\x13g'
a = a+z
for i,b in enumerate(a):
    c = ord(b)
    c = c - 7
    c = c + i
    c = chr(c)
    f += c
print(f)
```

Execute this line gives you the flag:

# kqctf{dont_be_mean_to_snek_:(}
