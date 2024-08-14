# Chapter 3: Replace Text With Emoji #

To this point, we have written scripts using the core of the Python language. These scripts were for us. We adorned them with comments and variable names indicative of the code's purpose, but we didn't expect anyone else to read them. And while we have learned how to direct a computer like an actor, the world for which we programmed was small: our dramatic stage limited to the machine in front of us. This is a proven way to get started, but also terribly dull and constraining.

Modern machines are connected through networks to an interesting and diverse world, and our problems will be easier to solve if we learn to leverage the libraries of code shared by the world-wide community of programmers and data scientists.[^fn1] This is where the real fun happens.

```{admonition} Learning Outcomes
In this chapter, you will learn about character sets and functional abstraction by adding emojis to the stories we read. It is our first step toward engaging with the world around us, and by the end of the chapter, you will be able to:

*   Understand the importance of software internationalization and what it means to localize it [design];
*   Describe the purpose and importance of standards [design];
*   Use Unicode and recognize different encoding standards [CS concepts and programming skills];
*   Employ decomposition to stage the design and implementation work you do as you incrementally build toward a problem's full solution [design];
*   Explain the difference between and work with mutable and immutable objects [CS concepts and programming skills];
*   Design and implement functions as well as enumerate their software engineering benefits [CS concepts and programming skills];
*   Understand the operation of functions and the basic runtime difference between functions and methods [CS concepts and programming skills];
*   Use docstrings and think about what a function assumes and what it guarantees [programming skills];
*   Discuss what makes functions and scripts well-specified [programming skills];
*   Articulate the design tie between abstraction and decomposition and describe what it means to adhere to an abstraction barrier [design];
*   Recognize the use of functional abstraction and the basics of data abstraction [CS concepts];
*   Create and use Python lists [programming skills];
*   Begin to use Python modules in your coding projects [programming skills].
```

## Internationalization

I ended the last chapter with a disclosure: What we think of as an individual character is simply a Python string of length 1. This is a beautiful simplification that makes it easy to write a script that processes characters and strings. But you might wonder, "Strings in whose native tongue?" To create programs that solve the world's problems, our computers and the software that runs on them must handle the character sets of the world's many human languages.

The software community uses the term *internationalization* when it describes the process of building programs and systems that adapt to the multitude of human languages and dialects. You will also hear the software community talk about the process of *localization*, which takes internationalized software and configures it for a specific language and locale.

This might be a bit abstract, and so let's take a specific example. I'm writing this book on a MacBook Pro running MacOS Mojave. Does Apple sell this laptop and operating system around the world? You bet it does. Does every person purchasing a MacBook Pro speak English like me, or want to interact with it in English? Definitely not. MacOS Mojave is internationalized so that Apple can sell its laptops around the world. A particular laptop is then localized when sold to a particular customer, like me. We can see this by looking at the "Language & Region" controls in the "System Preferences." Mine says that English is my preferred language, but I could change it to one of many other options.

In this chapter, we are going to focus on just one piece of this large and complex problem of internationalization: computing with the multitude of characters in the world's many human languages. Lucky for us, much of the complexity in even this well-defined problem is hidden from you as a programmer. While we don't have to understand everything about *the problem of encoding* every character in every human language, we also can't be ignorant of it.

## Encoding

Let's begin with a demonstration of this encoding problem using `script32.py` from the last chapter's ALE 2.7.[^fn2] We'll feed a small excerpt[^fn3] from *The Cat in the Hat*, written not in English (`CatInTheHat-excerptE.txt`) but in Spanish (`CatInTheHat-excerptS.txt`) to this script. Notice that the Spanish version uses punctuation marks that appear in Spanish but not English. It also correctly includes accents that help readers distinguish the Spanish word *el* (meaning 'the') from the different word *él* (meaning 'he').

```{code-block} python
---
lineno-start: 1
---
### A portion of the excerpt files
lines_in_English = '''The Cat in the Hat!
And he said to us,
"Why do you sit there like that?"'''

lines_in_Spanish = '''¡El Gato Ensombrerado!
Y él nos dijo,
"¿Por qué se sientan así?"'''
```

```{admonition} You Try It
What happens when you feed the Spanish excerpt to `script32.py`?
```

The script works with both inputs, but why? 

The short answer is that we were lucky. The program I used to translate the English excerpt into Spanish used the same character encoding scheme that Python uses when reading files as plaintext. If the translation program's character encoding scheme and Python's default were different, our script would have thought each character was one thing when in fact it was something very different.

It might help you to relate this idea to the concept of false friends in English and Spanish. False friends are words in Spanish that look a great deal like English words, but the Spanish words mean something different than their English doppelgängers. An example false friend is the Spanish word *largo*, which means long but looks like the English word large. Same encoding, different meanings.

Back in the world of Python, we can specify what encoding the built-in function `open` should use when reading a file by setting its `encoding` input parameter. Python's default value for this parameter is `utf-8`. If we set this parameter to `latin-1`, we'll get some garbage in the dialogue that's printed. In telling Python to use the wrong encoding for the file, I've done something like telling you that `CatInTheHat-excerptS.txt` is written in English. You'd be confused when trying to interpret the Spanish text as English, and so is Python when I tell it to read a utf-8-encoded file as a latin-1-encoded one.

```{admonition} You Try It
Go ahead and try this by using each one of the three different with-as-statements included in `script32.py`.
```

## Standards

`utf-8` and `latin-1` are two of a large number of different encoding *standards*. You'll find standards all throughout our physical and virtual worlds. A standard is simply a documented and widely agreed-upon method for doing something.

We like to think that well-crafted standards are the way we have always done things, but more often than not the path to a standard was filled with conflict and heated disagreement. Take the painted lines on our roads and highways. I'm old enough to remember when the standard color of the centerline that separates traffic traveling in different directions was changed (in the early 1970s) from white to yellow. Perhaps Oregon felt some vindication for it was the last U.S. state in 1954 to stop using yellow painted lines on its roads and join the other 47 states in adopting white as the standard color for the country's new interstate highway system. And this earlier standard traces its roots to a painted centerline along Trenton's River Road in Wayne County, Michigan. Edward N. Hines, the chairman of the Wayne County's Board of Roads, insisted on the painting of a white line after seeing a milk wagon leave a white trail on the road in front of him.[^fn4]

