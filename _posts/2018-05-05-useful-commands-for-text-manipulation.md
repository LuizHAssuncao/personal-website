---
layout: post
title: Useful Commands for Text Manipulation
draft: true
---

Long story short: I have used Windows for a long time in my career, since I was primarily using
Microsoft technologies. And it was a nice period with beautiful windows helping
me to complete my tasks. Until I try a *nix environment full-time and figure
out the terminal powerful.

Yes, I know. Windows has the Command Prompt, but it is not the same thing.
Windows was created to democratize the access of a personal computer in the easiest
way. While one of the Unix philosophy is to create programs that do only one thing
(and do that only one thing very well).

It may seem awkward to use these tools in the beginning, however afterwards you can realize a boost in your productivity.

In this article, I want to show some useful commands for text manipulation
tasks. Let's take a look.

<!--more-->

* [cat](#cat) (concatenate files and print on the standard output)
* [grep](#grep) (file pattern searcher)
* [cut](#cut) (cut out selected portions of each line of a file)
* [sed](#sed) (stream editor)
* [awk](#awk) (pattern-directed scanning and processing language)
* fmt (simple text formatter)
* tr (translate characters)
* nl (line numbering filter)
* wc (word, line, character, and byte count)
* [sort](#sort) (sort or merge records (lines) of text files)

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
$ cat people # Display file
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
$ sort -r people
Id;Name;Age
4;Rose;20
3;Mary;27
2;Peter;32
1;Luiz;24
```

## sed
_sed_ is a stream editor, that is, the program can receive a input text and
iterates in lines doing specified operations. You can add, delete or replace
text. To use all the power of _sed_, it would be nice to know regular expressions.

```bash
$ sed 's/Luiz/Harry/g' people > new_people
# 1 - s  - substitute operation
# 2 - Luiz - search term
# 3 - Harry - term to be written
# 4 - g - replace all the ocurrences
```

```bash
# replace 'e' to 'a' only in the first and second lines
$ sed '1,2 s/e/a/g' people > new_people
```

## awk
_awk_ is a text processor. It is a programming language by itself designed for
text processing. You can use _awk_ to extract data or as a reporting tool. It is
possible to use _if/else_, _while_, _for_ and even declare variables.
If you want to counting fields, calculate totals or reorganize structure, _awk_
will be your best friend (ok, it has a steep learning curve...).

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
