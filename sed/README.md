## Exit status

0  Successful completion.

1  Invalid command, invalid syntax, invalid regular expression or a GNU sed extension command used with --posix.

2  One or more of the input file specified on the command line could not be opened (e.g. if a file is not found, or read permission is denied). Processing continued with other files.

4  An I/O error, or a serious processing error during runtime, GNU sed aborted immediately.

 
## Replacement
 

The command sed allows you to replace all the occurrence of a string string s1 in a file by another one.<br>
Both commands below do the same job.

```sh
sed 's/string1/string2/' file.txt
```

Replace some chars by others

```sh
$ echo "Hello World !" | sed 'y/aelo/4310/'
```

You can also modify the standard input

```sh
cat file.txt | sed 's/string1/string2/' -
sed 's/string1/string2/' < file.txt
```
Commands above prints on the standard output, to modify a file you can redirect the standard output:

```sh
sed 's/string1/string2/' < file.txt > output.txt
```

To update the input file, use `-i`

```sh
sed -i 's/string1/string2/' input.txt
``` 

Sed can also help us reading one specific line from a file.

```sh
# READ THE THIRD LINE
sed -n ’3p' file.txt
# READ THE FIRST LINE
sed -n ’$p' file.txt
```

## Remove lines

To remove lines

```sh
# REMOVE LINE 3
Sed '3d'
# REMOVE LINES FROM 5 TO 8
Sed '5,8d' file.txt
# REMOVE LINES BEGINING WITH FOO
Sed '/^foo/d' file.txt
$ sed '2d ; # remove 2nd and 3rd lines; 3d'
```

The sed commands might be written in a script file

```sh
s/str1/str2 ; s/str1/str3
# SAME RESULT
s/str1/str2
s/str2/str3
```

sed -f script.sed input.txt

The same thing can hold in a line

```sh
sed -e 's/str1/str2' -e s/str2/str3 input.txt
sed 's/str1/str2' ; s/str2/str3 input.txt
```

Loop

```sh
# EACH 2 LINES, REPLACE THE NUMBER BY A CROSS
Seq 50 | sed 'n;s/.*/x/'
# SAME RESULT
$ sed 50 | sed '0~2s/.*/x'
# EACH 4 LINES, REPLACE THE NUMBER BY A CROSS
Seq 50 | sed 'n;n;n;s/.*/x/'
$ sed 50 | sed '0~4s/.*/x'
``` 

# Add

Appends a line

```sh

$ seq 3 | sed '3a soleil !'
1
2
hello
3
```
