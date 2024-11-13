# Chapter 15: Embrace Runtime Debugging #

Does this quote by Brian Kernighan and P.J. Plauger resonate with you?

*"Everyone knows that debugging is twice as hard as writing a program in the first place."*[^fn1]

Or based on your experience, perhaps you think this underestimates the difficulty of debugging. It does when I consider how I typically spend my time. Nonetheless, Kernighan and Plauger follow up this general truth with a biting insight:

*"So if you're as clever as you can be when you write it, how will you ever debug it?"*

Design and subtle coding errors are a fact of life, and these two influential figures remind us that programming is hard enough without the added challenge of a "too-clever program." But what do you do when you've written your script as simply and directly as you can, and it still fails with some difficult to discern error?

You could employ the ever-reliable debugging print-statement, but this tool is a blunt instrument. It tells you facts about your script's execution, but the fact that helps you debug your program might be buried in a mound of uninteresting ones. You undoubtedly experienced this when you stuck a debugging print-statement into the body of a loop. Unless your script died immediately after the debugging print, you're left searching for that proverbial needle in a haystack of debugging outputs. What you need is for your program to pause its execution at each debugging print-statement so that you can check if everything looks ok before allowing it to proceed. In other words, debugging is best done when we slow down the program's execution to match the speed of human thought.

To achieve this, you could couple each of your debugging print-statements with an input-statement that pauses the script until you're ready to proceed, but even this isn't quite enough. A debugging print-statement provides you with the current values of the variables you thought were important *before* you started the debugging run. When a debugging print illuminates a problematic variable, this can immediately raise other questions in your mind, but the only way you can interrogate the rest of the runtime state is by stopping this execution, updating your debugging print-statement, and rerunning your script. Wouldn't it be nice if you had a debugging tool that allowed you to poke around in your program's state at any point in its execution to answer any question you had? With such a tool, you could fix your script twice as fast.

Debuggers are programs that do exactly this.[^fn2] By learning how to use a debugger, you can:

1. Start, stop, continue, and restart the execution of your script at any point in its execution; and
2. Inspect your script's runtime state without inserting print-statements into its source code.

In my experience, too many of us (myself included) wait too long to move from debugging print-statements to a debugger. A good part of the reason is that debuggers seem mysterious. How is it that the debugger can stop and then restart a script? How does it regain control at the point where we wanted our script to stop? What allows the debugger to answer our questions about our script's runtime state? What exactly is this runtime state I keep mentioning?

To solve these mysteries, we'll build pieces of a debugger in Python, and along the way, you'll learn a fundamental fact about computing systems and the basic structure of the Python interpreter. I hope that this knowledge encourages you to try using a debugger the next time your script falls into an infinite loop or fails in some cryptic manner. You'll be amazed at how you can use this powerful tool to quickly find your typos and fix your design errors.

```{admonition} Learning Outcomes
Learn about the duality of code and data in modern computing machines, which allows us to build tools that help us to debug and instrument our scripts. You will build your own simple debugger and an instrumentation tool. You'll also learn the meaning and importance of the acronym REPL, which describes the Python interactive interpreter environment. After completing this chapter, you will be able to:

*   Describe the basic operation of debuggers and code instrumentation tools, which are invaluable for finding and fixing design and subtle coding errors in your scripts [design and CS concepts];
*   Discuss the meaning and importance of the duality of code and data [CS concepts];
*   Explain the connections between breakpoints and runtime state [CS concepts];
*   Raise your own exceptions and pass information from the point of the exception to the exception handler [CS concepts and programming skills];
*   Use Python's built-in function `exec` to launch a script from within a script and explain how the two scripts interact [CS concepts and programming skills];
*   Use Python's built-in function `eval` to mimic the read-eval-print loop (REPL) at the heart of Python's interactive interpreter [CS concepts and programming skills].
```

## The duality of code and data

The first problem we solved in this book had us write a Python script that we eventually called `read32.py` and with which we read the data file `CatInTheHat.txt`. {numref}`Figure %s <c15_fig1_ref>` is a screenshot of both these files. Besides their contents, what's the difference between them? Take a moment to ponder this question, and then read on.

```{figure} images/Smith_fig_15-01.png
:name: c15_fig1_ref

Screenshot of an editor with the open files `read32.py` and `CatInTheHat.txt`.
```

Both are files containing text. Both sit in the memories of our computing systems. However, we think of one as a script, or code, and the other as data. But what exactly is code? What distinguishes it from data?

