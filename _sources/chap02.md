# Chapter 2: Grab the Dialogue

Did you try running other text files through the script we developed together in the last chapter? If you haven't, please do try it. Curiosity and experimentation are attitudes to nurture for they will make you a better problem solver.

Trying this will also solidify in your mind the difference between: (1) solving a particular instance of a problem; and (2) the construction of a script that solves many similar problems.

Recall that the last chapter started with the goal of reading a *particular* book. But partway through the design, we realized that our script could continually loop until there were no more lines to read. This change made our script independent of the input book's length. We also changed our script to use the Python `input` statement, which allowed us to ask the user what book they wanted read. With these two changes, we built a *general* solution that could read *any* book of *any* length.

```{margin} The Art in Writing General Scripts
There is real art in writing scripts that solve any instance of a general problem. To go from a script that read `CatInTheHat.txt` to one capable of reading any plain-text file, we added a few new statements and changed a few existing ones. There weren't a lot of changes, but some.

An difficult question to answer is: _When does the work to make a script solve a larger collection of problems become not worth the extra effort_?

There's no easy answer to this question. Throughout this book, we will write scripts that solve more than a single instance of a problem, and this will give you a good feel for the kinds of generality you should almost always add to the scripts you write. On the other hand, we will not add every new "feature" we can imagine to any individual script. Computer programs, as you have undoubtedly experienced in your life, can become large, buggy, and hard to use as they grow more "featuresome." Do what seems reasonable and then move on to a new problem and a new script. If you later discover you missed an important feature, you can always go back and modify your old scripts.
```

## The script is the solution

Writing scripts like this---so that they are useful in a range of situations---is what computer scientists strive to do. When we follow this approach, *the solution to our problem is not just the output of our script, but also the script itself*. The script is a solution because we can use it to easily solve lots of instances of our problem. You simply need to run the script, feed it the input appropriate for the current instance of the problem, and voila, the computer follows the script and computes your desired answer!

In this chapter, we'll again start with a script that solves a specific type of problem and then expand it to solve any instance of the problem.

## What do you want done when?

But there will be a new twist. We began in the last chapter with a problem that required us to work straight through a file's contents and print what we found. The active-learning exercises then asked you to do more. They had you *compute a piece of information* using the file's contents (i.e., its length) and use this information to add to the printed content. Finally, the chapter's problem set had you *compute several pieces of information* and *make two passes* over the content. 

Now think about why the last script made two separate passes over the file. It was because each pass had a different goal, which required a correspondingly different set of actions. 

This chapter will not only lead you to solve the general instance of a problem, but it will also introduce a general method for organizing your scripts so that they can process an input for which you have not one, but several goals.

```{admonition} Learning Outcomes
In this chapter, you will experience all 8-steps in our problem-solving process, and by the end of it, you will be able to:
*   Describe the importance of recognizing similarities and differences between problems when considering which past scripts might help you start writing a new script [design];
*   Use finite-state-machine (FSM) diagrams as a widely-useful method for identifying and ordering the tasks your script must perform while processing its input [design and CS concepts];
*   Convert FSM diagrams into pseudocode [design];
*   Think about strings as a sequence (i.e., an ordered collection of characters), understand the power of this sequence abstraction, and use several common operations on sequences [CS concepts and programming skills];
*   Perform string concatenation and understand operator overloading [CS concepts and programming skills];
*   Write for-loops that iterate over the items in a sequence [programming skills];
*   Become comfortable with all variations of if-statements [programming skills];
*   Develop test inputs, and recognize and employ a common design pattern for error handling [programming skills];
*   Explain off-by-one errors and recognize where this type of error often occur [CS concepts and programming skills];
*   Structure your script development so that you interleave testing and coding, to surface hidden assumptions and never go too long without an understanding of what works and doesn't [design and programming skills];
*   Interpret the two ways of performing function composition in Python [programming skills].
```

```{margin} Note
There's one other actor in the story---the narrator's sister, Sally---but she never speaks.
```

## A new problem

Instead of decorating our input story around its edges, let's have the textual output of our script look quite different than its input. In particular, let's turn `CatInTheHat.txt` into a theatrical script that a couple of actors --- playing the narrator, the Cat, the Fish, Things One and Two, and the Mother --- could perform.

What would be the problem specification? It could be something as seemingly simple as: find the dialogue in the story and print it after the name of the character who says it.

## Splitting the problem into small pieces

One of the keys to successfully writing a computational script is to break a large problem into small pieces and then write code to solve each of those pieces. Later, you can pull the pieces together in order to create the script that meets the entire specification.

What are the primary pieces in our problem specification? Well, we have to:

1. find the dialogue in this story;
2. identify which character says which lines; and
3. ideally transform some of the story into stage directions around this dialogue.

Of these three things, let's focus on finding the dialogue. Since each line of dialogue is enclosed in easily identifiable double quotes, we can precisely specify what we need to do. As incomplete solutions for the other two problem pieces, we will label the extracted dialogue with "ACTOR:" and skip adding any stage directions.

