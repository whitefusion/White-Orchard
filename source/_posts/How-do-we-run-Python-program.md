---
title: 'Python: How do we run Python program'
date: 2018-06-22 22:48:36
tags:
    - python
    - note
categories: language core
---
> This is the reading note for "Chapter 3: how you run programs, Learning python 5th edition". <br>

## Unix Script Basics
One can turn Python into executable programs if it was used in Unix, Linux or Unix-like system. In simple terms, Unix-style executable scripts are just normal text files containing Python statements, but with two sepcial properties: 
- **Their first line is special.** Scripts usually start with a line that begins with the characters `#!` (often called hash bang or shebang), followed by the path to the Python interpreter on your machine. For example: `#!/usr/local/bin/python`
- **They usually have executable privileges.** On unix systems, a command like `chmod +x file.py` usually does the trick. 

The first speical line at the top of the file tells the system where the Python interpreter lives . 

## The Unix env lookup trick
One can avoid hardcoding the path to the Python interperter in your script file by writing the special first-line comment like this:`#!/usr/bin/env python`. The **env** program locates the Python interpreter according to your system search path settings (in most Unix shells, by looking in all the directories listed in your **PATH** environment variable). 

## The input trick on Windows
Assuming no IDLE associated with .py file, Python program files show up as icons in file explorer windows and can automatically be run with a double-click of the mouse. However, the console windows appears and prints , and disappers as the on program exit. You can barely see your output. <br>
A trick to hack this is to add `input()` command at the end of your program. **input** reads and returns the next line of standard input, waiting if there is none yet available. 

## Module imports and reloads
**import** operations _run_ the code in a file that is being loaded as a final step. However, in a single session, you cannot import the same file more than once. Since **import** is expensive, it must find files, compile them to byte code, and run the code. <br>

If you really want to force Python to run the file again in the same session without stopping and restarting the session, you need to instead call the reload function available in the **imp** standard library module. The **reload** function expects the name of an already loaded module object, so you have to have successfully imported a module once before you reload it. **reload** is a function that is called while **import** is a statement. 

## The grander module story: attributes
Import operations execute files as a last step. Modules serve the role of _libraries_ of tools. A module is mostly just a package of variable names, known as _namespace_. the names within that package are called _attributes_. An attribute is simply a variable name that is attached to a specific object. The built-in **dir** function starts to come in handy - you can use it to fetch a list of all the names available inside a module. Names with leading and trailing double underscores (`__X__`)  are built-in names that are always predefined by Python. 

## Using exec to Run Module Files
The `exec(open('module.py').read())` built-in function call is another way to launch files from the interactive prompt without having to import and later reload. But it comes with the potential to silently overwrite variables you may currently be using.