In plain English, code is a set of instructions we expect the computer to follow, while data is what we tell, through our instructions, the computer to manipulate. But inside the machine, when we strip away these interpretations, both code and data are just sequences of bits stored somewhere in our computer's memory. The only difference between these bit sequences is in how we tell the machine to treat them.

This highlights the key insight to understanding debuggers: Because code and data are both just sequences of bits, *the same sequence of bits can sometimes be treated as code and other times as data*. This is called the *duality of code and data* in modern computing systems.

For example, sometimes we use a Python script as a set of instructions to direct our computer to complete a task. Other times, we tell a program (e.g., the editor in our IDE) to read and write our Python script as data. A debugger plays both these roles.

## Breakpoints and runtime state

The following is a typical way to use a debugger:

1. Start the debugger and ask it to open your script. Your script is being treated as data.
2. Tell the debugger where you want to place a *breakpoint*, which is a point (e.g., a file line) in your script where you'd like to pause its execution. The debugger is still treating your script like data, and I'll explain in a moment what exactly a breakpoint is and how the debugger modifies your script.
3. Tell the debugger to start the execution of (possibly its modified version of) your script under its control. You're now telling the debugger to stop treating your script as data and instead treat it like code. But you're also indicating that you'd like the debugger to regain control when your script completes, encounters an error, throws an exception, or hits a breakpoint.
4. Interact with your executing script as you normally would. Your computer is treating your script as code.
5. Upon encountering a runtime error, exception, or breakpoint in your script, the debugger's prompt returns. Importantly, your script's execution has not terminated, but *paused* at the point of the error, exception, or breakpoint. At this point, the debugger has access to your script and its *runtime state*, which includes the call stack and any variables (including their current values) *in scope* at that point. "In scope" is a technical term in computer science: a variable is in scope if it's valid to use it at that point in the program. Pulling back from the details, the debugger is again treating your script as data, but it also has access to your script's runtime state as data.
6. Tell the debugger to do something. This might have you asking the debugger to print the value of some variable or to add a new breakpoint. Some debuggers even allow you to change portions of the script's runtime state. Eventually, you'll decide to either continue the script's execution (returning to step 4) or terminate it.
7. When your script's execution terminates, the debugger is still running. It continues to have access to your script's source code (as data). You can set new breakpoints and restart your script's execution, which allows you to test new theories about what's wrong with it.

## Inserting a breakpoint

Let's drive home these concepts by building our own minimal debugger using nothing but Python statements. As we've practiced throughout this book, we'll start simply: the first version of our debugger will read the script we want debugged, insert a breakpoint, and write out the edited script. The following is some pseudocode to get us started:

```{code-block} python
---
lineno-start: 1
---
### Pseudocode for our Python script debugger

# Ask for the name of the script to be debugged
# Call our debugging routine with this script name
# .. Read in the original script
# .. Ask for the line on which to place a breakpoint
# .. Edit it to add the breakpoint
# .. Return the edited script
# Write out the edited script
```

Notice that our script's pseudocode separates the prompting of the user for the script-to-be-debugged from the function that will implement the debugger's functionality. The indented pseudocode statements represent the tasks done by the debugging function, and instead of running the modified script with the inserted breakpoint, we'll write it out so that we can examine it.

We can steal from prior scripts the work described in all but line 7, which adds the breakpoint to the script we want debugged. The next code block implements all the pseudocode except this line. Notice that the code encapsulates a few of our pseudocode tasks in functions and provides an ability to print the edited script as well as write it to a file.