With these decisions, {numref}`Figure %s <c02_fig1_ref>` shows how our script should print the first two pieces of dialogue from `CatInTheHat.txt`.

```{figure} images/c02_fig1.png
:name: c02_fig1_ref

The start of our script's output given `CatInTheHat.txt` as the its input.
```

## Reuse

We just practiced the first four steps of our problem-solving process: specify-imagine-decompose-decide. Step 5 asks if we can jumpstart the writing of our script by reusing anything from a previously written script. To do this well, we have to think about the *similarities and differences* between our previous problems and this current problem. 

Let's begin with our script that reads a text file, which I've reproduced below:

```{code-block} python
---
lineno-start: 1
---
### chap02/read32.py
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

This code queries the user for a book, opens the book, and reads each line from it. Will we need to do similar work in our current problem? Yes. If we are going to find each piece of dialogue in the story, we'll need to read each line in it. Let's then keep the first five lines from `read32.py`. Of course, we will also need to know when we've reached the end of the story and then close the open file, which the if-statement and with-as-statement do for us. Overall, we just need to adjust the `input` prompt and delete the printing of `the_line`, since the output of our new problem isn't every line in the story. After doing all this, we're left with the following:

```{code-block} python
---
lineno-start: 1
---
### chap02/script1.py
my_book = input('What book would you like as a script? ')

with open('txts/' + my_book) as my_open_book:
    while True:
        the_line = my_open_book.readline()
        
        if the_line == '':
            # We've read the entire book!
            print("\nThe End.")
            break

        # new pseudocode goes here
```

## Switching between goals

I put the comment about new pseudocode at the bottom of the while-loop because we need to do something with `the_line` before we loop back. One way to figure out what we want this pseudocode to do is to think about what our script should be doing when it first starts reading the story. In other words, what is our script's initial goal?

As we begin, our initial goal is to find the start of the first piece of dialogue. Once we find the start of this dialogue, our script switches to a new goal: scan to the end of this dialogue and print out what was scanned.

What happens then? We begin again searching to find the start of the next piece of dialogue, and this pattern repeats until the story ends.

## Finite State Machines

What we just described is formally called a *finite state machine (FSM)*. Each of our goals is a *state*, which consists of some work we want done while in that state. To move between states, our script looks for *events*. We start in some initial (or *start*) state, and we are done when the script enters one or possibly several *final* states.

{numref}`Figure %s <c02_fig2_ref>` is a diagram of our finite state machine, where the states are represented as labeled circles (also called *nodes*) and *transitions between states* as directed arrows. Each arrow is labeled with the event that cause the transition. The start state is identified by the special transition labeled `Start`.

```{figure} images/c02_fig2.png
:name: c02_fig2_ref

A complete FSM diagram for finding dialogue.
```

Let's take a moment and make sure we understand this diagram for it will be the template we use to write our script. The script needs to find each line of dialogue in our input file, and we know that dialogue is surrounded by double quotes. As our script begins, it starts (as indicated by the start arrow) in a state `S0`, which specifies that it should be searching the line it read for a double-quote character. If the line doesn't contain a double-quote character, it should just move to the next line in the story. When the script finds a double-quote character, it has found the start of a line of dialogue, and this event causes a transition to the state labeled `S1`. 

State `S1` has our script do a different task: it collects the characters after the double quote, which it will later print as a piece of dialogue. It continues to read lines and collect characters until it finds the next double-quote character in the story. This character indicates the end of the current piece of dialogue, and this event causes another state transition.

The transition out of state `S1` takes us back to `S0`, since we need to begin searching for the start of the next piece of dialogue. Our script will keep ping-ponging back and forth between these two states, collecting and printing dialogue, until what happens? Is there any other event to which our script should react? 

Yes! Our input story eventually ends (i.e., our script reads EOF, or end of file). {numref}`Figure %s <c02_fig2_ref>` shows that we expect this event to happen while the script is in state `S0`. Why? What assumption are we making about the input story when this event is a transition from `S0` to the FSM's designated end state (labeled `DONE`)?

While you think about this, I'll tell you that the FSM in {numref}`Figure %s <c02_fig2_ref>` is what is formally called a *deterministic finite state machine*. By deterministic, we mean that there is at most a single transition out of a state for each possible event. State `S1` has two transitions out, but they are labeled with different events. There are also *nondeterministic* FSMs, and these allow a single event to label multiple transitions out of a state. You'll encounter them if you continue to study computer science.

Both deterministic and nondeterministic FSMs are mathematical models of computation. They are defined by the quintuple of an input alphabet of events, a set of all states, a start state, a transition function, and a set of (possibly empty) final sets. The start state and final states are included in (i.e., are a subset of) the set of all states.

## Error handling in FSMs

Hopefully by now you have realized that the FSM in {numref}`Figure %s <c02_fig2_ref>` assumes that the input story is *well-formed*, and by this we mean that every piece of dialogue in it starts and ends with a double-quote character. It doesn't have a transition from `S1` on encountering `EOF` because we don't expect our story to end while we are in the middle of a line of dialogue.

A FSM's transition function maps states and input events to states. For example, the transition function `f` for our FSM in {numref}`Figure %s <c02_fig2_ref>` would return `S1` when called with `f(S0,'"')`. A transition function does not have to be defined for all combinations of input symbols and states. If the transition function is not defined for some pairs of current state and input event, the implementation of the FSM should probably raise an error when such pairs occur. In other words, a script corresponding to our FSM might raise an error if it finds `EOF` while in state `S1`.

We have just dipped our toes into the deep topic of FSMs. You will find that they are a great representation for modeling lots of mechanical, biological, and linguistic systems. But enough about the representation, let's see how we can use the diagram we've drawn to help us think about the code we want to write.

## Encoding the state information

The first thing to note about the FSM in {numref}`Figure %s <c02_fig2_ref>` is that it contains only three states, where the third state (`DONE`) represents the termination of our script. This means that our script has only to keep track of which of the first two states (`S0` and `S1`) it is in at any point in time, which we can do with a single *Boolean* variable. 

```{admonition} Definition
A Boolean variable may contain only one of two values, which are called `True` and `False` in Python.
```

Let's call this variable `looking_for_open_quote`, and define its behavior so that it is `True` when the script is in state `S0` and `False` when the script is in `S1`. With this decided and the frame from `script1.py`, we can start writing some code, with pseudocode notes for where pseudocode doesn't cover all possible cases.

We begin by initializing our state information (i.e., setting `looking_for_open_quote` to `True`) and then using that variable to separate the work in state `S0` from that in `S1`:

```{code-block} python
---
lineno-start: 1
---
### Start of chap02/script2.py
my_book = input('What book would you like as a script? ')

