# Glossary

**abstraction**: a description of an object where we focus on a coherent set of its characteristics, ignoring the rest of its details. Seemingly different objects (e.g., a string and a list) can share the same abstraction. Abstraction is a fundamental idea in computer science.

**abstraction barrier:** the separation we maintain between operations that adhere to the abstraction we've built and operations that violate that abstraction. Abstractions work best when both the client using the abstraction and the implementor of the abstraction adhere to the abstraction barrier.

**actual parameters:** the names we provide as the inputs to a function at a call site. The value of the actual parameters gain new names (i.e., the **formal parameters**) inside the function.

**algorithm (Chapter 3):** a sequence of well-specified steps that a computer can execute.

**aliases**: what we call two names that refer to the same object.

**attributes**: Python objects can be complex things with many different pieces to them. These object pieces are what computer scientists call attributes.

**bind:** the term used when pairing an actual parameter with its formal parameter during a function call.

**class**: see **type** (until Act II).

**command**: common way to direct the Python interpreter to do something, often of the form `command(input parameters)`.

**comments**: text following a `#` in Python. We use them to capture our pseudocode, and then after we have written some Python statements, we use them to explain what blocks of our code do.

**design pattern**: an approach to solving some programming problem that you'll see in many pieces of code.

**deterministic**: a process in which an input always produces the same output. See also **non-deterministic**.

**docstrings:** strings that we add immediately after a function definition that provide more information about the purpose, inputs, outputs, and assumptions of the function.

**encoding (Chapter 2):** taking a countable number of something and associating a number with each something, typically a dense manner (e.g., if we had 8 things, we would number them 0 to 7).

**encoding (Chapter 3):** taking an abstract concept or complex phenomenon and giving it a specific representation. For example, we might take the concept of the letter `'b'` and write it as a black, 12-point, Courier letter `'b'` or represent its sound in English with the word "bee." Or as another example, the concept of "long" as a Spanish word (i.e., "largo"). Compare this definition of encoding with the previous one (from Chapter 2). Think about how they are actually the same!

**error messages**: feedback we get from the Python interpreter when we asked it to do something it didn't understand.

**finite state machine**: a mathematical model for organizing a system that moves through a series of tasks. In these models, there is a finite number of tasks to perform.

**for-loop**: a type of looping statement in Python that works particularly well for iterating over a sequence. Contrast with **while-loop**.

**formal parameters:** the names we give to the input values to a function. These are available in the function body. See **actual parameters**.

**function (Chapter 1)**: other names for commands of the form `command(input parameters)`. An important set of functions are those that are built into Python (e.g., `help`). See also **method** and **procedure**.

**function (Chapter 3):** with **method** and **procedure**, ways to implement code abstraction, where we hide the implementation of some command behind a carefully designed interface, which tells you what inputs are required and what result will be produced.

**function composition**: every programming language has a set of rules for how it evaluates expressions built from a number of simple functions. Python evaluates nested functions (i.e., ones nested inside parentheses) innermost first, which should feel natural as it mimics what we do in basic algebra. You will also see a string of function applications (typically as method applications), and Python evaluates these in a left-to-right manner.

**immutable:** something that you cannot change after it is created (i.e., given its initial value or state). See **mutable**.

**infix operator**: another way to command the interpreter to do something for us, but the command sits *between* its input parameters, e.g., `2 + 3` where `+` is the command to perform addition.

**internationalization:** the process of making a piece of software work with multiple human languages. See **localization**.

**library:** another name for a collection of modules. Programming languages like Python are typically distributed with what is called a *standard library*, which you can think of as the modules and packages available by default wherever the programming language is installed. [The Python standard library](https://docs.python.org/3/library/) includes built-in constants (e.g., `True`), data types (e.g., `str`), and functions (e.g., the file I/O facilities) that we have repeatedly used in our scripts.

**literal object**: what computer scientists call an explicit value that we write into our scripts (e.g., `'Cosmo the dog'` is a string object whose value is that string).

**localization:** specializing an internationalized piece of software for the user's native language. See **internationalization**.

**method**: see **function** (until Act II).

**module:** a Python script from which we want to pull some function or other definitions to use in our own script. See **library**.

**mutable:** something that can change its value or state. See **immutable**.

**name**: what the assignment operator (`=`) allows us to do with the object that is the result of a command. We can also use this operator to give another name to an existing object.

**namespace**: a context that helps you keep track of what object a particular name resolves to. A name might have meaning in one namespace, but not another.

**non-deterministic**: a process where an input can produce a number of different outputs. See **deterministic**.

**object**: pretty much everything in Python is an object with which we can interact.

**off-by-1 error**: a very common error in programming, where you think you know where a sequence starts or ends, but you misjudge it by a single step.

**optional parameters**: some commands take lots of input parameters, but you need specify only a few of them and the Python interpreter assumes you meant the default for the other unspecified ones (e.g., `open` assumes you want to open a file for reading).

**overloading**: using the same operator (e.g., `+`) or method name (`find`) for distinctly different operations. The actual operation performed depends upon the type of operands you provide to the operator or method.

**package:** a whole set of helpful modules that a Python programmer wishes to distribute all together. There are many packages in Python. The [Python Package Index](https://pypi.org/) is an online repository of software for the Python programming language.

**procedure**: another term for **function**.

**result**: every Python function returns a result.

**return-statement:** the statement that directs a program's control flow from the current function back to the function that called it (i.e., back to the current call site). A return-statement may include an expression, which becomes the value of the call-site expression. If no expression is provided, Python returns the special value `None`.

**sequence**: an ordered collection of items.

**standard:** a documented and widely agreed-upon method for doing something.

**state**: information that describes the current context. A machine keeps state to help it remember what it is doing.

**statement**: you can think of these as sentences in the language of Python. The Python interpreter will read and execute them one at at time.

**stream**: the abstraction we'll use for an open file (e.g., an open text file is read as a stream of characters).

**syntax**: rules we need to follow when saying something in any programming language.

**type**: what defines the kinds of attributes you'll find on a Python object.

**Unicode:** a standard that attempts to map every character in every language into a set of numbers.

**utf-8 and latin-1:** two of many character encoding systems, which assign characters to specific bit patterns. See **encoding**.

**variable**: what we call names that refer to (i.e., name) different objects at different points in our script.

**while-loop**: a type of looping statement in Python that works particularly well for iterating until you hit some exit condition. Contrast with **for-loop**.

\[Version 20230706 --- through Chapter 3\]
