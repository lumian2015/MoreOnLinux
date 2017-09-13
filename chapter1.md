## Selected Environment variables {#selected-environment-variables}

### Type of shell in-use {#type-of-shell-in-use}

In UNIX there are two major types of shells: The Bourne shell and C shell. We can find the type of shell in-use in a terminal in the environment variable`SHELL`.

`$ echo $SHELL`

In our course VM, the output is`/bin/bash`, which is the path to the shell executable.

A _path can be absolute_, i.e., starting from the _root of the file system \(e.g._`/home/csci3150`_\), or relative_, i.e., starting from the current directory \(`.`\) \(we can list the current directory with`ls ./`and the parent directory with`ls ../`\).

### Home directory and working directory {#home-directory-and-working-directory}

Every time the terminal is opened, we land on the _home directory_, which is stored in the variable`HOME`.

`$ echo $HOME`

When you navigate among folders, the absolute path to the current working directory is always stored in`PWD`.

### Path to search for executables {#path-to-search-for-executables}

When you execute a command from the terminal, e.g.`ls`, the shell first looks into the directories stored in`PATH`sequentially for the first executable named`ls`. It then runs the executable. The value of`PATH`can be shown by executing,

`$ echo $PATH`

and in the course VM, you get the following sequence of folders

`/home/csci3150/bin:/home/csci3150/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin`

with each of them separated by a colon`:`. Users can run executables under these directories without specifying the path to these executables.

On the other hand, users need to specify the complete path to executables that are outside these directories, e.g. to run an executable named`hello_world`under the current directory by executing`./hello_world`.

We can use the following command to add a directory to the`$PATH`.

`$ export PATH="/path/to/dir:$PATH"`

