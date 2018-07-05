---
title: introducing-python-object-types
date: 2018-07-02 22:52:03
tags:
    - python
    - note
categories: language core
---
> This is the reading note for "Chapter 4: Introducing Python Object Types, Learning python 5th edition". 

Everything is an object in a Python script. </br>

Python programs can be decomposed into _modules_, _statements_, _expressions_ and _objects_:
- Programs are composed of moudles.
- Modules contain statements.
- Statements contain expressions.
- Expressions create and process objects.

## Core data types
![built-in-types][1]
The built-in types are often more efficient, they employ already optimized data structure algorithms that are implemented in C for speed. </br>
Program units such as functions, modules and classes are objects in Python. They are created with statements and expressions such as `def`, `class`, `import` and `lambda`. </br>
There are no type declarations in Python, the syntax of the expressions you run determines the types of objects you create and use. Once you craete an object, you bind its operation set for all time - you can perform only string operations on a string and list operations on a list. This means that Python is dynamically typed, a model that keeps track of types for you automatically instead of requiring declaration code, but it is also **strongly typed**, a constraint that means you can perform on an object only operations that are valid for its type. </br>


## Numbers
Python's core objects set includes the usual suspects: _integers_ that have no fractional part, _floating-point_ numbers that do, and more exotic types - _complex_ numbers with imaginary parts, _decimals_ with fixed precision, _rationals_ with numberator and denominator, and full-featured _sets_. 

## Strings
Strings are sequences of one-character strings; other, more general sequence types include _lists_ and _tuples_.  </br>

Sequence operations have _indexing_ expressions like `len(s)`, `s[0]` and `s[-1]`. </br>
In addition to simple positional indexing, sequences also support a more general form of indexing knowns as _slicing_:
```python
s = 'spam'
s[1:3] # give me everything from 1 up to but not including 3
s[1:] # the right bound defaults to the length of the sequence being sliced
s[:3] # the left bound defaults to zero
s[:-1] # equal to [0:-1]
```
As sequence, string also support _concatenation_ and _repetition_. 
```python
s+'xyz' # 'spamxyz'
s*8 # repeat 0 times
```

The string is immutable. _numbers_ , _strings_ and _tuples_ are immutable. _lists_, _dictionaries_ and _sets_ are mutable. <br/>

Strictly speaking, you can change text-based data in place if you either expand it into a _list_ of individual chars and join it back together with nothing between, or use the newer `bytearray` type. 

```python
# cast to list
s = 'abc'
L = list(s)
''.join(L)

# convert to byte array
B = bytearray(b'spam')
B.extend(b'eggs')
B.decode()
```
The `bytearray` supports in-place chagnes for text, but only for text whose chars are all at most 8-bits wide. 

String also comes with other type-specific methods like `find()`, `replace()`, `split()`, `rstrip()`. Advanced substitution operation known as _formatting_. 

```python
>>> S = 'Spam'
>>> S.find('pa')                 # Find the offset of a substring in S
1
>>> S
'Spam'
>>> S.replace('pa', 'XYZ')       # Replace occurrences of a string in S with another
'SXYZm'
>>> S
'Spam'
>>> line = 'aaa,bbb,ccccc,dd'
>>> line.split(',')              # Split on a delimiter into a list of substrings
['aaa', 'bbb', 'ccccc', 'dd']
>>> S = 'spam'
>>> S.upper()                    # Upper- and lowercase conversions
'SPAM'
>>> S.isalpha()                  # Content tests: isalpha, isdigit, etc.
True
>>> line = 'aaa,bbb,ccccc,dd\n'
>>> line.rstrip()                # Remove whitespace characters on the right side
'aaa,bbb,ccccc,dd'
>>> line.rstrip().split(',')     # Combine two operations
['aaa', 'bbb', 'ccccc', 'dd']

>>> '%s, eggs, and %s' % ('spam', 'SPAM!')
'spam, eggs, and SPAM!'

>>> '{}, eggs, and {}'.format('spam', 'SPAM!')
'spam, eggs, and SPAM!'

>>> '{:,.2f}'.format(296999.2567) 
'296,999.26'

>>> '%.2f | %+05d' % (3.14159, -42)
'3.14 | -0042'
```

One note here: although sequence operations are generic, methods are not. As a rule of thumb, Python's toolset is layered: generic operations that span multiple types show up as built-in functions or expressions (e.g. `len(X)`, `X[0]`), but type-specific operations are method calls (e.g. `aString.upper()`). 

Python provide us a variety of ways for us to code strings. For instance, special characters can be represented as backslash escape sequences, which Python displays in `\xNN` hexadecimal escape notation, unless they represent printable characters. So if a `\xNN` appears, that means the chars are not printable. 

```python
>>> S = 'A\nB\tC'            # \n is end-of-line, \t is tab
>>> len(S)                   # Each stands for just one character
5
>>> ord('\n')                # \n is a byte with the binary value 10 in ASCII
10
>>> S = 'A\0B\0C'            # \0, a binary zero byte, does not terminate string
>>> len(S)
5
>>> S                        # Non-printables are displayed as \xNN hex escapes
'a\x00B\x00C'
```

We can also use `triple quotes` , all the lines are concatenated together, and end-of-line characters are added where line breaks appear. 
```python
>>> msg = """
aaaaaaaaaaaaa
bbb'''bbbbbbbbbb""bbbbbbb'bbbb
cccccccccccccc
"""
>>> msg
'\naaaaaaaaaaaaa\nbbb\'\'\'bbbbbbbbbb""bbbbbbb\'bbbb\ncccccccccccccc\n'
```
Python also supports a _raw_ string literal that turns off the backslash escape mechanism. (e.g. `r'C:\text\new'`)

### Unicode strings
Python strings come with full _Unicode_ support. 

## Getting help 
The built-in `dir` lists variables assigned in the callers' scope when called with no argument. More usefully, it returns a list of all the attributes available for anyu object passed to it. 
```python
>>> dir(S)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__',
'__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__',
'__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__',
'__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__',
'__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__',
'__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count',
'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index',
'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower',
'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust',
'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex',
'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith',
'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```
The names with _dobule underscores_ represent the implementation of the string object and are avilable to support customization. The names without the underscores in this list are the callable methods on string objects.

Also we have `help` function. 
```python
>>> help(s.replace)
replace(...)
    S.replace(old, new[, count]) -> str
    Return a copy of S with all occurrences of substring
    old replaced by new.  If the optional argument count is
    given, only the first count occurrences are replaced.
```

Both `dir` and `help` also accept as arguments either a real object, or the name of a _data type_ (like `str`, `list` and `dict`). `help` allows you to ask about a specific method via type name (e.g. help on `str.replace`).

    [1]: ../images/built_in_types.png