```{code-block} python
---
lineno-start: 1
---
### chap15/pdb1.py
import sys

def my_pdb(script_fname):
    ''' First attempt at our debugger
        Input:   filename of script to be debugged
        Returns: edited script as list of lines where
                 each line ends in a newline character
    '''
    
    # Read in the original script (as a list of file lines)
    with open(script_fname) as fin:
        edited_script = fin.readlines()
    
    # Grab line number where we'll place the breakpoint
    breakpt = get_lineno(len(edited_script))
    
    # Convert breakpoint number into a list index
    breakpt_index = breakpt - 1
    
    # Add the breakpoint
    # TO BE WRITTEN
    
    return edited_script

def get_lineno(lines):
    ''' Asks the user for a line number
        Input:   number of lines in script
        Returns: user's line number
    '''
    while True:
        try:
            lineno = int(input('Line number in script? '))
            if lineno < 0 or lineno >= lines:
                print(f'The number must be in the interval [1,{lines}]')
                continue
            return lineno
        except ValueError:
            print('The line number must be an integer')

def print_it(edited_script):
    ''' Print out edited script with line numbers '''
    cols = len(str(len(edited_script)))
    for i, line in enumerate(edited_script):
        print(f'{i + 1:>{cols}} {line}', end='')

def write_it(edited_script, orig_fname):
    ''' Write edited script to a file with a `-db.py` suffix '''
    output_fname = orig_fname.replace('.py', '-db.py')
    with open(output_fname, 'w') as fout:
        fout.write(''.join(edited_script))
    print(f'Wrote {output_fname}')

def main():
    # Ask for the name of the script to be debugged, unless already provided
    if len(sys.argv) == 1:
        fname = input('What script would you like to debug? ')
    elif len(sys.argv) == 2:
        fname = sys.argv[1]
    else:
        sys.exit('Usage: python3 pdb1.py [script_to_be_debugged]')
    
    edited_script = my_pdb(fname)
    
    print_it(edited_script)
    # write_it(edited_script, fname)

if __name__ == '__main__':
    main()
```

If you run `pdb1.py`, giving it a script-to-be-debugged either on the command line or through the prompt, you'll see that it asks for a line on which to place a breakpoint, but it doesn't actually edit the script. Let's change the function `my_pdb` to insert a breakpoint.

## Inserting a new statement

A breakpoint is nothing more than the raising of a Python exception, which will give control back to our debugger if it catches all exceptions raised by the script-to-be-debugged. We can create our own Python exception and raise it as follows:

`raise Exception("My breakpoint")`

Let's assume our script-to-be-debugged is `guess32.py` from Chapter 5[^fn3] and we'd like to put a breakpoint before line 24. This line compares the user's guess against the secret. Notice that I said, "before line 24." I don't mean any point before line 24, but the point immediately before this line. In other words, line 23 (and everything before it at runtime) should have executed, but line 24 (and everything after it at runtime) should not have.

How we insert the raise-statement into `guess32.py` between lines 23 and 24 depends on whether we can *stretch the script without breaking it*. In other words, is the script that we give the debugger more like:

* a Python list, which is a data structure that makes it easy to stretch and then insert a new element; or
* an image file, which is a data structure in which we can change some of the image bits, but not add or remove bits?

Neither case is hard, but you must know which kind of data structure is holding the script's statements (or more generally instructions). In `my_pdb`, where we're reading the script from a text file using `readlines`, inserting a new statement is easy, as I'll illustrate in a moment.

In contrast, some debuggers work with the script's instructions in a data structure that resembles an image file. Since we cannot easily stretch the bit array in an image file, the debugger instead saves a copy of the statement that will execute after the breakpoint (i.e., line 24 in our example) and then overwrites this statement in the image file with the breakpoint statement.[^fn4] If the user wants to continue the script's execution, the debugger will first replace the breakpoint statement with the saved statement and then continue execution.

## Indenting that statement

Using `list.insert`, it's pretty easy to insert our breakpoint statement. We simply must remember that indexing starts at `0` while the user's counting of the lines in the script-to-be-debugged starts at `1`. We took care of this conversion immediately after grabbing the line number from the user (line 19 in `pdb1.py`). No off-by-one errors in our script, please!

The following inserts a breakpoint statement into `edited_script` at the correct index (e.g., between the original lines 23 and 24 in `guess32.py`):

`edited_script.insert(breakpt_index, 'raise Exception("My breakpoint")\n')`

But there's a problem with our new breakpoint statement. Do you see it? What indentation did we give this new statement? Remember that indentation in a Python script holds meaning. What indentation do we want for this new statement?

If you think about this, you'll realize that it is the indentation of the line we're replacing (e.g., line 24 in our example). We need to grab the whitespace at the start of this line and add it to the start of our new statement. We could do this using a regular expression, but this work is simple enough that we'll just build a loop with an if-else-statement. The next code block is an updated version of the function `my_pdb` that calculates the whitespace we need and then inserts the correctly indented breakpoint statement.

