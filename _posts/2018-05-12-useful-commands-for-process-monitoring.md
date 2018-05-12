---
layout: post
title: Useful commands for process monitoring
draft: true
---

Who never dealt with performance issues in a computer, so cast the first
stone. If you want to understand what is happening, you must collect
information to examine how programs running are affecting the computer
performance.
When I was a Windows user, I used to analyze this kind of data through Task Manager or
Performance Monitor. The native tools in Unix-like operating system is a little bit different.

While we were talking about [useful commands for text manipulation ] % post_url blog/2018-05-05-useful-commands-text-manipulation % in the previous post, I have decided
to show some useful commands for process monitoring.

<!--more-->

* [ps](#ps) (process status)
* [top](#top) (display and update sorted information about processes)
* [htop](#htop) (interactive process viewer)

## cat
> concatenate files and print on the standard output

_cat_ is one of the most basic commands. You can use to create, concatenate or
display the content of a file.

```bash
# create a file
$ cat > people
Id;Name;Age
1;Luiz;24
2;Peter;32
<ctrl-D>
```

```bash
# append file
$ cat >> people
3;Mary;27
<ctrl-D>
```

```bash
# concatenate files
$ cat > people2
4;Rose;20
<ctrl-D>

$ cat people2 >> people
$ cat people # display file
1;Luiz;24
2;Peter;32
3;Mary;27
4;Rose;20
```

## grep
_grep_ is a command that it allows you to search for lines
containing a match to a string. _grep_ is often used with shell pipes,
that it is a way to connect the output of one program to the input of another
program.

```bash
# find for 'Peter' in people file
$ grep 'Peter' people
2;Peter;32

# given the ps output, find for a process called 'vim'
$ ps | grep 'vim'
60873 ttys000    2:08.01 vim
```

## cut
_cut_ can be used to extract portions of each line of a file. It is useful when
you are manipulating a CSV file, for example.

```bash
$ cat cut_sample
Id;Name;Age
1;Luiz;24
2;Peter;32
3;John;19

# given a ';' delimeter, extract the second field
$ cut -d';' -f2 cut_sample
Name
Luiz
Peter
John

# find lines with 'Peter', then extract the second and third field
$ grep 'Peter' cut_sample | cut -d';' -f2,3
Peter;32
```
## sort
_sort_ command is obvious: it sorts the lines of a file.

```bash
# revert sort
$ sort -r people
Id;Name;Age
4;Rose;20
3;Mary;27
2;Peter;32
1;Luiz;24
```

## wc
_wc_ basically displays some statistics of a file, such as number of lines, number of words (separated by whitespace) and number of characters in specified files.

```bash
$ wc people
       5       5      53 people
# where the first 5 is the number of lines, the second 5 is the number of words, and 53 is the number of characters.
```

```bash
# print the number of files in a folder
ls | wc -l
```

## sed
_sed_ is a stream editor, that is, the program can receive an input text and
iterates in lines doing specified operations. You can add, delete or replace
text. To use all the power of _sed_, it would be nice to know a little about regular expressions.

```bash
$ sed 's/Luiz/Harry/g' people > new_people
# 1 - s  - substitute operation
# 2 - Luiz - search term
# 3 - Harry - term to be written
# 4 - g - replace all the ocurrences

$ cat new_people
Id;Name;Age
1;Harry;24  # Luiz was replaced to Harry
2;Peter;32
3;Mary;27
4;Rose;20
```

```bash
# replace 'e' to 'a' only in the first and second lines
$ sed '1,2 s/e/a/g' people > new_people

$ cat new_people
Id;Nama;Aga  # letter 'e' replaced to 'a'
1;Luiz;24
2;Peter;32
3;Mary;27
4;Rose;20
```

## awk
_awk_ is a text processor. It is a programming language by itself designed for
text processing. You can use _awk_ to extract data or as a reporting tool. It is
possible to use _if/else_, _while_, _for_ and even declare variables.
If you want to counting fields, calculate totals or reorganize structure, _awk_
will be your best friend (ok, maybe it has a steep learning curve...).

```bash
# print the second column (simulating _cut_ command)
$ awk -F';' '{print $2;}' people
```

```bash
# print the average age
# explanation: sum of the third column divided by the number of lines,
# excluding the first line
$ awk -F';' '{sum+=$3} END { print "Average age = ",sum/(NR-1)}' people
```
***

There are more commands that I did not cover. Even only these commands above
have many other variations when you change the parameters. The goal was not to
present a comprehensive list of commands, but it was to show the possibilities
you have in your tool belt when dealing with text manipulation problems.
And remember you have an uncountable combinations with the shell pipe. 

Enjoy it! :)
