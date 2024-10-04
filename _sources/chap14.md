# Chapter 14: The Dream of Bug Fixing #

While we may not like the Python interpreter's error messages, at least they give us a clue to the specific syntax or runtime errors it finds. They are a starting point for our efforts to fix our script and get the machine to do what we want. In contrast, it's harder for us to diagnose design errors (i.e., solving your problem incorrectly) and subtle coding errors (e.g., writing a script with an infinite loop or multiplying a value by the wrong constant). To check for these kinds of errors, we must test our scripts across a range of inputs, verifying that they produce the expected output in each instance. But what do you do when they inevitably don't?

You've been here with your own scripts, and you know that you need to dig into each script's execution and determine which of its tasks are correct and which went awry. The tool you've largely used to this point has been the print-statement. You inserted *debugging print-statements* at strategic points to see: (1) if the script's execution got to that point; and (2) if it did, what were the values of some important variables. Through this information gathering, you generated a hypothesis of what went wrong, and then inserted more debugging print-statements to test that hypothesis. It was slow and painful work.

It's not at all hard to get started debugging with print-statements, and that's why they're our first tool for finding and fixing errors. But as your scripts get longer and more complex, you'll long for better tools. And several exist for finding design and subtle coding errors, which we'll cover in the next two chapters. In Chapter 15, we'll build a *runtime debugging tool*, which gives you the functionality of debugging print-statements (and more) without you having to edit and rerun your script. In Chapter 16, we'll learn that it is possible, using *static analysis tools*, to find *some* of your script's coding errors without editing or running it.[^fn1]

The ordering of these next two chapters is toward tools that find bugs while requiring us to do less of this tedious debugging work. But what about this chapter? It gets us moving in this direction by covering the basics of static program analysis and engaging us with several simple static analysis tools (in particular, functions that ask and answer questions about other functions). We won't just get moving, however. We'll also answer everyone's ultimate bug-finding question: Can we build *an analysis tool that finds for us all the bugs in all our scripts*? Wouldn't we all love to have that tool!

```{admonition} Learning Outcomes
Learn about a fundamental limitation of computational tools: that there are specifications that we cannot write as a computational script. Alan Turing proved this fact in 1936, and you will work with simple decision programs to see why this is true. After completing this chapter, you will be able to:

*   Write scripts that take other scripts as input [design and programming skills];
*   Use such scripts to perform a simple static analysis that finds a syntax error in a function's source code [CS concepts and programming skills];
*   Work with function objects and access their attributes, in particular their source code [programming skills];
*   Develop scripts that grab function objects from a string defined at runtime [programming skills];
*   Build a tool that allows you to define and run lots of different program analysis functions [design and programming skills];
*   Discuss the differences between uncomputable, undecidable, intractable, and tractable problems [CS concepts];
*   Describe the structure of a (famous) proof that uncomputable programs exist [CS concepts];
*   Give examples of how normally bad things (e.g., programs with an infinite loop and those that are intractable) can be sometimes be useful [design and CS concepts].
```

## Finding all bugs

Let's develop a specification for how such a super tool might work. To begin, we know that this tool, which I'll call `t`, must be given a script to analyze, just as interpreters, debuggers, and static analysis tools require. I'll use `p` in referring to this script.

Is knowing the statements in `p` enough for `t` to do its work? We can find syntax errors without any additional information, since such errors arise solely from the structure of our code. But what about runtime errors? The Python interpreter tells us about such bugs while `p` runs, and so let's optionally provide `t` with an example input for `p`.