```{code-block} python
---
lineno-start: 1
---
### chap15/pdb2.py
import sys
import string

def my_pdb(script_fname):
    ''' First attempt at our debugger
        Input:   filename of script to be debugged
        Returns: edited script as list of lines where
                 each line ends in a newline character
    '''
    
    # Read in the original script (as a list of file lines)
    with open(script_fname) as fin:
        edited_script = fin.readlines()
    
    # Grab line number where we'll place the breakpoint
    breakpt = get_lineno(len(edited_script))
    
    # Convert breakpoint number into a list index
    breakpt_index = breakpt - 1
    
    # Add the breakpoint
    # .. grab the whitespace so we get the right indentation
    my_whitespace = ''
    for c in edited_script[breakpt_index]:
        if c in string.whitespace:
            my_whitespace += c
        else:
            break
    
    breakpt_statement = my_whitespace + \
        f'raise Exception("My breakpoint", {breakpt})\n'
    
    # .. insert a raise statement as our breakpoint
    edited_script.insert(breakpt_index, breakpt_statement)
    
    return edited_script

# the other functions and main are unchanged from pdb1.py
```

You may have noticed that I added an additional argument to the `Exception` object created on line 32, which we'll use in a moment. The first argument is our name for the exception and the second is the line number where we placed the breakpoint. You can add as many arguments as you like when you create an `Exception` object. We could also create our own exception class that derives from `Exception`, but the current approach is good enough for learning about debugging.

```{admonition} You Try It
Run `pdb2.py` on `guess32.py` adding a breakpoint at line 24. Look at the output file (`guess32-db.py`) and make sure that it inserted the breakpoint statement correctly. Go ahead and run `guess32-db.py`.

You might even try commenting out lines 25-29 in `pdb2.py`, which calculate the correct amount of whitespace needed at the start of the breakpoint statement. Then rebuild `guess32-db.py` using this new version of `pdb2.py`, and run the `guess32-db.py` it produces to see how the error message changes.

Don't forget to restore lines 25-29 in `pdb2.py` before reading on!
```

## Launching a script from another

We have edited our script-to-be-debugged, and we've seen its behavior has changed (i.e., it now raises our breakpoint exception). Our next task is to launch this edited script under the control of our debugger and have it catch the breakpoint exception. We know how to catch exceptions using a try-except-statement, but how do we start the execution of the edited script from within our debugger?

It's quite simple. Python includes a built-in function that allows you to execute a string as if it were a Python script. It's called `exec`, and we can experiment with it using the function strings from Chapter 14.

```{code-block} python
---
lineno-start: 1
---
hello_world = '''world = "world"
print("Hello", world)
'''

exec(hello_world)
```

```{code-block} python
---
lineno-start: 1
---
hello_str = '''def hello(s):
    print("Hello", s)
'''

exec(hello_str)
```

```{admonition} You Try It
Looking at the first code block above:

1.   Type the definition of `hello_world` in the interactive Python interpreter (i.e., copy lines 1-3 and paste them at the interactive Python interpreter's prompt). You created a string named `hello_world` that contains two Python statements within it.
2.   Next type `exec(hello_world)` at the interpreter's prompt and hit return. The interpreter will run the statements in `hello_world` and print what we expect.
3.   Finally type `world` at the interpreter's prompt and verify that the definition of this name is in the global namespace, just as if we had written `world = "world"` at the interpreter's prompt.

Now focus on the second code block above:

1.   Type the definition of `hello_str` (i.e., copy lines 1-3 and paste them at the interactive Python interpreter's prompt).
2.   Next type `exec(hello_str)` at the interpreter's prompt and hit return. The interpreter will run the statements in `hello_str` and print nothing. It prints nothing because the statements in `hello_str` only define the function `hello`.
3.   To verify this, type `hello("there")` at the interpreter's prompt and hit return. It runs because `hello` is in the global namespace.

Continue experimenting with other examples.
```

Let's use this new knowledge to launch an edited script within our debugger. This requires us to call `exec` with an argument that is the string that represents the edited script. The next code block rewrites `main` in `pdb2.py` (the function `my_pdb` is unchanged) to remove the writing of `edited_script` to a file and instead to call `exec` within a try-except-statement. 

