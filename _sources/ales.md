# Active-Learning Exercises

## ALE 1.1: Hello World

Using your chosen IDE, follow the steps below to practice with Python commands you learned in Chapter 1 and to become more comfortable creating named Python objects.

**Step 1.** In your IDE, make a new Python project.

**Step 2.** In the editor pane, write a script that prints `Hello World` using only the `print` command. Run your script to make sure it executes correctly.

**Step 3.** Now change your script so that it assigns the string literal `'Hello World'` to the variable `exclamation`, and then have your script print `Hello World` using this variable. Run this updated script to make sure it executes correctly.

**Step 4.** Change your script again. Assign the string literal `'World'` to the variable `what`, and have your script use two print statements to print `Hello World`. It's okay if `World` is on its own line, but one of these print statements must use the variable `what`. Run your script and fix any issues that arise.

**Step 5.** Replace both print statements in your script with the single statement: `print("Hello", what)`. Run the script.

1. Look carefully at the output and compare it to the two objects you gave as inputs to `print`.
2. In the console pane, type `help(print)` and hit return -- remember that this is how you invoke the interactive Python interpreter. Consider the explanation. It should help you understand why `print("Hello", what)` includes a space between the two words even though there was no space in either of the string literals passed as inputs to the print command. If not, ask a member of the teaching staff.

**Congratulations!** You are over the first big hump: You've started using a tool that will help you think computationally and will give you the freedom to solve problems creatively. We just looked at four different ways to print the same short phrase, and while not terribly creative, you will learn many more ways to format what you want to print and will learn to solve much more complex problems.

If you're interested in the history and lore behind the tradition of printing "Hello World" in your first program, please see [this wikipedia page](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program).

## ALE 1.2: Too long?

Let's solve a new problem that continues to use the context of reading bedtime stories.

My children loved the stories we read, and they would often grab another book immediately after we finished the current one. As night fell, I'd begin to consider the length of each book they brought me. If it was late, I wouldn't start a long book.

So let's write a script that makes this decision for us. It will count the number of lines in a book and then print "Let's read this" if the book's length is less than 20 lines and "We can't read this" if it isn't.

To get started solving this problem, let's practice Step 5 in our problem-solving process, which states that we should identify old scripts that can help us in writing our new one. Obviously, doing this can save us work, but more generally, this approach avoids the fears many of us have when we start a project by staring at a blank page.

**Step 1.** In your IDE, make a new Python project and paste into the editor pane the script we completed at the end of Chapter 1.

**Step 2.** Think about which statements in this old script you need to solve this new problem. For example, do you need to ask the user for the name of the book to be read? Yup, that would be helpful, and so we'll keep that `input` command and the `open` command that uses its result. Do we need to loop through all the lines in the open file? Yup, keep that while-loop.

Skipping ahead, do we need to print the lines we read from the open file? No, we don't. So we can delete that line.

Consider each statement in our previous script and ask whether you need it or not, leaving alone those lines that are helpful and deleting those that aren't. What's left is a frame into which you can write new pseudocode. Bam! You're well on your way to solving this new problem.

**Step 3.** To count the number of lines in our input file, we're going to want to keep track of the number of lines we've seen in previous iterations of the while-loop. What do you want to call this variable? It's a variable because the object it names changes from one iteration of the loop to the next.

**Step 4.** What happens to this variable inside the while-loop? Can you write Python code that implements your answer to this question?

**Step 5.** Does this variable need to be initialized before the while-loop? If yes, what value should you use to initialize it? HINT: It does, and the value you use to initialize it depends on the where in the while-loop you update it. Think about the proper initialization value for an update at each possible position in the while-loop body.

**Step 6.** When our scripts sees EOF returned from `readline`, it breaks out of the while-loop. Previously, there was nothing after this while-loop, and the script ended. Now, however, we want to test the value computed across the iterations of the while-loop and then print one of two phrases. This sounds like a job for an if-statement. But to this point, we have only used if-statements to *protect* a block of code. In this problem, we want the test in our if-statement to *select* one of two blocks of code. The syntax for this version of an if-statement looks like this:

```{code-block} python
---
lineno-start: 1
---
if the_test:
    # statements done on a True test outcome
else:
    # statements done on a False test outcome
```

**Step 7.** Write and test the complete script!

## ALE 1.3: Close the screen door!

Create yourself yet another Python project, and again copy in the entire script from the end of Chapter 1. For this exercise, we're not going to change its functionality, but simply make it more *robust*.

**Step 1.** A piece of this work involves creating a separate directory for its input files. By putting all the input text files in a subdirectory called `txts`, we'll avoid mistakenly overwriting one of our scripts while performing file operations on the input texts. Look at the changes to the `open` parameter in the second line of the following script:

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

while True:
    the_line = my_open_book.readline()
    print(the_line, end='')
    if the_line == '':
        # We've read the entire book!
        print("\nThe End.")
        break
```

We will learn more about this syntax in Chapter 2, but briefly, it treats the infix `+` operator as string concatenation when the `+` is placed between two string objects.

Update the script in your editor pane so that it matches the code above. Make sure you create the `txts` subdirectory in your IDE file browser and move your text files into that new subdirectory before you try running this new script.

**Step 2.** Now add a new statement to the end of your script, as illustrated in the following code:

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

while True:
    the_line = my_open_book.readline()
    print(the_line, end='')
    if the_line == '':
        # We've read the entire book!
        print("\nThe End.")
        break

my_open_book.close()
```

Adding this `close` statement doesn't add a new feature to our script.  Run this script to verify this claim. The purpose of this `close` statement is to tell the interpreter that we're done with the open file and it can close it.

You should always pair a `close` for every `open` you write, but it is often hard to remember to do this. When I first started programming, I often forgot this pairing, and now I use a story from my childhood to help me remember.

When I was a small boy, my parents would constantly remind me to close the screen door. Why did they do that? Yes, they wanted me to keep the bugs out of the house. We won't go, at this time, into the details of the bugs/errors that can occur if you don't close a file when you're done with it, but suffice it to say if you ever open a file, you should close it.

It's perfectly fine to do this at the end of your script, as I've done here, since the interpreter stops executing your script when it reaches the script's end. Putting this `close` command right before the end of the script ensures that closing the file is one of the last things the interpreter does for us before stopping execution.

**Step 3.** What if I wanted to close the file earlier in the script? I could do that, but if I'm making as many changes to the script as we made last time, I might mistakenly move the `close` to some point before I'm actually done reading the file. For example, let's move the `close` statement into the loop and execute the script again.

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

while True:
    the_line = my_open_book.readline()
    print(the_line, end='')
    if the_line == '':
        # We've read the entire book!
        print("\nThe End.")
        break

    my_open_book.close()
```

Yuk. We got a `ValueError: I/O operation on closed file.`

Notice that there's very little difference between the first and second pieces of code. Basically, four little spaces. This is a nightmare waiting to happen.

To avoid such nightmares, Python provides us with a piece of syntactic sugar that lets us automatically include a `close` with each `open`. It accomplishes this by turning the `open` into a `with-as` [compound statement](https://docs.python.org/3/reference/compound_stmts.html).

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()
        print(the_line, end='')
        if the_line == '':
            # We've read the entire book!
            print("\nThe End.")
            break
```

The block of code nested inside the `with-as` statement does the `open` and assigns our virtual finger to the name `my_open_book` just as before, but it also recognizes when control leaves this nested code block and automatically calls `close` on `my_open_book` for us. I wish I had something like this when I was a kid. 

This is one example of a way that designers include new language features that help you to avoid common errors and problems (i.e., errors and problems that repeatedly haunted programmers in older languages). Of course, you're protected only if you use these features! I encourage you to use this form for your own scripts.

Update the code in your editor pane and run it. Everything should continue to work as before, but your code is more robust!

## ALE 1.4: From story to contract

Let's turn an input story into a legal contract by numbering each paragraph, where a new paragraph is defined to start on a line of text that follows a blank line. Here is a specification for your solution script:

* If you look at a legal contract, you'll typically see that every paragraph in the document is numbered so that it is easy to refer to specific paragraphs in the negotiations surrounding the contract. The first paragraph in the contract is given the number 1, and the numbering continues sequentially until the last paragraph in the body.
* To make things simple, your script should print the string `'PARAGRAPH 1\n'` when numbering the first paragraph in the text. In other words, `PARAGRAPH 1` appears on its own line and then is followed by the first text line of the first paragraph. Follow this pattern for the rest of the paragraphs in the story.
* Your solution script will need to recognize blank lines (i.e., lines that contain only a newline character). 
* Make sure that your script uses the `input` function and `with-as` statement. HINT: What code have you written that you might want to use as your starting point for this problem?

The paragraphs in `CatInTheHat.txt` are separated by blank lines, and so you can use this input to test your script.

Good luck!

\[Version 20230709\]

## ALE 2.1: Counting stuff

The solutions to many problems employ sequence and string processing. We learned that Python strings are a type of Python sequence (i.e., an ordered collection of items, and for strings, the items are characters). In the first several exercises, you'll gain experience with the methods on sequences and on strings, which will also prepare you for the second programming problem set.

Exercises 1-5 use the following code (`chap02/count32.py`), which runs but doesn't count anything correctly.

```{code-block} python
---
lineno-start: 1
---
### chap02/count32.py

# Grab the text blurb to process
import sys
#print("sys.argv =", sys.argv)
if (len(sys.argv) == 1):
    blurb = input('Text: ')
elif (len(sys.argv) == 2):
    input_file = sys.argv[1]
    with open('txts/' + input_file) as my_open_file:
        blurb = my_open_file.read()
else:
    sys.exit("Usage: python3 count32.py [blurb.txt]")

# Count the number of non-blank lines in the blurb
# using the Python string method `split`.
lines = 0
# REPLACE ME with a for-loop
print(lines, "line(s)")

# Count the number of alphabet characters in the blurb
# using the Python string method `isalpha`.
letters = 0
# REPLACE ME with a for-loop
print(letters, "letter(s)")

# Count the number of words in the blurb, where a word
# is any sequence of non-whitespace characters surrounded
# by any amount of whitespace. Please use the Python string
# method `split` and one function on sequences.
words = 0  # REPLACE THE 0 with an expression
print(words, "word(s)")
```

Notice the four blocks of code in `count32.py` separated by blank lines. The first block is complete and working code; you deal with the commented-out print statement in a moment.

The other three blocks compute counts of things in the `blurb` produced by the first code block. What each counts is described by the variable in which they store their counts: `lines`, `letters`, and `words`. Or I should say that they will count these things after you finish your work in the next three exercises. Currently, the script simply sets these counts to zero.

This exercise explains what the first code block in `count32.py` does. You can ignore the `import` statement, which is discussed in an upcoming chapter. After this import-statement and the commented-out print-statement, there is an if-statement checking the length of the variable `sys.argv`, which we can do because `sys.argv` is a sequence.