Ok, but to write `t`, we need to be precise about `t`'s expectations for `p` and `p`'s optional input. Yikes, there are lots of programs out in the world (e.g., web browsers, apps on our smartphones, and the scripts we've written), lots of different kinds of inputs to these programs (e.g., integers, web pages, and image files), and no consistency in how many input values programs take. While this initially feels overwhelming, we can perform an incredibly wide range of analyses from a very simple encoding of `p` and its inputs: We'll take both as strings. Even more reassuring is the fact that this simplicity of encoding won't limit the claims we'll make about programs in general.

So what needs to be in the string that represents `p`? It turns out that it's sufficient to include two things: (1) the program's source code, just as we typed it; and (2) the point where execution begins in this code. For example, if an analysis tool asks whether `p`'s execution ever reaches a particular point in the source code, it first needs to know where execution begins.[^fn2] After that, the current statement determines which statement(s) will execute next. This is exactly what you do when you think about the execution flow in your own scripts.

To further simplify the notation and examples in this chapter, and without any loss of generality, I'll encapsulate the program statements we want to analyze inside a function `f`. To start `p`, we call `f`. To make this concrete, think about the design pattern we've been using at the end of our Python scripts:

```{code-block} python
---
lineno-start: 1
---
if __name__ == '__main__':
    main()
```

All we are saying is that `f` is this function `main`, possibly taking some input parameters.

Encoding a program's inputs as strings is even less complicated. Recall from earlier chapters that we can convert any Python object into and back out of a string representation (e.g., lists and dictionaries using `json` in Chapter 4; integers using `str` and `int` in Chapter 5; and our own `CitySqGrid` objects in Chapter 11). Think of our super tool `t` as requiring that input objects are converted into strings prior to its invocation.

Finally, `t` can be made to work with whatever number of inputs `f` requires. I'll demonstrate tools that analyze functions requiring either zero, one, or two input parameters. Because of the work you did in ALE 8.2, Step 6, you know how to convert code that takes a specific number of inputs into one where the number of inputs is known only when the tool is invoked.

Fantastic. We almost have a complete specification for our problem-to-be-solved: Write a tool `t`, which itself we'll encapsulate in a function, that takes a function `f` we want analyzed and one or more optional strings as its input parameters. The declaration might look something like this:

```{code-block} python
#   +-- analysis function
#   |
#   V
def t(f, s=None, s2=None):
#     ^  ^
#     |  |
#     |  +- optional inputs for f 
#     +- analyzed function 
```

Finally, when we call `t`, what do we want its behavior to be? I'm sure that you would love `t` to find all bugs in `f` when run on `s`[^fn3] and maybe even return a corrected version of `f`, but that's not what the Python interpreter does for syntax and runtime errors. Once it gets confused, it stops and tells us about its current issue. We're expected to fix that issue and then run the interpreter on our modified script to find the next bug.

In using `t`, we will follow this same incremental approach. However, `t` will say nothing about the error it finds, which is how you might have felt about some of the Python interpreter's error messages. Instead, `t` will return either `"Yes"` (meaning that there's a bug in `f` when run on `s`) or `"No"` (meaning that there are no bugs in `f` when run on `s`).

With this complete specification for `t`, let's rename it `found_bug` (evocative of its behavior) and assume that we can import it whenever we need it from a module of the same name. The following code block illustrates the behavior we desire.

```{code-block} python
---
lineno-start: 1
---
# NOT EXECUTABLE, since we haven't written `found_bug`!
from found_bug import found_bug

# Let our tool do its work!
if found_bug(f, s) == "Yes":
    print('Sorry, but function f has a bug on input s.')
else:
    print('Good work, no bugs!')
```

## Decision problems

Problem specifications for programs that take input and output only `Yes` or `No` have a special name in computer science: They are called *decision problems*. Our problem-to-be-solved is a decision problem, although it won't be the only decision program we'll consider in this chapter. In fact, you almost built many such programs in earlier chapters. Here are two:

```{code-block} python
---
lineno-start: 1
---
### chap14/read_story.py

def read_story(s):
    '''Is the story s short enough to read?'''
    lines = s.split('\n')
    if len(lines) < 20:
        return "Yes"
    else:
        return "No"
```

The function `read_story` recognizes that the question asked in ALE 1.2 (i.e., is the story my children picked out short enough to read before bed?) is a decision problem. 

```{code-block} python
---
lineno-start: 1
---
### chap14/contains_dquote.py

def contains_dquote(s):
    '''Does s contain a double quote character?'''
    if '"' in s:
        return "Yes"
    else:
        return "No"

```

The function `contains_dquote` handles an important question we asked in our script from Chapter 2 that pulled dialogue from a story. It too is a decision problem. 

## Uncomputable problems

I'm emphasizing decision problems because they'll play an important role in proving that we can't write `found_bug`. Whoa, you might say. And I'll say, "Yes." This is the first problem that will defeat us, and it's not because we're not smart enough. It's because Alan Turing proved in 1936 that no one could write an algorithm to solve it. No algorithm means no solution. Finding all bugs in all scripts is an example of an *uncomputable* problem. I'm sorry that we can't deliver our super tool.

You might also hear theoretical computer scientists call this category of problems *undecidable*. There's two reasons for this. First, and most simply, they like to use the terms compute, decide, and solve interchangeably. Second, and a bit more technical, it's because they often prove that a problem *P* is uncomputable by a technique called *reduction*, which is a fancy term for showing the solution to *P* depends upon the solution to some other problem (call it *D*). If a decision problem *D* has been proven to be undecidable and you can show that a solution to *D* is required in a solution to *P*, then *P* is undecidable too.

