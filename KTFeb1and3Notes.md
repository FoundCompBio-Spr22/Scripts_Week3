# Shell Scripts and More Bash Commands

## Variables

You can create a variable and assign it a value using `=`

`myVariable=2`

Note that bash is sensitive to spaces! Don't leave any spaces before or after your equals sign when assigning a value to a variable.

You can print the value of a variable by using `echo` and prefacing the name of the variable with `$`

`echo $myVariable`

```
# Notes 2/1/22

Scripts are series of commands, not just one command at a time, to tell a computer to do something specific.
working command line, terminal, bash commands all mean the same thing- they all work in the Unix environment.
shell- environment of commands we can use, we are using the bash shell
variable- generic name given to something that holds a piece of information
shells hold variables. 
  - ex: myVariable=2 (assigning a value to a variable)
  - = is the assignment operator
  - operator- changes a variable in some way.
  - value=variable 
command line does not allow spaces anywhere in this command. gives an error. 
to see the value of the variables we've made, use command: echo, which prints to the screen
echo prints things right back to us, and it can show us the values we created
  - echo $myVariable -> output = 2. 

```

## Math in Bash

In general, bash isn't very good for mathematical operations, but it can be done in several ways. Probably the easiest is to wrap a mathematical expression in _double_ parentheses and precede it with a `$`, since you pretty much always want the _value_ of the mathematical result.

- `echo $(( 2 + 2 ))`
  - `myNum = $(( 2 + 2 ))`
- `echo $(( 3 * 6 ))`
- `echo $(( 20 / 3 ))`
- `echo $(( 20 % 3 ))`

Compare the output of these last two lines? What's going on?

NOTE: bash can only handle _integers_ and not floating-point numbers (i.e., decimals)

Try this. What happens?

```
# Notes 2/1/22

Bash is generally not great for math, it is cumbersome and limited. Python is better. 
simple math at the command line is useful to know. 
wrap mathematical equation in double parenthesis to do it. 
include echo to print the answer back.
  - ex: echo $(( math ))
  - not space sensitive inside the double parenthesis. 
We can assignment mathematical results to a variable using myVariable=$((math)) then echo. 
  - ex: echo $(( 10 % 3)) -> remainder (from long division) here would be remainder of 1 
Floating point numbers- anything with a decimal place in them. Bash does not handle these numbers well. 

```

```
myVar=3
echo $myVar
((myVar++))
echo $myVar
((myVar++))
echo $myVar
```
What does the `++` operator do?

```
The ++ operator is called an increment operator, adds one to the value. 
```

## Downloading a file from the command line

