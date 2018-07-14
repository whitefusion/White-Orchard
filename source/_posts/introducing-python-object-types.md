---
title: 'Python: Introducing python object types'
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
![built_in_types][1]

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
Python strings come with full _Unicode_ support. Characters in the Japanese and Russian alphabets, for example, are outside the ASCII set. A distinct **bytes** string type represents raw byte values. Formally, in both 2.X and 3.X, non-Unicode strings are sequences of _8-bit bytes_ that print with ASCII characters when possible, and Unicode strings sequences of _Unicode code points_ - identifying numbers for characters, which do not necessary map to single bytes when encoded to files or stored in memory. </br>

Both 3.X and 2.X also support coding non-ASCII characters with \x hexadecimal and short \u and long \U Unicode escapes. 
```python
# code non-ASCII character coded in three ways
>>> 'sp\xc4\u00c4\U000000c4m'
'spÄÄÄm'

#Python 3.X has a tighter model that never allows its normal and byte strings to mix without explicit conversion. 
u'x' + b'y'            # Fails in 3.3 (where u is optional and ignored)
u'x' + 'y'             # Works in 3.3: 'xy'
'x' + b'y'.decode()    # Works in 3.X if decode bytes to str: 'xy'
'x'.encode() + b'y'    # Works in 3.X if encode str to bytes: b'xy'

# the unicode code points are the same as encoded hexidecimal byte code.
>>> '\u00A3', '\u00A3'.encode('latin1'), b'\xA3'.decode('latin1')
('£', b'\xa3', '£')
```
Unicode processing mostly reduces to transterring text data to and from files - text is encoded to bytes when stored in a file, and decoded into characters when read back into memories. 

## Lists 
Lists are positionally ordered collections of arbitrarily typed objects, and they have no fixed size. They are mutable. They support all sequence operations (indexing, slicing, concatenating, repeat). Sequence operations does not change original list. They have type-specific operations like `append(), pop(), insert(), remove(), extend(), sort(), reverse()`. Lists do not allow reference items that are not present. It reports a index error. List allows nesting which are good for matrixes.  </br>
A more advance operation supported is called _list comprehension_.
It can be used to iterate over any __iterable object__. 
```python
>>> diag = [M[i][i] for i in [0, 1, 2]]      # Collect a diagonal from matrix
>>> diag
[1, 5, 9]
>>> doubles = [c * 2 for c in 'spam']        # Repeat characters in a string
>>> doubles
['ss', 'pp', 'aa', 'mm']

# Lists, sets, dictionaries and generators can all be built with comprehensions
>>> [ord(x) for x in 'spaam']                # List of character ordinals
[115, 112, 97, 97, 109]
>>> {ord(x) for x in 'spaam'}                # Sets remove duplicates
{112, 97, 115, 109}
>>> {x: ord(x) for x in 'spaam'}             # Dictionary keys are unique
{'p': 112, 'a': 97, 's': 115, 'm': 109}
>>> (ord(x) for x in 'spaam')                # Generator of values
<generator object <genexpr> at 0x000000000254DAB0>
```

## Dictionaries
They are known as _mappings_. They store objects by key instead of by relative position. Mappings don't maintain any reliable left-to-right order. We can make dictionaries by passing to the `dict` type name either _keyword arguments_ or the result of _zipping_ together sequences of keys and values obtained at runtime. 
```python
>>> bob1 = dict(name='Bob', job='dev', age=40)                      # Keywords
>>> bob1
{'age': 40, 'name': 'Bob', 'job': 'dev'}
>>> bob2 = dict(zip(['name', 'job', 'age'], ['Bob', 'dev', 40]))    # Zipping
>>> bob2
{'job': 'dev', 'name': 'Bob', 'age': 40}
```
Missing keys are a situation need to be take care of. We can use `in`, `get` or `if` to get rid of it. 
```python
>>> value = D.get('x', 0)                      # Index but with a default
>>> value
0
>>> value = D['x'] if 'x' in D else 0          # if/else expression form
>>> value
0
```
The `sorted` call returns the result and sorts a variety of object types, like
```python
>>> D
{'a': 1, 'c': 3, 'b': 2}
>>> for key in sorted(D):
        print(key, '=>', D[key])
a => 1
b => 2
c => 3
```

## Tuples
They are immutable and used to represent fixed collections of items. Syntactically, they are normally coded in parentheses instead of square brackets, and they support arbitrary types, arbitrary nesting, and the usual sequence opeartion. 
```python
>>> T = 'spam', 3.0, [11, 22, 33]
>>> T[1]
3.0
>>> T[2][1]
22
>>> T.append(4)
AttributeError: 'tuple' object has no attribute 'append'
```