While I'm mentioning types of problems, now's a good time to say that we've been writing what are called solutions to *tractable* problems, which means we know of algorithms that can solve these problems *efficiently* (i.e., producing an answer takes a reasonable amount of time and other computational resources). Web search, which we discussed in Act II, is an example of a non-trivial, but tractable problem.

There are also *intractable* problems, which means that we know of algorithms to solve the problem, but they all take inordinate amounts of time (or other computational resources) for any reasonably large input. Interestingly, computer scientists have found ways to make some intractable problems useful. The fact that you routinely and securely communicate with your bank over the internet is because of an intractable problem. In particular, decrypting an encrypted message is tractable when you know the secret key, but it is an intractable problem (as far as we know) to computationally find the key given only the encrypted message. Your bank transactions are secure from any computational adversary.

Returning to our uncomputable problem-to-be-solved, I'll shortly sketch the argument that is a proof by contradiction, a technique common in logic, mathematics, and everyday life. Such proofs typically proceed by assuming some statement (e.g., I've already walked my dog or an algorithm exists that finds all bugs in all scripts) is true and then identifying consequences that flow from this assumption. If we can find a consequence that we know cannot be true, then we've found a contradiction in our reasoning. This falsity then flows all the way back (assuming you made no mistakes in your logic) to your initial assumption, which you can now safely say is false.

But before we jump into this argument, let's warm-up with a bit of program analysis that asks a decidable bug-finding question. This work will provide an important foundation for the next two chapters and illustrate that all is not lost in building tools that help us to uncover, understand, and ultimately fix the errors in our scripts. 

## An analysis that finds a bug

*Program analysis* is a big topic in computer science. There are simple analyses and quite complicated ones. We're going to write a fairly simple analysis that looks for a specific kind of error related to string literals. I've picked this example because it will remind you of the script we wrote in Chapter 2---the one that grabbed all the pieces of dialogue from a book (stored as a text file) and printed that dialogue as a rough sketch of a theatrical script. That script, which we called `script32.py`, processed its input looking for pairs of double-quote characters. We can use its structure to write a new script that finds *unpaired* double quotes in a Python script (i.e., instances where we find the opening double-quote character of a string literal but not its matching closing double quote).

Recall that, in writing `script32.py`, we realized that the input text could be split by the double-quote character into an alternating list of narratives and dialogues. If a double-quote character only appeared in Python scripts as a delimiter around string literals, then we could reuse our old script, stripping out the parts that captured the dialogue since we are only interested in the error condition (i.e., an unmatched double-quote character). The function `string_bug`, in the code block below, does this work.[^fn4]

```{code-block} python
---
lineno-start: 11
---
### chap14/string_bug.py

def string_bug(f):
    # Create a worklist of the form: non-string, string, ...
    work_list = f.split('"')
    items = len(work_list)
    
    # Handle the case of a function f that doesn't
    # contain any string literals
    if items == 1:
        # No double-quote string literals in f
        return "No"
    
    # Process just the strings in the worklist. String literals
    # defined using a single double-quote character cannot
    # contain a newline character.
    for i in range(1, items, 2):
        if '\n' in work_list[i]:
            return "Yes" 
    
    # Make sure the last string ended with a double quote, which
    # means the length of work_list should be odd.
    if items & 1 != 1:
        return "Yes"
    
    return "No"
```

Unfortunately, unlike most stories, it is common to use double-quote characters in other ways in Python. For instance, Chapter 2's `script32.py` included the line `i = the_line.find('"')`, which is how we expressed that the script should search for a double-quote character in `the_line`. Double quotes might also legally appear unpaired in a Python comment, and they're used in delimiting multi-line string literals. While we could add code to `string_bug` to handle these three cases, we don't need to be that complete in this quick exercise. We're just trying to get a feel for program analysis on some function `f`, and so let's specify that we won't analyze any `f` with `string_bug` that includes double quotes for anything other than delimiting a string literal.

## Running our simple analysis

Let's now create a function for `string_bug` to analyze. We'll start small and use a variant of "Hello world" nestled in a function called `hello`. There's no syntax error in this function, and so let's also create a copy that removes the second double-quote character, which I'll call `hellu`.

```{code-block} python
---
lineno-start: 2
---
### chap14/hello.py

def hello(s):
    print("Hello", s)
```

```{code-block} python
---
lineno-start: 1
---
### chap14/hellu.py

def hellu(s):
    print("Hello, s)
```

We run `string_bug` by passing the functions `hello` and `hellu` as strings containing their source statements. But how do we get those strings? `hello` and `hellu` are names of function objects, not strings.

