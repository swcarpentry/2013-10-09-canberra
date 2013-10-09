
# Advanced Shell

**Commands we'll learn**  
* `wc`
* pipes and redirects
* find

## Pipes and redirects

`cat` - concat files  
`sort` - sort lines  
`uniq` - avoid duplicates  
`grep` - global regular expression patterns  
`tail` - view end of file  
`head` - view top of file  


By now you're well aware of how to list files with `ls`.

```shell
# cd and pwd in your boot-camp folder
ls -l
```

redirect this output using a `>`

```
ls -l > files.txt
```

```
less files.txt
```

Append to this file

```
ls -l ../vc_example >> files.txt
# OR
ls -l . ../vc_example > files.txt
```

What if we list a folder that doesn't exist?

```
ls -l . ../vc_example data  > files.txt
```

`>` only redirects standard output. Not standard error. You have to capture that separately and can do both at once.


```
ls -l . ../vc_example data  > files.txt 2> errors.txt
```

```
cat files.txt
cat errors.txt
```
---


## Cat   
*- concatenate files*

Cat not just outputs contents of a file to screen but you can also use it to combine files. 

```
cat file1.txt file2.txt file3.txt file4.txt  > all_my_files.txt
```

or with wild cards

```
cat file*.txt  > all_my_files.txt
```

Try the following example:

```
cat Python/1_lists_and_dictionaries.md Python/2_functions_and_modules.md Python/3_documenting_code.md Python/4_object_orientation.md Python/5_matplotlib.md Python/5_numpy.md > all_python_files.md
```

## Pipes

Take the output of one command and pass it as input into another. 


```
command1 | command2
```

example

```
ls -l | less
```

```
ls -l | uniq | sort | less
```

Assuming you're in the bootcamp folder, then try this:

```
git shortlog -s
git shortlog -s | cut -f1
```

---

## wc - Count the words

The `wc` program (word count) counts the number of lines, words, and
characters in one or more files. 

```
ls -l | wc
```

```
cd shell
wc dictionary.txt
```


For each of the files indicated, `wc` has printed a line with three
numbers. The first is the number of lines in that file. The second is
the number of words. Finally, the total number of characters is
indicated. The final line contains this information summed over all of the files. 

**head and tail**

```shell
head dictionary.txt
head -n 5  dictionary.txt
tail dictionary.txt
tail -n 5 dictionary.txt
```


* * * *


### A sorting example

We can sort existing files. There's a file named `names.txt` in the shell folder. View it with:

```
cat names.txt
```

Now sort the contents of this file:

```
sort names.txt
```

Notice that the names are now printed in alphabetical order.


Enter the following command:

```
cat names.txt | sort -k 2 -n
```

The contents of the `names.txt` file are now read and sorted by column number 2 (going in numeric order rather than column name). This list is then piped into the `sort` command, so that it can be sorted. Notice there are two options given to sort:

1.  `-k 3`: Sort based on the third column
2.  `-n`: Sort in numerical order as opposed to alphabetical order


* * * *
**Short Exercise**

Combine the `wc`, `sort`, `head` and `tail` commands so that only the `wc` information for the largest file is listed


* * * *







Printing the smallest file seems pretty useful. We don't want to type
out that long command often. Let's create a simple script, a simple
program, to run this command. The program will look at all of the
files in the current directory and print the information about the
smallest one. Let's call the script `smallest`. We'll use your text editor to create this file. Create a file called `smallest.sh` and enter:


    #!/bin/bash
    wc * | sort -k 3 -n | head -n 1

cd into the `messy-folder`. Now enter the command `../smallest`. Notice that it says permission denied. This happens because we haven't told the shell that this is an executable file. If you do `ls -l ../smallest`, it will show you the permissions on the left of the listing.

Enter the following commands:

    chmod a+x ../smallest
    ../smallest

The `chmod` command is used to modify the permissions of a file. This
particular command modifies the file `../smallest` by giving all users
(notice the `a`) permission to execute (notice the `x`) the file. If
you enter:

    ls -l ../smallest

You will see that the file name is green and the permissions have changed. Congratulations, you just created your first shell script!

# Searching files

You can search the contents of a file using the command `grep`. The
`grep` program is very powerful and useful especially when combined
with other commands by using the pipe. 

```
ls /usr/bin | grep zip
```

use `- i` to ignore case.


```
grep able dictionary.txt
grep on dictionary.txt
grep on$ dictionary.txt
grep -n on$ dictionary.txt
grep -n on$ 
```

Let's make a new file with all the application names


```
ls /bin/ > dir_list.txt
ls /usr/bin/ >> dir_list.txt
less dir_list.txt
```

```
grep bzip dir_list.txt
grep -n bzip dir_list.txt
```


```
grep -h 'zip' dir_list.txt
```

```
grep -h '^zip' dir_list.txt
```

```
grep -h 'zip$' dir_list.txt
```

```
grep -h '^zip$' dir_list.txt
```

```
grep -h '^[A-Z]' dir_list.txt
```

```
grep -h '^[A-Za-z]' dir_list.txt
```

```
grep -h '^[0-9]' dir_list.txt
```


# Finding files

The `find` program can be used to find files based on arbitrary
criteria. Navigate to the `data` directory and enter the following
command:

    find . -print

This prints the name of every file or directory, recursively, starting from the current directory. Let's exclude all of the directories:

    find . -type f -print
    


This tells `find` to locate only files. Now try these commands:

    find . -type f -name "*1*"
    find . -type f -name "*1*" -or -name "*2*" -print
    find . -type f -name "*1*" -and -name "*2*" -print

To find only directories

```
find . -type d -print
 find . -type d -print | wc
```


Find only R files in this directory and all subdirectories

```
find . -name "*.R" -print
```





 