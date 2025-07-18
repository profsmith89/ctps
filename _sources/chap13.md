# Chapter 13: Rewrite the Error Message #

The last act began with the recognition that our world is wondrously complex, and we ended it with the realization that this complexity is sometimes disturbingly simple to overcome with the right tool. For example, a divide-and-conquer approach was exactly what we needed in Chapter 12 to tackle Beckett's challenge and recursion was the right technique for coding a short solution. Finding the right computational tool for your task is the theme of this final act. With the right tool, solving your problems becomes straightforward.

Well, almost. Even if you pick the right tool, you still need to get it to do what you want. In the previous two acts, you became proficient at problem solving by writing scripts, and that's half the battle when using computational tools. The other half is knowing what to do when your script fails to direct a tool in the way you'd like. My goal for you in this act is to have you become more skilled at understanding how your script can fail and more proficient at fixing these failures. And the best way I've learned to achieve this is to collect the types of mistakes we make in our scripts and then categorize how these mistakes manifest themselves in our computational tools. In a moment, I'll present such a categorization, and then we'll explore its implications in this chapter and the next few.

Beyond our own human propensity for errors, we also need to understand the limitations of our tools. Just like you, computational tools are good at some things and not so good at others. You can often force a computational tool to do an unnatural task (e.g., use a for- or while-loop to solve a problem that's more elegantly solved with recursion), but the work required and the errors that arise are often daunting. Sometimes it is not even possible to force a tool to do what you want, as we'll see in the next chapter.

The key to picking the right tool surfaces the last of our fundamental ideas in computer science: *pattern matching*. When we can identify the patterns in our problems and match them with the patterns our tools exploit, hard problems become magically easy to solve. We'll start in this chapter by looking at a tool to recognize and exploit the patterns in strings, and we'll play with some of the most powerful of these tools for other types of real-world data in the act's last two chapters.

I wish that the challenges ended here, but they don't. Even when a tool works, we must consider the social implications of a computational solution. Here we ask: Will our use of a tool unfairly harm some individuals or the fabric of society? Computational solutions that don't consider this critical question are all too common in our modern world, and we'll dive into these social issues in the act's last two chapters.

## The mistakes we make in problem solving

It's been quite some time since we looked at the list of steps in our problem-solving process. Let's return to that list but reframe the last three steps to reflect the knowledge we've gained over the past two acts.

Recall that the process begins with us precisely specifying our problem (step 1) and imagining a few specific instances of it (step 2). We next decompose it into its different tasks (step 3), decide which tasks are computational in nature (step 4), and identify existing algorithms, libraries, and our own previously written code that might help (step 5). This brings us to the last three steps, which we previously described as sketch, translate, and execute. We now know that these steps require us to:

1. determine a reasonable abstraction level at which to solve each task;
2. build a digital representation of each task and its associated data structures;
3. design or identify algorithms that work over these representations and data structures, which together allow us to solve the individual tasks and ultimately the overall problem; and
4. code these algorithms and data structures in a programming language like Python, taking advantage of its built-in functionality and the rich set of public libraries and modules from its user community.

We also learned that we need to revisit earlier steps when our scripts fail, but we never tried to categorize these failures. Let's do that now. Our goal will be to gain an understanding that allows us to proficiently handle each category and possibly even avoid a category of errors altogether.

There are basically four types of errors that we make, which are classified based on the ways that a script fails to execute or fails to execute according to a specification:

1. We sometimes write statements that **aren't legal Python**. These are *syntax errors* that stop the interpreter dead in its tracks. "What did you say?" it will ask. "I don't understand you."
2. We sometimes write statements that **are legal Python**, but what these statements ask the interpreter to do with the data we give it **isn't something it can do** (e.g., convert the word `"junk"` into an integer). These are *runtime errors* that stop the interpreter, again, dead in its tracks. "I can't do that," it'll say, occasionally in really-hard-to-understand error messages.
3. We sometimes write statements that **are legal Python and something the interpreter can do**, but **the result it produces makes no sense**. These are *design errors* (i.e., solving your problem incorrectly) and *subtle coding errors* (e.g., writing a script with an infinite loop or multiplying a value by the wrong constant) that are quite hard to diagnose and correct. We'll have to find ways to verify which parts of the script (including its data structures) are operating correctly and which incorrectly.
4. We sometimes write a script that **produces a sensible answer**, but it **takes too long** to run to completion (e.g., our script uses a brute-force approach on a problem with a very large input) or in some other way **misses an important problem constraint** (e.g., our script uses too much memory). These are *missed constraints*. They often require us to be smarter in our problem-solving approach, typically in the selection of an algorithm, as we discussed in the last act.

## Our problem-to-be-solved

To become more adept at fixing the syntax and runtime errors in your scripts, you need to understand and quickly process the Python interpreter's error messages. Let's assume we understand the information in the interpreter's error messages so that our focus is on quickly processing these messages. Now let me ask, "Have you found it easy to read these messages?" Here's a typical one, which I get when I run the Python interpreter on a script with a straightforward runtime error. In reading the script's output, the first two lines are lists that I asked the script to print; the error message starts with the "Traceback" line.

```{code-block} none
---
emphasize-lines: 1
---
chap13$ python3 broken.py
['this', 'is', 'a', 'test']
['his', 'is', 'another', 'test']
Traceback (most recent call last):
  File "/home/runner/chap13/broken.py", line 23, in <module>
    main()
  File "/home/runner/chap13/broken.py", line 20, in main
    f_undef_name()
  File "/home/runner/chap13/broken.py", line 5, in f_undef_name
    print(undef_name)
NameError: name 'undef_name' is not defined. Did you mean: 'f_undef_name'?
```

```{admonition} You Try It
The script `broken.py` is in the `chap13` files in this book's GitHub repository. We'll talk in a moment about how you interpret `"/home/runner/chap13/broken.py"`; you might see a different substring before `broken.py`. Make sure that you run the highlighted command for yourself. There are a lot of shell commands to try throughout this chapter.
```

A `NameError` is a mistake we learned about in Chapter 1. It occurs when we ask the interpreter to grab a value from a name that wasn't previously defined. In this error message, the interpreter tries to help us correct our mistake by suggesting a defined name that is close to what we typed. We'll need to look at the line in `broken.py` to see if its suggestion is the fix we need. Quick, what line contains the error?

```{admonition} You Try It

To answer the question of what line in `broken.py` contains the error, look at the error message above the previous "You Try It" block.[^fn1]

```

I've been coding in Python for quite some time, and I still forget if it is the first or the last of the traceback items. Imagine if we could run the Python interpreter and have it format its error messages in a manner we find easy to read. Perhaps something like this:

```{code-block} none
---
emphasize-lines: 1
---
chap13$ ./python32 broken.py
['this', 'is', 'a', 'test']
['his', 'is', 'another', 'test']

**Hit an ERROR** in broken.py on line 5
inside function f_undef_name:

4: def f_undef_name():
5:     print(undef_name)
6: 

NameError: name 'undef_name' is not defined. Did you mean: 'f_undef_name'?

**Call stack at ERROR**
Function f_undef_name was called from
  ↜ main in broken.py on line 20, which was called from
   ↜ <module> in broken.py on line 23
```

This output contains all the same information as the original error message, but I've rearranged it so that it first presents where the error occurred. The call stack[^fn2] comes last because I want to know the error first before I try to understand the context that brought me to this execution point. This output also shortens the filenames, and as a final flourish, it uses color to highlight the important information. This may not be exactly what you'd prefer, but I hope you see that a reorganization of the information could be extremely helpful.

Changing the Python interpreter's error message in this way will be our problem-to-be-solved, and we can enumerate the tasks involved:

1. Send the script to run along with all its input parameters to the Python interpreter.
2. Capture the interpreter's error output, if any, before it is displayed.
3. Recognize the patterns in this output and rewrite the error message.
4. Output this rewritten message as if the interpreter had written it.

Notice that we want only to insert ourselves where we want different work to occur.[^fn3] We don't want to figure out how to do all the work that the Python interpreter does to run the script and identify its errors. And as much as possible, we'd like to make it match what we do when running the Python interpreter. Did you notice that the command I ran in the last code block wasn't the Python interpreter? If you didn't, that's good. You'll learn exactly how the command I used works and how to build it by the end of this chapter.

```{admonition} Learning Outcomes
Learn how pattern matching can be the right tool for your problem. Working with regular expressions, a language to match patterns in strings, you'll write a script to capture the essence of the Python interpreter's error messages and rewrite them. To have it look like the interpreter outputs your rewritten message, you'll learn about the shell, common shell commands, and ways to write a Python script that automates your interactions with the shell. After completing this chapter, you will be able to do the following:

*   Understand the categories of mistakes we make in problem solving: syntax errors, runtime errors, design errors, and missed constraints [design and CS concepts].
*   Describe the difference between a graphical user interface (GUI) and a command line interface (CLI) [CS concepts].
*   Discuss Unix-like filesystem paths, their components, and how they help you move around in the filesystem [CS concepts].
*   Understand how the shell finds the programs corresponding to the commands you type and how you can turn your scripts into commands that the shell will run [CS concepts and programming skills].
*   Use redirection and pipes to change where a program gets its inputs and sends its outputs, including its error outputs [CS concepts and programming skills].
*   Use some simple wildcard expressions in your shell commands [programming skills].
*   Employ some of the most commonly useful shell commands [programming skills].
*   Describe the difference between data and executable files, and how the shell tells the difference [CS concepts].
*   Work with regular expressions and Python's `re` library [CS concepts and programming skills].
*   Write a script that launches programs as subprocesses and defines the communication channels between them [CS concepts and programming skills]. 
```

## From the GUI to the shell

As is typical, we'll solve our problem in parts. We'll begin with our first task, and to understand how to solve it, we'll need to learn more about the shell and how you can command it to do things. We've mostly used the shell to run the Python interpreter on our scripts. Infrequently, we issued it some commands to organize our files and directories (i.e., folders). But the shell is much more powerful. It can do all sorts of things for us.

When you want to do something on your computer, like move a file from one folder to another, you probably did it using the computer's *graphical user interface (GUI)*. You'd click on a file and drag it from one folder to another. Like the GUI, the shell also allows you to perform such operations, but you direct it with text commands, not mouse clicks.[^fn4]

As another example, if I wanted to see a list of the files and directories in a folder called `chap13`, I can use either of these interfaces:

* GUI: Put the cursor over the folder and double-click it.
* shell: Type `ls chap13` at the shell prompt.

The `ls` stands for "list directory contents," and it is a program that someone wrote. You can learn what it does by typing `man ls` at the shell prompt or in a browser search bar. `man` stands for "format and display the online manual pages."[^fn5] 

```{admonition} You Try It
Don't just read about these programs and shell commands. Bring up a terminal window on your machine and try the commands for yourself.
```

## Understanding paths

In the GUI, you can only double-click a folder if the folder in which it sits is open. To navigate to the folder containing the folder `chap13` in the shell, we use the `cd` command. `cd` stands for "change the shell working directory" or "change directory" for short.

You can see the name (technically *path*) for your working directory by typing `pwd`, which stands for "return working directory name" or "print working directory" in colloquial terms. Your working directory in a GUI is the one that you have open and active.

When I ran `python3 broken.py` in the earlier code block, I was in the working directory `/home/runner/chap13` in a terminal window on Replit.com, and this string is what computer scientists call an *absolute path*.[^fn6] Such paths start at the *root of the filesystem*, which is represented by a single slash (`/`) with nothing before it. In Replit's filesystem, `home` is the name of a folder in the root directory, and `runner` is a folder inside `home`. Finally, `chap13` is a folder inside `runner`.

Because there are absolute paths, we must also be able to specify *relative paths*. Using a relative path tells the shell that we want the path interpreted from our working directory, not the filesystem root. For example, if our working directory is `/home/runner` and within the folder `chap13` is the folder `data`, then we could type `ls chap13/data` to list the contents of the `data` folder. The string `chap13/data` is an example of a relative path that specifies where I want `ls` to do its work.

So far, I've talked about how to go deeper into folders with a relative path, but we may also want to navigate to the parent or grandparent folders of our working directory. We could, of course, specify a folder's *parent directory* (i.e., the folder containing our working directory) by typing out its absolute path from the root, but that might be a lot to type. So we have a way to say that in a relative fashion: using two period characters together (`..`) in a path means that we want the parent of the current working directory. The following are a few example commands that use this way of specifying a parent directory:

* To list the contents of the parent of the working directory, type

`ls ..`

* To change your working directory to the parent of the working directory, type

`cd ..`

* To change to the grandparent, type

`cd ../..`

A single period (`.`) in a relative path is another shorthand; it's another name for the working directory. It allows us to emphasize that we'd like the shell to interpret the path we give it from the working directory. If our working directory is `/home/runner`, the following are two equivalent ways of specifying a relative path to `data`:

1. `./chap13/data`
2. `chap13/data`

You might be wondering when using this shorthand is useful because the example shows that I must type more to do the same thing. I'm glad you asked!

## From paths to programs

When we want to run a program in a GUI, we double-click its icon. When we want to run a program in the shell, we type its name at the shell prompt followed by its input parameters, as we did earlier to invoke the Python interpreter on a particular script: `python3 broken.py`. But what did it mean when I typed this at the shell prompt: `./python32 broken.py`?

We just learned that a path that starts with `./` means we want the shell to look in the working directory for the name that follows. So `./python32` tells the shell that our command is called `python32`, and it's located in the working directory.[^fn7] OK, but typing just `python32` would have been fine too, right?

Wrong. Commands are *executable files*. They command our computer to do things, and we need to be careful how we command our computers. For example, we don't want to mistakenly command it to delete all our files. Even if `python32` is in my working directory, typing `python32` at the shell prompt won't start its execution. If you try, the shell should reply with "command not found." 

But wait, you say. The command `ls` wasn't in our working directory when we asked the shell to run it. Where does the shell look to find commands?

The answer is *shell variables*. As I hinted earlier, our shell is programmable. It has the concept of: (1) variables that can take on different values; and (2) built-in commands, like Python's built-in functions, that just work.[^fn8] Here is one of those built-in commands:

`echo $PATH`

This built-in command displays the value of the specified shell variable, and if you run this command, you'll see that the `$PATH` shell variable contains an ordered, colon-separated list of directories.[^fn9] These directories are where the shell will look for (non-built-in) commands. The executable file `ls` is in one of these directories, and `python32` is not. That's why typing `ls` at the shell prompt worked and typing `python32` didn't.

On the other hand, I might want to run a program that isn't in the `$PATH` list. I can run such a program by specifying it using its absolute path or an explicitly relative one. When I do this, the shell will look for that command only at that path. This is why `./python32` works despite the fact that this executable file is not in the `$PATH` list.

```{tip}
When you type `echo $PATH` at your shell prompt and hit return, you'll notice that the current working directory is not in `$PATH`. This is a security best practice. You should never add `.` to the list of directories in this shell variable. Only trusted directories should be in it.
```

So `./python32` is how we tell the shell that we want to run the program `python32` that we built and saved in our working directory. Later in this chapter, we'll see that `python32` is a Python script that begins by invoking the Python interpreter on this script's input parameters. But before we look at that code in detail, let's learn how to accomplish tasks 2 and 3.

```{tip}
I've written as if all operating system shells use the same commands and syntax, but they don't. What I've shown you is true for most Unix-based systems, which is what you get on Replit and in Google Colab. But even on a Unix-based system, there are many different shells that act similarly but not identically. Typically, you can learn which shell you're using by typing `echo $SHELL` and then ask `man` about it (e.g., if `echo $SHELL` returned `/bin/zsh`, you type `man zsh`, which is the command you can find in the directory `/bin`). While I'll describe only a small handful of the programs and shell commands that you'll find useful in problem solving, you can continue building your shell skills by using `man` to learn about `cp`, `mv`, `rm`, `mkdir`, `rmdir`, `cat`, `more`, `less`, and `which`. These are some additional, extremely useful shell commands.
```

## Redirecting inputs and outputs

One of the cool things we can do with a shell is rewire a program's inputs and outputs from the terminal (i.e., the default) to a file or even another program, which is exactly what we want to do in our second task. To illustrate, let's play with the `echo` command, which will print whatever are its input parameters to the terminal.[^fn10]

```{code-block} none
---
emphasize-lines: 1
---
chap13$ echo hi there
hi there
```

To change this default output location, we specify *output redirection*, which in the shell involves the greater-than character (`>`).

```{code-block} none
---
emphasize-lines: 1,2
---
chap13$ echo hi there > data/out.txt
chap13$ cat data/out.txt
hi there
```

We just told `echo` to send its output to a file called `out.txt` in the subdirectory called `data`. If your shell gave you an error, make sure that `data` folder exists in your working directory.[^fn11] The program `cat`, which I mentioned earlier as one of several useful programs you should learn, prints the contents of the file that's named as its input parameter. It helps us to verify that the output redirection did what I said it would. 

```{admonition} You Try It
Type `cat` at the shell prompt and hit return. Now type something and hit return. When `cat` isn't given an input file, it defaults to reading what you type in the terminal window and then prints that. When you're done playing, type Ctrl+D (i.e., hold the control key and then push the `d` key) to terminate the execution of `cat`.
```

You've now seen that `cat` can grab data from a file (in the same way our scripts open files to read their contents) or from what we type at the terminal. But we can also mix these two things so that `cat` reads the contents of a file as if we were typing its contents at the terminal. To do this, we use the input redirection character (`<`).

```{code-block} none
---
emphasize-lines: 1
---
chap13$ cat < data/out.txt
hi there
```

## Which output?

With this knowledge, let's try to capture the output of the Python interpreter when run on our broken script, which itself takes no input.

```{admonition} You Try It

Run `python3 broken.py > data/out.txt`. What was printed to the terminal, and what did you find in the file `out.txt`?[^fn12]

```

You just learned that the Python interpreter does not print its execution output and its error messages using same mechanism. If we want to capture a program's error output, we must use a different redirect symbol: `2>` where the `2` stands for *file descriptor 2*. On a Unix-like system,

* file descriptor 0 is called *stdin* and, by default, it is wired to the terminal;
* file descriptor 1 is called *stdout* and, by default, it is wired to the terminal;
* file descriptor 2 is called *stderr* and, by default, it is wired to the terminal; and
* file descriptors numbered greater than 2 are allocated to the files you open using a command like Python's built-in `open`.

You can learn a lot more about such things by reading the documentation for your favorite shell program or taking a class in systems programming. All we care about is that we now know how to capture the error message we want to rewrite:

```{code-block} none
---
emphasize-lines: 1,2,5
---
chap13$ python3 broken.py > data/out.txt 2> data/out2.txt
chap13$ cat data/out.txt
['this', 'is', 'a', 'test']
['his', 'is', 'another', 'test']
chap13$ cat data/out2.txt
Traceback (most recent call last):
  File "/home/runner/chap13/broken.py", line 23, in <module>
    main()
  File "/home/runner/chap13/broken.py", line 20, in main
    f_undef_name()
  File "/home/runner/chap13/broken.py", line 5, in f_undef_name
    print(undef_name)
NameError: name 'undef_name' is not defined. Did you mean: 'f_undef_name'?
```

## Pattern matching

With the second task understood, we turn to the third, which asks that we translate the error message that the Python interpreter produced into the one we want our user to see. To do this, we must: (1) recognize the patterns the interpreter uses in its error message; (2) grab the important data in these patterns that we want to use in our rewritten error message; and then (3) produce our error message with these data.

Recognizing and exploiting patterns is a huge piece of computational thinking. We've seen the power of patterns when we turned a divide-and-conquer problem into a recursive procedure. Here are a few other examples of applications that use pattern matching:

* If your job requires you to use computers to process data, you'll use a wide variety of tools built by computer scientists to find and exploit the patterns in your data (e.g., in physical, biological, or financial data).
* If you want your application to converse with a human, you'll use natural language tools that exploit the patterns found in human languages.
* If you want to scrape data from a bunch of websites around the Internet, you'll program your scraper to look for patterns on the websites it visits to identify the data it should capture.
* Even the syntax highlighter in your IDE uses pattern-matching tools to know which text to highlight in what color.

## Wildcards in the shell

We learned to use `ls` to list all the files and directories in our working directory, but what if we wanted to list just those files or directories with names matching a particular pattern? For example, what if we wanted to list all the Python scripts in our working directory?

This is an example of pattern matching, and to express our desire to the `ls` command, we need a syntax (or ideally a *language*) to describe the patterns we want. In most shells, *wildcard characters* allow you to perform a simple kind of pattern matching. These wildcards aren't literal values we want the shell to use, but like the joker in a deck of playing cards, they are characters that can take on any value. Let's see how to solve the challenge I just posed using shell wildcards.

We know that Python scripts end with the extension `.py`, and therefore we want to tell `ls` that we want it to list only the filenames that end in `.py`. Or stated another way, we don't care what comes before the `.py`. We can achieve this by typing `ls *.py` at the shell prompt.

In the shell, the star or asterisk character (`*`) is a wildcard character, and it will *match zero or more characters*, depending on where you put it. So the pattern `*.py` says to the shell that you want all the filenames that end in `.py`.

```{admonition} You Try It

Describe the kinds of filenames that `ls *e*.py` will list.[^fn13]

```

You can also ask the shell to *match any single character* using the `?` wildcard character.

```{admonition} You Try It

Can you describe what this weird-looking command `ls *.??` asks of the shell?[^fn14]

```

Finally, you can use square brackets to *create your own special, single-character wildcard*. For instance, the command `ls out[0-9].txt` would match any file that starts with `out`, ends with `.txt`, and has a single digit between the `t` in `out` and the `.` of the file extension.

## Regular expressions

*Regular expressions* *(REs)* allow us to express a larger set of patterns for matching text than we can with shell wildcards. They are well-studied in theoretical computer science, and they describe the set of *regular languages* found in that field's classification of formal languages. They also bring us back to the concept of finite state machines (FSMs). Regular expressions can be algorithmically translated into a FSM, which can then be straightforwardly translated into code. If you are a power user of search engines or word processors, you have already been exploiting the power of REs.

To solve our third task, we're going to use Python's `re` library, which allows us to use REs to match and capture sequences of characters in strings.[^fn15] 

## Finding simple words

Let's start learning how to use the `re` library with another challenge: Define a RE that finds all the words that contain nothing but the letters `a` to `z`.

To solve this challenge, you need to know that the `re` library asks you to specify your REs as Python strings. This means that you can match the word `word` and only that word with the RE `'word'`. In other words, most characters match themselves. Or in detail, this RE says that a matching string must start with the letter `w`, followed by the letter `o` and then `r` and finally `d`.

But what if we wanted to match the words `word` or `cord` or `ford`? As we saw with shell wildcards, REs allow you to use square brackets to designate a set of letters that match a single letter in the input string. Our RE would become `'[cfw]ord'`.

You can also use a dash in the square brackets to express a range of letters, as in:

* `'[a-z]'` matches any single letter in the range `a` to `z`.
* `'[a-zA-Z]'` matches any uppercase or lowercase letter.

To match a *sequence of characters*, REs use the `*` and `+` characters. We just met the `*` in shell wildcards, but we will change its meaning slightly when we use it in a regular expression. In REs, the `*` and `+` characters are *metacharacters* in that they don't match their literal character equivalents, nor do they function as wildcards. Instead, they specify how many times *the previous character* must be matched.

* `*` means zero or more times.
* `+` means one or more times.

So, to match a word containing the letters `a-z`, you'd write the RE pattern `'[a-z]+'`. This pattern ends its matching when the next character is something other than `a-z` (e.g., a space).

```{admonition} You Try It

Take a moment and run the script `play.py`, which you can find in this chapter's GitHub repository. When you start running it (i.e., type `python3 play.py` at the shell prompt), it asks you for a RE, which you type without any surrounding quotes. Try it with the REs we've discussed: `word`; `[cfw]ord`; `[a-z]`; `[a-zA-Z]`; and `[a-z]+`. Once you've specified a RE, `play.py` repeatedly prompts you for a string, and given one, the script tells you whether that string is in the language of the RE (i.e., whether that string satisfies the pattern specified by the RE). For example, `ford` satisfies the pattern `[cfw]ord`, but `lord` doesn't. `m` satisfies the pattern `[a-z]` but `me` doesn't. `word` satisfies the pattern `[a-z]+`, but `this word` doesn't. Do you see why `this word` doesn't?[^fn16] Try your own REs and strings. When you want to stop playing with the RE you specified, type `quit` at the `String:` prompt. Rerun the script to try another RE.
```

## Matching metacharacters

You might be wondering what you'd do if you wanted to match a literal `*` or `+` (or any other RE metacharacter). The answer is that you escape the character with a leading backslash. For instance, to match `2+3`, you'd like to write the RE `'2\+3'`.

```{admonition} You Try It

Feed the RE `2\+3` to `play.py` and verify that it matches the string `2+3`. What strings does the RE `2+3` match?[^fn17]

```

Interacting with the script `play.py` makes it look straightforward to write a RE and use it in a Python script: You simply need to escape any RE metacharacter with a backslash.

 

```{admonition} You Try It
The backslash character is a RE metacharacter, so feed the RE `\\` to `play.py` and verify that it matches the string `\` and no other strings. Then try feeding `\` as an RE to `play.py` and notice that the script fails with an error message. It says that a single backslash character is effectively an incomplete sentence; it needs at least one more character to be a valid RE.
```

Let's now take this knowledge into Python itself. Start the interactive Python interpreter, as I've done below, and run the three statements. The first loads the RE library. The second creates our pattern as a string literal named `p`. It correctly escapes the backslash. The third statement tries to compile the pattern named `p` (i.e., it tries to turn this pattern into code that we can use to check strings).

```{code-block} none
---
emphasize-lines: 1,5,6,7
---
chap13$ python3
Python 3.9.6 (default, Nov 10 2023, 13:38:27) 
[Clang 15.0.0 (clang-1500.1.0.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import re
>>> p = '\\'
>>> re.compile(p)
```

When you run these three statements, you'll find that an `re.error` occurs. In fact, it is the same error we experienced when we fed a *single* backslash to `play.py`. Why?

The answer requires you to understand that there are two languages at work in this second example (i.e., the language of REs and the Python programming language). And both these languages treat the backslash character in a special manner. When we write `p = '\\'`, the Python interpreter creates a string in your computer's memory with a single backslash, and we know that a single backslash is interpreted by the RE engine as an incomplete sentence.

So how do we fix this? We need to pass a string containing two backslashes to the `re.compile` function. In Python, this string is written as `'\\\\'`, which escapes each of the two backslashes the RE engine wants to see.[^fn18] Yuk. This is what's called "the Backslash plague."[^fn19]

To avoid this plague, I'll specify RE pattern strings in *Python's raw string notation*, which treats any backslash we type as a backslash and not an escape character. With this notation, the unwieldy string literal `'\\\\'` becomes `r'\\'`. As another example, the RE pattern matching a single asterisk character `*`, when written as a Python string literal, goes from `'\\*'` to `r'\*'`.[^fn20]

## Using REs

Now that we know how to specify a RE using the `re` library, how do we use it to find instances of a pattern in an input string? Well, there are two ways to do it:

1. If you plan to use your pattern in multiple statements, you'll want to *compile your RE into a pattern object*. You'd then *use the methods of this pattern object* to find instances of the pattern in strings without having to continually restate the RE.
2. You can also *use a function from the* `re` *library* to which you provide a regular expression and a string as parameters. This shortcut works well when you don't need to restate the RE in a later call to the `re` library.

In case those words don't make sense, the following applies these two approaches to our word-matching challenge. Both use the RE `'[a-z]+'` on the string `s1`, and both return this list of matches: `['this', 'is', 'a', 'test']`.

```{admonition} You Try It
Run the following code blocks in the interactive Python interpreter. Feel free to experiment with different REs and strings.
```

```{code-block} python
---
lineno-start: 1
---
### Setup for the following examples
import re

s1 = 'this is a test'
s2 = 'This is another test.'
```

```{code-block} python
---
lineno-start: 1
---
# Build and use a pattern object
p = re.compile(r'[a-z]+')
p.findall(s1)
```

```{code-block} python
---
lineno-start: 1
---
# Just use an RE function
re.findall(r'[a-z]+', s1)
```

Besides `findall`, our scripts will use the `match`, `fullmatch`, and `search` functionality of the `re` library, which all return a `re.Match` object if there's a match and `None` otherwise. On a successful match, the `re.Match` object contains the matching characters and the starting and ending indices of where in the input string you can find the matching. The `match` method succeeds if the pattern appears at *the start* of the input string; `fullmatch` succeeds if *the entire* input string matches the pattern; and `search` succeeds if the pattern appears *anywhere* in the input string.

Here are some examples using the test strings `s1` and `s2`. Think about what they'll do and then run them to check your thinking. This functionality isn't easy to pick up, so don't get discouraged if you find it confusing. I certainly do.

```{code-block} python
---
lineno-start: 1
---
# Match succeeds and a re.Match object is returned
p.match(s1)
```

```{code-block} python
---
lineno-start: 1
---
# Match fails because of the starting capital letter
p.match(s2) == None
```

```{code-block} python
---
lineno-start: 1
---
# Fullmatch fails on a string with multiple words
p.fullmatch(s1) == None
```

```{code-block} python
---
lineno-start: 1
---
# Search succeeds and a re.Match object is returned
p.search(s1)
```

```{code-block} python
---
lineno-start: 1
---
# Another more interesting search example
re.search('is', s1)
```

```{code-block} python
---
lineno-start: 1
---
# What's the result returned from this statement?
p.findall(s2)
```

We can capture capital letters either by changing our RE or changing the default of the optional argument `flags`[^fn21] to `re.compile` and the library's other functions:

```{code-block} python
---
lineno-start: 1
---
# Change the RE
p2 = re.compile(r'[a-zA-Z]+')
p2.findall(s2)
```

```{code-block} python
---
lineno-start: 1
---
# Leave the RE alone and change the `flags` parameter
p3 = re.compile(r'[a-z]+', flags=re.IGNORECASE)
p3.findall(s2)
```

## Finding filenames

Now that we understand the basics of the `re` library understood, we're ready to use patterns to change the Python interpreter's error message into one that we'd prefer to read. Let's again take this a step at time and first use REs to grab the filenames mentioned in the interpreter's error output.

Notice that the Python interpreter's error message contains absolute paths:

`File "/home/runner/chap13/broken.py", line 23, in <module>`

Because all the scripts we write appear in our working directory, the working directory's path is unnecessary and distracting information. We might as well delete it, shortening any absolute paths involving the working directory into nothing more than the filename. In other words, if `/home/runner/chap13` is our working directory, we'll grab `broken.py` from `/home/runner/chap13/broken.py` and discard the rest.

Let's write a function called `get_fname` that does this. It will contain something like the RE we just used to capture words containing both capital and small letters---that is, `r'[a-zA-Z]+'`. But because our filenames often also include digits and the underscore (`_`) character, we'll use a new metacharacter `\w`, which is a shorthand for `[a-zA-Z0-9_]`.

Our RE is now `r'\w+'`, which can match filenames without any file extension. The filenames in the interpreter's error message are all Python scripts, and we want to include in the match the extension `.py`. The only tricky bit here is that the period character is a metacharacter equivalent to the shell wildcard `?`; it matches any single character except a newline. Since we want to match an actual period, we'll escape it.

Our RE has become `r'\w+\.py'`. So far so good. But what if a directory name in the absolute path matches this pattern? We don't want that.[^fn22] We want only the last component of a file's absolute path, which is its filename. We can specify this in our RE by using yet another metacharacter: the dollar sign (`$`). This metacharacter introduces us to a new idea, and that's the matching of *a point in a string*. A dollar sign in a RE means that we want to match the end of a string or the point just before the next newline character, which is where we'll find the filename.

The RE `r'\w+\.py$'` will successfully match the kinds of filenames we use[^fn23] and pluck them from the end of the input strings. Let's use the `re.search` function from the `re` library so that we can get some practice with it and learn about a useful method on `re.Match` objects. Here's the code for our `get_fname` function:

```{code-block} python
---
lineno-start: 10
---
### chap13/rewrite.py
def get_fname(fullpath):
    '''Strip full pathname down to just the filename'''
    fname = re.search(r'(\w+\.py)$', fullpath).group(0)
    return fname
```

```{code-block} python
---
lineno-start: 1
---
### Test get_fname
path = '/home/runner/chap13/broken.py'
get_fname(path)
```

There are two things you should note in this code. I added a pair of parentheses around the part of the RE that matched the filename. Parentheses are RE metacharacters that create something called *groups*, and the value of a group is whatever the RE in the parentheses matches, if anything. Here, I capture the entire set of matching characters, but I could have captured just a part of the match in the parentheses. We'll use that functionality in a moment.

To access the match inside a set of parentheses, you use the `group` method on the returned `re.Match` object. The index for the first set of parentheses is `1`, the second `2`, and so forth. The index `0` is special; it returns the entire match, which is all we need in this example.

Don't worry if line 13 looks like a bunch of gibberish to you right now. It will get easier to read with practice. And then time will pass, and you'll forget the details, but again, that's OK. You will remember them with a refreshing read of the documentation.

## Python RE extensions

We have all the pieces we need to complete task 3. We can recognize patterns in the Python interpreter's error message, grab the data we want, and use the data to compose our own, more readable error message. The script `rewrite.py`, which you can find in the book's GitHub repository, contains a function named `rewrite_emsg` that takes the Python interpreter's error message as its input parameter and prints on `sys.stderr` our own rewritten error message.[^fn24]

Behind the interface of `rewrite_emsg` is a lot of interesting work. Because I wanted information that currently exists at the end of the Python interpreter's error message at the start of my error message, this function splits the processing of the input error message `e` into two pieces: (1) grab and process the "Traceback" lines, which is done in `process_traceback`; and (2) take some of what was learned from `process_traceback` and process and print the error followed by my rewritten traceback, which is done in `print_error` and aided by `print_stack`.

Understanding the code in `rewrite.py` is useful if you want to modify my error message, but even if you don't, there is one aspect of how `process_traceback` works that's worth highlighting. It deals with parentheses in REs, which I briefly mentioned earlier. Now I'll explain why they are useful.

The following code block extracts the RE defined on line 32 of `rewrite.py` and uses it in a `re.match` call similar to those that take place in `process_traceback`.

```{code-block} python
---
lineno-start: 1
---
import re

# Define the pattern for a traceback line
p = r'  File "(?P<fname>.*)", line (?P<lnno>\d+), in (?P<fun>[\w<>]+)'

# Define an example traceback line
line = '  File "/home/runner/chap13/test.py", line 7, in f_undef_name'

# Apply the pattern to the line
m = re.match(p, line)

# Print what the pattern captured in the parentheses
print('fname = ', m.group('fname'))
print('line number = ', m.group('lnno'))
print('function = ', m.group('fun'))
```

The three separate parentheses in the RE on line 4 extract three separate values from a match. The format of the content of each parenthesis pair is quite cryptic at first glance. To read a grouping like `(?P<lnno>\d+)`, you should understand the following:

* `\d+` is the pattern we want to match. It uses another escape shorthand `\d` that matches one or more digits `0-9`.
* `(?P<lnno> ...)` acts like normal parentheses in that it creates a group, but the leading `(?P` creates it as a *named group*. In particular, `(?` means that we're invoking an extension, and the `P` means it's a Python extension. The `lnno` in the angle brackets is the name we want associated with this group.

By naming the different parts extracted, you can see that I want the filename (`fname`), line number (`lnno`), and function name (`fun`) from the match. Lines 13−15 illustrate how to use these named values. The alternative is to use the index indicating where each match was expressed in the original RE, which, as you can imagine, makes code using the RE library even harder to read and understand.

## Putting it all together

If we design `rewrite.py` to expect the name of a file in which we put the Python interpreter's error message, we could type the following on the shell's command line to capture and replace the interpreter's message with ours:

`python3 broken.py 2> out2.txt; python3 rewrite.py out2.txt; rm -f out2.txt`

By placing a semicolon at the end of each command, the shell knows that it should launch the command it read up to the semicolon; let this command finish; and then launch the next command. You can think of the semicolon as a way to build a task list for the shell. The task list we wrote above says to do the following:

1. Run the Python interpreter on `broken.py`, grab its `stderr` output, and put that output in `out2.txt`. Anything written by `broken.py` to `stdout` is displayed on the terminal.
2. When the work in (1) is done, run the Python interpreter on `rewrite.py`, which expects a file containing an error message as its only input parameter. We give it `out2.txt` from the execution of `broken.py`. It rewrites the captured error message and prints the result to `stderr`, which is wired to the terminal in this command.
3. When the work in (2) is done, `rm` removes the file `out2.txt`. The `-f` flag says to do it silently. If you've set `rm` to request confirmation before deleting anything, it won't under this flag, and if `rm` encounters an error (e.g., the file `out2.txt` doesn't exist because the script fed to the Python interpreter in (1) didn't contain an error), it won't print a diagnostic message.

```{admonition} You Try It
Try this shell command line and its sequence of tasks. Try your own command line that includes semicolons.
```

## Shell pipes

While this command line with its sequence of tasks solves our problem, it is a lot to type. But we don't have to type it all. We can have the shell and the Python interpreter do more of this work for us. For example, we can get rid of the file `out2.txt` (and thus the need for the `rm` command above) by sending the Python interpreter's error output directly to `rewrite.py`. The shell doesn't only support redirection of a program's output to a file. It also allows us to "wire" the output of one program directly to the input of another. You accomplish this with the shell's pipe operator (`|`).

As a simple example of piping, imagine that I wanted to see the Python scripts in my working directory sorted by size, and I wanted the smallest scripts listed first. The `ls` command has an option `-l` that prints a lot of information for each item it lists, and most Unix-like systems come with a `sort` program. The `sort` program on my system has an option `-k` that allows me to indicate which column `sort` should use for ordering the input lines.[^fn25] Terrific! I just need to wire the output of `ls` into the input of `sort`, and my job is done.

```{code-block} none
---
emphasize-lines: 1
---
chap13$ ls -l *.py | sort -k 5
-rw-r--r--@ 1 profsmith  staff   431 Sep 25 14:53 broken.py
-rw-r--r--@ 1 profsmith  staff   587 Sep 26 10:16 python32.py
-rw-r--r--@ 1 profsmith  staff   901 Sep 26 12:46 play.py
-rw-r--r--@ 1 profsmith  staff  4775 Sep 26 13:10 rewrite.py
```

Unfortunately, it's possible but not as straightforward to wire the `stderr` of one program into the `stdin` of another, which is what we want to do. The following is the syntax to do it.[^fn26] It's shorter than our previous solution, but it is still not the elegant solution we want.

`python3 broken.py 2> >(python3 rewrite.py)`

## Scripting what the shell did

To get the elegance we want, we'll put the work we've been doing on the shell command line in a Python script. Why stop with a script that rewrites the Python interpreter's error message when we can write a script that launches the interpreter on the script we want to run, captures the interpreter's error output, and feeds that output to `rewrite.py`?

Let's call this new script `python32.py`, and it will take as input the script we want the Python interpreter to run.

`python3 python32.py broken.py`

{numref}`Figure %s<c13_fig1_ref>` illustrates how we want `python32.py` to operate. The first thing it will do is launch another process that runs the Python interpreter on its input parameter. The `sys.stdout` of this launched process will be left untouched (i.e., it will default to the terminal). However, the launched process's `sys.stderr` will be configured to be a pipe back to `python32.py`. These three things are possible using Python's `subprocess` library. We'll then have `python32.py` grab the bytes transferred through the pipe, convert them into a string, and send this string to `rewrite_emsg`, which `python32.py` will import from `rewrite.py`. As we already know, `rewrite_emsg` prints its rewritten error message to `sys.stderr`, but this is `python32.py`'s `sys.stderr`, which is wired by default to the terminal. Ta-da!

```{figure} images/Smith_fig_13-01.png
:name: c13_fig1_ref

A sketch of how we want our `python32.py` script to operate when it is fed to the `python3` interpreter and given `broken.py` as its command line input.
```

## Concurrency

Did you notice that there are two instances of the Python interpreter running in {numref}`Figure %s<c13_fig1_ref>`? There's the one that runs `python32.py` and the one that this script launches to run `broken.py` (or whatever is the input parameter to `python32.py`). This might remind you of the way we ran the client and server scripts in Chapter 5. Both involve the use of multiple processes, but in this case, we're running two instances of the same program (i.e., the Python interpreter) on different inputs (i.e., the scripts we want interpreted).

The emphasis in our networking discussion was in *the communication between two processes*. In this chapter's problem, we're still interested in two processes communicating, but we're using a different mechanism (i.e., pipes rather than the network). Both examples illustrate the concept of *concurrency* and some of its fascinating aspects.

We've spent a lot of time in this chapter talking about the shell, and there's a concurrency tie in. When you ask the shell to run a program by typing a command at the shell prompt, it does something very much like we just did in launching a subprocess in `python32.py`. Not only do you now know more shell commands, but you also understand a bit about how it works!

## Making python32 look like python3

The astute reader will point out that invoking the Python interpreter on `python32.py` is not the command I showed at the start of this chapter. It was `./python32 broken.py`. So I owe you one more explanation of shell functionality: how to turn `python3 python32.py` into `./python32`.

You can do this with any Python script you write that runs on a Unix-like system. It involves three small steps:

1. Tell the filesystem that your Python script is an executable.
2. Create a symbolic link to your Python script to lose the `.py` extension.
3. Add a line to the start of your script that tells the shell how to execute your file, since it can no longer use the extension as a hint about the file's contents.

Step 1 modifies the metadata that the filesystem keeps about each file. We saw some of this metadata when we did `ls -l` earlier. The first 10 characters of a long listing are what are called *file mode bits*, and 9 of them indicate whether the file is *readable (r)*, *writable (w)*, or *executable (x)*. By default, data files are readable and writable by their owner[^fn27], but they aren't executable.

To make a file executable, you must change its mode. The shell command we use to do this is `chmod`, which stands for "change file mode bits." The following transcript shows how to use this command, and it illustrates the file mode bits before and after.[^fn28]

```{code-block} none
---
emphasize-lines: 1,3,4
---
chap13$ ls -l python32.py
-rw-r--r--@ 1 profsmith  staff   587 Nov  3 19:10 python32.py
chap13$ chmod +x python32.py
chap13$ ls -l python32.py
-rwxr-xr-x@ 1 profsmith  staff   587 Nov  3 19:10 python32.py
```

Step 2 initially seems straightforward. We make a copy of our script (e.g., `cp python32.py python32`) or rename it (`mv python32.py python32`). Unfortunately, both are suboptimal solutions. The former creates two copies of the same file, and the latter means we won't get syntax highlighting in the IDE (because it can't read the `.py` extension and know it's a Python file).

There is a better solution. Create an alias, which in Unix terminology is a *symbolic link*, using the `-s` option on the `ln` command.

`ln -s python32.py python32`

Now we can use an IDE to edit our script with full color syntax highlighting through the name `python32.py`, and we can efficiently run our script through the name `python32`. We have two names for the same file.

Step 3 requires us to tell the shell how to run our script in a manner that doesn't have us invoke the Python interpreter on the shell's command line. Note that all we did by setting the executable file permission bit was tell the shell that it's OK to run this file as a command; the shell also needs to know what it should do to execute the file. By convention, the shell expects this information at the start of an executable file.[^fn29] This means that we need to put some directions for the shell at the start of `python32`. The format of these directions on Unix-like systems is as follows: 

`#!/usr/bin/env python3`

This line starts with a "shebang" sequence[^fn30] (i.e., `#!`), and the rest of it is the way the shell invokes the interpreter program. With this information, the shell turns `./python32` back into `python3 python32.py`. We now have a way to type one command (`./python32`), and the shell will launch the Python interpreter on a script (`python32.py`) that does all the work we had to previously write out on the shell's command line. We've shimmed the Python interpreter and made it look like it's producing our rewritten error message!

We began this chapter saying that the right tool will make our problems easy to solve. Regular expressions were the right tool for grabbing the data we wanted out of the Python interpreter's error messages, and the programming aspects of the shell were the right tool for automating all we wanted done behind one simple command, which we built. We saw several ways to execute `rewrite.py`, and one of them turned out to be an elegant solution to our problem.

[^fn1]: The undefined name is on line 5 of \`broken.py\`.

[^fn2]: From our visualization of the work of a function call in the ALEs for Chapters 3 and 12, recall that a frame is added to a program's stack in its data memory whenever we make a function call. Among other information, this frame contains the location of the calling statement. It's a stack because the frame on the top of the stack corresponds to the currently executing function, and it is removed when this function returns (i.e., last frame added is the first one removed).

[^fn3]: This approach should remind you of the shimming operation we performed in Chapter 5, where we wanted to make only small changes to a library's API or its operation. This same problem-solving approach is applicable with programs if you take advantage of the programmable nature of the shell (or whatever is the computational environment in which you run your programs).

[^fn4]: The shell is an example of a command line interface (CLI), where commands are expressed in text at a command prompt.

[^fn5]: Yes, you can ask the \`man\` command to describe itself (i.e., you'd type \`man man\`).

[^fn6]: In an absolute path, I've used forward slashes (\`/\`) to indicate where one folder's name ends and the next begins. This is the convention for the operating system I'm running. Some operating systems (e.g., Windows) use backward slashes (\`\\\\\`) rather than forward slashes.

[^fn7]: You won't find \`python32\` in the GitHub repository for \`chap13\`. You'll create it later in the chapter. But when it is eventually there, \`./python32\` says you want \`python32\` in the working directory!

[^fn8]: When I say "built-in," I mean that the shell knows what to do based solely on the command name (e.g., when we type the command \`echo\`). It doesn't search the filesystem looking for an executable file with this name.

[^fn9]: Yes, that dollar sign is part of the shell variable name. In Chapter 1 under the section on "valid names in Python," I described the rules involved in creating valid variable names in Python. Basically, these names must start with a letter or an underscore character. In contrast, the shell has a different set of rules. For example, shell variables names must start with a dollar-sign character.

[^fn10]: Notice that the shell considers \`hi\` and \`there\` to be literal strings and not shell variables, since they don't start with a dollar sign. The shell's syntax is different from Python's!

[^fn11]: Using output redirection will create the specified destination file if it doesn't exist (and overwrite it if it does), but not any directories along the path. To create a directory named \`data\` in your working directory, run \`mkdir data\`.

[^fn12]: In the directory \`data\`, the file \`out.txt\` contains the first two lines that were printed when I ran \`python3 broken.py\`, which are the lines that the script wanted to print. The error message from the interpreter was still printed to the terminal.

[^fn13]: It will list any file ending in \`.py\` as long as the string before these last three characters contains at least one instance of the letter \`e\`.

[^fn14]: It lists any file that ends in a two-character extension.

[^fn15]: Most RE libraries, including Python's, have been extended so that the patterns they match comprise more than the set of regular languages. For an excellent (and more comprehensive) introduction to Python's \`re\` library, please read A.M. Kuchling's "Regular Expression HOWTO" document (https://docs.python.org/3/howto/regex.html).

[^fn16]: The RE \`\[a-z\]+\` matches one word, but the string \`this word\` contains two words. The space character causes the RE to fail to match.

[^fn17]: It doesn't match the string \`2+3\`, but strings like \`23\`, \`223\`, and any number made from a sequence of twos before a single concluding three.

[^fn18]: Change what we assign the name \`p\` in our earlier exercise to assure yourself that the error disappears.

[^fn19]: See Kuchling's article, "Regular Expression HOWTO," referenced earlier, for the lengthy explanation of this phenomenon.

[^fn20]: You still type \`\\\*\` as the input to \`play.py\` since the \`input\` statement in its code effectively treats what you type like a raw string literal.

[^fn21]: For a full listing of the \`re\` library's flags, see the description of the module contents (https://docs.python.org/3/library/re.html\#contents-of-module-re).

[^fn22]: I'm taking you step-by-step through the development of a working RE, but it didn't go this smoothly when I did this work. I created what I thought might work, and then tried it on some examples. When it failed, I rethought the RE and tried again. Such is design work, as you know.

[^fn23]: In many file systems, file and directory names can include other characters (e.g., a hyphen or a period). It's possible to extend \`get\_fname\` to handle names with these and other allowable characters, but I leave this as an exercise to the reader.

[^fn24]: In Python, you specify that you want to print to standard error (\`stderr\`) by setting the print-statement's optional parameter \`file\` to \`sys.stderr\`.The \`sys\` module is where you find the definitions for \`sys.stdin\`, \`sys.stdout\`, and \`sys.stderr\`.

[^fn25]: Where is the file size in the \`ls\` listing? It's the fifth column. For example, \`broken.py\` is 431 bytes.

[^fn26]: Feel free to ask a generative AI system like ChatGPT to explain this cryptic syntax.

[^fn27]: There are three groups of rwx bits with each file. Starting from the left, the second through fourth bits state the permissions of the file's owner, which is typically the computer account under which the file was created. There's a lot encoded in these file permission bits, which you can start reading about in the Wikipedia article on file-system permissions (https://en.wikipedia.org/wiki/File-system\_permissions).

[^fn28]: To no longer allow a file to be executed, you'd type \`chmod -x the\_file\`.

[^fn29]: If the shell doesn't recognize the start of an executable file as information on how to execute the file, you'll probably get a cryptic error as it tries to interpret your file in an incorrect manner.

[^fn30]: Wikipedia, "Shebang (Unix)," accessed March 29, 2025. https://en.wikipedia.org/wiki/Shebang\_(Unix)