While the individual states eventually agreed upon today's standard, this U.S. standard is not the standard you find in many other countries. Despite the headaches these different international standards can cause drivers, there is an even greater need for an agreed-upon standard way of encoding characters on the Internet. Have you ever visited a website and seen garbage appear on your screen? If you have, you've probably encountered a character-encoding problem.

By the way, if you're interested, you can begin an exploration of the checkered history of encoding standards by reading about Unicode, the standard we use and explore next.[^fn5]

## Unicode

The Unicode standard maps each language character to a unique *code point* in a *codespace*. This approach attempts to take every character in every language in the world and maps them into a rigorously defined space of values. For example, the code point for the capital letter "A" is `U+0041`, while the code point for the division sign "÷" is `U+00F7`. The four digits at the end of each of these two code points represent a hexadecimal number, which we will learn more about in a later chapter. For now, we simply need to know the code point number for each character we want to specify.

You should notice a couple of things here. First, code points simply tell you the character. A code point does not tell you what font to use for that character. Nor does it tell you whether the character should be displayed in bold or italics, or even in what color. What sounds simple---display a capital "A" please---is actually a complex system of standards and interacting systems, each with their own special purpose and function. While we will discuss some of this, let's stick for the time being with the goal of how to specify different characters.

## A new problem

The other interesting thing about Unicode is that it includes not only letters, but symbols. In fact, the concept of a character is quite liberal, encompassing even emoji. For instance, the cat-with-wry-smile emoji (😼) is codepoint `U+1F63C`. Let's use that to make the stories our scripts read a bit more fanciful.

In particular, let's start our work with the script from Chapter 1 (slightly improved), which reads a specified children's book.

```{code-block} python
---
lineno-start: 1
---
### chap03/replace0.py
my_book = input('What book would you like to read? ')
print()   # print a blank line between question and script's output

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()
        print(the_line, end='')

        # Check for EOF
        if the_line == '':
            break

print("\nThe End.")
```

What we eventually want this script to do is to *replace* some of the words with emoji characters as it prints out the book's text. Which words? Let's replace every instance of "Cat" with 😼 and "Hat" with 🎩.

## Decomposition to reduce the problem's complexity

Before we try to solve this entire challenge in one go, let's ask if we can break it into simpler pieces. What do you think?

```{tip}
Remember! Decompose complex problems in a sequence of simple steps and you'll solve your challenges every time!
```

Of course we can. Forgetting about emojis for a moment, we might start by asking how we replace *one word with another* in a Python string. If we can accomplish this, then we can move to replacing *one word with an emoji*, which involves understanding how to print code points in a Unicode string. From there, we can move to replacing *every instance of a word in a line*, and ultimately *every instance of multiple different words*. Then bam, we've solved our problem!

## Strings as immutable sequences

We'll begin by learning how to replace one word with another. We could code this functionality ourselves, but we have already seen that Python provides a number of useful methods that operate on sequences and Python strings are sequences. Before reading on, skim the [Python documentation](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range) that lists the pre-programmed operations for sequences.

We have used a number of these operations in our previous scripts, but nothing in this list helps us replace part of a sequence with another sequence.

