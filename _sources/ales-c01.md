## Chapter 1 ##

## ALE 1.1: Hello World

Using your chosen IDE, follow the steps below to practice with Python commands you learned in Chapter 1 and to become more comfortable creating named Python objects.

**Step 1.** In your IDE, make a new Python project.

**Step 2.** In the editor pane, write a script that prints `Hello World` using only the `print` command. Run your script to make sure it executes correctly.

**Step 3.** Now change your script so that it assigns the string literal `'Hello World'` to the variable `exclamation`, and then have your script print `Hello World` using this variable. Run this updated script to make sure it executes correctly.

**Step 4.** Change your script again. Assign the string literal `'World'` to the variable `what`, and have your script use two print statements to print `Hello World`. It's okay if `World` is on its own line, but one of these print statements must use the variable `what`. Run your script and fix any issues that arise.

**Step 5.** Replace both print statements in your script with the single statement: `print("Hello", what)`. Run the script.

1. Look carefully at the output and compare it to the two objects you gave as inputs to `print`.
2. In the interactive Python interpreter, type `help(print)` and hit return. Consider the explanation. It should help you understand why `print("Hello", what)` includes a space between the two words even though there was no space in either of the string literals passed as inputs to the print command.

**Congratulations!** You are over the first big hump: You've started using a tool that will help you think computationally and will give you the freedom to solve problems creatively. We just looked at four different ways to print the same short phrase, and while not terribly creative, you will learn many more ways to format what you want to print and will learn to solve much more complex problems.

If you're interested in the history and lore behind the tradition of printing "Hello World" in your first program, please see [this wikipedia page](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program).

## ALE 1.2: Too long?

Let's solve a new problem that continues to use the context of reading bedtime stories: My children loved the stories we read, and they would often grab another book immediately after we finished the current one. As night fell, I'd begin to consider the length of each book they brought me. If it was late, I wouldn't start a long one.

So let's write a script that makes this decision for us. It will count the number of lines in a book and then print "Let's read this" if the book's length is less than 20 lines and "We can't read this" if it isn't.

To get started solving this problem, let's practice Step 5 in our problem-solving process, which states that we should identify old scripts that can help us in writing our new one. Obviously, doing this can save us work, but more generally, this approach avoids the fears many of us have when we start a project by staring at a blank page.

**Step 1.** In your IDE, make a new Python project and paste into the editor pane the script we completed at the end of Chapter 1 (`anybook.py`).

**Step 2.** Think about which statements in this old script you need to solve this new problem. For example, do you need to ask the user for the name of the book to be read? Yup, that would be helpful, and so we'll keep that `input` command and the `open` command that uses its result. Do we need to loop through all the lines in the open file? Yup, keep that while-loop.

Skipping ahead, do we need to print the lines we read from the open file? No, we don't. So we can delete that line.

Consider each statement in our previous script and ask whether you need it or not, leaving alone those lines that are helpful and deleting those that aren't. What's left is a frame into which you can write new pseudocode. Bam! You're well on your way to solving this new problem.

**Step 3.** To count the number of lines in our input file, we're going to want to keep track of the number of lines we've seen in previous iterations of the while-loop. What do you want to call this variable? It's a variable because the object it names changes from one iteration of the loop to the next.

**Step 4.** What happens to this variable inside the while-loop? Can you write Python code that implements your answer to this question?

**Step 5.** Does this variable need to be initialized before the while-loop? If yes, what value should you use to initialize it? HINT: It does, and the value you use to initialize it depends on the where in the while-loop you update it. Think about the proper initialization value for an update at each possible position in the while-loop body.

**Step 6.** When our scripts sees EOF returned from `readline`, it breaks out of the while-loop. Previously, the script printed "The End" and ended. Now, however, we want to test the value computed across the iterations of the while-loop and then print one of two phrases. This sounds like a job for an if-statement. But to this point, we have only used if-statements to *protect* a block of code. In this problem, we want the test in our if-statement to *select* one of two blocks of code. The syntax for this version of an if-statement looks like this:

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

Create yourself yet another Python project, and again copy in the entire script from the end of Chapter 1 (`anybook.py`). For this exercise, we're not going to change its functionality, but simply make it more *robust*.

**Step 1.** A piece of this work involves creating a separate directory for its input files. By putting all the input text files in a subdirectory called `txts`, we'll avoid mistakenly overwriting one of our scripts while performing file operations on the input texts. Look at the changes to the `open` parameter on line 2 below:

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

# Print every line in the book
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')

    # Check for EOF
    if the_line == '':
        break

print("The End.")
```

We will learn more about the infix `+` operator in Chapter 2, but briefly, it performs string concatenation when the `+` is placed between two string objects.

Update the script in your editor pane so that it matches the code above. Make sure the `txts` subdirectory exists in your IDE file browser and then try running this script.

**Step 2.** Now add the statement on line to the end of your script, as illustrated in the following code:

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

# Print every line in the book
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')

    # Check for EOF
    if the_line == '':
        break

my_open_book.close()

print("The End.")
```

Adding this `close` statement doesn't add a new feature to our script.  Run this script to verify this claim. The purpose of this `close` statement is to tell the interpreter that we're done with the open file and it can close it.

You should always pair a `close` for every `open` you write, but it is often hard to remember to do this. When I first started programming, I often forgot this pairing, and now I use a story from my childhood to help me remember.

When I was a small boy, my parents would constantly remind me to close the screen door. Why did they do that? Yes, they wanted me to keep the bugs out of the house. We won't go, at this time, into the details of the bugs/errors that can occur if you don't close a file when you're done with it, but suffice it to say if you ever open a file, you should close it.