The next code block extends `hello.py` to illustrate two different ways that we could create the string we need. On lines 7-9, I define a string variable named `hello_str` to which I assign a string literal that is just a copy of lines 4-5. Yes, I highlighted lines 4-5, hit copy, and then pasted this text between a pair of three single quotes. If you're thinking: "Two copies! That's error prone!" Bravo. A better solution is to use the `getsource` function in the Python `inspect` module, which returns the text of an object's source code as a string (see line 11). With `getsource`, when I change lines 4-5, that change will be reflected in the string returned on line 11.

```{code-block} python
---
lineno-start: 1
---
### chap14/hello.py
import inspect

def hello(s):
    print("Hello", s)

hello_str = '''def hello(s):
    print("Hello", s)
'''

hello_src = inspect.getsource(hello)
```

We could define a variable like `hello_src` for every function we wish to analyze, but again, that seems like unnecessarily repetitive work. Instead, let's build a script that automates the work we'll repeatedly do. Yes, this act is all about building tools that help us work more efficiently!

## A tool for running analyses

We just learned how to go from a function object to a string containing the Python statements that define it. It's also helpful in building tools to know how to go from a string containing the name of a Python function to that function's object. Python makes this easy using `import_module` from the `importlib` library, which allows you to import a module whose name you know only at runtime, and the built-in function `getattr`, which grabs an object in a namespace. It's easiest to understand these two utilities in the context of what we want to do.

```{code-block} python
---
lineno-start: 1
---
import importlib

analysis_name = 'string_bug'
analysis_mod = importlib.import_module(analysis_name)
analysis_fun = getattr(analysis_mod, analysis_name)
```

The small code block above defines the string `analysis_name` with the name of the analysis function we just wrote. To get the function object for this name from its module, we first call `import_module` with the name of the module containing the function, which in this case happens to be the same as the function's name. This call returns a module object (i.e. `analysis_mod`), and we use `getattr` to grab the attribute named `string_bug` (the second parameter in the `getattr` call) from this module object. The returned result is the function object for `string_bug`.

Combining this new knowledge with our knowledge of how to get the source code corresponding to a function object, we can write a helper routine for grabbing the source code for a function whose name we know only at runtime. Focus your attention on lines 11-13 and 17 in `grab_f`; I'll explain why I wrapped lines 11-13 in a try-except-statement in a moment.

 ```{code-block} python
---
lineno-start: 1
---
### chap14/our_tools.py
import importlib
import inspect

def grab_f(fun_name):
    '''A utility function that returns a string containing the
    source code for `fun_name`. Its implementation assumes that
    the function `fun_name` is in a Python script of the same
    name (i.e., `fun_name.py`).'''
    try:
        fun_module = importlib.import_module(fun_name)
        fun_object = getattr(fun_module, fun_name)
        fun_src = inspect.getsource(fun_object)
    except SyntaxError:
        # Use the helper function instead
        fun_src = grab_f_with_error(fun_name)
    return fun_src
```

We're now ready to build a tool that allows us to run any analysis (e.g., `string_bug`) on any function (e.g., `hello` or `hellu`). We'll call this analysis tool `analyze` and place it in `analyze.py`. Because we've talked about `found_bug` taking two parameters and shown `string_bug` taking just one (recall that finding syntax errors doesn't require an example input), we'll make `analyze` smart enough to handle either case. You should understand everything in the next code block.

```{code-block} python
---
lineno-start: 1
---
### chap14/analyze.py
import sys
import importlib
import our_tools

def analyze(analysis_name, fun_name, s=None):
    # Grab the analysis function from its module. This code assumes that the
    # `analysis_name` is in `analysis_name.py`.
    analysis_mod = importlib.import_module(analysis_name)
    analysis_fun = getattr(analysis_mod, analysis_name)
    
    # Grab function to analyze. The utility used assumes that
    # the function `fun_name` is in `fun_name.py`.
    f = our_tools.grab_f(fun_name)
    
    # Do the analysis
    if s == None:
        ans = analysis_fun(f)
    else:
        ans = analysis_fun(f, s)
    print(ans)
```

You can either call the function `analyze` from a script or run `analyze.py` in the shell (i.e., I configured `main` in `analyze.py` to call the function `analyze` with the parameters specified on the command line). I illustrate this second option in the transcript below.

```{code-block} none
---
emphasize-lines: 2
---
### Run the highlighted command in the shell
chap14$ python3 analyze.py string_bug hello
no
```

```{admonition} You Try It
Try analyzing the function `hello` using the analysis routine `string_bug`, as shown in the above code block. It should report that there are no mismatched double-quote characters in this function, which is what we expect.
```

## Grabbing a function with a syntax error