with open('txts/' + my_book) as my_open_book:
    # Set our FSM to the start state
    looking_for_open_quote = True

    while True:
       the_line = my_open_book.readline()
        
       if the_line == '':
            # We've read the entire book!
            print("\nThe End.")
            break

        # new pseudocode goes here
        if looking_for_open_quote:   # in s0
        #     do some work
        else:                        # in s1
        #     do other work
```

Notice that we've pulled the processing that every state does (i.e., reading a line of characters from the input file) out of each state's work. We could have duplicated that code in each state, but that was unnecessary when we had already done it. We will talk more, in the next chapter, about the benefits of collecting commonly executed code together in one place.

## This or that

You'll also notice a new syntactic structure for the Python if-statement at the end of the code block. Earlier in the block, we used an if-statement to check for our infinite while-loop's exit condition, and we either hit this condition or continued on with the loop's work. We were checking for something exceptional, and the if-block and its controlling condition was all the structure we needed.

With `looking_for_open_quote`, we need to decide which of two pieces of work we want to do, and we must do the work in one of these two states before continuing on. While we could write an if-statement to check one of the conditions and then follow it with another if-statement checking the other condition, Python provides a form of the if-statement that eliminates this unnecessary second check. In this form, the second if-statement becomes a simple `else`.

```{tip}
Why is an if-else-block better than two if-statements in this situation? Because the condition we're checking is either true or false. When it's not one, it must be the other. If you write two if-statements, you'll have to correctly write a condition and its complement. The if-else construct removes this unnecessary work (and potential error in logic or typing waiting to happen).
```

## Fill in each state

Let's expand our pseudocode for state `S0`. What does it need to do? For one thing, it can ignore any line that does not contain a double-quote character. But if the line contains a double-quote character, we know that the line, at least, is the beginning piece of a line of dialogue. I say "at least" because the dialogue that starts on this line may end on this line or extend to one or more following lines.

I want to pause here and highlight the fact that we are using the word "line" in two different contexts that we must keep straight. A line from the file, which is completely contained in the object called `the_line`, and a line of dialogue from the story, which can start anywhere in a file line and continue to an arbitrary point in that or a later file line.

For example, the first line of dialogue in *The Cat in the Hat* starts in the middle of the eighth file line and continues to the end of the ninth file line.

```{code-block} none
### NOT a script and therefore NOT runnable
8 And I said, "How I wish
9 We had something to do!"
```

```{margin} Incomplete Code
As our scripts grow longer, I won't always include the entire script in the code block. A clue to this happening is that the line numbering doesn't start at 1, as you see here. We're modifying the last code block at line 17, and we're focused only on what is taking place at that point in the script.
```

Ok, let's put these ideas into pseudocode and deal with the transition while in state `s0`.

```{code-block} python
---
lineno-start: 17
---
        if looking_for_open_quote:   # in s0
        #     do some work
        #     if found opening double quote
        #         move to s1
        #     else
        #         stay in s0