Scroll down to [the table of operations on *mutable* sequence types](http://docs.python.org/3/library/stdtypes.html#mutable-sequence-types). Mutable means that we can change an object's pieces, and with a mutable sequence, we can replace the sequence's items. The first example in this list of operations does exactly this using syntax familiar to us: `s[i] = x` where `s` is a mutable sequence and `x` is some object that should replace the current object in `s` at index `i`.

If we think back to the bookshelf example from the last chapter, this is like replacing this book on my shelf with a different one (e.g., a new version of it). I haven't made a new copy of my bookshelf; I simply replaced one item in it with another. In code as in life, the old version of my bookshelf no longer exists.

This could be the basis of replacing a word in `the_line` with a different word, but first we should test out this method with a simple example in the interactive Python interpreter.

```{code-block} python
---
lineno-start: 1
---
s = "just me"
s[0] = 'b'
```

Oops, this produces an error. The interpreter reports that a `str` object doesn't support item assignment. What it is trying to tell us is that, while a `str` object is a sequence, it is not a mutable sequence. Python strings are *immutable* sequences, and thus we cannot use any of the methods for mutable sequence types. Once you create a string object, you cannot change the contents of that string. However, string processing is a common operation in programming and Python does provide [a whole set of methods we can apply to strings](https://docs.python.org/3/library/stdtypes.html#string-methods). And in this list we find [`replace`](https://docs.python.org/3/library/stdtypes.html#str.replace)!

Because Python strings are immutable, the `replace` method states that it creates and returns a new string object with all the old subsequences replaced with the new subsequence. Make sure that you understand that `replace` left the original string object untouched when it created a new string object of the form we desired. We'll talk in a later chapter about why Python chose to make strings immutable, but for now we know enough to solve the first step in our challenge.

```{code-block} python
---
lineno-start: 1
---
### chap03/replace1.py
my_book = input('What book would you like to read? ')
print()   # print a blank line between question and script's output

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()

        # Having some fun with text substitution
        the_line = the_line.replace('Cat', 'Gato')

        print(the_line, end='')

        # Check for EOF
        if the_line == '':
            break

print("\nThe End.")
```

Line 10 of `replace1.py` applies the `replace` method to the object named `the_line`, and it takes the object `replace` returns and updates the name `the_line` to refer to it. Notice that we no longer have a way to access the string object that we read from `my_open_book`. That's ok, since we don't need that object any longer.[^fn6] 

Notice also that replacement is:

* Case sensitive. The letter 'c' is not the same as the letter 'C'. 'c' is Unicode code point `U+0063` and 'C' is `U+0043`.
* Not limited to the length of the replaced text. `'Gato'` was longer than `'Cat'`. 

```{admonition} You Try It
Try other substitutions by changing line 10 in `replace1.py`. Make sure you learn how `replace` handles multiple copies of the old subsequence in the input string, as illustrated with the statements in the next code block.
```

```{code-block} python
---
lineno-start: 1
---
s = 'Just me. Just me. Just me.'
s.replace('me', 'you')
```

## Encode an emoji in Unicode

On to the second step, which has us substitute the string `'Gato'` with the Unicode code point for the cat-with-wry-smile emoji. You've already done something like this when we talked about how to express a carriage return. We used the special character sequence `'\n'` where the `'\'` told the Python interpreter that the next character wasn't a simple `'n'` but an encoding of something we couldn't express in a single printable character. We will do the same thing here.

Python provides two ways to specify Unicode code points: We can use '`\u`' followed by four hexadecimal characters if the character we want is at the start of the Unicode code space, like the letter 'G'.

```{code-block} python
---
lineno-start: 1
---
print('G', '\u0047')
```

For Unicode code points with encodings requiring more than four hexadecimal digits, like the cat-with-wry-smile emoji, which is codepoint `U+1F63C`, we must use the '`\U`' escape. It tells the Python interpreter to expect eight hexadecimal characters. 

```{code-block} python
---
lineno-start: 1
---
print('G', '\U00000047')
print('G', '\U0001F63C')
```

Most of us don't find it easy to remember these seemingly random sequences of letters and numbers, and so there's one other escape sequence you might find useful.

```{code-block} python
---
lineno-start: 1
---
print('G', '\N{cat face with wry smile}')
```

```{tip}
Be careful not to confuse `\n` and `\N{emoji_name}`. 
```

If you want to learn more about Unicode and play with different emojis in your programs, please see Python's [Unicode HOWTO](https://docs.python.org/3/howto/unicode.html) page, the homepage for [unicode.org](https://home.unicode.org), and info about the [cat-with-wry-smile emoji](https://emojipedia.org/cat-with-wry-smile).

Using all this new knowledge, we can code a solution for steps 2 and 3:

```{code-block} python
---
lineno-start: 1
---
### chap03/replace23.py
my_book = input('What book would you like to read? ')
print()   # print a blank line between question and script's output

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()

        # Having some fun with text substitution
        the_line = the_line.replace('Cat', '\N{cat face with wry smile}')

        print(the_line, end='')

        # Check for EOF
        if the_line == '':
            break

print("\nThe End.")
```

## Multiple different replacements

Let's now extend our script so that it makes replacements for "Cat" and "Hat". This task is fairly straightforward because the two substitutions are independent, and by that I mean that one substitution doesn't insert something that the other is expected to replace. As such, we simply need to run every replacement on every line we read.

```{code-block} python
---
lineno-start: 1
---
### chap03/replace4.py
my_book = input('What book would you like to read? ')
print()   # print a blank line between question and script's output

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()

        # Having some fun with text substitution
        the_line = the_line.replace('Cat', '\N{cat face with wry smile}')
        the_line = the_line.replace('Hat', '\N{top hat}')

        print(the_line, end='')

        # Check for EOF
        if the_line == '':
            break

print("\nThe End.")
```

## Feeling overwhelmed?

While you should feel a real sense of accomplishment for all that you've learned, you might also be feeling a bit overwhelmed. How do you know if a programming language provides you with a piece of functionality like `replace`? And even if you know that it does, how do you know the name the language designer chose for that functionality? 

The simple answer is, you don't. Python provides us with a pretty decent set of documentation pages, and we are fortunate to live in a world where sensible questions fed to a search engine or chatbot will often produce a great start on a set of possibly helpful answers.

Of course, if you don't realize that the functionality you need already exists, you can always build the functionality yourself. Let's see how this is done.  

## Functions

Let's pretend that there is no string-replace method built into Python. How would we write a script that does what `replace4.py` does? Without access to Python's `replace` method, we would probably write some pseudocode that looks as follows. 

```{code-block} python
---
lineno-start: 1
---
### chap03/replace5.py
my_book = input('What book would you like to read? ')
print()   # print a blank line between question and script's output

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()

        # Having some fun with text substitution
        # my code to replace 'Cat' with '\N{cat face with wry smile}'
        # my code to replace 'Hat' with '\N{top hat}'

        print(the_line, end='')

        # Check for EOF
        if the_line == '':
            break

print("\nThe End.")
```

If we needed this functionality only once in our program, we might write code right where we have these lines of pseudocode. We, however, need the functionality twice, not once. Would we want to write out this code twice?

Probably not. Think about what happens if you find a mistake. You'd have to fix that mistake everywhere you copied the code. That is a bookkeeping nightmare. Furthermore, as the form of the pseudocode comments above illustrate, there is probably a lot of similarity in these two codes. Both replace a specified substring in a string with a second substring. The only difference between the two is the existing and new substrings specified. Wouldn't it be nice to write the code for our `replace` operation once and invoke that code whenever we needed it?

Yes it would. Programming language support for *functions* (also called *procedures* and *methods*) provides us with exactly this ability. By separating the implementation of a function from the possibly many places where you invoke it, you make it easy to fix a function's implementation if it contains a mistake or if you find some aspect of its behavior (e.g., how fast it runs, as we'll discuss in later chapters) is not what you hoped it would be.

## Function definitions

We are ready to expand our pseudocode to include the concept of a function definition. When you define a function, you should think about creating two important pieces: a *public interface* and a *hidden implementation*. 

```{code-block} python
---
lineno-start: 1
---
### chap03/replace6.py

# specify the interface for my_replace
#     specify the implementation of my_replace

my_book = input('What book would you like to read? ')
print()   # print a blank line between question and script's output

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()

        # Having some fun with text substitution
        # call my_replace on the_line, specifying 'Cat' is replaced by '\N{cat face with wry smile}'
        # call my_replace on the_line, specifying 'Hat' is replaced by '\N{top hat}'

        print(the_line, end='')

        # Check for EOF
        if the_line == '':
            break

print("\nThe End.")
```

The interface is an abstract way of thinking about the function's operation. We had no idea, for example, how Python's string-replace method worked when we used it in our earlier script.  We simply knew that the documentation said `str.replace(old, new)` returned "a copy of the string `str` with all occurrences of substring `old` replaced by `new`." This was sufficient knowledge for us to use the method. Similarly, the interface we specify for our `my_replace` function should be sufficient for others to know how to use it.

The rest of the function definition is its concrete implementation. An implementation is simply a set of Python statements just like the ones we have been writing, but with one twist: We need to learn how to send data to our function and return any results the function computes.