What do you expect to happen if you run this tool (`analyze.py`) with the same analysis function (`string_bug`) on the function `hellu`, which we saw earlier contains mismatched double quotes? The tool should print `Yes` because `hellu` contains the type of syntax error that `string_bug` finds.

```{admonition} You Try It
Try analyzing the function `hellu` using the analysis routine `string_bug` and verify that it reports that there is a bug in the analyzed function.
```

Our tool `analyze.py` worked, but it wasn't because `inspect.getsource` did its work for us. To see this, put a debugging print-statement into the function `grab_f` between lines 12 and 13, and run the tool again. You'll notice that this print never executes. Why is this?

It's because `importlib.import_module` won't complete when the imported function contains a syntax error. We're basically invoking the Python interpreter to read the definition of this function, and we know that the interpreter doesn't like syntax errors. As soon as it encounters a syntax error, the interpreter throws a `SyntaxError` exception.

This is a problem for us because we want our analysis routine to find the syntax error, not the Python interpreter. Now you see the reason for the try-except-statement in `grab_f`. It catches the `SyntaxError` exception and calls `grab_f_with_error`, which you can review in `our_tools.py`. This helper function grabs the source code of `fun_name` by opening its file the old fashioned way (i.e., the way we learned in Chapter 1) and scanning through the file lines until it discovers the definition of the function.[^fn5]

## Analyzing other functions

You now have a tool (`analyze.py`) with which you can run many different analysis functions on many different functions and their example inputs. Let's try several.

```{admonition} You Try It
*   Check for mismatched double quotes in `read_story` by running `python3 analyze.py string_bug read_story`. Do you agree with the response?
*   Use `read_story` as your analysis routine by running `python3 analyze.py read_story hello`. Then replace `hello` with `hellu` and run the analysis again. Finally, replace `hellu` with `string_bug`, which flips the two parameters in the previous bullet. Do the responses `Yes`, `Yes`, and `No` make sense? This demonstrates that we can use any decision program that takes one or two input parameters as our analysis function. We just have to know what question an analysis function asks and answers.
*   Run `python3 analyze.py read_story read_story`. Our tool even permits us to have an analysis function analyze itself. We'll take advantage of this capability in a bit.
*   Finally, run `python3 analyze.py string_bug string_bug` and consider its response. 
```

Whether the response you got from the last bullet's run was `Yes` or `No` depends upon whether the first non-comment statement in `string_bug` assigned `f.split('"')` or `f.split(DQC)` to `work_list`. Since `string_bug` isn't programmed to ignore a single double-quote character in a string literal, the source code of `string_bug` listed earlier in this chapter returns `Yes` when analyzing its own source code. The `string_bug` function you'll find in the book's GitHub repo, on the other hand, replaces this "offending" string literal with a global variable, causing the analysis to report `No`.

## Using s

We talked about providing our analysis functions with an example input `s`, but we haven't used it in any of our examples. Let's change that now.

We'll begin with an analysis function that takes an `f` and an `s` but performs no analysis; it ignores its two inputs and always returns `Yes`.

```{code-block} python
---
lineno-start: 1
---
### chap14/yes.py

def yes(f, s):
    '''An analysis function that does no analysis
    and always returns yes.'''
    return "Yes"
```

Try using it to analyze the function `hello`:[^fn6]

`python3 analyze.py yes hello world`

Feel free to replace `hello` with any other of our functions that take a single string input, and try running it. It also doesn't matter what you use in place of `world`, since `yes` ignores both input parameters.

These experiments are not all that interesting, and so let's modify `yes` to do something with `f` and `s`, which we'll call `yep`.

```{code-block} python
---
lineno-start: 1
---
def yep(f, s):
    '''A null analysis function with a backdoor to
    some special functionality. Always returns Yes.'''
    if s == 'beVerbose':
        # Print the inputs
        print('*** f ***')
        print(f, end='\n\n')
        print('*** s ***')
        print(s, end='\n\n')
    
    # Default answer
    return "Yes"
```

```{admonition} You Try It
Run `python3 analyze.py yep hello world` and then rerun this command replacing `world` with `beVerbose`. 
```

Like `yes`, `yep` always returns `Yes`, but it checks the value of its input parameter `s` and does something on a particular value of `s`. This simple function illustrates a key point to keep in mind: While `s` is an example of an input string to `f`, it is not fed directly to `f` but used by the analysis function. Ideally, the analysis `yep` would use the value of `s` in reasoning about `f`, but in this implementation, it prints the values of `f` and `s` before returning `Yes`. These print-statements technically keep `yep` from being a decision program, and let's fix that.

