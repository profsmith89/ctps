# Chapter 2 #

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