```

Notice that this pseudocode uses an if-statement to check for the event that causes the transition from `S0` to `S1`. This event is the presence of a double-quote character in `the_line`. So, how do we check for a double-quote character in the string object named `the_line`?

## Strings as a sequence of characters

To this point, I've been relying on your intuition for what constitutes a string in Python. We created them by defining string literals. We read file lines as strings and compared them against string literals (e.g., to test for EOF). And we printed them out to the terminal. Probably none of this work forced you to think about how we were representing strings in the computer; we simply operated on them as big blobs. However, to ask whether a string contains a particular character, we have to know if Python allows us access to the components of a string object. 

Short answer: it does. A string in Python is a *sequence* of characters. I emphasized sequence in this definition because sequence is a very useful abstraction for lots of different objects that we would like to manipulate in our scripts. The abstraction you should have in your mind for a sequence is *an ordered collection of items*. 

It doesn't matter if the items in a sequence are of the same type or kind, although in the case of a string, each item in the ordered collection is of the same kind (i.e., a character). We will soon play with the `list` datatype in Python, which allows you to create a sequence containing different kinds of things. For example, the objects on my office bookshelf when viewed from left to right could be represented as a Python `list` containing a stuffed animal, a picture of my kids, this course's textbook, an old CD, and then some other books.

## Membership test

What makes a sequence a very useful abstraction is that there are commonly used operations that we can perform on any Python sequence. For example, Python sequences permit us to easily test whether an item is a member of a given sequence, and a membership test is exactly what we need to answer the question of whether the string named `the_line` contains a double-quote character! 

With this knowledge, let's turn our pseudocode if-statement into Python code:

```{code-block} python
---
lineno-start: 17
---
        if looking_for_open_quote:   # in s0
            # do some work
            if '"' in the_line:
                # move to s1
            else:
                # stay in s0
```

If I wanted to know that there was no double quote in `the_line`, I could have written the expression: `'"' not in the_line`.

Both `in` and `not in` are binary infix operators that test whether the object to its left is a member of the one to its right. The result is `True` when this membership check succeeds, and `False` otherwise.

## Coding a transition

To transition from state `S0` to state `S1`, we simply need to set our state variable appropriately, which in this case means setting `looking_for_open_quote` to `False`. Staying in `S0` is even easier: `looking_for_open_quote` stays `True`, and this means we can delete the entire `else` clause since there's no work to be done (i.e., it is already `True` in this code block!).

```{code-block} python
---
lineno-start: 17
---
        if looking_for_open_quote:   # in s0
            # do some work
            if '"' in the_line:
                looking_for_open_quote = False
```

## Indexing and slicing

I left the comment "do some work" in the pseudocode because transitioning on seeing a double quote is not all the work we need to do. To capture and print each line of dialogue from the story, we need to capture the characters we read from an opening double quote until its matching ending one. Python makes this easy with a couple other common operations on sequences.

Specifically, all Python sequence types permit us to identify a sequence item by its index and to slice a subsequence from a larger sequence by using two indices. Let's look at this using line 8 of the file `CatInTheHat.txt`, which reads '`And I said, "How I wish`'.

```{code-block} none
### NOT a script and therefore NOT runnable
  +---+---+---+---+---+---+---+---+---+---+---+---+---+---+-   -+---+---+----+
  | A | n | d |   | I |   | s | a | i | d | , |   | " | H | ... | s | h | \n |
  +---+---+---+---+---+---+---+---+---+---+---+---+---+---+-   -+---+---+----+
  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14    21  22  23   24
-24 -23 -22 -21 -20 -19 -18 -17 -16 -14 -13 -12 -11 -10  -9    -3  -2  -1
```

Python, like many program languages, starts numbering at 0. If I want to know the first item in a Python sequence, I use square brackets and specify index 0. For instance, `the_line[0]` returns '`A`'.

I can also ask for the length of any sequence object with the Python built-in function `len`. For example, `len(the_line)` returns `24`. Notice that we count the letters, spaces, punctuation, and the special carriage return character at the end of this string. They are all items in the string currently named `the_line`.

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
the_line = 'And I said, "How I wish\n'
print(the_line[0])
len(the_line)
```

While counting from the front of a string is often what you need, sometimes you might find it easier to count backwards from the end of the string. This is possible in Python by using negative indices. The last character in our `the_line` example is accessed by writing `the_line[-1]`, and the initial capital `'A'` is `the_line[-24]` or `the_line[-len(the_line)]`. You will get an `IndexError` if the index you use for a given string `s` is less than `-len(s)` or greater than or equal to `len(s)`.

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
the_line = 'And I said, "How I wish\n'
print(the_line[-24])             # prints the first character
print(the_line[-len(the_line)])  # also prints the first character
the_line[len(the_line)]          # index out of bounds!
```

```{admonition} You Try It
Go ahead, and experiment with the code blocks above. Try to access other characters in `the_line` to make sure you understand how indexing works.
```