```{code-block} python
---
lineno-start: 1
---
### chap14/yex.py

def yex(f, s):
    '''A null analysis function with a runtime error
    in its backdoor. Meant to always return Yes.'''
    if s != 'beQuick':
        # Waste some time so it looks like we're
        # doing some analysis.
        cnt = 0
        while cnt < 1000000:
            cnt -= 1
    
    # Default answer
    return "Yes"
```

This version (called `yex`) replaces the print statements in `yep` with a loop that is meant to make this new analysis function look like it's working hard to analyze the input function (while all it does is waste time doing a useless calculation). 

```{admonition} You Try It
Run `python3 analyze.py yex hello world`. When you get tired of waiting for it to end (it won't), type Ctrl-C in your shell window or hit the stop button if you're running in Replit or Google Colab. 
```

I clearly made a mistake in implementing `yex`; the while-loop in `yex` is an infinite loop. Since `cnt` starts at `0`, I should have typed `+=` on line 11, but I mistakenly typed `-=`. This is the type of coding error that we don't notice until we run our function and test it under the right conditions. It's the kind of error we'd love `found_bug` to identify when we ask it to analyze `yex`.

```{admonition} Terminology
:class: tip
Because `yex` contains an infinite loop that's not in its specification, we'll say that the output of `yex` is _not defined under this input_. It exhibits _undefined behavior_.
```

```{tip}
A program with an infinite loop doesn't have to be the result of a design or coding error. This behavior might be in the program's specification. If you're on a Unix-based operating system, check out the program called `yes`, whose man page says that it will "be repetitively affirmative." Try running it without a command line parameter and then with your favorite single word. Type Ctrl+C to stop its execution. This program is [surprisingly useful](https://en.wikipedia.org/wiki/Yes_%28Unix%29).
```

## A non-trivial decision problem

Having seen examples of decision functions and discussed some program analyses, it's time to tackle a function specification with surprising consequences. This specification describes a function that takes two or three inputs:

* `f`, a string containing a function's source code;
* `s`, a string that is a valid input to `f`;
* `s2`, an optional string for when `f` takes two string inputs.[^fn7]

While this interface matches the one we defined for `found_bug`, this new function solves a different decision problem. It exhibits the following behavior:[^fn8]

* Return `Yes` when `f(s)` returns `Yes`;
* Return `No` otherwise.

Clearly this new function should return `No` when `f(s)` returns `No`, but `f` doesn't have to be a decision function. Our new function should return `No` for all other outcomes of the execution of `f` given `s`, including ones that are akin to error checking: 

1. `f` contains one or more syntax errors (i.e., it is not a valid Python function);
2. `f(s)` is undefined (e.g., `f` falls into an infinite loop, throws an exception, prints anything, or returns something other than `Yes` or `No`). 

Let's call this new function `yes_on_s`.[^fn9] Its specified behavior seems straightforward, but let's stress test our understanding through a few examples. I'll present them using the command line syntax we used with our `analyze` tool, but remember that you cannot run these command lines as we haven't written `yes_on_s`. Also, to avoid having to write out the source code for a function when I want it as an input string (i.e., in the `s` position), I'll write the function name with the suffix `_src`. For instance, in Example 3 below, I'm passing the source code for `string_bug` as the input to `read_story`.

```{admonition} You Try It
Work through each of the following examples. Don't take my word for what will be printed each time. Make sure you can describe what takes place in your own words. 
```

```{code-block} md
                                      +-- analysis function
                                      |
                                      V
**Example 1:** `python3 analyze.py yes_on_s hellu world` 
                                             ^     ^
                                             |     |
                                             +- f  +- s 
```

This should print `No`. The function `hellu` is not valid Python since it contains a syntax error.

**Example 2:** `python3 analyze.py yes_on_s read_story world`

This should print `Yes`. `read_story('world')` returns `Yes` because `'world'` is a one-word, one-line story and that meets the specification of "short enough to read."

**Example 3:** `python3 analyze.py yes_on_s read_story string_bug_src`

This should print `No`. The number of lines in `string_bug` is greater than 20, and since `read_story` returns `No` then `yes_on_s` would return `No`.

**Example 4:** `python3 analyze.py yes_on_s contains_dquote contains_dquote_src`

This should print `Yes`. The statements in `contains_dquote` do indeed contain a double-quote character. As I noted in an earlier You-Try-It exercise, it's possible for a function to read and analyze its own source code.

**Example 5:** `python3 analyze.py yes_on_s yep hello_src world`

This should print `Yes`. I'm taking advantage of the fact that `yes_on_s` can also handle a function `f` that takes two input parameters. Since `world` doesn't trigger `yep`'s backdoor, it returns `Yes` and so would `yes_on_s`.