## Files
To create a file object, you call the built-in `open` function like: 
```python
>>> f = open('data.txt', 'w')      # Make a new file in output mode ('w' is write)
>>> f.write('Hello\n')             # Write strings of characters to it
6
>>> f.write('world\n')             # Return number of items written in Python 3.X
6
>>> f.close()                      # Close to flush output buffers to disk
```
The best way to read a line today is not read it at all - file provide an iterator that automatically reads line by line in `for` loops an other contexts. 
```python
>>> for line in open('data.txt'): print(line)
```
Python 3.X draws a sharp distinction between text and binary data in files: text files represent content as normal `str` strings and perform Unicode encoding and decoding automatically when writing and reading data, while binary files represent content as a special `bytes` string and allow you to access file content unaltered. _binary files_ are useful for processing media, accessing data created by C programs and so on. Python's `struct` module can both create and unpack packed _binary data_. 
```python
>>> import struct
>>> packed = struct.pack('>i4sh', 7, b'spam', 8)     # Create packed binary data
>>> packed                                           # 10 bytes, not objects or text
b'\x00\x00\x00\x07spam\x00\x08'
>>>
>>> file = open('data.bin', 'wb')                    # Open binary output file
>>> file.write(packed)                               # Write packed binary data
10
>>> file.close()


>>> data = open('data.bin', 'rb').read()              # Open/read binary data file
>>> data                                              # 10 bytes, unaltered
b'\x00\x00\x00\x07spam\x00\x08'
>>> data[4:8]                                         # Slice bytes in the middle
b'spam'
>>> list(data)                                        # A sequence of 8-bit bytes
[0, 0, 0, 7, 115, 112, 97, 109, 0, 8]
>>> struct.unpack('>i4sh', data)                      # Unpack into objects again
(7, b'spam', 8)
```
To access files containing non-ASCII Unicode text, we simply pass in an encoding name if the text in the file doesn't match the default encoding. 
```python
>>> S = 'sp\xc4m'                                          # Non-ASCII Unicode text
>>> file = open('unidata.txt', 'w', encoding='utf-8')
>>> file.write(S)                                          # 4 characters written
4
>>> file.close()
>>> text = open('unidata.txt', encoding='utf-8').read()    # Read/decode UTF-8 text
>>> text
'spÄm'
>>> len(text)                                              # 4 chars (code points)
4
```
If needed, you can also see what's truly stored in your file by stepping into binary mode.
```python
>>> raw = open('unidata.txt', 'rb').read()                 # Read raw encoded bytes
>>> raw
b'sp\xc3\x84m'
>>> len(raw)                                               # Really 5 bytes in UTF-8
5
```

you can also encode and decode manually if you get Unicode data from a source other than a file.
```python
>>> text.encode('utf-8')                                   # Manual encode to bytes
b'sp\xc3\x84m'
>>> raw.decode('utf-8')                                    # Manual decode to str
'spÄm'
```
This is also useful to see how text files would automatically encode the same string differently under different encoding names, and provides a way to translate data to different encodings. 
```python
>>> text.encode('latin-1')                                 # Bytes differ in others
b'sp\xc4m'
>>> text.encode('utf-16')
b'\xff\xfes\x00p\x00\xc4\x00m\x00'
>>> len(text.encode('latin-1')), len(text.encode('utf-16'))
(4, 10)
>>> b'\xff\xfes\x00p\x00\xc4\x00m\x00'.decode('utf-16')    # But same string decoded
'spÄm'
```

## Sets
Sets are **unordered** collections of **unique and immutable** objects. The choice of new {...} syntax fro set literals makes sense, since sets are much like the keys of a valueless dictionary. 
```python
>>> X = set('spam')                 # Make a set out of a sequence in 2.X and 3.X
>>> Y = {'h', 'a', 'm'}             # Make a set with set literals in 3.X and 2.7
>>> X, Y                            # A tuple of two sets without parentheses
({'m', 'a', 'p', 's'}, {'m', 'a', 'h'})
>>> X & Y                           # Intersection
{'m', 'a'}
>>> X | Y                           # Union
{'m', 'h', 'a', 'p', 's'}
>>> X - Y                           # Difference
{'p', 's'}
>>> X > Y                           # Superset
False
>>> {n ** 2 for n in [1, 2, 3, 4]}  # Set comprehensions in 3.X and 2.7
{16, 1, 4, 9}
>>> list(set([1, 2, 1, 3, 1]))      # Filtering out duplicates (possibly reordered)
[1, 2, 3]
>>> set('spam') - set('ham')        # Finding differences in collections
{'p', 's'}
>>> set('spam') == set('asmp')      # Order-neutral equality tests (== is False)
True
```
Sets also support `in` membership tests, though all other collection types in Python do too: 
```python
>>> 'p' in set('spam'), 'p' in 'spam', 'ham' in ['eggs', 'spam', 'ham']
(True, True, True)
```

## Other numeric types
`decimal` numbers, which are fixd-precision floating numbers, and `fraction` numbers, which are rational numbers with both a numerator and a denominator. Both can be used to work around the limitations and inherent inaccuracies of floating-point match: 
```python
>>> import decimal                  # Decimals: fixed precision
>>> d = decimal.Decimal('3.141')
>>> d + 1
Decimal('4.141')
>>> decimal.getcontext().prec = 2
>>> decimal.Decimal('1.00') / decimal.Decimal('3.00')
Decimal('0.33')
>>> from fractions import Fraction  # Fractions: numerator+denominator
>>> f = Fraction(2, 3)
>>> f + 1
Fraction(5, 3)
>>> f + Fraction(1, 2)
Fraction(7, 6)
```

### Break your code's flexibility
The `type` object, returned by the `type` built-in function, is an object that gives the type of another object. 
```python
>>> type(type(L))                   # Even types are objects
<class 'type'>
```
By checking for specific types in your code, you effectively break its flexibility - you limit it to working on just one type. Without such tests, your code may be able to work on a whole range of types. </br>

### User-defined classes
The implied `self` object is why we call this an object-oriented model: there is always an implied subject in functions within a class. A class is composed of _state informatin_ and _methods_.


## Iteration and optimization
In a nutshell, an object is _iterable_ if it is either a physically stored sequence in or an object that generates one item at a time in the context of an iteration - a sort of 'virtual' sequence. More formally, they respond to the `iter` call with an object that advances in response to `next` calls and raises an exception when finished producing values such as _geneartor_. </br>
Python _file objects_ similarly iterate line by line when used by an iteration tool: file content isn't in a list, it's fetched on demand. Keep in mind that every Python tool that scans an object from left to right uses the iteration protocol. </br>
The list comprehension, though, and related functional programming tools like `map` and `filter`, will often run faster than a `for`  loop today on some types of code. `time` and `timeit` modules for timing the speed of alternatives, and the `profile` module for isolating bottlenecks. 


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