Indexing is helpful, but what our problem needs is the ability to grab a sequence of characters. To do this, we can use Python's *slicing* facility, which allows us to grab any subsequence of items within a sequence object. For example, `the_line[6:10]` would return the string `'said'`.

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
the_line = 'And I said, "How I wish\n'
print(the_line[6:10])
```

Notice that the first index in this expression (i.e., the one before the colon) is the index of the first character of the slice we want, but the second index in this expression is *one greater* than the index of the last item in our slice. Defining the slicing operation in this manner allows us to write `the_line[0:len(the_line)]` to create a copy of the string object named `the_line`. Since a programmer will often find herself writing `0` as the first index and `len(s)` as the second index in a slice, where `s` is a name for some string object, Python will assume you mean `0` as the first index if you elide it and `len(s)` as the second index if you elide it. To copy a string `s`, you can simply type `s[:]`. 

I tell you all this because we want the slice `the_line[12:]` as the start of our dialogue line. 

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
the_line = 'And I said, "How I wish\n'
print(the_line[12:])
```

But how do we know that the double quote sits at index 12 in this current string object? All that the `in` operator told us was that a double-quote character existed in `the_line`.

## For-loops

Programmers ask this type of question often enough that the developers of Python made it easy to answer. But I'm going to divert us for a moment to talk about how, for example, a beginning programmer would solve this problem in a programming language like C. This will introduce to you the other major looping construct in Python (and many other languages): the *for-loop*. More importantly, this introduction will help you to understand what takes place behind the scenes in the Python abstraction that we will eventually use.

```{margin} Fun!
Here is [a fun article](https://www.wired.com/story/why-you-hate-media-technically-speaking/) on the difference between dialogue and dialog.
```

The following code block fills in more of the work we need done in state `S0`. In particular, it uses a for-loop to find the location of the first double-quote character in `the_line` and then begins capturing the dialogue.

```{margin} Complete Script
You can see the entire script including this code by in the file `chap02/script2.py.`
```

```{code-block} python
---
lineno-start: 17
---
        if looking_for_open_quote:   # in s0
            # do some work; some of it follows
            for i in range(len(the_line)):
                if the_line[i] == '"':
                    dialog = the_line[i:]
                    break

            if '"' in the_line:
                looking_for_open_quote = False
```

The `for` statement iterates over the items in a sequence. The `in` keyword here plays a similar membership role as we saw earlier, but as part of the `for` statement, we are asking `i` to name each item in the specified sequence in order, and for each of those values, execute an iteration of the indented block of statements. 

To see this for-loop in action, we need to know what the sequence is in this particular case. We already know what value is produced by `len(the_line)`, and we pass the integer value computed by `len` to another Python built-in function called `range`. This built-in is quite interesting, but for now we simply need to think of it as computing a sequence object containing the integers from `0` to one less than the integer value passed to `range`.

Let's now look at the body of the for-loop in the code block above. It asks whether the character at index `i` in `the_line` is a double quote. When it is, the body of the if-statement names the slice of `the_line` from `i` to the end of `the_line` with the variable `dialog`. We end the if-statement body with a `break` because we don't need the for-loop to continue checking the rest of `the_line` once we've found a double quote. (Actually, there are other cases we need to handle, but not for our example from line 8 of `CatInTheHat.txt`.)

Our original if-statement that checked for a double-quote character in `the_line` is now performing redundant work when the for-loop finds a double quote. We can rewrite the work in state `S0` to integrate the transition work into the for-loop.

```{code-block} python
---
lineno-start: 17
---
        if looking_for_open_quote:   # in s0
            # do some work; some of it follows
            for i in range(len(the_line)):
                if the_line[i] == '"':
                    dialog = the_line[i:]
                    looking_for_open_quote = False
                    break
```

```{admonition} You Try It
Make sure you understand why the logic in the code block above and the one before it are equivalent. Then write a simple for-loop that prints the integers from 0 to 10.
```

## String find

As promised, Python makes it easy for us to find the index of the first occurrence of a character in a string using the `str.find` method. In particular, much of the work of the above for-loop becomes: `i = the_line.find('"')`. 

I say "much" because you might ask what the method returns if the character we seek isn't in the current string, and the answer to this question is integer value `-1`. In other words, given a string `s` and a substring `c`, `s.find(c)` returns an integer between `0` and `len(s)-1` if `c` occurs within `s` and `-1` if not. 

While `find` makes our life much easier, it doesn't replace the need for an if-statement in our new solution. Here's what our script looks like when we replace our previous for-loop solution with this "find+if" solution. I've also updated our comments to explain what we have and haven't yet accomplished.

```{code-block} python
---
lineno-start: 17
---
        if looking_for_open_quote:   # in s0
            i = the_line.find('"')
            if i != -1:
                # Found an open quote
                dialog = the_line[i:]
                looking_for_open_quote = False
                # FIXME! Need to handle short dialogue.
```

## Design patterns for error handling

When you been programming long enough, like any practiced skill, you'll start to see repeated patterns. This is one for error handling. A command, function, or method carves out one or more values in the range of its return value and distinguishes those values as error conditions. In our current example, `find` needs only the integers in the range `0` to `len(s)-1` to fully accomplish its stated functionality. The person who wrote the implementation of `find` was free to pick any value outside this range of valid results as an error condition. She might have picked several such values to indicate a number of different error conditions.