This change is temporary. If you want to make the changes permanent, you can edit`.bashrc`in your home directory and add the above command. In practice, however, we only include a few trusted directories for better security, e.g. to avoid spoofing programs \(see [the explanation from IBM](http://www.ibm.com/support/knowledgecenter/ssw_aix_71/com.ibm.aix.security/user_accounts_path_env_variable.htm)\).

There are other common environment variables in Linux and users can also define their own variables using`export`\(see more details [here](https://wiki.archlinux.org/index.php/environment_variables)\).

## Selected Bash Commands {#selected-bash-commands}

### Repeater,`echo` {#repeater-echo-}

`echo`simply repeats the input. Here is an example,

`$ echo "hello world"`![](/assets/echo.png)

### Task manager,`top`/`htop`

`top`is a built-in command line task manager which shows processes and resources utilization.

`$ top`![](/assets/top.png)The first line shows the average system load in the last 1 minute, 5 minutes, and 15 minutes; the forth line reflects the utilization of physical memory.

Below the fifth line, there is a table of processes with resources utilization shown. We take the third process as an example and name the information shown in the columns from left to right,

* _**Process ID \(PID\) **_: 2111
* _Run by the user _:`csci3150`
* _Scheduling priority \(PR\) _: 20
* _Nice value \(NI\) _: 0.
* Virtual memory \(VIRT\) allocated: 8064KB
* Physical memory \(RES\) allocated: 3548KB
* Shared memory \(SHR\) allocated: 3024KB
* _Status \(S\) _: "R", running
* _CPU usage \(%CPU\) _: 0.3
* _Memory usage \(%MEM\) _: 0.2
* _CPU time \(TIME+\) _: 0.09 seconds
* _Command line used to start the process \(COMMAND\)_: `top`

You may also refer to the [manual page of top](http://man7.org/linux/man-pages/man1/top.1.html) for details on the information shown.

To close the task manager, simply press`q`.

Besides`top`, you may install and use a more colorful task manager,`htop`.

To install`htop`,

`$ sudo apt-get -y install htop`

\(The`-y`option means automatically answer "Yes" for question asked\)

Try calling`htop`in a terminal,

`$ htop`![](/assets/htop.png)Same as `top`, it can be close by pressing `q`.

### Basic operations on files {#basic-operations-on-files}

#### Determine file type,`file`

`file`tries to identify the type of files using three different methods \(see [its manual page for details](http://man7.org/linux/man-pages/man1/file.1.html)\). For example, to determine the file type of`/bin/bash`, execute

`$ file /bin/bash`

and`file`reports`/bin/bash`is an 32-bit executable.

#### Showing file content,`cat`/`more`/`less`/`head`/`tail` {#showing-file-content-cat-more-less-head-tail-}

`cat`,`more`,`less`,`head`, and`tail`are commonly used commands for showing file content in a terminal, i.e., printing files' content to the terminal.

`cat`concatenates files and prints all content to the terminal at once. For example, to print the content of a file named`hello.txt`,

`$ cat hello.txt`

![](/assets/cat.png)

`more`and`less`are two other commands that do a similar job as`cat`once,`more`and`less`divide and print one screen at a time.

`head`prints a certain numbers of lines, 10 by default, from the beginning of a file. The number of printed lines can be controlled via the option`-n`, e.g. to print only the first 2 lines of`hello.txt`,

`$ head -n 2 hello.txt`

![](/assets/head.png)

`tail`is almost the same as`head`, except that it counts lines from the end of a file.

You may refer to the online post [here](https://2buntu.com/articles/1491/viewing-text-files-on-linux-cat-head-tail-more-and-less/) for more details and examples on these commands.

### Searching file content,`grep` {#searching-file-content-grep-}

`grep`searches files and prints the lines in which keywords are found, with keywords highlighted.

![](/assets/grep.png)

To search a phrase with one or more words, embrace the phrase with double quotes \(`""`\).

`grep`supports searching many files at the same time, e.g.`grep hello hello1.txt hello2.txt hello3.txt`.

Besides,`grep`supports regular expressions \(regex\) besides keywords in plaintext, and you may refer to [this guide](https://www.gnu.org/software/findutils/manual/html_node/find_html/grep-regular-expression-syntax.html) for more details on the use of regex for`grep`. Beware that the syntax for regex may differ among commands, i.e., the one works for`grep`may or may not work for other commands.

Another intersting thing is that we can use`grep -v`to search the lines that do not have the keywords. See the following example.

![](/assets/grep2.png)

## Useful Operators for Bash Commands {#useful-operators-for-bash-commands}

Below are some useful operators in Bash with simple examples of their usage.

### Ampersand Operator \(`&`\) {#ampersand-operator-}

Adding`&`at the end of a command allows you to start a process in background, e.g. to start and run a task manager in background,

`$ htop &`

![](/assets/htop&.png)

The command returns the number of processes currently in background inside the square barracks \(`[]`\), followed by the id of the last process put into background.

To bring the last "background-ed" process foreground, execute

`$ fg`

### NOT Operator \(`!`\) {#not-operator-}

The NOT operator`!`allows you specify exceptions when running a command. Let try to understand it via an example.

First, create a folder`test`

`$ mkdir test`

Then, we move into the folder`test`and create some files under this folder

`$ cd test`

`$ touch a b a.txt b.txt`

By default,`$ ls`shows the four files we created under the folder`test`. To show files except those ended with the extension`.txt`, we may use

`$ ls !(*.txt)`

Note that`*`represents any number of characters, and is known as one of the _wild characters_\(see the [documentation](http://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm) for more details\).

![](/assets/not.png)

### Pipe Operator \(`|`\) {#pipe-operator-}

By putting`|`between 2 commands, the output of the first command is piped to the second command as its input, e.g. to show the name of files with keyword`D`after listing the current directory,

`$ ls -1 | grep "D"`

![](/assets/pipe.png)

To search several words in a line, we can use the pipe operator and`grep`:`grep first_pattern file_name | grep second_pattern`.

_pipe_ ensures that data

* flows in one-way and,
* is output in the order of input, i.e. first-in-first-out

\(see more details on pipe [here](http://www.tldp.org/LDP/lpg/node10.html)\).

### Output Redirect Operator \(`>`\) {#output-redirect-operator-}

Adding a`>`to the end of a command, followed by a file name, redirects the _standard output stream \(_`stdout`_\)_\([What are standard streams](https://en.wikipedia.org/wiki/Standard_streams#Standard_output_.28stdout.29)?\) to a file, i.e. the output is written to the file instead of the terminal , e.g.

`$ ls >ls.out`

![](/assets/redirect.png)

The output of`ls`written to the file`ls.out`instead of the terminal. You may recap the output by executing`$ cat ls.out`.

However, errors are usually output to the _standard error stream \(_`stderr`_\) instead of_`stdout`_. To redirect_`stderr`_, we should specify its file descriptor_, which is`2`, explicitly before`>`, e.g.

`$ haha 2>haha.err`

![](/assets/haha.png)

The error message is then written to the file`haha.err`, instead of the terminal.

### Semi-colon Operator \(`;`\) {#semi-colon-operator-}

To run multiple commands in series, you may chain up the commands with semi-colons \(`;`\). The`;`operator marks the end of each command in a series of commands. For example, to create a file`myfile`, copy it as`myfile_copy`, create a folder`file_copy`, and move the copied file into the folder,

`$ touch myfile; cp myfile myfile_copy; mkdir file_copy; mv myfile_copy file_copy/`

Note that the command is in one single line \(although it may overflow in the html display\).

### AND Operator \(`&&`\) {#and-operator-}

If we replace`;`with`&&`between two commands, the second command only runs if the first command succeeds, i.e., the first command exits with status 0 [\(more on exit status of Bash commands\)](http://tldp.org/LDP/abs/html/exit-status.html). For example,

\(1\)`$ ls && echo ">> Success <<"`

vs.

\(2\)`$ haha && echo ">> Success <<"`

![](/assets/and.png)

Since`ls`is a valid command but`haha`is not,`echo ">> Success <<"`runs in \(1\), but not \(2\).

### OR Operator \(`||`\) {#or-operator-}

Opposite to`&&`, if`||`is used between two commands, the second command only runs if the first command fails, i.e., the first command exits with a non-zero status. For example,

\(1\)`$ haha || echo ">> Failed <<"`

vs.

\(2\)`$ ls || echo ">> Failed <<"`

![](/assets/or.png)

Since`haha`is an invalid command but`ls`is a valid one,`echo ">> Failed <<"`runs in \(1\), but not \(2\).

### Conditional Command Execution using AND Operator \(`&&`\) and OR Operator \(`||`\) together {#conditional-command-execution-using-and-operator-and-or-operator-together}

`&&`and`||`complement each other. We may achieve similar \(but not always the same\) outcome as if using`if ... then ... else ...`. For example,

\(1\)`$ haha && echo ">> Success <<" || echo ">> Failed <<"`![](/assets/com2.png)Since`haha`is an invalid command,`echo ">> Success <<"`is skipped, but`echo ">> Failed <<"`runs.

On the other hand, for

\(2\)`$ ls && echo ">> Success <<" || echo ">> Failed <<"`![](/assets/com3.png)since`ls`runs successfully,`echo ">> Success <<"`runs.**Then, since**`echo ">> Success <<"`**also runs successfully,**`echo ">> Failed <<"`**is skipped.**

By reordering \(1\) a bit,

\(3\)`$ haha || echo ">> Failed <<" && echo ">> Success <<"`![](/assets/com4.png)we see that **both**`&&`**and**`||`**have the same precedence and are evaluated from left to right \(see the guide **[**here**](http://tldp.org/LDP/abs/html/opprecedence.html)**\). **Hence,`echo">> Failed <<"`runs because`haha`fails, and`echo ">> Success <<"`runs because`echo ">> Failed <<"`succeeds.

### Command Combination Operator \(`{}`\) {#command-combination-operator-}

By putting multiple commands within a`{}`, the commands are run \(or not run\) as a whole. For example, to print both "&gt;&gt; Success &lt;&lt;" and "XD" if the first command succeeds, and "&gt;&gt; Failed &lt;&lt;" otherwise, we should group the first two`echo`s with`{}`,

`$ haha && { echo ">> Success <<"; echo "XD"; } || echo ">> Failed <<"`

Without`{}`,

```
$ haha && echo ">> Success <<"; echo "XD" || echo ">> Failed <<"
```

`$ haha && echo ">> Success <<"` is treated as the first sequence of commands, while `echo "XD" || echo ">> Failed <<"` is another. Therefore, `echo ">> Success <<"` is skipped when haha fails, but`echo "XD"` runs.![](/assets/combination.png)

## Reference

[10 Useful Chaining Operators](http://www.tecmint.com/chaining-operators-in-linux-with-practical-examples/)

* However, some of the examples do not work as expected...

## Extra {#extra}

### Bash Commands {#bash-commands}

* More than searching file content:[`sed`and`awk`](http://tldp.org/LDP/abs/html/sedawk.html)