```{margin} Running in the Shell
On the webpage describing how to get started with your chosen IDE, there are complete instructions on how to run Python scripts in the IDE's Shell panel.
```

**Step 1.** Uncomment the print-statement, and run `count32.py` in your IDE's shell as follows:

`python3 count32.py HomeView3.txt`

It should print:

`sys.argv = ['count32.py', 'HomeView3.txt']`

`0 line(s)`

`0 letter(s)`

`0 word(s)`

That square bracket notation is how Python prints a sequence. As you can see, `sys.argv` contains the "words" on the command line after `python3`. The Shell defines a word as any sequence of non-whitespace characters surrounded by any amount of whitespace, where whitespace is any space, tab, or return characters (i.e., invisible characters that represent horizontal or vertical space).

Notice that `count32.py` expects to find the input files specified on the command line in a folder called `txts`. This folder should be visible at the same level of the file explorer as the `count32.py` script.

**Step 2.** Re-comment the print-statement. And now let's look at the `if`, `elif`, and `else` statements.

In the chapter, we learned that the else-statement captures all the cases where the condition in the proceeding if-statement evaluated to `False`. An elif-statement stands for "else if", and it generalizes the selection of which code block to run based on the evaluation of a sequence of conditions.

For instance, in `count32.py`, the first code block wants to know if the number of words in `sys.argv` is 1, 2, or something other than 1 or 2. The if-statement asks whether `sys.argv` has length 1. If not, the interpreter skips to the elif-statement and checks whether `sys.argv` has length 2. If not, the interpreter does the work in the else-statement. If either of the first two checks had evaluated to `True`, then the statements dependent upon that condition (and not any of the others) would have been executed.

The following is another way of looking at the if-elif-else block of statements:

1. The if-statement condition is `True` when you don't give the script an input parameter. It assumes you want to provide a `blurb` of text by being prompted with an input-statement. This is useful for testing the script with short blurbs.
2. The elif-statement condition is `True` when you give the script a single command-line parameter, which the script assumes is the name of the file whose contents you want to become the `blurb`.
3. The else-statement executes when you give our script more than one command-line parameter. This is an error, and the script exits after telling you how to properly use it.

If you wanted to select between four independent conditions, you'd insert another elif-statement either before or after the current one. In general, you can add as many elif-statements as you need between the first if-statement and the optional final else-statement.

Let's try this. Modify the first code block in `count32.py` to set blurb to the concatenation of the last two string elements of `sys.argv` when `len(sys.argv) == 3`. You should separate these two "words" with a space.

**Bonus information.** Look again at the body of the original elif-statement. A lot of this should look familiar, but if you look closely, you'll see that we use the method `read` and not `readline`. The `read` method reads all of the lines in the file and names the resulting (possibly very long) string `blurb`.

If you want to verify this, print `blurb` after the completion of the first code block.

**More bonus information.** Look at the body of the else-statement. To this point, we have only ever exited our scripts by having the Python interpreter run off the bottom of the script. If you ever need to exit your script without running off the bottom of it, you should call `sys.exit()`. You might read about other methods to end the execution of your Python script, but I recommend that you use this approach. It safely cleans up the execution state of your script so you don't encounter any strangeness. You probably feel everything is strange enough already!

## ALE 2.2: Counting lines

Let's count things! We'll start with another way to count the lines in a file, or in our case now `blurb`. To make this more interesting, we'll make `count32.py` count non-blank lines. What is a blank line? A line containing only a `\n` character.

You are going to replace the first comment that says `# REPLACE ME with a for-loop` with a for-loop. Ideally, the sequence in this for-loop would be an ordered collection of the lines in the blurb.

How do we turn `blurb` into a sequence where each element is a line in the blurb? We will use the Python string `split` method. If the blurb was `'1,22,333'` and I asked the Python interpreter to give me the result of `blurb.split(',')`, it would return `['1', '22', '333']`. The interpreter found each comma in the blurb and made the strings to the left and right of each comma into elements in the resulting sequence. Notice that the commas do not appear in the resulting list of strings.

**Step 1.** Use the interactive Python interpreter to try this and other examples. Make sure you try examples with a blank line!

**Step 2.** Update `count32.py` so that it counts the non-blank lines in `blurb`. What character do you want to split on? What condition will you test before incrementing the variable `lines`?

## ALE 2.3: Counting characters

Next, we will count the number of English alphabet characters in `blurb`. This again has us practice writing a for-loop, but the for-loop sequence this time should contain the numbers from `0` to one less than the length of the blurb. Recall that the chapter used the `range` function to produce such a sequence.

You'll also need to know how to test to see if a character is from the set of characters in the English alphabet. You can do this in Python with the string method called `isalpha`, which takes no input parameters.

**Step 1.** Try `'b'.isalpha()` and `'a2c'[1].isalpha()` in the interactive Python interpreter. Make sure you can explain what takes place in the second of these examples. Try a few other examples.

**Step 2.** Update `count32.py` (i.e., replace the second `REPLACE ME` comment) so that it counts the English alphabet characters in `blurb`.

## ALE 2.4: Counting words

Now, let's count the number of words in `blurb`, where a word is defined like the Shell defined words above. Amazingly, we can do this in one short expression that uses `len` and `split`.