```{margin} Example Situation
You might know that the error condition is impossible because of some processing you did immediately prior.
```

One of the things you'll learn as your programming skill grows is that you should always check and handle the error conditions that might occur in the commands, functions, and methods you use. The only reason not to do this is because you have carefully reasoned out why the error condition cannot occur.

Unfortunately, just because a design pattern is commonly used doesn't mean it isn't without headaches. This pattern requires us to add the statement `if i != -1:` to our script, which we are supposed to read as "if `find` returned a valid index into `the_line` then we found an opening double-quote character at index `i`." Yea, it doesn't look much like that to me either and that's why I stuck in the comment at the start of the body of our inner if-statement.

While not an issue with the structure of the logic in our current script, many programmers also dislike this design pattern because it can break up your script's flow with lots of checks for uncommon error conditions. When you are constantly distracted by infrequently-true error checks, it can make it very hard for you and others reading your script to understand how the script performs its primary function. We can solve this headache by using a different design pattern for error handling, which keeps exceptional events out of the main flow of your algorithm. We will talk about this other design pattern in a later problem-to-be-solved.

## Never go too long without testing

We've made some great progress on our script, and now we have a choice of how to proceed. On the one hand, we can continue to write code to handle all the cases that might occur in state `S0` (e.g., the case we've flagged in our "FIXME!" comment). Our minds are focused on state `S0`, and it may be enticing to continue working on it until we've handled every case we can imagine. On the other hand, we could switch and start writing some code for state `S1`. In particular, we could write `S1` code that complements the code we just finished in `S0`. The benefit of this approach is that it would allow us to see if we can get our script to run on some subset of the possible inputs.

```{tip}
When you have the opportunity to test what you've written, take it. It's a good practice to build your script in pieces that you can regularly test. I like the feeling of accomplishment when I get another small piece of my larger problem to work. Plus, it is often efficient from an overall time perspective if you don't spend a lot of time coding a design that was doomed to failure based on one of your early design decisions. Regular testing will keep you from wasting a lot of time on doomed designs. The exercises and problem sets that come with this book are structured to encourage you to follow this approach.
```

So what do we need to write for state `S1` that integrates with what we just finished writing for `S0`? When our script is in state `S1`, it is looking for the close quote matching the open quote seen in state `S0`. This sounds a lot like the work we just finished.

In both states, we use the `find` method on strings to determine whether `the_line` contains a double-quote character, and if it does, we either start capturing (state `S0`) or finish capturing (state `S1`) the `dialog`. We also include a FSM transition in each case, which involves setting `looking_for_open_quote` appropriately. Finally, we will need to print in state `S1` the dialogue we captured, as the specification required us to do. Here's that code for states `S0` and `S1`.

```{margin} Complete Script
You can see the entire script including this code by in the file `chap02/script3.py.`
```

```{code-block} python
---
lineno-start: 17
---
        if looking_for_open_quote:   # in s0
            i = the_line.find('"')
            if i != -1:
                # Found an open quote
                dialog = the_line[i:]
                looking_for_open_quote = False
                # FIXME! Need to handle short dialogue.

        else:                        # in s1
            i = the_line.find('"')
            if i != -1:
                # Found a close quote
                dialog += the_line[:i+1]
                print("\nACTOR:", dialog)
                looking_for_open_quote = True
            else:
                dialog += the_line
            # FIXME! Is this all the work in state S1?
```

## Concatenation, overloading, and shorthands

There are a number of interesting new things in the code for state `S1` (i.e., the else-block).

The first is that this code, unlike the code for state `S0`, does something for both the case of finding a double quote in `the_line` and not finding one. When the script finds a closing double quote, it adds the characters from the start of the line until that point to the variable `dialog`. By add, I of course mean concatenate. As we learned in Chapter 1's exercises, the `+` operator concatenates when its operands are strings and adds when its operands are numbers. This is called *overloading*: the operation of the operator depends on the type of its operands.

The `S1` code also adds all the characters in `the_line` when it doesn't find a double-quote character. This is because everything in the current file line is part of the running dialogue.

But wait, these two concatenation operations don't look quite like what we saw in the last chapter. What is the meaning of this `+=` operator where we expect the concatenation we just discussed to occur? Well, `+=` just a shorthand for a concatenation that is really an append. In other words, the statement `dialog += the_line` is equivalent to the statement: `dialog = dialog + the_line`.

## Off-by-one and other potential errors

Now look carefully at what we append to `dialog` when the script finds a double-quote character. Do you fully understand why `the_line[:i+1]` includes the ending double quote? If not, you may wish to read the documentation about the string method `find` and review how the indices in slicing work. If you left off the `+1` in `the_line[:i+1]`, this would be an example of what's called *an off-by-one error*. They are quite common in programming, and you should train yourself to be alert to situations where they can occur.