```{code-block} python
---
lineno-start: 62
---
### chap15/pdb3.py
import sys

# my_pdb is unchanged from pdb2.py

def main():
    # Ask for the name of the script to be debugged, unless already provided
    if len(sys.argv) == 1:
        fname = input('What script would you like to debug? ')
    elif len(sys.argv) == 2:
        fname = sys.argv[1]
    else:
        sys.exit('Usage: python3 pdb3.py [script_to_be_debugged]')
    
    edited_script = my_pdb(fname)
    
    # print_it(edited_script)
    # write_it(edited_script, fname)
    
    # Run the edited script and catch our breakpoint exception.
    # Remember to turn the list of strings into one big string.
    try:
        exec(''.join(edited_script), globals())
    except Exception as msg:
        if msg.args[0] == 'My breakpoint':
            print(f'pdb3: Breakpoint at line {msg.args[1]}')
        else:
            print('pdb3: script died with non-breakpoint exception')
            print(msg)
```

```{tip}
What I've done on line 84 is an often-used trick in Python scripts to convert a list of many strings into a single continuous string. It uses the handy string method `join`. The string object on which you invoke `join` (i.e., the empty string in my example) is inserted between each element of the iterable that is the parameter to `join` (i.e., `edited_script` in my example).
```

It's worth pointing out two other interesting aspects of the try-except-statement and the `exec` call used in this code block:

1. In an except clause (line 85), we can declare a variable after the exception name (inserting the keyword `as` in-between). This variable (`msg` on line 85) is another name for the `Exception` object that was caught, and with it, we can access the arguments we specified when creating the `Exception` object. We access them using the `Exception` object's `args` attribute (see lines 86-87), which acts like a list.
2. In the `exec` call on line 84, we explicitly defined one of its optional parameters, which is the dictionary where global names should be placed. We want this to be the same dictionary as our current global namespace, which we can access with the built-in function `globals`. We need this extra argument here and not in our earlier examples because there might be an import-statement defined in the script we wish to debug, as there is in `guess32.py`. If you remove this argument from the `exec` call, you'll see that the debugging run of `guess32.py` dies saying that the name `random` is not defined (i.e., this module was imported into the wrong namespace).

```{admonition} You Try It
Run `pdb3.py` on `guess32.py` adding a breakpoint at line 24. After you input your guess at the prompt, the execution of `guess32.py` will hit our inserted breakpoint, which gives execution control back to our debugging script. That's how `pdb3.py` is able to print the message that a breakpoint occurred at line 24. 
```

While `pdb3.py` isn't yet a useful debugger, I hope that you now see that a debugger is nothing more than a program that reads a script as data, modifies it as it would any other data file, and then tells the machine to treat this data as code. And by launching the script within a try-except-statement, the debugger can regain control of the machine on a breakpoint or an error inside the script. First mystery solved!

```{admonition} You Try It
This is as far as we'll go in implementing a debugger, which means that you're now ready to read the documentation about [the actual Python Debugger](https://docs.python.org/3/library/pdb.html) called `pdb`. I recommend invoking it from the command line as in `python3 -m pdb guess32.py`, which starts the debugger and loads `guess32.py` into it. Once loaded, try the following debugging commands in order: 

*   `b 17`, which sets a breakpoint at line 17;
*   `r`, which runs the program from its current stopped point until it hits a breakpoint, an exception, or the script's execution ends;
*   `n`, which runs just the statement on which the debugger is currently stopped;
*   `p secret`, which prints the value of the name `secret`; and
*   `r`, at which point you can use your knowledge of the program's secret to quickly win the game. 

Three other helpful `pdb` commands are:

*   `q` to quit `pdb`; and
*   `h` to ask for a listing of the debugging commands you can run; and
*   `h command`, which describes what the specified command does.
```

## Instrumenting a script

Since we don't want to reimplement what we already have in the Python debugger `pdb`, let's instead get a feel for accessing a script's runtime state by modifying `pdb3.py` so that it *instruments* a script. What does this mean? Basically, we'll build a script that allows us to specify:

1. Where to put print-statements in a script; and
2. What we want those print-statements to say.

We've been doing this by hand and messing up our scripts along the way. An instrumentation tool could be a much cleaner approach. And it is trivial to do now that we have `pdb3.py`. We simply:

1. Replace our breakpoint statement with a print-statement; and
2. Have the user specify what they'd like printed.

The code in `pin32.py` below should look very similar to the code in `pdb3.py`. I've renamed the function `my_pdb` to be `my_pin`, and I've added a parameter to it, which I'll explain in a moment. I've also stripped away the try-except-statement that was surrounding the `exec` call in `main`. Since we're adding a print-statement rather than raise-statement, there's no longer any need to catch exceptions.