**Step 1.** Read [the Python documentation about the string split method](https://docs.python.org/3/library/stdtypes.html) (search the webpage for `str.split` and pay attention to what it does when you don't provide an input parameter to `split`). Try it on a few examples in the interactive Python interpreter.

**Step 2.** Update `count32.py` (i.e., replace the `REPLACE THE 0` comment) so that it counts the number of words in `blurb`.

## ALE 2.5: Counting sentences

Finally, let's try counting the number of sentences in `blurb`. What constitutes a sentence in English can be quite tricky to define. For this exercise, we will keep it simple: any sequence of characters and whitespace that ends in a `'.'`, `'?'`, or `'!'` is a sentence. This means that your script will incorrectly report that the string `'Mr. and Mrs. Smith left for vacation.'` contains three sentences. We will avoid this shortcoming by never passing your script any tricky input.

You may be tempted to dive right in and start writing some Python code. You might try to use the `split` method. A bit of advice, don't do either.

**Step 1.** Take a moment and think about the problem as stated. Think in pseudocode about what you need to do. **HINT:** Why does the string `'Mr. and Mrs. Smith left for vacation.'` contain three sentences? Is there [a method common to all sequences](https://docs.python.org/3/library/stdtypes.html#common-sequence-operations) that might help you simply solve this problem?

**Step 2.** Add a new code block to the end of `count32.py`, which counts the number of sentences in `blurb`.

To test your solution, force the variable blurb to each of the following string values by placing an assignment to blurb just before your new code block. These assignments use Python's ability to easily create multiline strings using triple quotes.

```{code-block} python
---
lineno-start: 1
---
blurb = '''This is a test. A test of what? A test
with every type of sentence in it!'''

# blurb = 'Mr. and Mrs. Smith left for vacation.'

# blurb = '''Phronsie still stood just where Polly left her.
# Two hundred candles! oh! what could it mean! She gazed up
# to the old beams overhead, and around the dingy walls, and
# to the old black stove, with the fire nearly out, and then
# over everything the kitchen contained, trying to think how
# it would seem.'''

print("Blurb:", blurb)

sentences = 0 # REPLACE THE 0 with an expression
print(sentence, "sentence(s)")
```

## ALE 2.6: Abstractions

Let's take some time to understand that [the set of common operations on sequences](https://docs.python.org/3/library/stdtypes.html#common-sequence-operations) is different than [the set of methods available on Python strings](https://docs.python.org/3/library/stdtypes.html#string-methods). Every string is a Python sequence, but not every Python sequence is a string (i.e., able to use the Python string methods). Explain to a friend (or member of the teaching staff) how this connects to the idea of abstraction.

## ALE 2.7: Problem not solved!

Although we made great progress in this chapter to create a Python script that converts a story into a theatrical script, we didn't completely solve the problem. While this was on purpose (i.e., we were practicing solving a few instances of the problem before writing a script to solve any instance), it is important to understand which cases of dialogue across file lines that the chapter's script does and doesn't handle. 

Let's review the test inputs that the chapter's last script (`chap02/script4.py`) does handle correctly:

* `Talkative1.txt`: This blurb of text has at most one double-quote character on a file line. And it contains only one line of dialogue.
* `Talkative2.txt`: This blurb of text also has at most one double-quote character on a file line, but it contains two lines of dialogue.
* `HomeView3.txt`: In this blurb, a line of dialogue can start and stop on a single file line, but there's at most two double-quote characters on a file line. It continues to handle multiple lines of dialogue in the input file.

What happens if a file line contains more than two double-quote characters? Let's try with a cleaned-up version of `script4.py` that we will call `script32.py`. If you look at `script32.py`, you'll notice that it:

* Takes the input filename from the command line to make it a bit easier to test the script with multiple different input files (i.e., it removes the need for the script to prompt the user for the input file).
* Cleans up the comments so that it is a bit easier to understand what the different blocks of statements do. 

**Step 1.** Run `script32.py` on `HomeView6.txt`. You should find that the first piece of dialogue was printed correctly, but the script fails to recognize the start of the second piece of dialogue in the first file line of `HomeView6.txt`. Do you see the reason why?

Don't read on until you have a reason.

If you answered that `script32.py` handles at most two double-quote characters on a file line and the first file line of `HomeView6.txt` contains three double quotes, you're absolutely correct! At some point in designing this script, we have to stop thinking about handling *a specific number* of double-quote characters on a file line and write a script that handles *any number* of double-quote characters on a file line.

Recall that we started Chapter 2 talking about the desire to make our scripts handle more than a single instance of a problem, and this is a terrific example of thinking in that generalized way.

**Step 2.** We are not going to code a solution here to the general problem of finding every piece of dialogue within and across file lines, but you will in the programming problem set at the end of Chapter 3. Instead, this exercise will help you to discover the pattern that you'll need to solve this general problem (i.e., understanding this pattern will help you to structure your code).

* Characterize the text before and after the double quote character on each of the following two file lines. Remember that our task is to find dialogue, and so your characterization should be related to this goal.

`I said, "Just one quote\n`

`per file line, please."\n` 

* Now characterize the text before and after each double quote character in this file line:

`"Just one?" you said.\n`

* And what about a file line with three double quote characters followed by one with a single double quote character:

`"But your line had two," I shouted. "And\n`

`that's not one, except for this file line."\n`

* Finally, characterize the text before and after the double quote characters in this line with four double quotes:

`"Wait!" and you shake your head. "Will this end?"\n`

These examples ask you to label the text on either side of a double-quote character as if you split the file line on this character. Did you spot the pattern that emerges? Understanding this pattern is the key to writing code that processes a file line with any number of double quotes in it!

\[Version 20230709\]

## ALE 3.1: Two kinds of loops

* Use while-loops when you want to loop until you hit a some exit condition.
* Use for-loops when you want to iterate over a sequence.

As a bit of practice, consider what the string-find method looks like behind the scenes:

```{code-block} python
---
lineno-start: 1
---
def find(the_string, pattern):
    for i in range(len(the_string)):
        if the_string[i] == pattern:
            return i
    else:
        return -1
```

Let's try it out, i.e., run the next code block.

```{code-block} python
---
lineno-start: 1
---
a_string = 'Cosmo the dog'

# a_string.find('m')
find(a_string, 'm')
```

**Step 1.** Look carefully at the function body of `find`. By reading the code, do you understand the purpose of the for-loop? Can you describe the conditions under which it returns?

Yes, you can put an `else` on a for-loop. It means "do the else-block if you exhausted the sequence without hitting an exit condition."

**Step 2.** Complete the last statement in the following code block so that it fails to find a `pattern` of your choosing in `a_string`.

```{code-block} python
---
lineno-start: 1
---
a_string = 'Cosmo the dog'

# a_string.find(...)
find( )
```

**Step 3.** Think about the connection between a for-loop and a while-loop. Can you create rewrite the `find` function we've been using so that it uses a while-loop instead of a for-loop?

```{code-block} python
---
lineno-start: 1
---
def find_w(the_string, pattern):
    # Put your code here
```

**Step 4.** Create a code block that tests your `find_w` function.

## ALE 3.2: Getting comfortable with \`range\`

What is that `range` command? We discussed it as a function that produces an integer sequence whose length is its input parameter. Used in a for-loop, the loop can then assign each element of the generated sequence to its loop variable (e.g., variable `i` in the example below) and execute the loop body for each element.

```{code-block} python
---
lineno-start: 1
---
for i in range(10):
    print(i)
```

Questions:

1. What is the first number printed? Why is that the default for the first value produced by \`range\`? What does it make easy? There is a way to specify the starting value of the range produced. Can you figure out how to change the starting value?
2. What is the difference between our input value to `range` and the final number printed? Why is this? When you change the starting value, what is the final value printed? What does the single parameter to `range` represent if it isn't the final value printed?

Feel free to change the code block above as you answer these questions!

## ALE 3.3: Return with something

Python functions always return a value. Whether you explicitly use a return-statement without an expression or end your function without a return statement, the Python interpreter will return the special object `None`.

Execute the following code block, which illustrates the ways we can end a function's execution.

```{code-block} python
---
lineno-start: 1
---
def my_f1():
    return None

def my_f2():
    i = 1

def my_f3():
    i = 1
    return 

def my_f4():
    i = 1
    return i + 2

print(my_f1(), my_f2(), my_f3(), my_f4())
```

Make sure you can explain why each function returns the value that was printed.

## ALE 3.4: Visualize a function call

Understanding the execution of a program with functions can be confusing at first, and sometimes it helps to watch a step-by-step visualization of a script's execution. We talked through the execution of `replace7.py` in the chapter, and now we'd like you to review this execution with the help of a nifty online tool called [Python Tutor](https://pythontutor.com/). 

I've used the site to configure a simplified version of our `replace7.py` script. By "simplified" I mean that I ripped out the code that read the text from a file and we greatly reduced the amount of input text. In a moment, you'll see that this simplified script still requires 324 steps to fully execute. After you start stepping around in this code, you'll begin to appreciate, even at a mere 324 steps, why we humans are glad to let computers keep track of the work in every step of our scripts. It's tedious work!

**Step 1.** [Click this link](https://pythontutor.com/visualize.html#code=def%20my_replace(s,%20old,%20new):%0A%20%20%20%20i%20=%200%20%20%20%20%20%20%20%20%20%20%20%23%20tracks%20where%20we%20are%20in%20the%20input%20string%0A%20%20%20%20j%20=%20len(old)%20%20%20%20%23%20skip-ahead%20amount%20for%20index%20calculations%0A%20%20%20%20new_s%20=%20s%5B0:0%5D%20%20%23%20the%20new%20string%20we're%20building%0A%0A%20%20%20%20while%20i%20%3C%20len(s):%0A%20%20%20%20%20%20%20%20if%20s%5Bi:i+j%5D%20==%20old:%0A%20%20%20%20%20%20%20%20%20%20%20%20new_s%20=%20new_s%20+%20new%0A%20%20%20%20%20%20%20%20%20%20%20%20i%20+=%20j%0A%20%20%20%20%20%20%20%20else:%0A%20%20%20%20%20%20%20%20%20%20%20%20new_s%20=%20new_s%20+%20s%5Bi:i+1%5D%0A%20%20%20%20%20%20%20%20%20%20%20%20i%20+=%201%0A%0A%20%20%20%20return%20new_s%0A%0Amy_open_book%20=%20%5B%0A%20%20%20%20'And%20...',%0A%20%20%20%20'The%20Cat%20in%20the%20Hat!',%0A%20%20%20%20'And%20...',%0A%20%20%20%20''%0A%20%20%20%20%5D%0Awhile%20True:%0A%20%20%20%20the_line%20=%20my_open_book.pop(0)%0A%0A%20%20%20%20%23%20Having%20some%20fun%20with%20text%20substitution%0A%20%20%20%20the_line%20=%20my_replace(the_line,%20'Cat',%20'%5CN%7Bcat%20face%20with%20wry%20smile%7D')%0A%20%20%20%20the_line%20=%20my_replace(the_line,%20'Hat',%20'%5CN%7Btop%20hat%7D')%0A%0A%20%20%20%20print(the_line)%0A%0A%20%20%20%20if%20the_line%20==%20'':%0A%20%20%20%20%20%20%20%20%23%20We've%20read%20the%20entire%20book!%0A%20%20%20%20%20%20%20%20print(%22%5CnThe%20End.%22)%0A%20%20%20%20%20%20%20%20break&cumulative=false&heapPrimitives=nevernest&mode=edit&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false). It will take you to Python Tutor's visualization page, which is preloaded with our simplified script. Scroll up and down in the leftmost panel and make sure you recognize the code. The variable `my_open_book` has become a list of text lines from which we pop one line at a time for the processing that takes place in our infinite loop. You should otherwise recognize all the code.

**Step 2.** Click the "Visualize Execution" button under the code. Then, click the "Next" button that should have appeared under the code. You may need to scroll up to see the location of the green and red arrows. Notice what the interpreter did on this first execution step. It added the name `my_replace` to the namespace of what's called the "Global frame". What `my_replace` names is a function object, as represented by the arrow in the rightmost panel. 

Keep clicking "Next" and watch the arrows. At step 6, we see another object placed in the global frame's namespace. At step 8, `the_line` appears and `my_open_book` loses its first element as a result of the `pop` operation. You can click "Prev" if you want to replay any steps.

Keep stepping and watch what happens at the call to our `my_replace` function. Make sure you understand why `s`, `old`, and `new` were initialized with the values you see in the frame named `my_replace`. Notice that the program is using this `my_replace` frame while it is executing inside the `my_replace` function. It won't touch the global frame again until it hits the `return` statement.

**Step 3.** What happens between steps 43 and 44?

That's right! The frame that was created for first execution of `my_replace` disappears. That's because we don't need it anymore after we're done with the first function call. The machine will create another frame as scratchpad space for its next invocation of `my_replace`. Given the way we've written our `my_replace` function, nothing done in one execution hangs around to affect any later executions. The only thing we care about from an execution of our function is the value we return to the code that called the function.

Go ahead and use the slider to move around in the code and see what happens at different parts of its execution. Feel free to edit the code (click the link below the script's listing) and then click "Visualize Execution" to return to the visualization mode. You can even put in your own example scripts. Just keep them short and self-contained (i.e., no file I/O)!

## ALE 3.5: Two different sequences

We've discussed two different types of objects that adhere to the Python sequence abstraction: strings and lists. The following code block defines one of each of these objects.

```{code-block} python
---
lineno-start: 1
---
# Two sequences
a_string = 'Cosmo the dog'
a_list = ['Cosmo', 'CatInTheHat.txt', True, 10]
```

**Step 1.** What does this mean that both of these objects adhere to the same abstraction? Here are a few examples that might help you come up with an answer to this question:

```{code-block} python
---
lineno-start: 1
---
# What is the length of a sequence?
c1 = len(a_string)
c2 = len(a_list)
print(c1, c2)
```

```{code-block} python
---
lineno-start: 1
---
# Getting an element in a sequence
e1 = a_string[3]
e2 = a_list[2]
print(e1, e2)
```

```{code-block} python
---
lineno-start: 1
---
# Slice out a piece of the sequence
s1 = a_string[6:9]
s2 = a_list[:2]
print(s1, s2)
```

```{code-block} python
---
lineno-start: 1
---
# Test if an object is a member of the sequence
print('m' in a_string)
print(0 in a_list)
```

**Step 2.** But strings are not lists and lists are not strings. In addition to sharing an abstraction, strings and lists have their own methods. We've used the string-find method, and we can see that it doesn't work on lists.

```{code-block} python
---
lineno-start: 1
---
i = a_string.find('m')
print(i)
```

```{code-block} python
---
lineno-start: 1
---
i = a_list.find(10)
print(i)
```

What is the equivalent to string-find with a list? Search the online documentation for this answer and try it out on `a_list` (i.e., fix the broken statement containing `a_list.find(10)`.

## ALE 3.6: What happens when?

Modules are a powerful way of building off code that others have written and published. However, it often isn't obvious what the `if __name__ == '__main__'` design pattern is doing for us. To better understand this pattern, consider these two silly scripts:

```{code-block} python
---
lineno-start: 1
---
### chap03/module32.py
print("module32.py: At start of script")

# Initialize a variable in this module
i = 42
print("module32.py: Initialized i")

def still42():
    return i == 42

def main():
    print("module32.py: At start of main")

print("module32.py: Before check of __name__ =", __name__)
if __name__ == '__main__':
    main()
```

```{code-block} python
---
lineno-start: 1
---
### chap03/import.py
print("import.py: At start of script")

import module32

def main():
    print("import.py: At start of main")

    print(f"i = {module32.i}; ")
    print(f"still42? {module32.still42()}")

print("import.py: Before check of __name__ =", __name__)
if __name__ == '__main__':
    main()
```

**Step 1.** Put these two scripts into a directory.

**Step 2.** In the Shell, run: `python3 import.py`

Look at the output and then at the code in `import.py` and `module32.py`. Make sure you understand how the Python interpreter moved through each of the files, which statements in each file it executed, and what the value of the special variable `__name__` was as the interpreter processed each file.

**Step 3.** In the Shell, run: `python3 module32.py`

Compare this output to the last, focusing on the lines that start with `module32.py`. Remember that modules in Python are just scripts that we import to get access to some functions and objects we don't want to write for ourselves!

## ALE 3.7: Imports and the namespace?

Let's use the scripts from ALE 3.6 to see what happens to the global namespace as we use two different forms of the import-statement. 

**Step 1.** In the same directory that you placed `import.py` and `module32.py`, start the interactive Python interpreter (i.e., run `python3` at the Shell prompt).

At the interactive Python interpreter prompt, run: `dir()`

This is a list of the names that the interpreter has defined before it reads any of your script.

Next, in the interactive interpreter run: `from module32 import still42`

You should recognize the printed output from the work you did in ALE 3.6. Notice that the import-statement doesn't just read a file, but it executes it too.

Finally, in the interactive interpreter, again run: `dir()`

Notice what has been added to the namespace. Is it what you expected given the form of the import-statement we used?

**Step 2.** Type `exit()` or the key combination *control-d* at the interactive interpreter's prompt. This quits the interpreter so that we can again start from a clean environment.

**Step 3.** Restart the interactive Python interpreter (i.e., run `python3` at the Shell prompt), and at the interactive Python interpreter prompt, verify that we have a clean environment by running: `dir()`

Next, run: `import module32`

Did the printed output change? It won't because the form of the import-statement doesn't affect the execution of the script; it only affects what gets put into the namespace.

Finally, in the interactive interpreter, again run: `dir()`

Notice what has been added to the namespace. Is it what you expected given the form of the import-statement we used? You might also run `dir(module32)` to see what's in that particular namespace.

**Step 4.** Quit the interactive Python interpreter as you're done!

## ALE 3.8: Scope and the Danger of Globals

Computer scientists talk *scope rules* when they explain how the interpreter knows which version of a name (i.e., a variable or function) you mean at some point in your script. Scope is a concept closely related to namespaces, which we've been discussing since the first chapter. I've put off a discussion of scope because it gets ... complicated. However, now that you know how to build your own functions, it is time to say a few words about scope.

When we started writing our scripts, we wrote statements without enclosing them in a function, as illustrated with this short script from Chapter 1:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
my_open_book = open('CatInTheHat.txt')
# Read the first line using that thing that you computed
the_line = my_open_book.readline()
print(the_line)
```

Since the names `my_open_book` and `the_line` do not reside within a function, they are names in Python's *global* scope. This was a fine thing to do when our scripts were very short. But as they grew longer, we've learned to chunk our work into sensible pieces and encapsulate those pieces in reusable functions.

With functions, we gained a new scope. Names like `i`, `j`, `old`, and `new` in Chapter 3's `my_replace` function are in Python's *local* scope, which is separate from Python's global scope.

```{code-block} python
---
lineno-start: 1
---
def my_replace(s, old, new):
    """Returns a string replacing all occurrences of old with new."""
    i = 0           # tracks where we are in the input string
    j = len(old)    # skip-ahead amount for index calculations
    new_s = s[0:0]  # the new string we're building

    while i < len(s):
        if s[i:i+j] == old:
            new_s = new_s + new
            i += j
        else:
            new_s = new_s + s[i:i+1]
            i += 1

    return new_s
```

What the difference? A local variable like `j` is available only within the enclosing function. It is not known outside of the body of this function. This is a Good Thing because any names we use within this function won't conflict with any names used within another function. Think about how hard it would be to write your script if you had to know all the names used by the authors of every module you imported!

Global variables, on the other hand, are names that are known anywhere in your module (i.e., anywhere in the file that holds your script). This includes inside any functions you define in that module. Global names within a module can be made global to any other module by importing them.

The following code block illustrates global and local names using an example without any import-statements.

```{code-block} python
---
lineno-start: 1
---
### chap03/scope.py
capitalize = True   # a global variable setting a mode for the script

def my_print(s):
    if capitalize:
        s = s.capitalize()
    print(s)

def my_scramble(s):
    new_s = s[1:] + s[0]
    return new_s

s = 'just some fun'
my_print(s)
print(my_scramble(s))
#print(new_s)  # produces a NameError
``` 

Notice that `capitalize` is defined outside of any function and is thus available within any of the functions (e.g., in `my_print`, where it tells us whether we should be capitalizing the first letter of the string we're about to print). Namespaces keep this global name from colliding with the method of the same name in the `str` object.

The name `s` is both a global and a local name. It is a global name when it is defined on line 13, and a local name inside both `my_print` and `my_scramble`, as formal parameters are local names. These local `s` names hide the global `s` within the two functions. By hide, I mean that you cannot access the global `s` inside these functions; Python assumes you mean the local name. Again, this defined behavior is typically what we want.

Finally, the name `new_s` inside `my_scramble` is a local variable. If you try to print the value of this variable by removing the first comment character on the script's last line, you'll get an error. The name is thrown away with the local scope of `my_scramble` as `my_scramble` returns. Go ahead and try it. 

**The danger.** As you begin writing functions, you'll find that you need to reference a value in the function that you haven't sent to the function as a parameter. What should you do?

1. You could add a new formal parameter and then change all the call sites to provide a value for that formal parameter.
2. Or, you'll think to yourself, "I could just put the value I need in the global scope and not have to change the function or any of the call sites."

Resist the urge to do the second thing except in one limited case, which I'll describe in a moment. But first, let's see what happens to our script if we remove the formal parameters---heck, the global variable is already named what the two function bodies reference, right?

```{code-block} python
---
lineno-start: 1
---
### chap03/scope_broken.py
capitalize = True   # a global variable setting a mode for the script

def my_print():
    if capitalize:
        s = s.capitalize()
    print(s)

def my_scramble():
    new_s = s[1:] + s[0]
    return new_s

s = 'just some fun'
my_print()
print(my_scramble())
``` 

The script dies with an `UnboundLocalError` on line 6, where we tried to update the global variable within the function `my_print`. Python gives you a way to fix this with a global-statement, but you're just heading down dark and bumpy road with more problems to come. For example, did you really want to update the global variable or just a local copy of it?

The only time in this book that you'll be encouraged to use a global variable is in situations like we saw with `capitalize`. Think about how we're using this name: We set it once at the start of the script and then only ever reference its value. We never try to change it. Global variables are great things to read, and a real pain to update. You'll feel this pain if you ever have to debug a global variable that you read and write all over your (probably long) script.

If you want to know more about scope and the scope rules in Python, I recommend [this Real Python tutorial](https://realpython.com/python-scope-legb-rule/).

\[Version 20230709\]

## ALE 4.1: This and that

Python allows you to assign multiple values to multiple names in one assignment-statement.  Here are the semantics:

* The lefthand side of the assignment operator must be a comma-separated list of names.
* The righthand side of the assignment operator can be
    * a comma-separated values, or
    * a value that adheres to the sequence abstraction.

The first value on the left is given the first name on the right; the second on the left is given the second on the right;  and so forth. For the operation to complete without an error, the comma-separated lists on both sides of the assignment operator must have the same number of elements. Similarly, the length of the sequence value on the right must be the same as the number of comma-separated names on the left.

This is not an operation you'd want to do all the time, since it can quickly become confusing to lump different things together in a single assignment statement. There are, however, two cases where this functionality is very helpful:

```{code-block} python
---
lineno-start: 1
emphasize-lines: 5
---
# CASE 1: Unpacking a sequence into its component pieces
# so that you can operate on each separately
answer = '16.83 million books are in the Harvard library'

num, magnitude, units = answer.split()[0:3]

print("num =", num)
print("magnitude =", magnitude)
print("units =", units)
```

```{code-block} python
---
lineno-start: 1
emphasize-lines: 5
---
# CASE 2: Swapping two values
x = 0
y = 1

x, y = y, x

print(f'x = {x}, y = {y}')
```

**Step 1.** Write an equivalent sequence of statements to implement the swapping of two values without using multiple assignment.

```{code-block} python
---
lineno-start: 1
---
# Swapping two values without multiple assignment
x = 0
y = 1

# YOUR CODE HERE

print(f'x = {x}, y = {y}')
```

 

**Step 2.** Look at the code in CASE 2 and the code that you wrote. Which is easier to understand?

## ALE 4.2: Tuples, a box for everything

A *tuple* is yet another kind of Python sequence.  They may look like a Python `list`, but they are immutable.

Be careful.  Tuples are always printed with surrounding parentheses, but you don't need parentheses to create a tuple. A comma is sufficient.

```{code-block} python
---
lineno-start: 1
---
t1 = 'is blue', 'Who changed the sky?!?'
print(t1)

t2 = 'is blue', 1
print(t2)

t3 = 0, 1
print(t3)

t4 = (0, 1)
print(t4)

print(t3 == t4)
```

**Step 1.** Run the code in the code block above. Notice the different ways you can make a tuple.

**Step 2.** And here is some stuff not to do. Execute each code statement and talk with your partner(s) about what's wrong.

```{code-block} python
# an error
x, y = 0, 1, 2
```

```{code-block} python
# not an error, but hard to understand
x, y = 0, (1, 2)
```

**Step 3.** We've been looking at tuples with just two values in them, but a tuple can also contain a single value or many values.

```{code-block} python
---
lineno-start: 1
---
a_big_tuple = 'a', 2, 'c', 4
print(a_big_tuple)

a_big_tuple = 'a', 2, 'c', 4,
print(a_big_tuple)

a_big_tuple = ('a', 2, 'c', 4,)
print(a_big_tuple)
```

Notice that you can add a comma after the last element in a tuple, and the Python interpreter happily throws it away. However, you need this to be able to define a tuple with a single element:

```{code-block} python
---
lineno-start: 1
---
a_tuple = 0,
print(a_tuple)

not_a_tuple = (0)
print(not_a_tuple)

print(a_tuple == not_a_tuple)
```

Before you execute the following code block, answer for yourself if the expression that is the parameter to `print` will evaluate `True` or `False`, given the definitions of the variables from above.

Now run the code block to check your work.

```{code-block} python
print(a_tuple[0] == not_a_tuple)
```

```{tip}
We've played around with the syntax of tuples, and now I'll mention one reason why they are useful. One of their most convenient uses is to return multiple values from a function. Remember that a function returns only a single object. A tuple is a convenient way to box up every result you have computed in a function and return them in a single object, which the caller can pull apart using multiple assignment!
```

## ALE 4.3: But it must be true!

When you're first coding a solution to a problem, you will find yourself making assumptions. You could document them in a comment, but it is often more helpful to include an *assert-statement* in your code.

The assert-statement takes a condition, which is what you expect to be `True` every time you hit this point in your code. As long as the condition evaluates `True`, execution proceeds as if the assert-statement wasn't there (i.e., your assumption holds). However, if the assert condition evaluates to `False`, the statement throws an `AssertionError`, which tells you that something about your thinking was wrong.

**Step 1.** To experience an assertion failure, run the following silly example.

```{code-block} python
---
lineno-start: 1
---
the_sky = 'is red'
assert(the_sky == 'is blue')
print('It must be because execution got here!')
```

**Step 2.** Be careful with your parentheses in this statement.  `assert` is **not** a command, but a kind of Python statement. We can rewrite the code block above in the follow ways:

```{code-block} python
---
lineno-start: 1
---
# Adds a space after `assert`
the_sky = 'is red'
assert (the_sky == 'is blue')
print('It must be because execution got here!')
```

```{code-block} python
---
lineno-start: 1
---
# Removes the parentheses on the assert condition
the_sky = 'is red'
assert the_sky == 'is blue'
print('It must be because execution got here!')
```

Run the two code blocks to prove to yourself that they're all equivalent.

**Step 3.** An assert-statement also takes an optional second argument, which is the string you'd like printed when the assertion fails. Make sure you see the difference between the first assert-statement, which doesn't do what we want, and the two that follow, which do.

```{code-block} python
---
lineno-start: 1
---
# The INCORRECT way to include an assert message
the_sky = 'is red'
assert(the_sky == 'is blue', 'Who changed the sky?!?')
print('It must be because execution got here!')
```

```{code-block} python
---
lineno-start: 1
---
# The CORRECT way to include an assert message
the_sky = 'is red'
assert the_sky == 'is blue', 'Who changed the sky?!?'
print('It must be because execution got here!')
```

```{code-block} python
---
lineno-start: 1
---
# The CORRECT way to use parentheses around the assert condition
the_sky = 'is red'
assert (the_sky == 'is blue'), 'Who changed the sky?!?'
print('It must be because execution got here!')
```

**Step 4.** Where you put the closing parenthesis makes all the difference. Keep in mind that parentheses mean many different things in Python depending upon the context, as we just saw with tuples!

```{code-block} python
---
lineno-start: 1
---
# Parentheses surrounding a comma-separated list of objects
# AFTER A FUNCTION NAME mean: "Here are the arguments."
print(the_sky == 'is blue', 'Who changed the sky?!?')

# Parentheses surrounding a comma-separated list of objects
# IN SOME OTHER CONTEXTS mean: "Make a tuple."
a_tuple = ('is blue', 'Who changed the sky?!?')
print(a_tuple)
```

Can you explain what was wrong in this INCORRECT use of assert?

```{code-block} python
assert(the_sky == 'is blue', 'Who changed the sky?!?')
```

## ALE 4.4: A Mashup

Let's combine what we learned in the ALEs 4.1-4.3 with the basics of web programming from the chapter to create what's called a *mashup*.

The `qweb8.py` script at the end of the chapter queries a single web resource. Had we programmed it to query multiple resources, we would written what's called a mashup: an application that grabs content from multiple web services to power its own new service. We can build an amazing range of applications by taking advantage of the information available to us around the Internet. Look at what the researchers at IBM accomplished with [Watson, a software program running on 100 servers that was able to quickly sift through 200 million pages of information](https://www.techrepublic.com/article/ibm-watson-the-inside-story-of-how-the-jeopardy-winning-supercomputer-was-born-and-what-it-wants-to-do-next/). In 2011, IBM Watson took on the best-ever human contestants in the TV quiz show Jeopardy, and it beat them. 

Today, we talk to our cell phones and home appliances running software agents (e.g., Apple's Siri, Amazon's Alexa, Google's Assistant, and Microsoft's Cortana), which do much the same work: You ask them a question. They analyze and parse the wave form that is your question, trying to understand what you want to know. Then they query many different resources around the Internet and evaluate what they get back to determine if the information is pertinent to the question they think you asked. Finally, they take the best ranked response and present it to you.

```{margin}
As September 2020, none of the assistants I tried could answer this seemingly simple question. In July 2023, I asked ChatGPT 3.5 this question, and it answered it, although it admitted that its data was two years old.
```

While these software agents are quite sophisticated, we can build a script that does mashup work. In particular, let's say we want to know: *does Harvard Library contain more books than Wikipedia has articles?* 

The script `mashup32.py` (listed below) breaks this question into its two pieces and asks each to [Wolfram Alpha](https://www.wolframalpha.com/), a site on the Internet with the goal of accepting free-form questions and returning relevant and clearly presented answers. We can think of it as a voice assistant without the voice interface.

```{code-block} python
---
lineno-start: 1
---
### chap04/mashup32.py -- not executable without a Wolfram Alpha dev key
import requests
import xmltodict

def walpha(query, appid):
    print(f'Asking Wolfram|Alpha "{query}" ...')
    response = requests.get(
        'http://api.wolframalpha.com/v2/query',
        params={'input': query,
                'appid': appid,
                'podtitle': 'Result'
                }
    )
    assert(response.status_code == 200)

    jlike_response = xmltodict.parse(response.text)
    answer = jlike_response["queryresult"]["pod"]["subpod"]["plaintext"]
    print(f'... answered "{answer}"')
    return answer

def main():
    # Compare wikipedia's and Harvard library's knowledge base.

    # Grab Mike's Wolfram Alpha develop key
    with open('walpha-id.txt') as f:
        appid = f.readline().strip()

    h_query = 'how many books are in the harvard library'
    h_answer = walpha(h_query, appid)
    h_num, h_magnitude, h_units = h_answer.split()[0:3]

    w_query = 'how big is wikipedia'
    w_answer = walpha(w_query, appid)
    w_num, w_magnitude, w_units = w_answer.split()[0:3]

    assert(h_magnitude == w_magnitude)
    h_num, w_num = float(h_num), float(w_num)
    if h_num > w_num:
        print(f'Harvard Library has more {h_units} ({h_num} {h_magnitude})',
              f'than Wikipedia has {w_units} ({w_num} {h_magnitude}).')
    elif h_num < w_num:
        print(f'Wikipedia has more {w_units} ({w_num} {w_magnitude})',
              f'than Harvard Library has {h_units} ({h_num} {h_magnitude}).')
    else:
        print("They're the same size ({h_units} and {w_units})!")

if __name__ == '__main__':
    main()
```

**Step 1.** See if you can understand the general logic of this script. It does contain two programming language features we haven't studied yet:

1. It accepts and manipulates *floating-point numbers*, which are simply numbers with a decimal point and possibly a fractional part. We'll cover this data type in Chapter 7.
2. The Wolfram Alpha API also doesn't allow me to choose the format I'd like for the results; it only returns results in XML. No problem! I just looked up and used a library that converted this XML into a JSON-like dictionary.

**Step 2.** Would you like to run `mashup32.py`? If you try, you'll find that you don't have a file called `walpha-id.txt`. This file exists in my personal world --- I have a Wolfram Alpha developer `appid`. Without an `appid`, the Wolfram Alpha API won't accept your query. I'd say most sites with APIs require you to register as a developer so that they can identify bad actors (e.g., someone who tries to overload their servers). 

If you want to build your own mashup scripts, just realize that you'll probably have to register and receive an `appid` on the sites you use. Since this `appid` is specific to you, you shouldn't share it with others or store it in public code repositories like GitHub. You'll notice that I grab mine from a local file on my machine so that I can share with you my script, but not my `appid`. You too should do something like this when you write your scripts.

```{tip}
Don't ever write your private information as a literal in the scripts you develop or store it in GitHub.
```

**Step 3 (optional).** What might you try to do now that you have some skill in web programming? For example, `mashup32.py` uses the size of the English version of Wikipedia. You may consider this unfair because Harvard Library's book count includes books written in many different languages. You could sign up for your own Wolfram Alpha `appid` and change `mashup32.py` to ask your own version of the question. Perhaps a software agent of this type might make an exciting project for you.

## ALE 4.5: JSON makes my eyes ache

The `qweb7.py` script illustrated how we can pull apart a response from Harvard's LibraryCloud API and determine if the Harvard Library system has a copy of *The Cat in the Hat* by Dr. Seuss*.* But this script printed only the titles of each resource in the response.

I've copied `qweb7.py` below, except that it asks for the top 4 results. See if you can extend the code in the following ways. Each will require you to read through the dumped JSON response and figure out the sequence of indexing operations you need to do to access the information you need to print. Be careful as some JSON fields may be a Python dictionary in one item and a Python list in another, as our code illustrated with the `'titleInfo'` field.

1. Print the author's name.
2. Print the type of resource (i.e., the item is a text or a still image).
3. If the resource is a text and the item includes an abstract, print it. The abstract for *The Cat in the Hat* is "Two children sitting at home on a rainy day are visited by the Cat in the Hat who shows them some tricks and games."

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb7.py
import requests
import json

def main():
    print('Searching HOLLIS for "The Cat in the Hat"')

    # Concatenate the first 3 components of a URL for HTTP
    protocol = 'https'
    hostname = 'api.lib.harvard.edu'
    path = '/v2/items.json'
    url = protocol + '://' + hostname + path

    # Describe the query string as a Python dictionary
    query = {'q': 'The Cat in the Hat',
             'limit': 4
    }

    # Add a field to the request header saying what we accept
    accept = {'Accept': 'application/json'}

    response = requests.get(url, params=query, headers=accept)

    # Read the response body in JSON format and print it
    j = response.json()
    print("response.json() =", json.dumps(j, indent=4))

    print()

    if j['pagination']['numFound'] == 0:
        print('Zero results')
    else:
        # Process each returned response
        for i, item in enumerate(j['items']['mods']):
            # Print title info
            ti = item['titleInfo']
            if type(ti) == list:
                # Lots of title info; just print the first
                ti = ti[0]
            print(f"Title #{i}: ", end='')
            if 'nonSort' in ti:
                print(ti['nonSort'], end='')
            print(ti['title'])

if __name__ == '__main__':
    main()
```

\[Version 20230709\]

## ALE 5.1: Too many guesses

In this chapter, we learned how to build a guess-the-number game that ran until the player correctly guessed the secret number, but some games limit the number of guesses a player has. For example, the online game [Wordle](https://www.nytimes.com/games/wordle/index.html) permits you to make only six guesses, and if you haven't guessed the secret word within those six guesses, the game ends. Wordle's designer was inspired in part by the game Mastermind, and I own a Mini Mastermind game that also allows no more than six guesses of the hidden pattern of colored pegs.

**Step 1.** Your task in this exercise is to change `guess32.py` into a game that gives the player a maximum of six guesses. If player doesn't guess the secret number within six turns, the game should print an appropriate message and terminate.

Start your work from our existing `guess32.py` script, practicing Steps 5-8 of our problem-solving process.

```{code-block} python
---
lineno-start: 1
---
### chap05/guess32.py
import random

def main():
    print('## Welcome to GUESS THE NUMBER! ##')

    secret = random.randint(1, 100)
    # print(f'DEBUG: The secret number is {secret}')

    while True:   # our game loop
        
        # Grab a guess from the player
        while True:
            try:
                guess = int(input('Please input your guess: '))
                break
            except ValueError:
                print('Guesses must be an integer. Try again...')
        # print(f'DEBUG: You guessed {guess}')

        # Check guess against the secret
        if guess < secret:
            print('Too small!')
        elif guess == secret:
            print('Exactly! You win!')
            break
        else:
            print('Too big!')

if __name__ == '__main__':
    main()
```

**Step 2.** When you've got a working solution, take some time to compare it with the solution produced by your friends. When you're looking at each other's code, think about these questions:

1. What creative things did you do that they didn't do? Does their code include creative ideas that you didn't think about doing?
2. Did you take different approaches to limiting the number of iterations of the game loop? Of the approaches taken, notice where the comparison that ends the game loop is done. What makes its placement a good idea in each different solution?
3. In the different solutions, how easy is it to know what to change if you wanted to provide the player with 10 guesses instead of 6? If the limit is buried in the code of the script, how might you make it easier to find and be sure you made all the changes necessary?

## ALE 5.2: So many ways to pick

Our guess-the-number game used the `randint` function in [Python's `random` library](https://docs.python.org/3/library/random.html). This function is quite convenient if we want to pick a random integer in a closed range. 

**Step 1.** Using `randint`, what statement (or small number of statements) could you write to randomly choose an even integer between 2 and 100? Put your code in the next code block, and run your solution several times to make sure you did it correctly.

```{code-block} python
---
lineno-start: 1
---
# Randomly choose an even integer in the range [2, 100]
import random

# YOUR SOLUTION HERE
```

**Step 2.** The `random` library also provides a `randrange` function that allows you to specify a `step` value that is the distance between each integer in the range. Use `randrange` instead of `randint` to again randomly choose an even integer between 2 and 100.

```{code-block} python
---
lineno-start: 1
---
# Randomly choose an even integer in the range [2, 100]
import random

# YOUR SOLUTION HERE
```

**Step 3.** Moving away from integers, the `random` library also includes a function called `choice` that makes a random selection from a given sequence. Build yourself a sequence object containing 5 jokes from [icanhazdadjoke.com](http://icanhazdadjoke.com), and then use the choice function to randomly choose one of them. Run your code multiple times.

```{code-block} python
---
lineno-start: 1
---
# Randomly choose one of five bad dad jokes
import random

# YOUR SOLUTION HERE
```

**Step 4.** Finally, copy your solution to Step 3 into the code block below. Now read about the function `random.seed` in the `random` library, and figure out how to use it to print out *the same bad dad joke every time* the code block is run. Gain certainty about your solution by wrapping it in a loop that iterates 25 times and prints the random choice each time. Your dad would be so proud.

```{code-block} python
---
lineno-start: 1
---
# Randomly choose the SAME bad dad joke with each choice
import random

# YOUR SOLUTION HERE
```

```{tip}
You've just learned how you can seed the pseudorandom generator inside Python's `random` library to start at the same place in the pseudorandom sequence on each run. Use this you want to test your code using the same pseudorandom sequence.
```

## ALE 5.3: Catching exceptions

Our guess-the-number game used a try-except-statement to give the user another opportunity to input a guess if what they typed wasn't "a string in an integer suit," as defined by Python's `int` type conversion function.

**Step 1.** The following code block repeats that *design pattern* we used in `guess32.py`. Go ahead and run it a few times to make sure you understand how it works; feel free to insert other print statements if you want to know what executes and what doesn't under different inputs.

```{code-block} python
---
lineno-start: 1
---
while True:
    try:
        guess = int(input('Please input your guess: '))
        break
    except ValueError:
        print('Guesses must be an integer. Try again...')
print('Your input guess:', guess)
```

**Step 2.** What input-checking code would you write if we asked the user to input one of the four `suits` in a deck of playing cards? The following code block will help get you started:

```{code-block} python
---
lineno-start: 1
---
suits = ['clubs', 'diamonds', 'hearts', 'spades']

# YOUR SOLUTION HERE

print('Your input suit:', suit)
```

**Step 3.** What did you learn in Step 2 about using a try-except-statement?

## ALE 5.4: Adding metadata to your messages

The networked guess-the-number game we've developed has the server send the client a message that it simply prints. In more interesting network conversations, one endpoint wants to send some data to the other endpoint and include with these data some *information about the data*. In computer science, this information about the data is called *metadata*.

In this exercise, you are going to modify our guess-the-number game so that server sends messages that include metadata (the client messages are unchanged). The server will continue to send text that the client should print for the user, and it will add metadata indicating the color of the text when it is printed. The game will use color to add a visual hint about how good each guess was.

Here are the new rules for our guess-the-number game:

* If the absolute value of the difference between the user's guess and the secret is greater than 25, the client should print "Too small!" or "Too big!" in blue text, which will indicate that the user's guess is very cold.
* If the absolute value of the difference between the user's guess and the secret is between 1 and 5 inclusive, the client should print "Too small!" or "Too big!" in red text, which will indicate that the user's guess is very hot.
* When the user guesses the secret, the client should print "Exactly! You win!" in green text.
* In all other situations, the client should print the server's response in black text.

The messages we send between the server and the client will be split into two pieces: a header containing the metadata; and a body containing the text to be printed. The metadata indicates the color of the text in the message's body. The escape sequences should NOT be in the message's body.

Remember that our messages are just text strings, and so a message containing a header and a body is just the concatenation of two text strings. We aren't doing anything except appending a header to the front of the text string we're currently sending from the server to the client.

**Step 1.** To design the server's messages, you need to answer these questions:

1. What are all information you'll put directly (or in an encoded fashion) in the message header?
2. How will the client know where the message header ends and the message body begins? 

There are several ways to answer these questions. Debate a few ideas with your friends, and then settle on the simplest approach. This will inform how you'll encode the values you enumerated into the previous question.

**Step 2.** To code this new version of the game, you need to know how to print text in color on a terminal screen. This involves sending some text to `print` that it interprets as an escape sequence. I demonstrate how this is done in the modified client code below:

```{code-block} python
---
lineno-start: 1
---
### chap05/ale04-client.py
from socket32 import create_new_socket

HOST = '127.0.0.1'  # The server's hostname or IP address
PORT = 65432        # The port used by the server

# Color codes for terminal printing
default = '\033[0m'
black =   '\033[30m'
red =     '\033[31m'
green =   '\033[32m'
blue =    '\033[34m'

def main():
    print('## Welcome to ' + blue + 'GUESS '
         + green + 'THE ' + red + 'NUMBER'
         + default + '! ##')

    with create_new_socket() as s:
        s.connect(HOST, PORT)

        while True:
            # Grab a guess from the player
            while True:
                try:
                    guess = int(input('Please input your guess: '))
                    break
                except ValueError:
                    print('Guesses must be an integer. Try again...')

            s.sendall(str(guess))
            response = s.recv()
            print(response)

            if response == 'Exactly! You win!':
                break

if __name__ == '__main__':
    main()
```

Notice that I've named the different escape sequences with a descriptive name. To turn on the printing of text in red, you simply print the `red` escape sequence. To turn off printing text in red, you need to insert the escape sequence of a different color or use the return-to-default-color escape sequence, which I've named `default`. I've printed the capitalized name of the game in our three different colors as an example.

**Step 3.** Start `ale04-server.py` by making a copy of the `guess-server.py` script developed in the chapter. Adapt `ale04-client.py` (above) and `ale04-server.py` so that they play this enhanced guess-the-number game.

\[Version 20230709\]

## ALE 6.1: Capturing color

We've learned that digital images are nothing more than large two-dimensional arrays of pixels, which we can load into a pixel array and manipulate. Except for the `edges.py` script in which we ran a prepackaged convolution filter on a color image of my dog, we limited ourselves to grayscale images containing 8-bit pixels. With 8 bits, we could represent 256 different shades of gray from black (`0x00`) to white (`0xff`).

While black-and-white photos can be quite artistic, more often most of us capture color images. I used to take color pictures with a digital single lens reflex (DSLR) camera, but now I take all my photos (and videos) with my cellphone. The manufacturer describes the camera on the back of my cellphone as containing a 12-megapixel sensor, where mega- means $10^6$ or 1 million. 

The manufacturer-quoted number isn't an exact measure. The sensor in my cellphone is actually a 2D array of size 4290 by 2800, and the color pictures taken with this camera produce just over 36 megabytes (MB) of image data (by "just over" I'm telling you that the 36MB measure also isn't exact). Despite the inexactness of both numbers, you can use them to determine **the size of each color pixel in bytes**.

```{admonition} You Try It
Write out an equation that divides the size of the color images taken by my cellphone by the total number of pixels in my cellphone's sensor. That equation will tell you the number of bytes per pixel. In other words, you'll know how many bytes there are per pixel in a color (i.e., RGB-encoded) image. Do the math to figure out the answer.
```

You should have found that the simplest color images (i.e., those that use `RGB` mode) use three 8-bit numbers to represent the color of a pixel. In Python, each RGB pixel is a tuple of three 8-bit numbers, where the first byte (i.e., 8-bit value) represents how much red is in the pixel. The next indicates how much green the pixel contains, and the last value is the amount of blue. Three zeros would indicate the absence of any of the three colors and would display as black; recall that 0 represents black in every mode. Three values of 255 would appear as a white pixel.

## ALE 6.2: Diagonal fade to a color

In this chapter, the script `edge1.py` created a grayscale image containing a diagonal fade. For this exercise, you'll change this script so that it fades not from black to white, but from black to one of red, green, or blue. To do this, we'll need to change the mode of the image we create from `'L'` to `'RGB'`.

```{margin} HINT
`pixels[i,j]` no longer holds just a single integer. Focus on the color component you want and leave the other black (i.e., `0`).
```

**Step 1.** Given what you learned in ALE 6.1, complete line 16 in the script (`ale02.py`) to create a diagonal fade from black to red. 

```{code-block} python
---
lineno-start: 1
---
# chap06/ale02.py
from PIL import Image

# Width and height of our image
sz = (100, 100)

# Create a single plane of black and white pixels, initialized to black
im = Image.new('L', sz)

# Create direct access to the pixels in the image
pixels = im.load()

# Set the color of each pixel
for i in range(sz[0]):
    for j in range(sz[1]):
        # pixels[i,j] = ???

im.save('ale02.png')
```

**Step 2.** Try not just a diagonal fade from black-to-red, but also create ones from black-to-green and black-to-blue.

## ALE 6.3: Creating a checkerboard

Starting again with `edge1.py`, which creates an image containing a diagonal fade, change the body of the innermost for-loop in `edge1.py` so that it creates a checkerboard pattern. The checkerboard pattern should adhere to these rules:

* Each pixel contains either the color black or white; and
* The color of any pixel is not the same as its neighbors to the north, south, east, and west, i.e., if the current pixel is at `(i,j)`, the pixel to the north is at `(i,j-1)`, to the south `(i,j+1)`, to the east is `(i+1,j)`, and to the west is `(i-1,j)`. Don't worry about neighbors that are not within the pixel array.

You'll  only need to change line 17 in `ale03.py`. It currently sets every pixel in the `pixels` array to the color white.

```{code-block} python
---
lineno-start: 1
---
# chap06/ale03.py
from PIL import Image

# Width and height of our image
sz = (100, 100)

# Create a single plane of black and white pixels, initialized to black
im = Image.new('L', sz)

# Create direct access to the pixels in the image
pixels = im.load()

# Set the color of each pixel
for i in range(sz[0]):
    for j in range(sz[1]):
        # Create a checkerboard
        pixels[i,j] = 255

im.save('ale03.png')
```

## ALE 6.4: Counting with Python integers

If you look carefully at the script named `edge1.py`, you'll see that it doesn't do arithmetic on pixels, but on the loop indices `i` and `j`, which are values produced by the `range` function. If you pass these names to the built-in function `type`, it will tell you that these names refer to Python integers.

From experience, we know that Python integers can hold values greater than 255 and less than 0, and so it is not the addition in our script that overflows and forces the saturation, but the act of trying to store the result of this addition into a location in the array `pixels` that causes an issue. In particular, the PIL library checks to see if the value we're trying to store overflows (or underflows) the 8-bit, unsigned representation used for each pixel in the `pixels` array.

That's great, but if we need to be careful with the values we put into the pixels of a black-and-white image, **do we need to be careful about the values we put into a Python integer?** In other words, **will addition on Python integers ever overflow?**

We could go read some documentation or google an answer to this question, but we can also experiment and see what happens.

**Step 1.** We have been using integers to enumerate the things in a set (e.g., shades of gray between black and white). Integers are also great when we simply want to count, and so, let's see how far we can count with Python integers before we hit an overflow condition.

The script `ale04_count.py` contains an infinite loop with no exit condition. Inside this loop, we add one to the variable `i`, which we initialized to `0`, which made it a Python `int`.

```{code-block} python
---
lineno-start: 1
---
### chap06/ale04_count.py
i = 0
next_bound = 1

while True:
    i += 1
    if i == next_bound:
        print(f'Reached {i}')
        next_bound *= 10
```

Because we're not interested in seeing every value that `i` takes on, there's a bit of extra code in this script to print only when `i` reaches a number that's a power of 10. Printing takes time, and we don't want to unnecessarily slow down our script. With a friend, walk through the statements in `ale04_count.py` and make sure you understand the entire script.

**Step 2.** If you were to run this script, when will it stop running? Think about this question for a bit, jot down the reason for your answer, and then read on.

**Step 3.** When we run `ale04_count.py`, we fly through the first bunch of powers of 10, showing that our computers can count much faster than we can. My computer gets to 100 million in about 10 seconds, and then it starts to take a noticeably long time between prints. If I listen carefully, I can hear its fan spin up as the processor starts working really hard. 

Notice that there's no break statement anywhere in the infinite loop, and so the only way that this loop will end is if a runtime exception occurs. Given the statements in the loop's body, we're waiting for either the addition or the multiplication to overflow.

```{tip}
Help! I Want It To Stop!

I hope you tried running `ale04_count.py`, and at some point, you'll want it to stop. Here's how:

*   If you started the script in a Shell, push the control-key and the letter-c-key at the same time, which we'll abbreviate as ctl+c. This key sequence tells the Shell to kill the execution of the currently running script.
*   If you clicked a play button in your IDE, the play button probably turned into a stop button, and if you click this stop button, the IDE will kill the execution of your running script.
```

If you haven't tried running `ale04_count.py`, now that you know how to stop it, run it now. Watch its output for a bit and then force it to stop using one of the two methods above.

**Step 4.** Let's think about the work your processor is doing while running `ale04_count.py`. It's not hard work; it's just adding 1 over and over again. But to get to a big number, it will take a lot of additions. Let's make this concrete by doing a quick calculation: 

If it took 10 seconds to get to 100 million, estimate how long will it take to get to a trillion, which is 1/30th of the [US debt](https://www.usdebtclock.org/) (as of 2022) and therefore a number we may want to represent in a script?

**Step 5.** You should have found that the answer is a lot longer than you'd like to wait. Maybe `ale04_count.py` isn't the best way to find if addition overflows.

Edit the script in the code block below (`ale04_double.py`), so that it doubles `i` on each loop iteration rather than adding 1 to `i` (i.e., write Python code for the comments on lines 4 and 8).

Because this script will increase `i` so much faster than our last script, I've inserted a call to `time.sleep` and specified that the script should pause for one-half a second each time it passes the `next_bound`. If you comment out this call, you'll see why this is necessary.

```{margin} Tie Back To Bits
When you complete it, `ale04_double.py` adds another bit (i.e., another binary digit) to the length of `i` for each execution of its loop body. Make sure you fully understand why this is true.
```

```{code-block} python
---
lineno-start: 1
---
### chap06/ale04_double.py
import time

# FIXME: initialize i appropriately
next_bound = 1

while True:
    # FIXME: update i so it doubles each time
    if i > next_bound:
        print(f'Passed {i}')
        next_bound *= 10
        time.sleep(0.5)
```

**Step 6.** The two scripts in this exercise demonstrate two different *growth rates*: the first exhibits *linear growth* (i.e., two successive values differ by a constant amount); and the second *geometric growth* (i.e., two successive values differ by a constant ratio). We'll encounter growth rates again in Act II.

**Step 7.** Will addition on Python integers ever overflow? Let's finally answer this question, which was our goal in this exercise. When you run `ale04_double.py` and see `1099511627776`, the script has passed a trillion. It probably took less than 10 seconds to pass that value on your computer. 

You can let `ale04_double.py` continue to run for a while, but at some point, kill its execution. This script clearly demonstrates that we can count pretty high in Python. In fact, Python does not restrict the size of an integer. As long as your computer has the storage necessary to hold whatever big number you can dream up, Python can handle it. This is a very nice feature of this language, and one that is not true in every other programming language.

## ALE 6.5: Color the duck's world

Our earlier exercises created their own images. In this exercise, you will start learning how to modify an existing digital image. You'll also learn a new set of methods for accessing and changing the value of a particular pixel in your open image.

**Step 1.** In your favorite JPEG viewer, open the color image named `duck.jpg`, which you can find in `chap06/images` of the book's GitHub repo. It should look like a perfectly normal picture that you might have taken with your own cellphone camera.

**Step 2.** The script `ale05.py` uses the Python PIL library to open, read, and recolor a single input image. Read through the script and then we'll briefly discuss what's new in it.

```{code-block} python
---
lineno-start: 1
---
### chap06/ale05.py
from PIL import Image

# Grab the image filename
imfile = 'images/' + input('Filename of image: ')

with Image.open(imfile) as im:
    # Apply a filter that enhances the red and desaturates blue/green
    for x in range(im.size[0]):
        for y in range(im.size[1]):
            r, g, b = im.getpixel((x,y))
            im.putpixel((x,y), (r, g//50, b//50))

    im.save('ale05.png')
```

Because we're working with an existing image, the first thing we have to do is open the image file as we did in `edges.py`.

An existing image contains a 2D image array just like we encountered when we built a new image, and this means that we'll still need our familiar nested for-loops if we are going to apply a filter to each pixel in the image.

How big is the image we opened? Well, we knew the size of the images we created because we specified them. Luckily, we can also ask an existing image for its width and height in pixels through the attribute called `size` because `Image.open` fills in this attribute. This attribute returns a 2-tuple of `(width,height)` over which we can iterate.

The body of our innermost for-loop looks a bit different than what we've previously seen. It uses two new methods on the image object: `getpixel` and `putpixel`. These two methods each take a parameter, which is a 2-tuple representing the `(x,y)` coordinate of the pixel we wish to get or put. In the coordinate system of the images we manipulate, `x` indexes the columns (of which there are `width` columns) and `y` indexes the rows (of which there are `height` of them).

So far, this should remind you of the x-y Cartesian plane you probably encountered at some point in your prior schooling. The only twist in the image world is that index `(0,0)` is the upper left corner of the image.

With `getpixel`, the pixel comes back as a tuple, which we can pull apart with our assignment statement. To write a pixel using `putpixel`, we not only want to specify the `(x,y)` coordinate of the pixel in the image, but also the RGB tuple `(r,g,b)` we want to write.

I'll discuss the `//` operator in a moment.

**Step 3.** Run `ale05.py` and feed it the image file `duck.jpg`. Can you an understand why it displays a reddish version of our input picture?

We enhanced the red in our image by desaturating the blue and green values of each pixel. In particular, we scaled down the green and blue values by some fixed factor, which we did through division.

If you look up the division operator in Python, you'll see that there are two: `/` and `//`. The single forward-slash operator (`/`) is called *division*, and if you feed it two integer operands, it will produce a *floating-point value*. A floating-point number is number with a decimal point, which in Python are given the type `float`. For example, `42` is an `int` in Python, but `42.0` is a `float`. We need float-point values because we can't represent a number like one-half (i.e., one divided by two) as an integer. We need both a whole and a fractional part. We'll talk more about floating-point numbers in Chapter 7, but right now we need to scale our green and blue values so that they continue to be integers, since that's what we're storing in our image files.

The double forward-slash operator (`//`) is called *floor division* in Python, and if you feed it two integer operands, it will produce an integer result. This integer result is equivalent to what you'd get if you applied the floor function to the floating-point result from the `/` operator. With positive results, you can think of `//` returning just the whole number part of the result. 

**Step 4.** Can you edit `ale05.py` so that it highlights the blue aspects of the input image? How about the green aspects?

**Step 5.** Notice that the PIL library functions are smart enough not to care if we feed a JPEG or PNG file to our script. You can test this by running `ale05.py` with other input files. Try some of your own digital photos. Remember to put your digital photos in the images directory!

```{admonition} Not Too Big!
The amount of time it takes to process an image is proportion to the size of the image in bytes. Why? Because the bigger your image is the more pixels it has, and the script has to read and write each one. I've been using images of 100-200 kilobytes. Don't use ones with many megabytes.
```

\[Version 20230710\]

## ALE 8.1: Parameters and Parentheses

We used the `Image.putpixel` method many times in this chapter. [Its documentation](https://pillow.readthedocs.io/en/stable/reference/Image.html#PIL.Image.Image.putpixel) says that it takes two input parameters: an `xy` coordinate of the pixel in the Image object, and the `value` we want there. The script `zero.py` used this method in the function `zero_image_lowest_bits` as follows:

```{code-block} python
---
lineno-start: 14
---
### chap08/zero.py
def zero_image_lowest_bits(src, dest):
    '''Zeroing lowest 4 bits in all channels of input image'''
    for x in range(src.size[0]):
        for y in range(src.size[1]):
            r, g, b = src.getpixel((x,y))
            new_r = zero_lowest_bits(r)
            new_g = zero_lowest_bits(g)
            new_b = zero_lowest_bits(b)
            dest.putpixel((x,y), (new_r, new_g, new_b))
```

**Step 1.** What's wrong with this version of the statement on line 23?

```{code-block} python
---
lineno-start: 23
---
            dest.putpixel(x, y, new_r, new_g, new_b)
```

**Step 2.** Would this version of `zero_image_lowest_bits` work? It clearly passes two actual parameters that match the two formal parameters in the `Image.putpixel` interface.

```{code-block} python
---
lineno-start: 15
---
def zero_image_lowest_bits(src, dest):
    '''Zeroing lowest 4 bits in all channels of input image'''
    for x in range(src.size[0]):
        for y in range(src.size[1]):
            r, g, b = src.getpixel((x,y))
            new_r = zero_lowest_bits(r)
            new_g = zero_lowest_bits(g)
            new_b = zero_lowest_bits(b)
            
            coordinate = (x,y)
            new_pixel = (new_r, new_g, new_b)
            dest.putpixel(coordinate, new_pixel)
```

## ALE 8.2: Erase the Duck

Now that we know how to read and write the pixels in a digital images, we're going to use that knowledge to solve a common problem: removing an unintended subject in a sequence of pictures containing no moving objects except for our unintended subject. This exercise was inspired by [John Nicholson's 2014 Nifty Assignment titled The Pesky Tourist](http://nifty.stanford.edu/2014/nicholson-the-pesky-tourist/), and we thank him for sharing it with the world.

How will our script remove the unintended subject (i.e., the noise in our digital image)? Our script will:

1. Expect you to give it *N* pictures, for *N* greater than 2, that are all of the same size and orientation;
2. Visit each pixel location in this common image frame, reading the pixel values there for each of the input pictures; and 
3. Compute the median of these values as the value of each pixel in the final filtered image.

Ready to get started?

**Step 1. Reading multiple input files.** The fifth step of our problem-solving process says that we should begin a new problem by considering what previously written scripts can help us with our current problem. In this case, the script from ALE 6.5 would be a great start as both that script and the one we're writing visit each pixel in an input image.

That script is repeated and renamed below:

```{code-block} python
---
lineno-start: 1
---
### chap08/ale02.py
from PIL import Image

# Grab the image filename
imfile = 'images/' + input('Filename of image: ')

with Image.open(imfile) as im:
    # Apply a filter that enhances the red and desaturates blue/green
    for x in range(im.size[0]):
        for y in range(im.size[1]):
            r, g, b = im.getpixel((x,y))
            im.putpixel((x,y), (r, g//50, b//50))

    im.save('images/out.png')
```

Change this script so that it reads *exactly three* input files from *the command line or from user input*.

**Step 2. Opening multiple input files.** Now that our script knows the names of three input files, we need it to open the three files.

Edit the `with` `Image.open` line so that it repeats the open-as phrase in the with-statement three times. Each open-as phrase should be separated by commas. This will cause the script to open (and later close) multiple image objects. It should look something like the following, with each `open_as_phrase` replaced with `Image.open(filename_var) as image_object_name`:

```{code-block} python
with open_as_phrase1, open_as_phrase2, open_as_phrase3:
```

The resulting with-statement might get quite long, and you can split it across multiple lines at a comma by using the line continuation character in Python, which is the backslash (`\`).

Take a look at [the Style Guide for Python Code (PEP 8) under "Maximum Line Length"](https://www.python.org/dev/peps/pep-0008/#maximum-line-length) to see an example of how this is done. As it says there, we prefer to use Python's implied line continuation in most long-line situations rather than this explicit line continuation syntax, but this is a common situation where you can't use anything but the explicit syntax.

**Step 3. Testing what we've done so far.** Take a moment and add some statements to your script so that it prompts the user to ask which input file the script should modify in the nested for-loops and then save the changed image in `out.png`.

As you do this work, don't worry about checking for all kinds of bad user input: You're just testing what you've written in steps 1 and 2, and we'll delete this extra code in a moment. Do, however, make sure that the correct file was displayed!

If you have access to the this book's Github repo, you can test your script using the image files with the prefix name `duck` or `photobomb` in the `images` directory under `chap08`.

When you're done testing your script, get rid of the prompt you just added, but make sure you leave the statements you added in Steps 1 and 2!

**Step 4. Finding the statistical median.** Alright, you are now ready to erase an unintended subject. The nested for-loops provide the logic we need to visit each pixel in an image, but the statements in the body of these nested for-loops are not the work we need done. The work we need is to grab the pixel values at each location `(x,y)` in the three input images and compute the median of these values.

```{margin} Read about the Function
You should [read about `statistics.median`](https://docs.python.org/3/library/statistics.html#statistics.median) before attempting to complete this step.
```

Although we could write a function that computed a median given a list of numbers, let's take advantage of the `statistics` library in Python, which has conveniently provided us with a `statistics.median` function. You'll notice that the `statistics.median` function may produce a value that is not one of the input data points. This will be a problem for us because we don't want our input integer values turned into an output floating-point value. Our color values are integers and need to stay as such. While you might force the value returned from `statistics.median` back into an integer, we can also simply use `statistics.median_low`. Ahh, what a helpful and a well-designed library!

Once you have a median value (and think about what that means for a pixel), you'll want to store the result in a new output image.

```{tip}
Write pseudocode reflecting what was just discussed. Don't try to jump right into Python code! Make sure you see the script's complete flow in pseudocode before you start translating these new pieces of logic into Python. It will save you time in the long run!
```

```{margin}
If you are unfamiliar with this children's game, [this Wikipedia article](https://en.wikipedia.org/wiki/Where%27s_Wally%3F) provides some background.
```

To test your code with our images, start with the three `photobomb` images in the `images` directory. If those work, check your code with the three `duck` images. The `photobomb` images were created by copying a single picture of Harvard Yard and then inserting Waldo from *Where's Waldo?* fame into this image in three different locations. The three `duck` images are a real sequence of three pictures with a common background and moving character. As such, many pixels---even those not obscured by the duck---are subtly different from picture to picture.

**Step 5. The need for more input files.** Congratulations on erasing the CS50 debugging duck and Waldo from our images! As wonderful as our current script is, it isn't very robust. For example, it expects exactly three input images, and the unintended object can't overlap with itself in any of the three images.

To see this second point for yourself, run your solution to the previous step with the first three images with the prefix `apsu`, which have a `.png` extension. Look at the resulting image very closely, and you'll see a shadowy image of the person's feet where the unintended subject overlapped with his previous self. 

**Step 6. Accepting and computing with any number of input files.** We will eliminate the problem identified in Step 5 by building a script that accepts more input images. The unintended object or subject could then overlap with itself/himself in some of the input images as long as the majority of the images contained only the desired background.

To begin, make a backup copy of your current script, and then follow the directions below:

1. Eliminate the statements that allowed the user to input the image filenames. Your final script will accept the input image filenames from the command line.
2. Fix the "proper usage" check so that it expects *at least 3 command line arguments*. Remember that the your program's name (but not the `python3` interpreter) counts as one of the command line arguments.
3. Time to write some pseudocode again. Think about a data structure that we can use to collect all the input filenames as well as keep track of our image objects that you'll open using these filenames. To this point, you were probably using a unique variable for each input filename and the result of each of the three `Image.open` commands. This worked because we knew that there were exactly three of each. We now need a data structure that can expand while the program runs to accommodate all the filenames and open-file objects we'll need. Can you think of a data structure that will allow us to work with a variable-length list of objects? Yes, we just said it! A Python `list` will do this quite nicely. We'll talk a lot more about different types of common data structures in the next act. Go through your script and add pseudocode for every piece of work that needs to change because you're using lists instead of single variables.
4. When you think you've got all the logic you think you need in pseudocode, start translating this pseudocode into Python. Don't forget that you can ask for the size of the `sys.argv` list and subtract `1` from this value to determine the number of input files. This might be very useful in creating a bound you could use in one or more for-loops that walk over the elements in a list.
5. While translating pseudocode into Python code, you'll undoubtedly realize that the nice with-statement we've been using requires us to know the exact number of files we want to open at the time we write the script. Yup, you are going to have to open each input image file and later explicitly close each one (using `Image.close`).

Once you've done all this work, run your script as follows:

```{code-block} none
python3 ale02.py apsu1.png apsu2.png apsu3.png apsu4.png apsu5.png apsu6.png
```

Did this produce a much cleaner resulting image?  It should! Go show your family and friends your amazing script. (You might rename your final script from `ale02.py` to `erase32.py`.)

```{margin} Hint
What's different in how we gathered the original data (i.e., images) in these two cases?
```

**Step 7. Why median, but not mode?** Take a look at the files `images/photobomb-mode.jpg` and `images/duck-mode.jpg`. These files were produced by our solution script, except that we used `statistics.mode` instead of `statistics.median` to compute the pixel we placed in the output image. As you'll see by looking at these two images, we successfully erased Waldo using the `mode` function, but not the duck. Why is this?

And when you figure out why median is more robust an approach than mode, please make sure you can explain why is it the first duck that remains. This doesn't have anything to do with the statistical properties of the function, but choices that the library designer made. What are you learning about quantitative reasoning with data here?

## ALE 8.3: Create and Fix Your Own Photobomb

In 8.2, you created a script that fixed photobombs in our pictures. Now it is your turn.

**Step 1.** Take a sequence of 3 or more pictures with your smartphone and download them to your computer. Here are some recommendations that will help you make the next step work well:

1. Find a place where you can set your phone and it won't move for your sequence of pictures. Ideally, use some kind of tripod.
2. In the scene you want to capture in a picture, nothing should be moving. If you want to capture a person in front of a beautiful backdrop, make sure that nothing in the whole scene will be moving, including your subject. In other words, the person needs to stand/sit still for a few pictures.
3. Now decide on the photobomb(s) that will be in your pictures and that your script will eliminate. If that's another person, have them move after you take each picture. Make sure that they move enough that they don't appear stationary. If you want to include two photobombing people, try it!
4. Take enough pictures that, for all points in the scene you want, the majority of the pictures show that each point unobscured.
5. On your computer, reduce the size of your images --- ideally no more than a few hundred kilobytes. The larger the images the longer your script will take to run!
6. Make sure you saved the reduced images in RGB, not RGBA, format.

**Step 2.** Run your images through the script you built in 8.2. Show your friends and family!

\[Version 20230722\]