In general, you can think of a function as a script that performs a specific action using data you send it. And just as we have been running our scripts numerous times, you can run this new function numerous times with different data and get back possibly different results.

## The actual function definition and its invocation

Let's now fill in our pseudocode with actual Python code.

```{code-block} python
---
lineno-start: 1
---
### chap03/replace7.py
def my_replace(s, old, new):
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


my_book = input('What book would you like to read? ')
print()   # print a blank line between question and script's output

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()

        # Having some fun with text substitution
        the_line = my_replace(the_line, 'Cat', '\N{cat face with wry smile}')
        the_line = my_replace(the_line, 'Hat', '\N{top hat}')

        print(the_line, end='')

        # Check for EOF
        if the_line == '':
            break

print("\nThe End.")
```

In Python, function definitions begin with the keyword `def`. We follow this keyword with the name we would like to use for our new function, which is `my_replace` in this example. 

What is this name exactly? We previously talked about variable names, which permit us to give names to the values our script computes and manipulates. Function names serve a similar purpose, except that the value they name is a list of code statements that we need executed at one or more points in our script. For our problem, this need occurs on lines 26 and 27 of `replace7.py`.

After the function's name in the function definition, we write inside parentheses a list of what are called the *formal parameters of the function*. In our example, we want our function to take three strings, which we name `s`, `old`, and `new`. `s` is the string in which we want to make replacements. `old` is the substring we want to replaced, and `new` is the substring that should replace `old` wherever it is found in `s`. In general, the formal parameters specify the input values needed for the function to perform its work.

To complete the function interface definition in Python, we end this line with a colon (`:`), as is done with any compound Python statement. The indented lines that follow are the function's implementation. In the example above, the implementation, which is also called *the body of the function*, comprises lines 3-15. The body is the set of statements the Python interpreter will remember as named `my_replace`. 

A function's interface tells the Python interpreter, as well as humans reading the definition, how to invoke the function. To *invoke* (or *call*) a function, we use its name and specify what input values will be matched with each of the function's formal parameters. These input values are called the *actual parameters*. Lines 26 and 27 illustrate two such calls with their parenthesized lists of actual parameters.

When a function call is executed by the Python interpreter, each actual parameter is bound to its corresponding formal parameter. In the simplest case, this binding is done, as it is done here, in list order: the first actual parameter is bound to the first formal parameter; the second to the second; and so forth.[^fn7]

In plain English, what does this binding mean? It means that we give another name to each object in the actual parameter list. For example, `the_line` names a string object, and as part of the invocation of the `my_replace` function when the Python interpreter executes line 26, that string object will also be given the name `s`.

## Function execution

It's important to realize that we have switched from talking about writing a function to a discussion of what happens when it executes. It might be helpful to state all together what happens when we tell the Python interpreter to execute `replace7.py`, which includes a function call. 

We start at the beginning of our script's execution, i.e., immediately after we have typed `python3 replace7.py` and hit return. Like a script reading a plaintext file, the Python interpreter starts reading from line 1. Our script's first line is a comment, and the interpreter moves to line 2. The statement on line 2 is a function definition, and like a new variable definition, it causes the Python interpreter to add the name `my_replace` to the currently active namespace and associate this new name with the list of statements in the function's body. To be clear, none of the statements in the function's body are themselves executed at this point.

When the Python interpreter is done with this work, it moves on to line 18, where it asks the user for a filename. It opens that named file on line 21 and adds `my_open_book` into the current namespace. Line 22 then tells the interpreter that the indented statements below it are the body of an infinite loop. The interpreter continues sequentially, reading the statement on line 23, where it proceeds to create a string object containing the first line of the input file and associating the name `the_line` with this string object. Because this is the first time we have encountered the name `the_line`, the interpreter also adds this name to the current namespace. The interpreter ignores line 25, which contains only a comment, and finally we are at line 26, the first of the two calls of our function `my_replace`.

In executing line 26, the interpreter looks up what it knows about `my_replace` in its namespaces and finds the definition it processed earlier. It binds the actual parameters on line 26 to the formal parameter names in the function's definition, and it begins the execution of this function by next processing line 3.

Execution inside the function proceeds as you would expect. Because the formal parameter names are in the current namespace and they have had values bound to them, when line 4 is executed, the Python interpreter knows that it should assign the value `3` (i.e., the length of the string `'Cat'`) to the name `j`. 

We won't step through every statement in the body of `my_replace`, but you should note that the `while` loop in this body eventually terminates because `s` (i.e., the first line read from the input file) has a finite length and this `while` loop increments `i` on every execution of its body (i.e., technically on every possible execution path through the loop's body). This means that the Python interpreter eventually reaches line 15, which is a new type of statement for us.

Line 15 tells the interpreter that we are done with the function's execution and that processing should return to the statement that began this particular instance of the function's execution. For us at this time, this is line 26. 

Return-statements in Python may include an expression, and the value of this expression is the value that the Python interpreter uses as the result of the function call. Looked at from the perspective of the function call (on line 26), a function call in Python acts like an expression, and like any expression, its execution produces a value.

Continuing our example, the function call on line 26 returns a new string object that is a copy of `s` with its `old` subsequences (i.e., `'Cat'`) replaced by `new` subsequences (i.e., `'\N{cat face with wry smile}'`). Line 26 assigns that returned string object to the name `the_line` so that we can then on line 27 replace any `'Hat'` substrings in it with the Top Hat emoji.

```{tip}
A function returns if the interpreter reaches the end of the function body without encountering a return-statement. The result returned is the special object `None`, which is also the result when a return-statement has no expression.
```

## Abstraction, decomposition, and algorithms

Let's pause for a moment and rise above the details of building a function and learning how it executes. Our goal was to print a story from a file to our terminal screen while replacing occurrences of some words with some funny emoji. This short description pretty well sums up what our script does. It describes *the script's purpose*. 

While writing the Python code, we learned a couple of different ways to replace a portion of a string with another string, with the last version of our script implementing our own string replace. Notice that the script's purpose doesn't include this level of detail. The Python interpreter needs these details (i.e., how it should replace words in a string), but in solving the problem, we don't particularly care if the interpreter uses `my_replace` or Python's built-in `str.replace` method.

