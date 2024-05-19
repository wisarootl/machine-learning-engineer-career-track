# Manipulating files and directories

- print working directory

```shell
pwd
```

- listing

```shell
ls <absolute / relative directory>
```

- change directory

```shell
cd <absolute / relative directory>
```

- move up directory

```shell
cd ..
```

- move to home

```shell
cd ~
```

- copy to last parameter

```shell
cp <seasonal/summer.csv> <seasonal/winter.csv> <backup>
```

- move files to last parameter / rename

```shell
mv <seasonal/summer.csv> <seasonal/winter.csv> <backup>
```

- remove file

```shell
rm <seasonal/summer.csv>
```

- remove directory

```shell
rm -r <seasonal>

# or

rmdir <seasonal>
```

- create new directory

```shell
mkdir <directory_name>
```

# Manipulating data

- concatenate (print all file contents)

```shell
cat <file_name>
```

- display one page of file a a time

```shell
less <file_name1> <file_name2>
:n # display next file
:q # quit
```

- display first 10 line

```shell
head <file_name>
```

- display first x line

```shell
head -n <x> <file_name>
```

- list everything below a directory

```shell
ls -R
```

- `-F` prints a `/` after the name of every directory and a `*` after the name of every runnable program

```shell
ls -F -R
```

- help

```shell
man <command>
```

- cut: the example command means "select columns 2 through 5 and columns 8, using comma as the separator".
- -f (meaning "fields") and -d (meaning "delimiter")

```shell
cut -f 2-5,8 -d , values.csv
```

- print command history

```shell
history
```

- rerun command

```shell
!<command> # run most recent of <command> with the arguments
!<number> # run <number> th command in the history
```

- prints lines from `winter.csv` that contain `bicuspid`.

```shell
grep bicuspid seasonal/winter.csv
```

# Combining tools

- store a command's output in a file (using `>`)

```shell
tail -n 5 seasonal/winter.csv > last.csv
```

- The pipe (`|`) symbol tells the shell to use the output of the command on the left as the input to the command on the right.

```shell
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

- combine many commands

```shell
cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10
```

1. select the first column from the spring data;
2. remove the header line containing the word "Date"; and
3. select the first 10 lines of actual data.

- wc (short for "word count") prints the number of characters, words, and lines in a file

```shell
wc seasonal/spring.csv
```

- specify many files at once

```shell
cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv
# same as
cut -d , -f 1 seasonal/*
cut -d , -f 1 seasonal/*.csv
```

- wildcards

- `?` matches a single character, so `201?.txt` will match `2017.txt` or `2018.txt`, but not `2017-01.txt`.
- `[...]` matches any one of the characters inside the square brackets, so `201[78].txt` matches `2017.txt` or `2018.txt`, but not `2016.txt`.
- `{...}` matches any of the comma-separated patterns inside the curly brackets, so `{*.txt, *.csv}` matches any file whose name ends with `.txt` or `.csv`, but not files whose names end with `.pdf`.

- sort

```shell
sort <file_name>
```

- remove duplicate lines

```shell
uniq <file_name>
```

- stop running command `Ctrl` + `C`

# Batch processing

| Variable | Purpose                           | Value               |
| -------- | --------------------------------- | ------------------- |
| HOME     | User's home directory             | /home/repl          |
| PWD      | Present working directory         | Same as pwd command |
| SHELL    | Which shell program is being used | /bin/bash           |
| USER     | User's ID                         | repl                |

- print variable

```shell
echo $HOME
# or
set | grep HISTFILESIZE
```

- set variable

```shell
export VARIABLE_NAME=value
# or
VARIABLE_NAME=value
```

- call variable

```shell
head -n 1 $VARIABLE_NAME
```

- loop command line

```shell
for filetype in gif jpg png; do echo $filetype; done
```

```shell
for filename in seasonal/*.csv; do echo $filename; done
```

# Creating new tools

- edit a file

```shell
nano names.txt
# Ctrl + O to write the file out
# Enter to confirm file name
# Ctrl + X to exit the editor
```

- record history to a file

```shell
history | tail -n 3 > steps.txt
```

- save commands to re-run later (run shell command from .sh file)

```shell
bash dates.sh
```

- re run with arguments

- unique-lines.sh contains `$@`

```shell
sort $@ | uniq
```

```shell
bash unique-lines.sh seasonal/summer.csv seasonal/autumn.csv
```

- column.sh contains `$1`, `$2`

```shell
cut -d , -f $2 $1
```

```shell
bash column.sh seasonal/autumn.csv 1
```

- loop in .sh

```shell
for filename in $@
do
    head -n 2 $filename | tail -n 1
    tail -n 1 $filename
done
```