**Example 6:** `python3 analyze.py yes_on_s yep hello_src beVerbose`

This should print `No`. The string input `beVerbose` triggers `yep`'s backdoor and prints stuff, and that action would cause `yes_on_s` to return `No`.

**Example 7:** `python3 analyze.py yes_on_s yex hello_src world`

This should print `No`. Because `yes_on_s` is analyzing `yex` on the inputs and not actually running it, it would recognize and not get caught in `yex`'s infinite loop.

## An indecisive decision function

In addition to understanding `yes_on_s` by reasoning about its behavior on different inputs, we can also use `yes_on_s` to build other functions, even without an implementation. We'll then reason about the behavior of these new functions given the specification of `yes_on_s`.

Let's begin with a function called `yes_on_self` that takes a single parameter `f` and returns `Yes` when `f(f_src)` returns `Yes` and No otherwise. This function's behavior might be easier to understand when viewed as Python statements:

```{code-block} python
---
lineno-start: 1
---
### chap14/yes_on_self.py
import our_tools
from yes_on_s import yes_on_s   # pretend it exists

def yes_on_self(f):
    return yes_on_s(f, our_tools.grab_f(f))
```

We can rewrite Example 4 using this new function:

**Example 8:** `python3 analyze.py yes_on_self contains_dquote`

This should print `Yes`, since it is equivalent to the command line in Example 4.

```{admonition} You Try It

Replace `contains_dquote` in Example 8 with `read_story` or `string_bug`. What would these commands return?[^fn10] 
```

When you're comfortable reasoning about the behavior of `yes_on_self`, think about what Example 9 would print.

```{code-block} md
                                      +-- analysis function
                                      |
                                      V
**Example 9**: `python3 analyze.py yes_on_self yes_on_self`
                                                   ^
                                                   |
                                                   +- f
```

To help you get started, recall that `yes_on_s` returns `Yes` when `f` is valid Python and `f(s)` is defined, both of which are true given the code we've written for `yes_on_self` coupled with our assumption that `yes_on_s` exists. It's left for us to determine whether the `f(s)` part of `yes_on_s`'s specification returns `Yes` or `No`.

While we don't have an implementation for `yes_on_s`, we know its behavior and that's sufficient for us to reason out the answer. As humans, we're quite good at gaining insight from reflecting on an object's behavior. Here are the two cases we need to consider:

**Case 1:** Assume `yes_on_self` (in the analysis function position) returns `Yes`. What does this assumption imply that the call to `yes_on_s` inside `yes_on_self` returned? It must have been `Yes` (i.e., `yes_on_self` returns whatever `yes_on_s` returns). Inside the call to `yes_on_s`, we can ask what `f(s)`, or more precisely `yes_on_self(yes_on_self_src)`, returned. It must have been `Yes` for `yes_on_s` to return `Yes`. No contradictions in this logic, and that's good. 

**Case 2:** Assume `yes_on_self` (in the analysis function position) returns `No`. Again, what does this assumption imply about the strings returned by `yes_on_s` and `f(s)`? Both would have been `No`. Oh no.

Our reasoning finds that both `Yes` and `No` are valid outcomes, but the command in Example 9 cannot print both answers. As MacCormick says in his text, if you find this indecision mysterious, you are not alone.

## Insight from indecision

As we saw with the Unix program `yes` (which contains an infinite loop) and when talking about intractable problems, we can sometimes turn a program with an undesirable behavior into a useful tool. Let's do that with the core of `yes_on_self`. In particular, we'll define a new function called `trouble` that makes the same call to `yes_on_s` as `yes_on_self` did, but flips the response before returning it. Again, this behavior might be easier to understand when viewed as Python statements:

```{code-block} python
---
lineno-start: 1
---
### chap14/trouble.py
import our_tools
from yes_on_s import yes_on_s   # pretend it exists

def trouble(f):
    if yes_on_s(f, our_tools.grab_f(f)) == "Yes":
        return "No"
    else:
        return "Yes"
```

We can repeat Example 8, replacing `yes_on_self` with `trouble`, to create Example 10 and verify that `trouble` works as we expect, at least when we don't ask it to analyze itself.

**Example 10:** `python3 analyze.py trouble contains_dquote`

This should print `No`, since the source code of `contains_dquote` does include a double-quote character but `trouble` turns this `Yes` into a `No`.

The implication in this new function's name arises when we ask it to analyze itself.

```{code-block} md
                                      +-- analysis function
                                      |
                                      V
**Example 11:** `python3 analyze.py trouble trouble`
                                              ^
                                              |
                                              +- f
```

