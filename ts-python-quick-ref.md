# Python Quick Reference

[TOC]

A range of common python usages and idioms

## xkcd
On April 30, 2018, https://xkcd.com/1987/ got published. This wasn't the first time that Randall Munroe posted about Python (probably the most famous comic was his [import antigravity](https://xkcd.com/353/) one which became an actual module in the stdlib as an easter egg), but it was the first comic that people construed as negative towards Python.

## Python from the Command Line

### The -m option
The "m" stands for modules as in running modules in Python's path as scripts themselves.

#### Running IDLE from command line within a VirtualEnvironment
```python
python -m idlelib
```
Can also add a python program at the end of the command line to execute that script when IDLE starts

#### Functional http server
One cool thing you can do is to quickly open a functional http server for a particular directory. This comes in handy if you have something in the directory that you would like to share with someone on your network.
```python
python -m http.server
```

## Programming in one lesson
Almost everything you do in programming can be reduced to one basic pattern:

    Input -> Process -> Output

This IPO pattern is simple, but powerful. Once you learn it, you will begin to see it everywhere.

### Linux processes
Every time Linux creates a process, it automatically connects a standard input and a standard output to that process.

    STDIN -> PROCESS -> STDOUT

This is the IPO pattern in its purest form.

It allows you to redirect inputs and outputs on the command line

    PROCESS <INPUT >OUTPUT

and pipe processes together

    PROC_1 | PROC_2 | PROC_3

### Calling a function
Function calls are another example of IPO.
For example, Python’s map function

    OUTPUT_LIST = map(PROCESS, INPUT_LIST)

### User defined functions
You also use this pattern when you write your own functions.

```
def function(INPUT):
    PROCESS
    return OUTPUT
```

### Comprehensions
Comprehensions and generator expressions also follow this pattern

    OUTPUT_LIST = [PROCESS for INPUT in values]

### The for loop
Even a for loop is an example of IPO

```
for INPUT in values:
    PROCESS
    OUTPUT
```