This is *abstraction* at work, and it is one of the benefits of using functions. With them, we can hide necessary but unimportant-to-the-script's-main-purpose details. In fact, even though the implementation of `my_replace` was sitting there in lines 3-15 of `replace7.py`, did you take the time to read and understand exactly how it works? Be honest. Probably not many readers did, and that, my friend, is abstraction at its finest! We used `my_replace`. We talked about it. You (hopefully) understood its interface and could use it yourself, but you didn't ever bother to worry about how it exactly did what you thought about it doing.

Abstraction is a big idea in computer science, but don't let that frighten you. It is something that each of us does all the time in life. As you gain experience in problem solving, you'll find that abstraction goes hand-in-hand with *decomposition*, i.e., the breaking down of a large, complex problem into simpler steps, as we did to solve this chapter's problem. We began by abstractly thinking about replacing a part of a string with a different string, and we weren't initially concerned with how we would accomplish this replacement. Only later did we ask whether this replacement was something we had to write for ourselves or was a piece of functionality someone else had already written for us.

The result of all this abstraction and decomposition is an *algorithm*. An algorithm is simply a sequence of well-specified steps that when executed allow a computer (or us acting like a computer) to solve a problem. Lines 3-15 are an algorithm for replacing all occurrences of the `old` substring in string `s` with the `new` substring. Lines 18-35 are an algorithm for reading a story from a file, replacing each occurrence of `'Cat'` and `'Hat'` with some fun emoji and printing the modified story to the terminal screen.

## Definition before use

I said, "a sequence of well-specified steps" when defining an algorithm, and by "well-specified" I mean that the Python interpreter (or a computer in general) needs to know how to execute each specified statement it encounters. In `replace7.py`, the interpreter knows how to execute line 23, for example, because `readline` is a piece of built-in functionality. It knows how to execute line 26 because the Python interpreter had earlier encountered the definition of `my_replace`.

What would happen if we placed the definition of `my_replace` (lines 2-15) at the end of our script instead of at its beginning? In this case, the interpreter would encounter the name `my_replace` before seeing its definition, and it would exit with the message: `NameError: name 'my_replace' is not defined`. "But it is!" you would say. "But it isn't when I read the script from start (top) to end (bottom)," the interpreter would reply.

```{tip}
Computer programs are not mystery novels. Computers do not like ambiguity and do not understand suspense. You must give definitions to the names you use before you use them.
```

## Python's special variables

A design pattern that we will frequently use to avoid this sort of problem is illustrated in `replace32.py` below. Pay particular attention to the two statements I appended to the very end of the script, and what I did to the statements that described the main action of the script. 

```{code-block} python
---
lineno-start: 1
---
### chap03/replace32.py

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


def main():
    my_book = input('What book would you like to read? ')
    print()   # print a blank line between question and script's output

    with open('txts/' + my_book) as my_open_book:
        while True:
            the_line = my_open_book.readline()

            # Having some fun with text substitution
            the_line = my_replace(the_line, 'Cat', '\N{cat face with wry smile}')
            the_line = my_replace(the_line, 'Hat', '\N{top hat}')

            print(the_line, end='')

            # Check for EOF
            if the_line == '':
                break

    print("\nThe End.")

if __name__ == '__main__':
    main()
```

At the end of the script, I used one of those special names we saw in Chapter 1, when we looked at  Python's built-in `dir` command and discussed namespaces. The if-statement asks whether the special variable `__name__` is equal to the string `'__main__'`.  The Python interpreter sets `__name__` to this string if it is reading a script on which it was invoked. In other words, is this script the main thing it is being asked to do? Using our theater imagery, is it the star of the show? If it is, `__name__` has the value `'__main__'`. We'll see in a moment why the interpreter might give this special variable another value, but for now, this if-statement protects the invocation of a function called `main`.

In addition, I wrapped the "main" part of our script (formally lines 18-35 of `replace7.py`) in a function definition called `main`. So, instead of the Python interpreter starting execution with the input-statement (line 18), it simply remembers that statement as part of the function definition of `main`. By putting lines 40-41 as the last statements in our script, we can list `main` before `my_replace` or `my_replace` before `main`. Either way, both definitions will exist in the namespace before the interpreter gets to our script's computational work.

```{tip}
Use this design pattern of wrapping your script's "main" work in a function called `main`, and protect its invocation with: `if __name__ == '__main__'`. You can then place `main` as the first or last routine in your script.
```

## Docstrings