It's perfectly fine to do this toward the end of your script, as I've done here, since the interpreter stops executing your script when it reaches the script's end. Putting this `close` command right before the end of the script ensures that closing the file is one of the last things the interpreter does.

**Step 3.** What if I wanted to close the file earlier in the script? I could do that, but if I'm making many changes to my script, I might mistakenly move the `close` to some point before I'm actually done reading the file. For example, let's move the `close` statement into the loop and execute this new script.

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

# Print every line in the book
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')

    # Check for EOF
    if the_line == '':
        break

    my_open_book.close()

print("The End.")
```

Yuk. We got a `ValueError: I/O operation on closed file.`

Notice that there's very little difference between the last two code blocks. Basically, four little spaces. This is a nightmare waiting to happen.

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

        # Check for EOF
        if the_line == '':
            break

print("The End.")
```

The `with-as` statement does the `open` and assigns our virtual finger to the name `my_open_book` just as before, but it also recognizes when control leaves the indented code block and automatically calls `close` on `my_open_book`. I wish I had something like this when I was a kid. 

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

## ALE 1.5: Using the while condition

Chapter 1 develops `anybook.py` using a while-loop with a condition that always true. This allowed me to separately explain that our solution needs both a looping construct (i.e., the ability to repeatedly execute a block of code for each line in the input file) and an ending condition that stops the loop.

```{code-block} python
---
lineno-start: 1
---
### chap01/anybook.py
my_book = input('What book would you like to read? ')
my_open_book = open(my_book)

# Print every line in the book
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')

    # Check for EOF
    if the_line == '':
        break

print("The End.")
```

* Line 6 in `anybook.py` provides the looping construct; lines 7-12 are the loop body.
* Line 11 checks the loop's exit condition (i.e., it checks to see if there are no more lines to read).
* Line 12 breaks out of the loop when the exit condition is true( i.e., execution continues on line 14).

Python's while-statement allows you to specify all three of these things in a compact form. To do this, we must move the exit condition into the while-condition and flip its sense. Why flip its sense? Because the while-condition (i.e., where we had previously written `True` to create an infinite loop) indicates the condition under which the loop *continues*. It's now not an exit condition, but a continue condition.

The loop in `anybook.py` continues whenever the line we've read from `my_open_book` is not the empty string. So, we flip the exit condition from `the_line == ''` into the continue condition `the_line != ''` and replace `True` in line 6 with this continue condition.

But moving this condition from line 11 in `anybook.py` and having it replace `True` in line 6 means that we move the use of the value named `the_line` before its definition (on line 7). In other words, the first time we hit line 6, `the_line` will be an undefined name. There are two solutions to this problem, and you'll code both of them.

**Step 1.** One solution is to lift the reading of the first line in `my_open_book` to be before the while-loop, as shown in `ale05a.py`. Your job is to figure out what belongs in this while-loop's body. As a hint, look at what we previously did in the loop body in `anybook.py` and remember that we definitely don't need lines 10-12. Make sure to run your solution to verify that it works.

```{code-block} python
---
lineno-start: 1
---
### chap01/ale05a.py
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

# Read the first line in the book, if it exists
the_line = my_open_book.readline()

# Loop printing every line in the book
while the_line != '':
    # REPLACE ME with the correct loop body

print("The End.")
```

**Step 2.** Another solution defines the name `the_line` but doesn't set it to the value of the first line in `my_open_book`. What value should we give `the_line` if not the first line in the file? Well, any value that allows the while-loop to continue will work, and for our script, that's any value except the empty string. The script `ale05b.py` sets up this version of our while-loop, and your job is again to write the loop's body (and verify what you write works).

```{code-block} python
---
lineno-start: 1
---
### chap01/ale05b.py
my_book = input('What book would you like to read? ')
my_open_book = open('txts/' + my_book)

# Define the name `the_line` and set it to a junk value
the_line = 'JUNK'

# Loop printing every line in the book
while the_line != '':
    # REPLACE ME with the correct loop body

print("The End.")
```

**Step 3.** Which solution, including the original `anybook.py`, do you like best? Why do you like it? There's no definitely right answer to the first question.

**Step 4.** You'll probably run into experienced programmers that tell you not to write infinite loops (i.e., our original `while True` loop). I'm not one of those programmers. I believe in using the solution that best expresses a solution to the problem in front of me, and because the problem in Chapter 1 doesn't have a reason to define `the_line` before the while-loop, an infinite loop with an explicit exit condition check feels natural to me. It also makes the while-loop body easy to read: grab a line from the input file; print it; and check to see if we're done with the file. Of course, you can switch the order of the print and check if you're uncomfortable printing the empty string.

The script we wrote in Step 1 minimizes the work that the script does when it runs, and the tradeoff is that you have to write the `readline` command twice. You might also not like the fact that the reading of a line and the printing of it are split over two loop iterations (i.e., you read it in one loop iteration; jump back to the `while` and check the continue condition; and finally print what you read in the next loop iteration if what was read wasn't the empty string). In other problems, you'll have values you need to check the continue condition before you start the loop, and nothing might need to be split over loop iterations. When this occurs, you should definitely use the compact form of the while-loop.

The script we wrote in Step 2 keeps the work that goes together (i.e., read and print) within one loop body, and it puts the exit condition in the while-condition (with its sense flipped, of course). The cost is that we need to make sure we get into the loop (where we do `readline`) on the first iteration.

No matter which of these solutions appeal to you just remember that the definition of `the_line` before the while-loop in Steps 1 and 2 are really part of that loop's work.

Overall, each of the three approaches is a fine solution, and you should use the one that makes the most sense to you.

\[Version 20240806\]