An example data file is available here: [chiari.summary_statistics.csv](https://raw.githubusercontent.com/IntroToCompBioLSU-Spr20/Scripts2_Week4/master/chiari.summary_statistics.csv)

While we could copy and paste the contents of this file into a new file using the graphical interface, this approach becomes tedious and difficult with lots of files or large files. Instead, we'd like to be able to download the file contents directly from the command line. To do this, we can use the `curl` command. To start, try executing this command

`curl https://raw.githubusercontent.com/IntroToCompBioLSU-Spr20/Scripts2_Week4/master/chiari.summary_statistics.csv`

What happens? How could we save the contents that we're downloading to a file directly?

```
# Notes 2/1/22

a command line way to download contets of a file: curl
command is: curl then the URL. 
when we used this command, it displayed the contents of the file in the terminal, it did not save it. 
Use the > to make a new file or >> to append the file. 
curl URL > new file name. Then we get a summary of the file instead of all the contents. 
to view the file: use cat- shows the text written in the file. use head -n# of lines you want to limit the info in the output. 
file extension .csv (comma separated values, often times a table)
every line represents a new entry, and every value on the line is separated by commas in CSV. 

```

## Searching Within Files

Sometimes you may want to see if a certain text pattern exists in a file without viewing the entire file. In this case, the `grep` command is incredibly powerful. The basic syntax is

`grep <PATTERN> <PATH_TO_FILE>`

For instance, to search for the string `five` in the file `test.txt`, we could use

`grep five test.txt`

Take a look at the output. Note that the _entire_ line containing five is returned and not just the string itself.

Wildcards and additional symbols can be used to make searches general or more specific. For instance, using

`grep fi* test.txt`

will match any string that starts with `fi`.

`grep ^five test.txt`

will only look for the string `five` if it is at the very beginning of a line.

`grep five$ test.txt`

will only look for the string `five` if it is at the end of a line.

There are many other possible patterns that can be used with `grep`. If you search online for grep cheat sheets, you can find a lot more information.

```
# Notes 2/1/22

grep __ (what we are searching for) path to file to find certain things within the file. 
to make it more specific, anchor the search pattern to the beginning or end of line
grep and $ meand the end of a line 
to anchor to the beginning of a line ^,__
There are ways to extract values from the middle of a line as well. 

```

## Redirecting Output Streams

In the examples above, the output of the display commands (like `cat`, `head`, and `tail`) were all sent to the Terminal screen. However, there are many times when it is preferable to send this output to a file or even as the input to another command!

To send the output of any command to be stored in a file, you can use the redirection operators `>` and `>>`. These commands differ in whether they overwrite or append to a file. It's always safter to use `>>` if you're worried about losing the contents of an existing file. Note that if you overwrite a file's contents, there's __no__ way to get it back.

As one example, here's a way to send the contents of all text (`.txt`) files into a new text file called `allContents.txt`.

`cat *.txt >> allContents.txt`

You can also send the output of `head` or `tail` to a file in the same way

`head -n 20 test.txt >> testHeader.txt`

Sometimes the output of one command is also convenient to use as the input to another command. For instance, you could extract the first few lines of a file with `head` and then target the last few of these header lines with `tail`. To do this, we use a special type of redirection called piping. The pipe symbol is `|`. Here's one example of piping with `head` and `tail`:

`head -n 10 test.txt | tail -n 5`

Note that, in this case, `tail` does not need a filename as an argument. It is taking its input from the pipe.

As usual, this output is printed to the screen by default. But it can also be redirected to a file, even after piping:

`head -n 10 test.txt | tail -n 5 >> myLines.txt`

Sometimes we need to use or store the output from a (series of) bash command(s), but a pipe won't accomplish what we need. One of these cases is storing output from commands in a variable. In this case, we can wrap the bash commands with backticks to indicate that they should be executed first, and the resulting value should be stored in the variable

```
myVariable=`ls | head -n1`
```

```
# Notes 2/1/22

outputs can be sent to files, and into other commands, not just the terminal screen
use pipe "|" use to do it. 
  - Ex: extract the 57th line from a file. head -n57 __file__ | tail -n1 
  - ^ extract lines 1-57 and the last line of the command (line #57).  
variables can also hold text/strings. 
we cannot pipe output into a variable. but we can:
  - myVariable=`ls | head -n1` then echo $myVariable saves the file name. 
  - `_` (back ticks) tells command line to execute this first. 
```

## Parsing File Contents

Another very powerful command is called `awk`, which can do lots of different forms of text parsing. For the purposes of this course, we will focus on using `awk` to extract individual columns (generally separated by spaces or tabs) from a file. The syntax we will use looks like this

`awk '{print $1}' test.txt`

This will print out the first column. To print the third column, we would use

`awk '{print $3}' test.txt`

To print both the first and third columns, we could do this

`awk '{print $1,$3}' test.txt`

```
#Notes 2/1/22

sed - stream editor command. used to find and replace patterns. 
sed 's/,/ /g' File.csv >> (redirect to new file)(find commas and replace with spaces)
awk used to extract columns from a file.
  - `awk '{print $4}' test.txt` extracts just the numers in the 4th column. 
There are many ways to use sed and awk. 
  - `awk '{print $2,$4}' test.txt` gives content from both columns. 

```

## Command line find-and-replace

You'll often need to clean up the contents of data files before doing additional analyses. In this case, we'd like to replace commas with spaces. To do this from the command line, we can use a powerful tool called `sed`. `sed`, which is short for "stream editor", can do many different things, but we'll just use it for simple find-and-replace for now. Try executing this

`sed 's/,/ /g' chiari.summary_statistics.csv`

What do you see? What's different compared to the original contents of the file?

What about when you run this?

`sed -i "_backup" 's/,/ /g' chiari.summary_statistics.csv`

Note that this syntax will stay the same for any find and replace operation that we want to do. The only thing that will change is the text to find and replace (between the slashes).

## Introduction to Shell Scripts

_What is a shell/bash script?_

Fundamentally, a bash script is just a file containing a series of bash commands. Scripts are plain text files, but the text in this file is special.

The first line of a script tells the computer in which language (i.e., shell) we're writing our script. This line starts with `#!` - also known as a shebang. The shebang tells Terminal that we're about to indicate which language we're going to use. Follow the shebang with the path to the shell that you'd like to use. Yes, the shell itself is a program!

`#! /bin/bash`

Let's start by creating your first script - `myScript.sh`

- `nano myScript.sh`
- Add the shebang line
- Add two commands in the body of the file
  - `echo "Hello, "$USER"!"`
  - `echo "I'M A SCRIPT AND I WORK!"`

To run the script, make sure it's located in your current working directory. Then, type

`./myScript.sh`

Did it run? If not, why not? How can you fix the problem?

_Command-line Arguments_

Scripts can be written to use command-line arguments. To access the arguments from inside the script, bash reserves the special variables `$1`, `$2`, `$3`, ...

For practice, go back to `myScript.sh` that we created earlier and change `$USER` to `$1`. Now, run it by typing `myScript.sh <YOUR_NAME>`

```
#Notes 2/3/22

when using grep to find a particular number in a specific column, use:
  * (most general wildcard for command line, can represent any number of any character)
  * when using grep has a slightly different meaning- it means a repitition of any
  character. a wildcard for any character in grep is a period (.). 
  grep .* means any combination of numbers and letters (general wildcard, same as * on 
  command line)
  so, from the example last class: (grep ^.*,*,*,14) anchoring from beginning. but we can 
  also anchor from the end.
  
#Scripts: 
use more than one command at a time, they can be very complicated. 
start off as a plain text file (nano FileName.sh) .sh abbreviates "shell" used commonly for scripts- it is still a plain text file.
First line indicates that it is a script: #! 
Then give a path that tells the operating system to look for the shell itself (bash command): #! /bin/bash (bin stands for binary, bash is a program within it). 
Create a hello world program, shows code is working: echo "Hello World!"
Save File, Exit nano. 
Cat FileName.sh -> #! bin/bash. "Hello World!" 
./FileName.sh -> permission denied. 
add execute permissions: chmod u+x FileName.sh (color coded) now ./FileName.sh works.

Allow script to accept command line arguments
open script in nano. 
  command: echo $1 (1 is a variale that holds first command line argument in a script). 
strings are a collection of characters together, we can "glue" then together in echo commands. 
  Command: echo $1" is now in a sentence."
save and exit nano
  ./FileName.sh Biology -> Biology is now in a sentence. 
as scripts become more complicated, leave comments to help keep track of what is going on. the more the comments the better. 
  # anything types with a pound sign is treated as a comment. seen when we open the script. 
Some variables are always available to us: 
  $USER stores name of user logged in. 
```
```
# If.. Else 2/3/22
flow control, means sometimes we need script to do different things. There are different types of flow control- the most common being an if else statement. 
  If <test_something>
  then <do_this_thing1> (if true)
  else <do_this_thing2> (if false)
  fi
We will use this a lot. We need some kind of comparison.

Ex: 
a=2
if [ $a -lt 3 ] (means if a<3) **(must include spaces)**
then echo "$a is less than 3." (print out "statement")
else echo "$a is NOT less than 3." (print out "statement")
fi (tells operating system we are finished, this is just if backwards). 

if else statements can be nested, tests more conditions if the first one is true.
good practice to indent echo statements to make things more readable.
almost always if else statements are written in a script. 
```
```
Practice Exercise #1 Commands: ctl c is a way to kill a command. 
  5. sed 's/pattern/replace_with/g' (s and g have meanings)  
```