I added a comment at the end of the code for state `S1` to remind myself that we haven't carefully considered all the cases that can occur when looking for dialogue. We might have, but we have neither tested on a lot of different examples nor carefully thought through the cases we might encounter. But that's ok for now, since we simply want to see if we can get the script to work for the cases we think we have covered!

## Testing

Typically, you start testing with small inputs that exercise your script in a limited number of ways. This is a great method for incrementally learning what works and clearly seeing what doesn't. Being methodical may not be your style, but it makes testing and debugging less of a nightmare. 

Here's the full script we've developed to this point:

```{code-block} python
---
lineno-start: 1
---
### chap02/script3.py
my_book = input('What book would you like as a script? ')

with open('txts/' + my_book) as my_open_book:
    # Set our FSM to the start state
    looking_for_open_quote = True

    while True:
        the_line = my_open_book.readline()

        if the_line == '':
            # We've read the entire book!
            print("\nThe End.")
            break

        # new pseudocode goes here
        if looking_for_open_quote:   # in s0
            i = the_line.find('"')
            if i != -1:
                # Found an open quote
                dialog = the_line[i:]
                looking_for_open_quote = False
                # FIXME! Need to handle short dialogue.

        else:                        # in s1
            i = the_line.find('"')
            if i != -1:
                # Found a close quote
                dialog += the_line[:i+1]
                print("\nACTOR:", dialog)
                looking_for_open_quote = True
            else:
                dialog += the_line
            # FIXME! Is this all the work in state S1?
```

Our first test input is `Talkative1.txt`, which contains two paragraphs from the Indian fairy tale titled *The Talkative Tortoise*. When you run `script3.py` and tell it to read `Talkative1.txt`, it should work as we expect: The single line of dialogue (across multiple file lines) is printed. That's not what will probably happen on most of your first test runs, but one can always hope!

```{tip}
When testing a script that repetitively does something, first test with an input that requires only one iteration of your repetitive action. This is what `Talkative1.txt` did; it caused our script to visit state `S0`, then state `S1`, and then exit. Once one iteration works, try an input that causes two iterations. Proceeding like this separates the testing of the work within an iteration from the work a script does to move from one iteration to the next.
```

Let's now try our script on a test input file containing two lines of dialogue. Run `script3.py` on `Talkative2.txt`. Success again!

So far so good, but I said we had skipped the case of a story with a line of dialogue fully contained on one line of the input file. The file `HomeView3.txt` contains this case, which is a paragraph from the first chapter of *Five Little Peppers* by Margaret Sidney (a pseudonym for Harriett Lothrop). Run `script3.py` on this input and let's see if we really need extra code to handle this case.

Hmmm, I guess we do. This test run failed to do what we expected; it prints a bunch of text that isn't dialogue and ends at the start of the second quote in the input file.

## Beware of hidden assumptions

Our script is short enough that we can probably analyze our problem with `HomeView3.txt` by thinking through the script's operation. This will not always be the case, and we will soon learn how to use print statements to help us understand the state of our script at any point in its execution.

Consider what happens when our script grabs line 1 of `HomeView3.txt`, i.e., the variable `the_line` holds the string: `'"Oh dear!" exclaimed Polly as she sat over in the corner by the window\n'`. Since the script just started, it is `looking_for_open_quote`, and it finds one at index `0` of `the_line` (i.e., the variable `i` will be `0`). Because `0` is not equal to `-1`, the script then sets the variable `dialog` to the contents of `the_line` (since `the_line[0:]` makes a copy the entire string). It also sets `looking_for_open_quote` to `False`, indicating a transition to state `S1`, and proceeds to the next iteration of the while-loop not realizing that it missed the close quote. While in state `S1`, it mistakenly recognizes the double quote on file line 3 as the missing close quote, and it thus prints much more than the actual first piece of dialogue. It then switches back to state `S0`, in which it finds the last double quote in the input file, but it prints nothing more because it finds the end of the file before any other double quotes.

The design issue here is a subtle one. Our FSM diagram from earlier says that we should transition from state `S0` to state `S1` on seeing a double-quote character, but this FSM also assumes that events come *one at a time* during our processing. In `HomeView3.txt`, `the_line` contained *two* events. We cannot blithely consider that we are done with the processing of `the_line` once we've found *a* double-quote character since that's not what our FSM diagram says to do. It says to start processing the input in state `S1` on the character immediately following an opening double quote. It doesn't say to not check the characters in `the_line` beyond the opening double quote for one or more other double-quote characters.

Now that we know what we did wrong, we can fix this problem in at least two different ways. We could faithfully adhere to our FSM diagram and write some code that enters state `S1` using what remains in `the_line` after an open quote, or we could check in state `S0` for short lines of dialogue and skip the transition to `S1` in these cases. 

Sometimes one choice is better than the other, but in this case, there's probably not much difference. Let's maintain our current loop structure that processes file lines and decide to no longer adhere to our (initially helpful) FSM diagram. 

Yes, this is a choice! The FSM diagram was meant to help us think about the problem. It was not meant to constrain our implementation.