### References
[Wikipedia: IPO model](https://en.wikipedia.org/wiki/IPO_model)

-----------------------------------------------------------------------------

## Testing

### doctest module

Were you aware you can also write your own simple tests for functions by putting the tests into the function's documentation?
The doctest module can help with that.

Here, you can use the doctest module and -m option to test your function:

```python
#!/usr/bin/env python
def my_func(a, b=2):
    """
    >>> my_func(5)
    10
    >>> my_func(5, 3)
    15
    >>> my_func('Hello')
    'HelloHello'
    """
    return a * b
```
Note that in the comments for the function there are tests with a few different sets of parameters. And yep, you guessed it, we can use -m doctest here to run a test.

In this instance, call python -m like so (note that the -v gives you more output):

```
$ python -m doctest -v demo1.py 
Trying:
    my_func(5)
Expecting:
    10
ok
Trying:
    my_func(5, 3)
Expecting:
    15
ok
Trying:
    my_func('Hello')
Expecting:
    'HelloHello'
ok
1 items had no tests:
    demo1
1 items passed all tests:
   3 tests in demo1.my_func
3 tests in 2 items.
3 passed and 0 failed.
Test passed.
```

## Statements

### Comprehensions

A ur-Comprehension could be written as follows:
```python
new_range = [i * i        for i in range(5)   if i % 2 == 0]
```
This is conceptually represented as:
```python
*result*  = [*transform*  *iteration*         *filter*     ]
```
The filter piece answers the question, "should this item be transformed?" If the answer is yes, then the transform piece is evaluated and becomes an element in the result. The iteration [*] order is preserved in the result.

This
```python
new_list = [expression(i) for i in old_list if filter(i)]
```
is equivalent to:
```python
new_list = []
for i in old_list:
    if filter(i):
        new_list.append(expressions(i))
```

The basic syntax is
```python
[ expression for item in list if conditional ]
```
which is equivalent to:
```python
for item in list:
    if conditional:
        expression
```

#### Comprehensions Examples
```python
x = [i for i in range(10)]

squares = [x**2 for x in range(10)]

multiplied = [item*3 for item in list1]

listOfWords = ["this","is","a","list","of","words"]
items = [ word[0] for word in listOfWords ]

[x.lower() for x in ["A","B","C"]]

string = "Hello 12345 World"
numbers = [x for x in string if x.isdigit()]

[x+y for x in [10,30,50] for y in [20,40,60]]

def double(x):
  return x*2
[double(x) for x in range(10)]
[double(x) for x in range(10) if x%2==0]

[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]

matrix = [
...     [1, 2, 3, 4],
...     [5, 6, 7, 8],
...     [9, 10, 11, 12],
... ]
The following list comprehension will transpose rows and columns:
>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

```

#### Comprehensions Resources
https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions
http://blog.cdleary.com/2010/04/learning-python-by-example-list-comprehensions/
http://effbot.org/zone/python-list.htm

### With
See below
http://cmdlinetips.com/2016/01/opening-a-file-in-python-using-with-statement/

## File Processing

### Catching a File Not Found error (unable to open file)

```python
try:
    f = open('foo.txt')
except IOError:
    print('error')
else:
    with f:
        print f.readlines()
```

### Read One Line At A Time

This will actually read a text file one line at a time ...
```python
with open(filename, "rU") as f:
    for eachLine in f:
        print(eachLine)
```
Other options here: http://cmdlinetips.com/2011/08/three-ways-to-read-a-text-file-line-by-line-in-python/
```python
## Open the file with read only permit
f = open('myTextFile.txt')
## Read the first line 
line = f.readline()

## If the file is not empty keep reading line one at a time until the file is empty
while line:
    print(line)
    line = f.readline()
f.close()
```

## String Processing

### Convert Digits in a String into a Number
Use the regular expression library (first line)
The second line returns a list of sets of digits which may be in the string
The third line gets the first set of digits in the list returned (the [0]) and converts to an integer
```python
import re
digit_list = re.findall('\d+', your_string)
int(digit_list[0])
```

### Create a String from a Repeated Character

To simply repeat the same letter 10 times:

```python
string_val = "x" * 10  # gives you "xxxxxxxxxx"
```
And if you want something more complex, like n random lowercase letters, it's still only one line of code (not counting the import statements and defining n):

```python
from random import choice
from string import lowercase
n = 10
string_val = "".join(choice(lowercase) for i in range(n))
```

## Printing
Use .format() for printing 

### Using Commas in Numbers

For floating point:
```python
f=1546139978.505344
'{:20,.2f}'.format(f)
```

For integers:
```python
i=1546139978
'{:,d}'.format(i)
```

### Additional Print Capabilities
In #Python you can use print+join on a list, but look closer what the print function can do by itself ...
```python
>>> row = ["1", "bob", "developer", "python"]
>>> print(','.join(str(x) for x in row))
1,bob,developer,python
>>> print(*row, sep=',')
1,bob,developer,python
```

## Input

### Taking input from console in Python

```python
input1 = input()
num1 = int(input()) # also float() or str() etc
x, y = input("Enter a two value: ").split()
x, y, z = input("Enter a three value: ").split() 
# taking multiple inputs at a time  
# and type casting using list() function 
x = list(map(int, input("Enter a multiple value: ").split())) 
print("List of students: ", x) 
```


## Dictionaries / Dictionary Processing
### Check whether key is in the dictionary
```python
if 'name' in mydict:
```
Also, "name" in dict will work with any iterable and not just dictionaries.

### Creating dictionaries

See https://developmentality.wordpress.com/2012/03/30/three-ways-of-creating-dictionaries-in-python/


#### Dictionary literals
Perhaps the most commonly used way of constructing a python dictionary is with curly bracket syntax:

```python
d = {"age":25}
```

As dictionaries are mutable, you need not know all the entries in advance:

```python
# Empty dict
d = {}
# Fill in the entries one by one
d["age"] = 25
```

#### From a list of tuples
You can also construct a dictionary from a list (or any iterable) of key, value pairs. For instance:

```python
d = dict([("age", 25)])
```

This is perhaps most useful in the context of a list comprehension:

```python
class Person(object):
    def __init__(self, name, profession):
        self.name = name
        self.profession = profession
 
people = [Person("Nick", "Programmer"), Person("Alice","Engineer")]
professions = dict([ (p.name, p.profession) for p in people ])
print(professions)
# {"Nick": "Programmer", "Alice": "Engineer"}
```

This is equivalent, though a bit shorter, to the following:

```python
people = [Person("Nick", "Programmer"), Person("Alice","Engineer")]
professions = {}
for p in people:
    professions[p.name] = p.profession

This form of creating a dictionary is good for when you have a dynamic rather than static list of elements.

#### From two parallel lists
This method of constructing a dictionary is intimately related to the prior example. Say you have two lists of elements, perhaps pulled from a database table:

```python
# Static lists for purpose of illustration
names = ["Nick", "Alice", "Kitty"]
professions = ["Programmer", "Engineer", "Art Therapist"]
```

If you wished to create a dictionary from name to profession, you could do the following:

```python
professions_dict = {}
for i in range(len(names)):
    professions_dict[names[i]] = professions[i]
```

This is not ideal, however, as it involves an explicit iterator, and is starting to look like Java. The more Pythonic way to handle this case would be to use the zip method, which combines two iterables:

```python
print(zip(range(5), ["a","b","c","d","e"]))
# [(0, "a"), (1, "b"), (2, "c"), (3, "d"), (4, "e")]

names_and_professions = zip(names, professions)
print(names_and_professions)
# [("Nick", "Programmer"), ("Alice", "Engineer"), ("Kitty", "Art Therapist")]
 
for name, profession in names_and_professions:
    professions_dict[name] = profession
```

As you can see, this is extremely similar to the previous section. You can dispense with the iteration, and instead use the dict method:

```python
professions_dict = dict(names_and_professions)
# You can dispense with the extra variable and create an anonymous
# zipped list:
professions_dict = dict(zip(names, professions))
```

### IN operator
Multiple comparisons in #Python? Just use "in":
```python
>>> colors = 'blue red green yellow black white orange'.split()
>>> color = random.choice(colors)
# is this a primary color?
>>> color == 'red' or color == 'blue' or color == 'yellow'
True
>>> color in 'red yellow blue'.split():
True
```

### Conditional Expressions
One liner for 2 expressions and 1 condition (also known as a ternary operator).
See https://stackoverflow.com/questions/394809/does-python-have-a-ternary-conditional-operator?rq=1

    expression1 if condition else expression2

```python
>>> a = 1
>>> b = 2
>>> 1 if a > b else -1 
-1
>>> 1 if a > b else -1 if a < b else 0
-1
```


## Sets
You want to compare 2 sequences in #Python? set operations are your best friend:
```python
>>> a = {1, 2, 3, 4, 5}  # or set() cast a list
>>> b = {1, 2, 3, 6, 7, 8}
# unique to a
>>> a - b
{4, 5}
# unique to b
>>> b - a
{8, 6, 7}
# in both sets
>>> a & b
{1, 2, 3}
# in either one or the other
>>> a ^ b
{4, 5, 6, 7, 8}
```

## Shell Processing
### Execute Shell Command and return output
```bash
    import subprocess
    # Define the command to execute as a single string
    setCommand = "export {}={}".format(varName, VarValue)
    # Execute the command, make sure ", shell=True" is at the end
    subprocessOutput = subprocess.check_output(setCommand, shell=True)
    # the output from the command is in the return string (subprocessOutput)
```

## Directory Listing
```python
import os

for dirname, dirnames, filenames in os.walk('.'):
    # print path to all subdirectories first.
    for subdirname in dirnames:
        print(os.path.join(dirname, subdirname))

    # print path to all filenames.
    for filename in filenames:
        print(os.path.join(dirname, filename))

    # Advanced usage:
    # editing the 'dirnames' list will stop os.walk() from recursing into there.
    if '.git' in dirnames:
        # don't go into any .git directories.
        dirnames.remove('.git')
```

## File Manipulations
### File Renaming
```python
import os
os.rename(oldfilename, newfilename)
```

### File copying
```python
import shutil
shutil.copyfile(originalfilename, copiedfilename)
```

### File Names and Extensions

```python
import os
pathname="/a/b/c.txt"
# Get just the filename and extension
fnmext = os.path.basename(pathname)
print(fnmext)
# Get just the filename, with the path before, if already there
os.path.splitext(pathname)[0]
os.path.splitext(fnmext)[0]
# Get just the extension
os.path.splitext(pathname)[1]
os.path.splitext(fnmext)[1]
```

### Not Overwriting a File
Use 'x' with open in #Python to write to a file that doesn't already exist (like bash's noclobber)
```python
>>> with open('hello', 'w') as f:
...     f.write('hello')
...
>>> with open('hello', 'x') as f:
...     f.write('hello')
...
FileExistsError: [Errno 17] File exists: 'hello'
```

## Colours

You can use a string specifying the proportion of red, green, and blue in hexadecimal digits:

    #rgb    Four bits per color
    #rrggbb Eight bits per color
    #rrrgggbbb  Twelve bits per color

For example, '#fff' is white, '#000000' is black, '#000fff000' is pure green, and '#00ffff' is pure cyan (green plus blue).

You can also use any locally defined standard color name. The colors 'white', 'black', 'red', 'green', 'blue', 'cyan', 'yellow', and 'magenta' will always be available. Other names may work, depending on your local installation.

### Colours under Linux
The set of colours is maintained in:

    /usr/share/X11/rgb.txt
    


## python markdown

The Python Markdown package should be installed
( sudo -H pip3 install -U markdown )

This can either be used inside a python program
(see http://pythonhosted.org/Markdown/reference.html for details)
or operate from the command line
(see http://pythonhosted.org/Markdown/cli.html for details).

Also see http://pythonhosted.org/Markdown/extensions/index.html
for useful extensions

## Object Introspection
Below is a routine which will print out the name of the object and any variables set, in a recursive manner.
```python
def rec_str(obj, extra):
   return str(obj.__class__) + '\n' + '\n'.join(
       (extra + (str(item) + ' = ' + 
       (rec_str(obj.__dict__[item],extra + '    ') if hasattr(obj.__dict__[item], '__dict__') else str(obj.__dict__[item])))
       for item in sorted(obj.__dict__)))
```
Examples of its use ...
```python
class A():
   def __init__(self):
       self.attr1 = 1
       self.attr2 = 2
       self.attr3 = 'three'

class B():
   def __init__(self):
       self.a = A()
       self.attr10 = 10
       self.attrx = 'x'
               
class C():
   def __init__(self):
       self.b = B()
                                               
class X():
   
   def __str__(self):
       return rec_str(self, '')
       
   def __init__(self):
       self.abc='abc'
       self.attr12=12
       self.c = C()
       
       
x = X()

print(str(x))
```

Output:
```
<class '__main__.X'>
abc = abc
attr12 = 12
c = <class '__main__.C'>
    b = <class '__main__.B'>
        a = <class '__main__.A'>
            attr1 = 1
            attr2 = 2
            attr3 = three
        attr10 = 10
        attrx = x
```

Alternatively, you can make a class NicePrint and for each class that you want to give this type of str output, inherit from this class, like:

```python
class X(NicePrint):
    ...
```
Note that the subclasses do not have to inherit from NicePrint.

The code for NicePrint:
```python
class NicePrint():
    
    @staticmethod
    def rec_str(obj, extra):
        return str(obj.__class__) + '\n' + '\n'.join(
            (extra + (str(item) + ' = ' + 
            (NicePrint.rec_str(obj.__dict__[item],extra + '    ') if hasattr(obj.__dict__[item], '__dict__') else str(obj.__dict__[item])))
            for item in sorted(obj.__dict__)))
            
    def __str__(self):
        return NicePrint.rec_str(self, '')
```

## tkinter
http://infohost.nmt.edu/tcc/help/pubs/tkinter/web/index.html

### Fonts
http://infohost.nmt.edu/tcc/help/pubs/tkinter/web/fonts.html

Depending on your platform, there may be up to three ways to specify type style.

As a tuple whose first element is the font family, followed by a size (in points if positive, in pixels if negative), optionally followed by a string containing one or more of the style modifiers bold, italic, underline, and overstrike.

Examples: ('Helvetica', '16') for a 16-point Helvetica regular; ('Times', '24', 'bold italic') for a 24-point Times bold italic. For a 20-pixel Times bold font, use ('Times', -20, 'bold').

You can create a “font object” by importing the tkFont module and using its Font class constructor:
```python
    import tkFont
    font = tkFont.Font(option, ...)
```
where the options include:

    family  The font family name as a string.
    size    The font height as an integer in points. To get a font n pixels high, use -n.
    weight  'bold' for boldface, 'normal' for regular weight.
    slant   'italic' for italic, 'roman' for unslanted.
    underline   1 for underlined text, 0 for normal.
    overstrike  1 for overstruck text, 0 for normal.

For example, to get a 36-point bold Helvetica italic face:
```python
    helv36 = tkFont.Font(family='Helvetica', size=36, weight='bold')
```
If you are running under the X Window System, you can use any of the X font names. For example, the font named '-*-lucidatypewriter-medium-r-*-*-*-140-*-*-*-*-*-*' is a good fixed-width font for onscreen use. Use the xfontsel program to help you select pleasing fonts.

To get a list of all the families of fonts available on your platform, call this function:
```python
    tkFont.families()
```

## Python Debugging
### Using pdb
#### Printing variables
https://stackoverflow.com/questions/21961693/how-to-print-all-variables-values-when-debugging-python-with-pdb-without-specif

pdb is a fully featured python shell, so you can execute arbitrary commands.

locals() and globals() will display all the variables in scope with their values.

You can use dir() if you're not interested in the values.

When you declare a variable in Python, it's put into locals or globals as appropriate, and there's no way to distinguish a variable you defined and something that's in your scope for another reason.

When you use dir(), it's likely that the variables you're interested in are at the beginning or end of that list. If you want to get the key, value pairs

Filtering locals() might look something like:
```python
>>> x = 10
>>> y = 20
>>> {k: v for k,v in locals().iteritems() if '__' not in k and 'pdb' not in k}
{'y': 20, 'x': 10}
```
If your locals() is a real mess, you'll need something a little more heavy handed. You can put the following function in a module on your pythonpath and import it during your debugging session.
```python
def debug_nice(locals_dict, keys=[]):
    globals()['types'] = `__import__`('types')
    exclude_keys = ['copyright', 'credits', 'False', 
                    'True', 'None', 'Ellipsis', 'quit']
    exclude_valuetypes = [types.BuiltinFunctionType,
                          types.BuiltinMethodType,
                          types.ModuleType,
                          types.TypeType,
                          types.FunctionType]
    return {k: v for k,v in locals_dict.iteritems() if not
               (k in keys or
                k in exclude_keys or
                type(v) in exclude_valuetypes) and
               k[0] != '_'}
```

## Python Packages
### pip
Find pip packages mactching a term:

```
pip search <term>

e.g.
pip search yaml
```

#### Installing using pip for a specific version of python

    python<x> -m pip install <package>

where <x> is the python version, ie python3.7 etc.

May also need to add --user at the end if there are permission problems.

#### Upgrading pip

    python<x> -m pip install --upgrade pip --user

where <x> is the python version, ie python3.7 etc.

### pipenv

