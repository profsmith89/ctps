## Chapter 3 ##

## ALE 3.1: Two kinds of loops

Here are a few rules-of-thumb for deciding which type of loop to use:

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

**Step 1.** [Click this link](https://pythontutor.com/visualize.html#code=def%20my_replace%28s,%20old,%20new%29%3A%0A%20%20%20%20i%20%3D%200%20%20%20%20%20%20%20%20%20%20%20%20%23%20tracks%20where%20we%20are%20in%20the%20input%20string%0A%20%20%20%20j%20%3D%20len%28old%29%20%20%20%20%20%23%20skip-ahead%20amount%20for%20index%20calculations%20%0A%20%20%20%20new_s%20%3D%20s%5B0%3A0%5D%20%20%20%23%20the%20new%20string%20we're%20building%20%0A%20%0A%20%20%20%20while%20i%20%3C%20len%28s%29%3A%20%0A%20%20%20%20%20%20%20%20if%20s%5Bi%3Ai%2Bj%5D%20%3D%3D%20old%3A%20%0A%20%20%20%20%20%20%20%20%20%20%20%20new_s%20%3D%20new_s%20%2B%20new%20%0A%20%20%20%20%20%20%20%20%20%20%20%20i%20%2B%3D%20j%20%0A%20%20%20%20%20%20%20%20else%3A%20%0A%20%20%20%20%20%20%20%20%20%20%20%20new_s%20%3D%20new_s%20%2B%20s%5Bi%3Ai%2B1%5D%20%0A%20%20%20%20%20%20%20%20%20%20%20%20i%20%2B%3D%201%20%0A%20%0A%20%20%20%20return%20new_s%20%0A%20%0Amy_book%20%3D%20%20%5B%20%0A%20%20%20%20'And%20...',%20%0A%20%20%20%20'The%20Cat%20in%20the%20Hat!',%20%0A%20%20%20%20%5D%20%0Awhile%20len%28my_book%29%20%3E%200%3A%20%0A%20%20%20%20the_line%20%3D%20my_book.pop%280%29%20%0A%20%0A%20%20%20%20%20%23%20Having%20some%20fun%20with%20text%20substitution%20%0A%20%20%20%20the_line%20%3D%20my_replace%28the_line,%20'Cat',%20'%5CN%7Bcat%20face%20with%20wry%20smile%7D'%29%20%0A%20%20%20%20the_line%20%3D%20my_replace%28the_line,%20'Hat',%20'%5CN%7Btop%20hat%7D'%29%20%0A%20%0A%20%20%20%20print%28the_line%29%20%0A&cumulative=false&heapPrimitives=nevernest&mode=edit&origin=opt-frontend.js&py=311&rawInputLstJSON=%5B%5D&textReferences=false). It will take you to Python Tutor's visualization page, which is preloaded with our simplified script. Scroll up and down in the leftmost panel and make sure you recognize the code. The variable `my_open_book` has become a list of text lines from which we pop one line at a time for the processing that takes place in our infinite loop. You should otherwise recognize all the code.

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

What is the equivalent to string-find with a list? Search the online documentation for this answer and try it out on `a_list` (i.e., fix the broken statement containing `a_list.find(10)`).

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

## ALE 3.7: Imports and namespaces?

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

## ALE 3.9: Slicing with bad indices

For those diligent souls that looked through the implementation of `my_replace` (first defined in `replace7.py`), you may have noticed that the slice taken on line 8 (i.e., `s[i:i+j]`) could, in certain circumstances, specify invalid indices for the string `s`.

Exactly what circumstances? Well, the while-loop on line 7 takes `i` from `0` to `len(s) - 1`, which are all valid indices in `s`. The slice in the condition on line 8, however, adds `j` to `i` in specifying the end of the slice. Thus, whenever `j > 2` and `i > len(s) - j`, `i + j` will be an index outside the valid indices of `s`.

And yet this implementation of `my_replace` works. Why?

**Step 1.** In the interactive Python interpreter, try the following:

```{code-block} python
---
lineno-start: 1
---
>>> s = 'Learning never exhausts the mind. Leonardo da Vinci'
>>> s[34:len(s)+10]
``` 

The end index of this slice is clearly beyond the length of our input string, but the interpreter happily (i.e., without raising an error) returns `Leonardo da Vinci`. If you read the notes about slicing in [the Common Sequence Operations for sequence types in the Python documentation](https://docs.python.org/3/library/stdtypes.html#common-sequence-operations), you'll learn that the designers of Python have chosen to treat start and end indices greater than `len(s)` as `len(s)`.

**Step 2.** This robustness is a nice feature of Python, but you shouldn't expect it to also be true in other programming languages. Change `ale09.py` so that it doesn't use this feature of Python (i.e., it never asks on line 8 for a slice that includes invalid indices). When you run this script, it checks what it gets back from `my_replace`.

```{code-block} python
---
lineno-start: 1
---
### chap03/ale09.py
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

def main():
    s = 'Learning never exhausts the mind. Leonardo da Vinci'
    soln1 = 'Learning never exhausts the 🧠. Leonardo da Vinci'
    soln2 = 'Learning never exhausts the mind. Leonardo da VINCI'

    # Check a replacement in the input string
    s_new = my_replace(s, 'mind', '\N{brain}')
    print(s_new)
    assert s_new == soln1, "my_replace didn't work correctly"

    # Check a replacement at the end of the input string
    slice = my_replace(s, 'Vinci', 'VINCI')
    s_new(s_new)
    assert s_new == soln2, "my_replace didn't work correctly"

if __name__ == '__main__':
    main()
```

## ALE 3.10: Concatenating lists (or slicing produces sequences)

There's another head-scratching implementation detail in `my_replace` (first defined in `replace7.py`) that you may have noticed: Why is `s[i:i+1]` added to the end of the growing `new_s` instead of the simpler `s[i]` (line 12 in `replace7.py`)?

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
```

The else-block inside the while-loop above executes when the substring starting at index `i` in `s` doesn't match the substring we wish to replace (i.e., `old`). When this is true, the algorithm wants to take the sequence element at index `i` in `s` and concatenate it to the end of `new_s`.

You might think that `s[i]` and `s[i:i+1]` always produce the same result, since both operations seem to say, "Please give me the item at index `i` in `s` and nothing more." But they don't. Let's see this with two simple examples.

**Step 1.** Let's start with a string, which we know acts like a sequence. In the interactive Python interpreter, try the following:

```{code-block} python
---
lineno-start: 1
---
>>> s = 'Python is fun'
>>> s[1]
'y'
>>> s[1:2]
'y'
``` 

When our sequence object is a string, `s[i]` yields the same result as `s[i:i+1]`. This means that `my_replace` could use `s[i]` if `my_replace` was ever to only operate on strings. Go ahead and try changing line 12 in `replace7.py`, whose `main` function only calls `my_replace` with strings.

**Step 2.** Now let's experiment with lists, which is the other type on which we'd like `my_replace` to work. Again in the interactive Python interpreter, try the following:

```{code-block} python
---
lineno-start: 1
---
>>> s = [1, 2, 3]
>>> s[3]
3
>>> s[3:4]
[3]
``` 

When our sequence object is a list, `s[i]` returns *the object* stored at index `i` in `s`, but `s[i:i+1]` returns *a list* containing only the object stored at index `i` in `s`. This means that we cannot use `s[i]` on line 12 in `replace7.py` if `my_replace` is to operate on lists and strings. Go ahead and try it---you'll a `TypeError` saying that you cannot concatenate an integer to a list.

```{code-block} python
---
lineno-start: 1
---
>>> s = [1, 2, 3]
>>> s + s[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
>>> s + s[3:4]
[1, 2, 3]
``` 

Try changing line 14 in `bookshelf1.py` and then run it. This script's `main` function calls `my_replace` with both strings and lists, and it will fail to complete successfully.

The takeaway lesson is: If you want an object that is the item in a sequence, use indexing. If you want an object that is of the same type as the sequence, use slicing.

\[Version 20240813\]