## Function composition

Once we have thought through the problem and outlined a sensible solution, implementing it is fairly straightforward.

```{code-block} python
---
lineno-start: 1
---
### chap02/script4.py
my_book = input('What book would you like as a script? ')

with open('txts/' + my_book) as my_open_book:
    # Set our FSM to the start state
    looking_for_open_quote = True

    while True:
        the_line = my_open_book.readline()

        if the_line == '':
            # We've read the entire book!
            print("\nThe End.")
            break

        # new pseudocode goes here
        if looking_for_open_quote:   # in s0
            i = the_line.find('"')
            if i != -1:
                # Found an open quote
                dialog = the_line[i:]
                if '"' not in dialog[1:]:
                    looking_for_open_quote = False
                else:
                    # Grab entire dialog from this line ...
                    short_dialog = dialog[1:].split('"')[0]
                    print("\nACTOR: " + '"' + short_dialog + '"')
                    # ... and stay in state S0

        else:                        # in s1
            i = the_line.find('"')
            if i != -1:
                # Found a close quote
                dialog += the_line[:i+1]
                print("\nACTOR:", dialog)
                looking_for_open_quote = True
            else:
                dialog += the_line
            # FIXME! Is this all the work in state S1?
```

In `script4.py`, we protect the transition to state `S1` (within the `S0` work) with an if-statement that makes sure that the rest of the file line (i.e., `dialog[1:]`) does not contain a double-quote character. If it does, we slice out the short line of dialogue, print it, and stay in state `S0` looking for the next open quote (i.e., the body of the new `else` within the `S0` work). 

The syntax on the righthand side of the `=` operator where we set `short_dialog` is a bit intimidating, and let's take a moment and make sure we understand exactly what it does. The key to understanding is to remember that *the Python order of computations takes place left to right*.

With that in mind, we can read `dialog[1:].split('"')[0]` as saying: Take the object named `dialog` and treat it as a sequence. We want every item in this sequence except the first (i.e., the 0th item), as specified by the slice `dialog[1:]`. Next, split this result into a list of subsequences using the double-quote character as the splitting points, as specified by `.split('"')`. The double-quote character is not included in the resulting subsequences, and the list returned by `split` is itself a sequence. We index into this list to grab the first item (i.e., at index `0`), which turns out to be everything in `the_line` between the opening double quote and the ending double quote! And this explains why the subsequent `print` concatenates opening and closing double-quote characters back onto the `short_dialog` as we print it.

The righthand side of the `short_dialog` assignment statement is an example of *function composition*, and we will see it used often. In fact, we saw an earlier example in our for-loop code (i.e., `range(len(the_line))`). This example looks a lot like function composition in mathematics, where you evaluate the stuff in the innermost pair of parentheses first and then work your way out to the outermost parentheses. Overall, try not to let either of these different forms of function composition overwhelm or intimidate you. When you come across them, just take them a step at a time. You'll eventually come to appreciate that

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
dialog = '"At least I\'ll try."' 
short_dialog = dialog[1:].split('"')[0]
short_dialog
```

is easier to read *and comprehend* than

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
dialog = '"At least I\'ll try."' 
dialog_and_other_stuff = dialog[1:]
list_of_dialog_and_other_stuff = dialog_and_other_stuff.split('"')
short_dialog = list_of_dialog_and_other_stuff[0]
short_dialog
```

Function composition removes the need to give names to the intermediate results, and without names, our attention is not drawn to this work, which necessary for the computation but not really important to the big picture. This point is important. For human readers, your script should be like a good story: It highlights the main action while hiding the details of exactly how the characters interact and get from place to place. But unlike a literary story, your script does contain these details, through the mechanism of function composition. The Python interpreter needs to know these details, since it does only what it is exactly told to do. Function composition keeps humans from being distracted by the details the interpreter needs to know.

## Abstraction as information hiding

As another example where we hid details to help with human understanding, recall how we replaced a for-loop and its associated code with an invocation of the `find` method on a string. Both computed a similar result, but we preferred the `find` string method over the for-loop implementation because it was both more concise and more expressive of what this step in our script was doing. In the next chapter, we will see how we can create our own compact representations---abstract away the details---so that each line of our script more closely resembles each step in the pseudocode we are trying to implement.

## There is no character

Before we go, I want to correct something you may have come to believe. While I have used the term "character" to describe single items in the sequence known as a Python string, there is no distinguished thing that is a character in Python. What we think of as a character is just a Python string of length 1. In other words, when I earlier wrote that `the_line[0]` returns `'A'`, I was careful to write the result as a Python string. Go ahead and see what `type(the_line[0])` tells you about the type of the object returned from the string index method. We will talk more about types when we reach Act II.

```{code-block} none
---
emphasize-lines: 2, 3
---
### Run the highlighted statements in the interactive interpreter
> the_line = 'And said, "At least I\'ll try."' 
> type(the_line[0])
<class 'str'>
```

\[Version 20230811\]