```{code-block} python
---
lineno-start: 1
---
### chap15/pin32.py
import sys
import string

def insert_print(edited_script, breakpt_index, ws):
    '''Inserts a user-defined print statement in `edited_script`,
       which is a list of Python statements, at the index
       specified, and with the indentation from `ws`.
    '''
    # Ask for the print-statement argument
    printarg = input("What is print's argument? ")
    
    print_statement = ws + f'print(f"{printarg}")\n'
    
    # Insert our print statement
    edited_script.insert(breakpt_index, print_statement)

def my_pin(script_fname, insert_code):
    '''Adds code at a user-specified line number.
        Inputs:  filename of script to be debugged
                 function responsible for inserted code
        Returns: edited script as list of lines where
                 each line ends in a newline character
    '''
    
    # Read in the original script (as a list of file lines)
    with open(script_fname) as fin:
        edited_script = fin.readlines()
    
    # Grab line number where we'll place some new code
    breakpt = get_lineno(len(edited_script))
    
    # Convert breakpoint number into a list index
    breakpt_index = breakpt - 1
    
    # Compute the leading whitespace at `breakpt_index`
    my_whitespace = ''
    for c in edited_script[breakpt_index]:
        if c in string.whitespace:
            my_whitespace += c
        else:
            break
    
    insert_code(edited_script, breakpt_index, my_whitespace)
    
    return edited_script

# get_lineno, print_it, and write_it are unchanged from pdb3.py

def main():
    # Ask for the name of the script to be debugged, unless already provided
    if len(sys.argv) == 1:
        fname = input('What script would you like to instrument? ')
    elif len(sys.argv) == 2:
        fname = sys.argv[1]
    else:
        sys.exit('Usage: python3 pin32.py [script_to_be_instrumented]')
    
    edited_script = my_pin(fname, insert_print)
    
    # Run the instrumented script
    exec(''.join(edited_script), globals())
    
    print('pin32: All done!')

if __name__ == '__main__':
    main()
```

The function `my_pin` is nearly an exact duplicate of `my_pdb`; the only two differences between them are: (1) the extra parameter `insert_code`; and (2) the lines 31-35 in `pdb3.py` that became a call to `insert_code` on line 44 in `pin32.py`. The point of these two changes is to remove what we want inserted from the work to set up the insertion. The function `my_pin` sets up for an insertion of new Python statements, and the passed-in function `insert_code` is responsible for inserting whatever new statements we want in the edited script.

If you look at the function `insert_print` in `pin32.py`, you'll see that I no longer insert a statement that raises an exception. This function inserts a print-statement whose argument is defined by the user of our tool. I've factored the code like this because an instrumentation function (`my_pin`) is a very useful tool, and we'll play with a different `insert_code` function in a moment. First, however, try `pin32.py`.

```{code-block} none
---
emphasize-lines: 1
---
chap15$ python3 pin32.py guess32.py
Line number in script? 24
What is print's argument? The secret is {secret}
## Welcome to GUESS THE NUMBER! ##
Please input your guess: 33
The secret is 61
Too small!
Please input your guess: 61
The secret is 61
Exactly! You win!
pin32: All done!
```

To be even more efficient in my cheating, I could have stuck the print-statement right after `guess32.py` generated the secret (and before I was asked for my guess), i.e., line 18. Cheating isn't our goal, but I hope you see that we could insert a debugging print using `pin32.py` anywhere in any script. No longer do we have to edit our scripts by hand to insert debugging prints!

I hope you also notice that `pin32.py` regained control after the exec-statement (i.e., the transcript includes the "pin32: All done!"). This means that when an `exec` statement completes, execution continues in the script that called `exec`. It is a blocking call, which is a type of call we first discussed at the end of Chapter 4.

```{admonition} You Try It
If you're so inspired, you might extend the functionality of `pin32`. Try, for example, wrapping line 85 (`main`'s call to `my_pin`) in a loop that allows you to add multiple instrumentation points into a target script. Make sure you translate the user-inputted line numbers into the correct index in the `edited_script` list as you process instrumentation points beyond the first! 
```

## REPL

Let's take this line of thought one step farther, which will help you to understand the acronym REPL that you might have heard mentioned when people talk about the interactive Python interpreter. REPL stands for *Read-Eval-Print Loop*. You should think of this loop as: (1) read a string that represents an expression; (2) evaluate that expression; (3) print the result of the evaluation; and (4) loop back to read the next expression. 

