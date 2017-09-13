## Elements in Bash Scripts {#elements-in-bash-scripts}

So far, we have been executing commands inside the Bash shell. To create the simplest Bash _script_, you may gather a list of commands, put it into a file, e.g.[`myscript.sh`](https://eric-lo.github.io/3150/3-Script/code/myscript.sh), and then execute`bash ./myscript.sh`.

Nevertheless, we can do more than executing commands sequentially and independently, e.g. establish a control flow for automating repeated tasks. Let us learn the basic elements for writing Bash scripts.

### Running a script {#running-a-script}

To run a bash script, you may pass it as an argument to the command`bash`, e.g. to execute`myscript.sh`

`$ bash ./myscript.sh`

Alternatively, we may put a [_shebang_](https://en.wikipedia.org/wiki/Shebang_%28Unix%29),`#!/bin/bash`, at the beginning of a script. This shebang tells the OS to use`/bin/bash`to parse and run the script.

Then, mark the script as executable using`chmod`\(option`+x`means marking the file as executable, and option `a`before `+x` means this change applies to all users"\),

`$ chmod a+x ./myscript.sh`

And the script can be run directly,

`$ ./myscript.sh`

### Variable {#variable}

#### Global variables {#global-variables}

Since there are **no data type in Bash script**\(unlike C\), the same variable may hold numbers or characters at different times. Simply define and assign a value to a variable using the syntax`[variable name]=[value]`, e.g.

`mynum=3150`

`mystr="hello world"`

**Beware that spaces are not allowed before or after the assignment operator**`=`**.**

By default, variables are _global_ and its assigned value are visible to statements that run after the assignment.

To make a variable _local_, specify`local`explicitly before the variable name, e.g.

`local mynum=315`

Other than assigning a value, we may "grow" a variable by appending values using the operator`+=`, e.g.

`mystr="csci"`

`mystr+="3150"`

and`mystr`now contains "csci3150".

To retrieve the value of a variable, we use the syntax`${variable name}`. The`$`means value retrieval, while`{}`is used to separate the variable name from other characters. The following script [variable.sh](https://eric-lo.github.io/3150/3-Script/code/variable.sh) shows an example on appending values to a variable, and when`{}`is necessary.

```
#!/bin/bash

mynum=3150
course_code="csci"
mystr=${course_code}

# grow mystr from csci to "csci3150" 
echo "Before: ${mystr}"
mystr+=${mynum}
echo "After: ${mystr}"

# when curly braces are necessary 
echo "${course_code}3150 is a course about OS"
```

### Array {#array}

To declare an empty array`myarray`,

`myarray=()`

We may also assign values on declaration as follow, with each value separated by a space,

`myarray=("three" 1 "five" 0)`

The array index starts from 0 \(as in C\), so if we want to replace the third item as`eight`, we can execute

`myarray[2]="eight"`

We may add an item with any positive index, e.g.

`myarray[5]="!"`

To get the first item,

`${myarray[0]}`

To get the whole array,

`${myarray[@]}`

To get the number of items in the array,

`${#myarray[@]}`

To get the keys used in the array,

`${!myarray[@]}`

Look into and execute [array.sh](https://eric-lo.github.io/3150/3-Script/code/array.sh) which includes examples on the usage listed above.

#### Strings {#strings}

Strings can be though as an array of characters, and we may get its length by

`${#mystr}`

Also, the string can be split as if an array, see [string.sh](https://eric-lo.github.io/3150/3-Script/code/string.sh) for an example.

```
#!/bin/bash 

mylongstr="this is a long string"

echo "My string: ${mylongstr}"
echo ""

echo "Number of characters in string: ${#mylongstr}"
echo ""

echo "Splitting my string like an array:"
for word in ${mylongstr[@]}; do
    echo "${word}"
done
echo ""
```

### Arithmetic {#arithmetic}

To do simple arithmetic on **whole numbers **in Bash, you can put the arithmetic expression inside a double parentheses after a dollar sign`$(())`.

For example, to store the sum of two variables`a`and`b`into`mysum`,

```
#!/bin/bash 

a=2
b=3

mysum=$((a+b))
echo "Sum of a=${a} and b=${b} is ${mysum}"
```

Other than summation, we can do subtraction \(`-`\), multiplication \(`*`\), division \(`/`\) and also get the remainder of division \(`%`\).

Besides, you can increment/decrement counters using the operator`++`and`--`, e.g. to increment or decrement variable`counter`

`(( counter++ ))`, or`(( counter-- ))`

The`$`can be omitted in this case, as we are not retrieving the value. We will find the increment/decrement syntax handy in the examples on [loops](https://eric-lo.github.io/3150/3-Script/lab3.html#loop) later on.

### Conditional statements, if ... then ... else ... {#conditional-statements-if-then-else-}

The format of "If-then-else" statements in Bash script is as follows:

**if **_expression _**then **_statement _**else **_statement _**fi**

or

**if **_expression _**then **_statement _**elif **_expression _**then **_statement _**else **_statement _**fi**

Let illustrate the syntax with an example [condition.sh](https://eric-lo.github.io/3150/3-Script/code/condition.sh),

```
#!/bin/bash 

A=1

if [ $((A)) -eq 0 ]; then
  echo "A equals to 0"
elif [ $((A)) -gt 0 ]; then
  echo "A is greater than 0"
else
  echo "A is smaller than 0"
fi
```

which prints "A is greater than 0" for`A=1`.

Note that we use arithmetic expression on variable`A`, i.e.`$((A))`, instead of simply use`${A}`when`A`contains a number, let us look into the difference between the two.

First, we assign an expression to`A`instead of a number, e.g. replace`A=1`with`A=1+2+3+4`. The script runs correctly without an error, since`$((A))`is expanded to`$((1+2+3+4))`and is evaluated to`10`when the conditions,`$((A)) -eq 0`and`$((A)) -gt 0`, are checked.

Then, we replace`$((A))`with`${A}`, and run the script. The script ends up with errors, since the expression`1+2+3+4`is not evaluated when the synxtax is`${A}`.

To summarize,`$(())`works when`A`is either a number or an arithmetic expression, while`${}`works only when`A`is a number.

#### Useful expressions {#useful-expressions}

##### Numeric values {#numeric-values}

* Less than,`-lt`
* Less than or equal,`-le`
* Equal,`-eq`
* Not equal,`-ne`
* Greater than or equal,`-ge`
* Greater than,`-gt`

and you may refer to the [example on conditional statement](https://eric-lo.github.io/3150/3-Script/lab3.html#conditionex).

##### Strings

We may check if a variable is empty using`-z`, e.g.

```
#!/bin/bash 

mystr="This is an OS course"

if [ -z "${mystr}" ]; then
  echo "Ops... the string is empty."
else
  echo "The string is not empty."
fi
```

##### Files and directories

We may check if a file exists and is _regular_\(see [definition of regular files](http://tldp.org/LDP/abs/html/fto.html)\) using`-f`, e.g.

```
#!/bin/bash 

if [ -f example.txt ]; then
  echo "File example.txt exists."
else
  echo "Ops... example.txt does not exist."
fi
```

We may also check if a directory exists using`-d`, e.g. to create directory`testdir`if it does exists, otherwise report its existence

```
#!/bin/bash 

if [ ! -d testdir ]; then
  mkdir testdir
else
  echo "Directory testdir exists"
fi
```

See the GNU manual [here](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html) for more expressions for files and strings

### Loops {#loops}

#### for-loop {#for-loop}

The format is

**for **item **in **list **do **_statements _**done**

An example [forloop.sh](https://eric-lo.github.io/3150/3-Script/code/forloop.sh),

```
#!/bin/bash 

A=("a" "b" "c")

for i in {1..10}; do
  echo "${i}"
done

for char in ${A[@]}; do
  echo "${char}"
done
```

prints 1 to 10 and then 'a' to 'c', with each number or character on a new line.

#### while-loop {#while-loop}

The format is

**while **_expression _**do **_statements _**done**

An example [whileloop.sh](https://eric-lo.github.io/3150/3-Script/code/whileloop.sh),

```
#!/bin/bash 

A=0

while [ $((A)) -lt 10 ]; do
  echo $((A))
  (( A++ ))
done
```

prints 0 to 9, with each number on a new line.

### Input / output {#input-output}

#### Input arguments \(applies to both functions and scripts\) {#input-arguments-applies-to-both-functions-and-scripts-}

The input arguments are obtainable from`$@`.

The number of arguments is obtainable from`$#`.

To specify the _n_-th argument, use`$n`, e.g. first argument is`$1`.`$0`is a special argument that stores the name of the script. For example, this script \([argument.sh](https://eric-lo.github.io/3150/3-Script/code/argument.sh)\) prints the arguments to the script.

```
#!/bin/bash 

echo "You called $0, with"

if [ $# -eq 0 ]; then
    echo "no arguments..."
    exit 0
fi

counter=0
for i in "$@"; do
    (( counter++ ))
    echo "Arg[${counter}]: ${i}"
done
```

Try: Please remove the double quotes from`"$@"`in above script and try the following command`./arguments.sh First "Second argument"`

#### Capture`stdout`of commands {#capture-stdout-of-commands}

We may capture and store the output of a command to a variable, e.g. to execute the command`ls`and store the result into the variable`output`

`output=$(ls)`

Note that the syntax`$(command)`means executing`command`\(which is different from`$((expression))`for evaluating arithmetic expressions\).

The exit status of the last executed command is always stored in`$?`.

See the following script, [capture.sh](https://eric-lo.github.io/3150/3-Script/code/capture.sh), which includes examples on the above usage,

```
#!/bin/bash 

# capture stdout 
output=$(ls)
echo ""
echo "Output of ls: ${output}"

# where is the exit status? 
echo ""
haha; echo "haha gives $?";

echo ""
echo "hello_world"; echo "echo gives $?"
```

#### Go through a file

We may loop through a file using a while-loop \([file\_displayer.sh](https://eric-lo.github.io/3150/3-Script/code/file_displayer.sh)\),

```
#!/bin/bash 

if [ $# -lt 1 ]; then
    echo "$0 [file to display]"
    exit 1
fi

if [ ! -f $1 ]; then
    echo "Cannot display non-regular files"
    exit 1
fi

while read line; do
  echo "${line}"
done < $1
```

The script prints the file specified as the first input argument, e.g. to print`forloop.sh`,

`$ ./file_displayer.sh forloop.sh`

Except that trailing spaces are trimmed, e.g. indentations before`echo`and`exit`\(you may compare the output against that from`$ cat forloop.sh`to see the difference\).

The`while`-loop evaluates`read line`, with content read \(operator`<`\) from the file named in`$f`. Note that if we omit both the redirection operator \(`<`\) and file name,`read`will read input from standard input stream \(`stdin`\) instead.

### Functions {#functions}

The format is

**function **function\_name **{ **_statements _**}**

For example in [function.sh](https://eric-lo.github.io/3150/3-Script/code/function.sh),

```
#!/bin/bash 

function addition {
  result=$(($1 + $2))
}

function main {
    local a=1
    local b=2
    result=
    addition ${a} ${b}
    echo "${a}+${b}=${result}"
}

main
```

we have two functions. The first one named`addition`adds up the first two input arguments. The second one named`main`calls`addition`and prints the result of adding`a=1`and`b=2`.

After executing it, you get "1+2=3" printed.

We can use`addition "$@"`to pass the input arguments to the function. For example in [function2.sh](https://eric-lo.github.io/3150/3-Script/code/function2.sh),

## Examples on Content Processing {#examples-on-content-processing}

### String splitting with`IFS` {#string-splitting-with-ifs-}

`IFS`stands for "_Internal Field Separator_", which is an environment variable that determines boundaries of words, i.e., it contains word _delimiters_. By default, the delimiters are space, tab, and newline character.

We may change`IFS`during runtime. For example, to print only words between full-stops \(.\), we may assign a full-stop to`IFS`, see [ifs.sh](https://eric-lo.github.io/3150/3-Script/code/ifs.sh)

```
#!/bin/bash 

mystr="name.email.phone.remarks"

IFS='.'
for word in ${mystr[@]}; do
  echo $word
done

# restore  IFS 
IFS=" "$'\n'$'\t'
```

### String splitting using`awk`, and print designated fields {#string-splitting-using-awk-and-print-designated-fields}

If we want to

1. split a string "first\_name,middle\_name,last\_name" based on a comma \(`,`\), and
2. print only "first\_name" and "last\_name" with a space in between

we may use the command`awk`as follows

`$ echo "first_name,middle_name,last_name" | awk -F',' '{print $1" "$3}'`

This command first pipes the string output by`echo`as the input of`awk`. Then,`awk`takes the character specified after the option`-F`as a delimiter to break the string.`awk`prints the first field \(`$1`\) and the third field \(`$3`\), with a space \(`" "`\) in between.

Given a comma separated value \(csv\) file [data.csv](https://eric-lo.github.io/3150/3-Script/code/data.csv), we may list only the first and third column of the file using the following script [process\_csv.sh](https://eric-lo.github.io/3150/3-Script/code/process_csv.sh),

```
#!/bin/bash 

while read line; do
  echo "${line}" | awk -F',' '{print $1" "$3}'
done < data.csv
```

### Counting lines, words, and characters,`wc` {#counting-lines-words-and-characters-wc-}

`wc`allows us to count characters \(with option`-c`\), words \(with option`-w`\) and lines \(with option`-l`\). It goes through files when they are provided. Otherwise, it processes input from`stdin`.

For example in [file\_summary.sh](https://eric-lo.github.io/3150/3-Script/code/file_summary.sh),

```
#!/bin/bash 

for f in "$@"; do
    counter=0
    wc -l ${f}
    while read line; do
        (( counter ++ ))
        chars=$(echo -n ${line} | wc -c)
        words=$(echo -n ${line} | wc -w)
        echo "Line $((counter)): ${chars} chars, ${words} words"
    done < ${f}
done
```

files are accepted as input arguments. For each file, the script prints the total number of lines together with the file name, followed by the number of characters and words in each line. Note **the option **`-n`** suppresses **`echo`**from adding a newline character at the end of a string**.

## Exercise {#exercise}

### Variables {#variables}

1. Run [global\_vs\_local.sh](https://eric-lo.github.io/3150/3-Script/code/global_vs_local.sh) and study the difference between global variables \(`global1`and`global2`\) and local variables \(`local1`\)

### Content processing {#content-processing}

1. How to print each matching phrase on a newline using`grep`?
2. Write a script to output a summary on a set of files in csv format. The set of files are given as input arguments. Each line shows \(1\) the file name, \(2\) the total number of characters, \(3\) the total number of words, and \(4\) the total number of lines, in a file
3. How to modify the script in this [example](https://eric-lo.github.io/3150/3-Script/lab3.html#ex4) to avoid trailing spaces being trimmed? \(or think about why the spaces are  "trimmed" in the first place? :\) \)

## Reference {#reference}

* [Bash Programming - Introduction HOW-TO](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)
  * [Variables](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-5.html)
  * [Arithmetic](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-10.html#ss10.2)
  * [If ... then ... else ...](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-6.html)
  * [Loops](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-7.html)
  * [Functions](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-8.html)
* [Bash Guide for Beginners](http://tldp.org/LDP/Bash-Beginners-Guide/html/)
  * [Array](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_10_02.html)
* [Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/)
  * [Local variables](http://tldp.org/LDP/abs/html/localvar.html)
  * [I/O redirection](http://www.tldp.org/LDP/abs/html/io-redirection.html)
  * [Internal field separator`IFS`](http://tldp.org/LDP/abs/html/internalvariables.html)