I added one other new line to `replace32.py`, and that is the string on line 4.[^fn8] This is called a *docstring*, and it is simply a string literal that, as [PEP 257](https://www.python.org/dev/peps/pep-0257/) states, "occurs as the first statement in a module, function, class, or method definition."[^fn9] Its purpose is to explain anything important that the function's interface leaves unsaid.

For example, the interface to `my_replace` indicates that we need to call it with three input parameters, but it provides no clues as to what value will be returned from the function's execution. A docstring should tell us what's returned. We might also use the docstring to say a bit more than the function's name tells us about its purpose. 

Unlike line 4, a docstring does not have to be a single line in length. Some people use docstrings as a function's formal specification. It outlines the contract between the person who wrote the function and the many people who will use it. John Guttag in his book on Python[^fn10] helpfully describes a function specification as having two primary pieces: (1) a description of the *assumptions* that the function makes about, for instance, its input parameters; and (2) the *guarantees* that the function provides if the assumptions are met. 

```{tip}
Because the functions I write are described in detail in this book, I often won't include a docstring. You should, however, use them in your code writing. This is a case of do as I say and not as I do!
```

## Getting a feel for abstraction

Functions provide us with a particularly useful abstraction. With experience, you'll gain an intuitive sense for what makes a good procedure, i.e., a useful procedural abstraction. Let's start you on this experiential journey.

Some software engineers argue that a function should have a single, well-defined job. Our `my_replace` function is a good example of this. As we develop programs that solve more complex problems than the ones we have seen to date, we will begin to understand why these engineers say that if a script performs a number of distinct tasks in sequence, you should create a procedure for each of those tasks. Of course, in programs that solve quite complex tasks, you may sensibly choose to gather a sequence of procedure calls into the body of a procedure that itself describes a coarser-grained action. This, again, is decomposition at work.

However, these same engineers will tell you that your functions should be as general as possible. Are these not conflicting pieces of advice?

Consider the following: If the problem we were trying to solve required us to compute both the square and cube of a number, should we create two separate functions, one to compute a number's square and another to compute its cube?[^fn11] Or instead, should we create a single function called power that can compute both answers? While our hypothetical square and cube functions would take only a single input parameter, our power function would take two parameters: a base and an exponent.

Do you feel that writing `square(6)` and `cube(5)` throughout your script is better than `power(6, 2)` and `power(5, 3)`? Or does it bother you that the implementation of the `square` and `cube` functions are nearly identical? Different people give different answers.

```{tip}
Abstraction and decomposition are more art than science. You should take anyone's rules about what you should do around functional abstraction as guidelines, not absolutes.
```

On top of considerations of readability, understanding, and maintainability to which we have alluded, the particular abstractions employed by software designers are sometimes also influenced by other considerations, like performance and security. Take the Python built-in function `pow` that takes three formal parameters: a base, an exponent, and an optional modulus. It's not obvious to first-time programmers why `pow(base, exp, mod)` is a useful abstraction. Well, this abstraction exists because the computer will be quicker to compute `pow(base, exp, mod)` than the semantically equivalent `pow(base, exp) % mod` expression, based on some tricks hidden in Python's implementation of `pow`. 

## Another kind of abstraction

Let's return to our `my_replace` function and think about it in terms of these abstraction and decomposition guidelines. As mentioned earlier, `my_replace` has a single, well-defined job: it makes a copy of the input string `s`, finds all occurrences of `old` substring in this copy, replaces these occurrences with the `new` substring, and returns the modified copy.

Now let's ask, does `my_replace` work only on string objects? Our docstring says so, but that's just documentation. What we want to know, in the words of the second principle mentioned above, is it more general?  In other words, will it execute successfully with any sequence object, which strings are simply one particular kind? For example, we discussed my office bookshelf as another example of a sequence object. Can we use `my_replace` to replace a subsequence in a bookshelf object?

Notice that we're talking about two kinds of abstraction here. In this chapter, we have been discussing *functional abstraction*, which allows us to think about the effect of some block of statements (i.e., a function's body) on the state of our program without having to have detailed knowledge of the function's body. Our new question talks about *data abstraction*. This type of abstraction has us think about a specific object as an instance of a more general object. In our example, a string is a specific type of the more general sequence object. When thinking about a string object as a general sequence object, we ignore the characteristics of the object that make it a string, and we focus only on those characteristics that are true of every sequence object. 

Strings and bookshelves are both a kind of sequence, and as such, we can ask for their first elements. In fact, every (non-empty) sequence object has a first element, and thus we can ask that question of any sequence object. On the other hand, not every valid question about a string object makes sense for some hypothetical bookshelf object. I can ask if a string is capitalized, but it's nonsensical to ask if a bookshelf is capitalized.

Data abstraction allows us to think about a piece of data without having to have detailed knowledge of all its characteristics. What's particularly powerful about this is that it allows us to write functions that operate on both strings and bookshelves.

Is `my_replace` one such function? Does it operate not on string objects per se, but any sequence object? Is its single, well-defined job to make a copy of the input sequence `s`, find all occurrences of the `old` subsequence in this copy, replace these occurrences with the `new` subsequence, and return the modified copy? There's one way to find out. Let's try it!

## Lists are sequence objects

I've copied our `my_replace` function into `bookshelf1.py`. The first block of code in `main` (lines 21-25) uses `my_replace` as a function that processes string objects, as we did in `replace32.py`. Unsurprisingly, this call to `my_replace` should work, and we include it simply to reassure ourselves that no sleight-of-hand is taking place.

```{code-block} python
---
lineno-start: 1
---
### chap03/bookshelf1.py

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


def main():
    # Create a string object with a line from The Cat in the Hat
    the_line = 'The Cat in the Hat!'
    print(the_line)
    the_line = my_replace(the_line, 'Hat', '\N{top hat}')
    print(the_line)

    # Create a representation of the objects on my shelf
    stuffed_lion = '\N{lion face}'
    kids_pic = 'kids.jpg'
    textbook = 'cs32.scriv'
    favorite_cd = 'BornToRun.mp3'
    novel = 'CatInTheHat.txt'

    # Create a sequence object that represents my shelf
    shelf = [stuffed_lion, kids_pic, textbook, favorite_cd, novel]
    print(shelf)

    # Make it look like our textbook has been opened on my shelf
    shelf = my_replace(shelf, [textbook], ['\N{open book}'])
    print(shelf)

if __name__ == '__main__':
    main()
```

More interestingly, the rest of `main` invokes `my_replace` on a Python list, a different kind of Python sequence object. Unlike Python strings, Python lists are mutable sequence objects. Python lists are also a more general kind of sequence object than strings in that you can use lists to group together lots of different types of Python objects. While I've created a representation of my office bookshelf using a list of Python strings (lines 27-32), we could easily insert an integer into this list, and the Python interpreter would have happily accepted it. Lists are a powerful data structure that we'll use often.

The simplest way to create a new Python list is to enclose a list of Python objects in square brackets, as on line 35. The second and third parameters in the call to `my_replace` on line 39 are also Python lists, albeit with only a single item in each of the constructed lists. What this call does then is invoke `my_replace` with three Python list objects. During the call, the formal parameter `s` provides another name for the Python list object named `shelf`, and `old` and `new` name the previously unnamed Python lists `[textbook]` and `['\N{open book}']`. 

If everything goes well, `my_replace` will operate on these formal parameters (i.e., these Python lists) using only their sequence object abstractions. And it will return to line 39 a new Python list object that looks a lot like the original `shelf`, but with an open book emoji replacing the name of the file in which I'm writing this book.

We are now ready to run our little experiment. 

```{code-block} none
---
emphasize-lines: 2
---
### Run the highlighted command in the shell
chap03$ python3 bookshelf1.py 
The Cat in the Hat!
The Cat in the 🎩!
['🦁', 'kids.jpg', 'cs32.scriv', 'BornToRun.mp3', 'CatInTheHat.txt']
['🦁', 'kids.jpg', '📖', 'BornToRun.mp3', 'CatInTheHat.txt']
```