This loop is essentially what the Python interpreter does. It reads the first line in your script, which it then evaluates. The evaluation might update some variables' values in the current namespace, insert some new names, or cause a *side effect*, which is the technical term for file input/output or printing to the terminal. Together these roughly constitute the "print" portion of the acronym REPL. Finally, the interpreter moves on and reads the next line in your script (i.e., it loops to start the steps all over again).

The "Eval" and "Print" steps in the Python interpreter's implementation of this loop is exposed to you as a Python programmer through the built-in function `eval`. It differs from `exec` in that it expects its string argument to be an expression. If you pass `eval` a Python statement, it will raise an exception.

```{admonition} You Try It
Start the interactive Python interpreter, type `eval('3+4')`, and hit return. It should print `7`. Remember that the parameter to `eval` must be a string!

Next, change the string parameter to `'print(3+4)'` and hit return on this `eval` statement. Again, it should print `7`. Recall that a function call is an expression we can evaluate.
```

In `pin32.py`, we allowed the user to interrupt a script at some line number and print the value of some part of the script's runtime state. But if we changed this single, inserted print-statement into a read-eval-print loop, the user of our tool could query as much of the script's runtime state as they wanted before allowing the program to continue. The function `insert_repl` in `repl32.py` does exactly this:

```{code-block} python
---
lineno-start: 1
---
### chap15/repl32.py
import sys
from pin32 import my_pin

def insert_repl(edited_script, breakpt, ws):
    """ Insert code to create a read-eval-print loop

        while True:
            p = input("repl> ")
            if p == "#q":
                break
            print(eval(p))
    """
    s = ws + 'while True:\n'
    edited_script.insert(breakpt, s)
    s = ws + ' '*4 + 'p = input("repl> ")\n'
    edited_script.insert(breakpt + 1, s)
    s = ws + ' '*4 + 'if p == "#q":\n'
    edited_script.insert(breakpt + 2, s)
    s = ws + ' '*8 + 'break\n'
    edited_script.insert(breakpt + 3, s)
    s = ws + ' '*4 + 'print(eval(p))\n'
    edited_script.insert(breakpt + 4, s)
```

The `repl32.py` script uses the `my_pin` function from `pin32.py` to handle the boilerplate work involved in instrumenting a script. It just provides the function (i.e., `insert_repl`) that specializes the work of the `my_pin` instrumentation routine, and `repl32.py` starts all this work with the call:

`edited_script = my_pin(script, insert_repl)`

```{admonition} You Try It
Run `python3 repl32.py guess32.py` and answer `24` to the line-number question. Input a guess and then at the `repl>` prompt, type `3 + 4`. At the next prompt, type `secret` or `guess`. You might also type something like `secret == guess`. When you're done, type `#q` at the `repl>` prompt to continue the execution of `guess32.py`. Feel free to experiment with your own target script.
```

We've built ourselves a powerful and flexible tool with which we can investigate the state of a running program. We've also built ourselves an interpreter than uses a script's interrupted execution context as its environment. Including the real Python debugger `pdb`, you now have several tools with which you can slow down a program's execution so that you can better understand its behavior and more quickly fix the bugs within it. Happy bug squashing!

[^fn1]: Brian W. Kernighan and P.J. Plauger, *The Elements of Programming Style* (New York, NY: McGraw-Hill, 1978, 2nd edition), 10.

[^fn2]: Microsoft Visual Studio Code (https://code.visualstudio.com/docs/editor/debugging) and Replit (https://docs.replit.com/replit-workspace/workspace-features/debugging) have built-in support for debugging. Python ships with a standalone interactive debugging tool called pdb (https://docs.python.org/3/library/pdb.html). Google Colab uses an IPython-enhanced version of pdb (https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/01.06-Errors-and-Debugging.ipynb), although the interface is not very user-friendly, in my humble opinion.

[^fn3]: If you compare the script from Chapter 5 with the one in the \`chap15\` directory, you'll see that they're not exactly the same. In this chapter's version, I created a function \`grab\_guess\` that encapsulates the work we need to do to grab and validate the user's input. This allows you to place a breakpoint in \`grab\_guess\` and learn about how you can move up and down the call stack in an actual debugger.

[^fn4]: While I made it sound like this operation is a one-for-one replacement, this isn't always the case. In general, you need to remove and save enough of the original program to fit the encoding of a breakpoint exception.