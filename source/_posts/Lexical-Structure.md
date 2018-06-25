---
title: 'Javascript: Lexical Structure'
date: 2018-06-16 00:17:42
tags:
    - language
    - core
    - javascript
    - note
---
> This is the reading note for "Chapter 2: Lexical Structure, Javascript: The definitive guide 5th edition". <br>

Lexical structure is the lowest-level syntax of a language. <br>

Javascript are written using the **Unicode character set**. It supports virtually every written language currently used on the planet. <br>

Javascript is a **case-sensitivity** language. But HTML is not case-sensitive. This could be confusing from time to time. For example, the HTML _onclick_ event handler attributes is sometimes specified as _onClick_ in HTML, but it must be specified as _onclick_ in Javascript code. <br>

Javascript ignores spaces that appear between tokens in programs. For the most part, Javascript also ingnores line breaks. But it doesn't treat every line break as a semicolon: it usually treats line breaks as semicolons only if it can't parse the code without the semicolons. More formally, Javascript treats a line break as a semicolon if the next nonspace character cannot be interpreted as a continuation of the current statement.<br>

Some computer hardware and software can not display or input the full set of Unicode characters. Javascript defines special sequences of six ASCII characters to represent any 16-bit Unicode codepoint. The Unicode _escapes_ begins with the character __"\u"__ and are followed by exactly four hexadecimal digits. But it will be ignored in comments. <br>

Unicode allows more than one way of encoding the same character. For example, `\u00e9` display the same as `e\u0301`, but they have different binary encodings and are considered different by the computer. <br>

In Javascript, `//` or `/* */` are treated as comments marks. <br>

A _literal_ is a data value that appears directly in a program. <br>

An _identifier_ is simply a name, it is used to name variables and functions and to provide labels for certains loops in Javascript code. A Javascript identifier must begin with a letter, an underscore (_) or a dollar sign($). It is common to use only ASCII letters and digits in identifiers. However, Javascript allows identifiers to contain letters and digits from the entire Unicode character set. <br>

Semicolon (;) are used to separate statements. Usually, it was omitted. 