Personally, I always have a hard time clearly articulating what strange question this command asks. But we can reason about the possible answers as we did with `yes_on_self` and see where it leads.

**Case 1:** Assume `trouble` (in the analysis function position) returns `Yes`. What does this assumption imply that the call to `yes_on_s` inside `trouble` returned? It must not have been `Yes` so that the else-clause executed. Inside the call to `yes_on_s`, we can ask what `f(s)`, or more precisely `trouble(trouble_src)`, returned. It also must not have been `Yes` for `yes_on_s` to return a response that wasn't `Yes`. But this creates a contradiction between this consequence and our original assumption, and we now know that Example 11 does not print `Yes`.

**Case 2:** Assume `trouble` (in the analysis function position) returns `No`. Again, what does this assumption imply about the strings returned by `yes_on_s` and `f(s)`? Both would have been not `No`. Again, a contradiction. Example 11 does not print `No`.

Double trouble. There are only two values Example 11 can print, and we just proved that neither is possible. The only conclusion left to us is that the function `trouble` cannot exist.

But, you say, `trouble` must exist because we listed its source code in the previous code block. It's true that that block does contain some Python statements, but the function `trouble` isn't valid Python. It contains a syntax error because `yes_on_s.py` doesn't exist. Everything else in `trouble` is valid Python, and so the contradiction we found is really pointing at the function `yes_on_s` and saying that it cannot exist.

## Specifications without implementations

Let's remind ourselves of what we just did. We wrote a specification for a decision problem involving one function analyzing another in which the analysis function would return `Yes` if and only if the analyzed function when executing on the given input would return `Yes`. For any other returned value, or if there was some sort of syntax or runtime error in the analyzed function, the analysis function would return `No`. We then followed a line of reasoning that proved that it was impossible to write code that adhered to the specified behavior. And from this, we learned an important fact about computation: *There exist behaviors for which we cannot write a program, in Python or any other programming language.* These are uncomputable problems.

```{tip}
Before you try to solve a given problem, check to see if it is known to be uncomputable or intractable. The former will have you toiling forever trying to code a solution. The latter you can code, but it'll take practically forever for your script to finish executing.
```

We used a number of tools, without naming most of them, commonly employed by those who prove theories about programs. If you're interested in learning more, I encourage you to pick up MacCormick's *What Can Be Computed?* (or a similar book). He uses the impossibility of `yes_on_s` to show that:

1. Other uncomputable problems exist.
2. One of those problems is determining if a Python function `f` running on input `s` is going to throw an exception. And with this result, our dream of writing a program to find all bugs in all our scripts is dashed.

While it is true that we can't expect a computational tool to find all our bugs, this doesn't mean we can't find lots of specific kinds of bugs in many different kinds of programs. On to the next two chapters to learn how you can more quickly and easily find many important bugs!

\[Version 20241004\]

[^fn1]: When computer scientists talk about static analyses, they mean analyses that ask questions about a program or how a program will execute without running it. Runtime debugging involves asking questions about a program's state while it executes. All the functions in this chapter perform static analysis.

[^fn2]: The programs we've written and those we'll discuss in this chapter have a single program point where execution begins (e.g., the first line in a Python script or the function \`main\` in other programming languages), or they can be easily converted into such a program.

[^fn3]: Unless it's important to the context, I'll simplify my explanations and talk about analyzing a function \`f\` that takes a single input \`s\`.

[^fn4]: Notice that \`string\_bug\` treats its work as a decision problem. It returns \`Yes\` if it finds an instance of an unmatched double-quote character and \`No\` otherwise. Also, because it finds a syntax error, it takes only the text of the input function \`f\`. It doesn't need an example input.

[^fn5]: The helper function's implementation expects the \`def fun\_name:\` source code line to be unindented. This shortcoming is sufficient for our work in this chapter.

[^fn6]: Note that \`yes\` refers to the function \`yes\`, while \`Yes\` is the string output of our decision programs (and decision functions).

[^fn7]: This optional third argument allows us to work with the full range of decision functions we've seen in this chapter.

[^fn8]: For functions \`f\` that take two string arguments, mentally replace \`f(s)\` with \`f(s,s)\`. All the specification asks is that this new function returns \`Yes\` when \`f\` returns \`Yes\` on the input string or strings.

[^fn9]: With the function names and the flow of the argument that follows, I borrow heavily from Chapter 3 of *What Can Be Computed: A Practical Guide to the Theory of Computation* by John MacCormick \[Princeton University Press, 2018\]. I am grateful for what John's textbook taught me about making this material accessible to everyone.

[^fn10]: It would print Yes with the first change and No with the second.