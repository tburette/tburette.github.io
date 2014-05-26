---
layout: post
title: So You Want To Write Your Own CSV code?
---

So You Want To Write Your Own CSV code? Fields separated by commas and rows separated by newline. Easy right? You can write the code yourself in just a few lines.

<br>

Hold on a second...

##What if there are commas in the fields?
You need to enclose the field with quotes (__"__).
Easy right?

But can only some fields but not all be quoted?

##What if there are quotes in the fields
You need to double each instance of quote in the field and god forbid you forget to enclose the field in quotes.

Also make sure not to mistake for a quoted empty field (`...,"",...`) from a double quote

##What if there is a newline inside a field?
Of course you must enclose the field using quotes

##What are the accepted newline characters?
CRLF? CR? LF? What if there are multiple newlines?

##What if the newline characters change?
E.g.: newlines within a fields a re different from newlines at the end of a line


Still with me?

##What if there is an extra comma at the end of a line?
Is there an empty field at the end or is that just a superfluous comma?

##What if there is a variable amount of field per line?

##What if there is an empty line?
Is that an EOF, a single empty field or no field at all?

##What about whitespace?
What if there is heading/trailing whitespace in the fields? 

What if the CSV you get always has a space after a comma but it's not part of the data

##What if the character separating fields is not a comma?
Not kidding. 

Some countries use a comma as decimal separator instead of the comma. In those countries Excel will generate CSVs with semi-colons. Some files use [tabs instead of comma](https://en.wikipedia.org/wiki/Tab_separated_values) to avoid this specific issue. Some even use [non displayable ASCII characters](https://ronaldduncan.wordpress.com/2009/10/31/text-file-formats-ascii-delimited-text-not-csv-or-tab-delimited-text/).

Don't forget to account for it when reading an arbitrary CSV file. No there's no indication which delimiter a file uses.

##What if the program reading CSV use multiple delimiters?
Some program (including Excel) will assume different delimiters when reading a file from the disk and reading it from the web. Make sure to give it the right one!

##What if there is non ASCII data?
Just use utf8 right? But wait...

##What if the program reading the CSV use an encoding depending on the locale?
A program can't magically know what encoding a file is using. Some will use an encoding depending on the locale of the machine.

Meaning if you save a CSV on a machine and open it it another it may silently corrupt the data.

##What if I put a BOM in my file?
After all Byte Order Masks can determine the unicode encoding used, that's what they are for right? (actually they are used to determine the endianness but I won't get into that).

If you include a BOM Excel will interpret the csv as a text file, not a CSV. This means breaks within lines are not handled. 

<br>
<br>

__Do you really still want to roll your own code to handle CSV?__

CSV is not a well defined file-format. The [RFC4180](http://tools.ietf.org/html/rfc4180) does not represent reality. It seems as every program handles CSV in subtly different ways. Please do not inflict another one onto this world. Use a solid library. 


If you have full control over the CSV provider and supplier and the data they emit you'll be able to build a reliable automated system.

If a supplied CSV is arbitrary, the only real way to make sure the data is correct is for an user to check it and eventually specify the delimiter, quoting rule,... Baring that  you may end up with a error or worse silently corrupted data.


Writing CSV code that works with files out there in the real world is a difficult task. The rabbit hole goes deep. Ruby CSV library is 2321 lines.

Discussion on [Hacker News](https://news.ycombinator.com/item?id=7796268) and [Reddit](http://www.reddit.com/r/programming/comments/26g24y/so_you_want_to_write_your_own_csv_code/).