It worked! Our docstring and comments are misleading. We should replace the word "string" in them with the word "sequence".

## Abstraction barriers

We'll expand our discussion of abstraction, its beauty, and its power in Act II, but before leaving this topic, I want to put in a word of warning about building abstractions. Of all the statements in `my_replace`, the one that's easiest to get "wrong" is: `new_s = s[0:0]`.

What do I mean by wrong? I mean we write something that looks like it should be correct, but it breaks what's called the *abstraction barrier*. The interesting thing about abstraction barriers is that there is no real barrier. Nothing stops us, when we write statements inside a function like `my_replace`, from reaching behind the abstraction we're trying to create and using operations that do not adhere to the abstraction. It's what Toto did in *The Wizard of Oz* when he pulled back the green curtain and exposed the man behind the Great and Powerful Oz.[^fn12] Let's think about what we don't want the Toto in each of us to expose in this statement. 

The statement begins the creation process for the new sequence object that we eventually return. Why do we create a new sequence object? Because sequence objects can be either mutable or immutable, and we need to code `my_replace` so that it works in both cases. If we reached behind the abstraction and used our knowledge that Python lists are mutable, we could write a simpler set of statements in the body of `my_replace`, but this version of our function would no longer work for strings. In this case, Toto shouldn't expose the mutability characteristic of the actual object passed as the first parameter to `my_replace`.

But historically, we started thinking of `my_replace` as a function for replacing substrings of strings, which are immutable sequences. A routine that is written to work for immutable sequence objects also works without change for mutable sequence objects. Yet even starting from this direction, it is still easy for our inner Toto to miscode the statement and break the abstraction. Consider an implementation of `my_replace` that codes this statement as: `new_s = ''`. This initializes `new_s` not as *any kind* of sequence object, but *one particular kind* of sequence object. At no point in the body of `my_replace` can we assume that `new_s` is one specific kind of sequence object.

Yet yet! We also cannot have `my_replace` return just any kind of sequence object on any particular call. If the first parameter of a particular call to `my_replace` is a string, our function had better return a string. If it is a list, it had better return a list. This is what makes `new_s = s[0:0]` the "right" statement to use. `s[0:0]` is a valid operation on any sequence object, and it makes a copy of the specific object referenced. If our sequence object is actually a string, `s[0:0]` returns a new string object. If it is a list, it returns a new list object. The fact that the start and end indices of the slice operation are the same simply says that we want a copy of the specified sequence object, but without any of the items currently in it. That, my friends, is a well-behaved Toto.

```{tip}
Abstraction is a very powerful technique, but it is harder to build and abide by an abstraction than you might at first think.
```

## Methods

You may have wondered as we went through this exercise why the statement that used the built-in string-replace method looked like this:

```{code-block} python
---
lineno-start: 1
---
the_line = the_line.replace('Hat', '\N{top hat}')
```

And the statement that used our `my_replace` function looked like this:

```{code-block} python
---
lineno-start: 1
---
the_line = my_replace(the_line, 'Hat', '\N{top hat}')
```

Or why I refer to the first as a *method* and the other as a *function*. 

Let's start with how the two are the same. When executed, a method invocation is transformed into a function call. In particular, `the_line` in `the_line.replace` becomes the first actual parameter in the eventual function call. As such, I was telling the truth when I explained that both method and function do mean: go do something complex based possibly on some input data I provide and return to me the result of the computation.  

However, the two representations do not use the same syntax because the abstraction barrier in each representation is different. Function calls may make, as we have discussed at length, some assumptions about the kind of objects being passed as parameters, but we only know about these assumptions through the generous description provided in the function's docstring or by carefully analyzing the statements in the function's body. In other words, little can be gleaned from the syntax of the function invocation about its abstraction barrier.

The syntax of a method call, on the other hand, tells us something quite specific about its abstraction barrier. Using our example, we are not calling any replacement routine, but one written specifically to work with the kind of object that comes before the name of the method. The replacement routine may not take advantage of all the specific characteristics of this object (e.g., it may treat a string object like a more general sequence object), but we know the specific routine invoked is allowed to do so. 

We will peel back more of this magic in Act II, when we talk about object-oriented programming. For now, just mentally translate the first syntax into the second if you're ever confused about what is happening in a particular Python statement.

## Modules

When we wrote our short `bookshelf1.py` script, we used our `my_replace` function from `replace32.py`. To do this, we copied the function definition (including its body) and pasted it into `bookshelf1.py`. Yet, we just finished talking about invoking the function numerous times while defining it only once. Why can't we use that single definition from `replace32.py` in `bookshelf1.py` instead of making another copy of it, continuing our desire to avoid the problems of maintaining multiple copies of the same operation?

Well, we can! Python allows you to reference definitions contained in other `.py` files using what it calls *modules*. Since a module is nothing more than a file just like the scripts we have been writing, the script we should have written that doesn't duplicate the definition of `my_replace` is as follows:

```{code-block} python
---
lineno-start: 1
---
### chap03/bookshelf2.py
from replace32 import my_replace

def main():
    # Create a string object with a line from The Cat in the Hat
    the_line = 'The Cat in the Hat!'
    print(the_line)
    the_line = my_replace(the_line, 'Hat', '\N{top hat}')
    print(the_line)

    # Create a representation of the objects on my shelf
    stuffed_lion = '\N{lion face}'
    kids_pic = 'kids.jpg'
    textbook = 'cs32.scriv'
    favorite_cd = 'BornToRun.mp3'
    novel = 'CatInTheHat.txt'

    # Create a sequence object that represents my shelf
    shelf = [stuffed_lion, kids_pic, textbook, favorite_cd, novel]
    print(shelf)

    # Make it look like our textbook has been opened on my shelf
    shelf = my_replace(shelf, [textbook], ['\N{open book}'])
    print(shelf)

if __name__ == '__main__':
    main()
```

Isn't that easy? The from-statement names a module, which is just a filename with the `.py` extension stripped off. It then lists what definitions we want to import from that module. This particular form of Python's import-statement places the imported definition names directly into the importing script's global namespace, and that is why we did not have to make any changes to the function invocations later in the script.

This form works well if we want one or just a few definitions from a module. You will also use a form of the import-statement that imports the module name itself into a script's global namespace. With this form, you can access any definition in that module by specifying its namespace. Here's what that looks like using our bookshelf example:

```{code-block} python
---
lineno-start: 1
---
### chap03/bookshelf3.py
import replace32

def main():
    # Create a string object with a line from The Cat in the Hat
    the_line = 'The Cat in the Hat!'
    print(the_line)
    the_line = replace32.my_replace(the_line, 'Hat', '\N{top hat}')
    print(the_line)

    # Create a representation of the objects on my shelf
    stuffed_lion = '\N{lion face}'
    kids_pic = 'kids.jpg'
    textbook = 'cs32.scriv'
    favorite_cd = 'BornToRun.mp3'
    novel = 'CatInTheHat.txt'

    # Create a sequence object that represents my shelf
    shelf = [stuffed_lion, kids_pic, textbook, favorite_cd, novel]
    print(shelf)

    # Make it look like our textbook has been opened on my shelf
    shelf = replace32.my_replace(shelf, [textbook], ['\N{open book}'])
    print(shelf)

if __name__ == '__main__':
    main()
```

The syntax of the function invocation should look familiar. Does it remind you of a Python string's method invocation? It should, and this is on purpose. Both use the dot notation to mean, "Use the specified namespace to find the definition of the attribute after the dot."

There are other forms of the import-statement that you can use (and ones you should not). Take a look at the [Python documentation on modules](https://docs.python.org/3/tutorial/modules.html) to learn more about them and when to use (or why not to use) them.

## Revisiting '\_\_main\_\_'

A module is just another name for a Python script, which contains definitions and other Python statements. A definition, you'll recall, is a statement that starts with the keyword `def`, as we did when we defined our `my_replace` function. These definitions are typically what we want to import from a module (i.e., another Python script).

Did you wonder why we didn't see any of the work in the `main` function of `replace32.py` when running `bookshelf3.py`? We didn't because there's only one non-indented, non-definition statement[^fn13] in `replace32.py`, and that's the funny-looking if-statement we added to the bottom of the file to protect the invocation of `main`. You probably felt that this if-statement was unnecessary work when we discussed it earlier, but now we see its power.

When the Python interpreter executes the if-statement at the bottom of `replace32.py`, it is doing it after processing of the `import` statement at the top of `bookshelf3.py`. One of the things that occurred during this processing is that the interpreter set `__name__` to the name of the imported module. As such, `__name__ == '__main__'` evaluates to `False`, giving us access to just the definitions in `replace32.py`!

## Pure functions

Let's go a little deeper here. We needed only the definition of `my_replace` in our `bookshelf` scripts, and `my_replace` is what computer scientists call a *pure function*. Pure functions are the computational analogue of mathematical functions in that the result of a pure function depends only on its current inputs. Pure functions maintain no state across invocations nor do they have other types of observable side effects. We will talk more about side effects later, but the canonical example of a side effect is the terminal printout that occurs when we execute a print-statement.

We might, however, want to use a non-pure function defined in a module. This function might need us to initialize some state before we can use the function, and this is the purpose of having the Python interpreter run some top-level statements when it first processes a module through an import-statement. 

An example where this is needed is in a pseudorandom number generator, which uses a seed to initialize the deterministic algorithm that generates the uniformly distributed, seemingly random numbers it produces. If the algorithm starts with the same seed each time it is run, it will produce the same "random" number sequence. This is great for testing, but not so great for game play, Monte Carlo simulations, or a wealth of other stochastic methods used to model physical, biological, and social processes. We will use such generators in Chapter 5.

In this chapter, you've learned a lot about how to organize the code in your scripts. The topics covered are enough to move us into a new set of interesting problems, but probably not enough practice to make you comfortable in writing your own functions. For more practice, I encourage you to try this chapter's active-learning exercises on the companion website. They'll also explain the dangers of using global variables rather than the parameter-passing and value-return mechanisms of functions.

\[Version 20240813\]

[^fn1]: A community of volunteer enthusiasts helped make the Python programming language itself a world-wide phenomenon. Watch this [short video](https://www.youtube.com/watch?v=WGCaK-N2dsA) to learn more.

[^fn2]: \`script32.py\` is a cleaned-up version of \`script9.py\` from Chapter 2.

[^fn3]: The code block assigns the contents in the two excerpt files as strings to two Python variables. I've surrounded these strings with a pair of triple quotes. This is a way in Python to define a string that includes newline characters. For such strings, you can use either a single (as I did) or double quote character and triple it.

[^fn4]: [ https://en.wikipedia.org/wiki/Road\_surface\_marking](https://en.wikipedia.org/wiki/Road_surface_marking)

[^fn5]: [ https://en.wikipedia.org/wiki/Unicode](https://en.wikipedia.org/wiki/Unicode)

[^fn6]: You might wonder what happens to this now unnamed, original string. Great question! Briefly, there's a process that periodically finds all unnamed objects and recycles their storage. This is appropriately called *garbage collection* in the computer science literature.

[^fn7]: The process of matching actuals with formals is called *the binding mechanism*. Python supports several binding mechanisms, but we'll stick with this simple one for now. If you really want to know the details at this point, please read [this section in the Python tutorial](https://docs.python.org/3/tutorial/controlflow.html#more-on-defining-functions).

[^fn8]: Unlike in the excerpt code earlier, I used three double quote characters here. You can use either three single or three double quotes in docstrings.

[^fn9]: PEP stands for Python Enhancement Proposal. These proposals provide information to the Python community about a language feature, design decision, or recommended guideline.

[^fn10]: John V. Guttag. 2021. *Introduction to Computation and Programming Using Python: With Application to Computational Modeling and Understanding Data* (Third Edition). The MIT Press.

[^fn11]: Put aside for a moment the fact that both these operations are concisely expressed in Python using its power operator \`\*\*\`, which by the way illustrates that the designers of Python thought about this exact issue when deciding what operators to define!

[^fn12]: If you don't know this movie reference, here's [a link to the scene](https://www.youtube.com/watch?v=YWyCCJ6B2WE).

[^fn13]: In a Python script, the interpreter executes each non-indented (i.e., top-level) statement it comes across, in order starting with the ones at the start of the file.