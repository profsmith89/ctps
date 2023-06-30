# Glossary

**abstraction**: a description of an object where we focus on a coherent set of its characteristics, ignoring the rest of its details. Seemingly different objects (e.g., a string and a list) can share the same abstraction. Abstraction is a fundamental idea in computer science.

**aliases**: what we call two names that refer to the same object.

**attributes**: Python objects can be complex things with many different pieces to them. These object pieces are what computer scientists call attributes.

**class**: see **type** (until Act II).

**command**: common way to direct the Python interpreter to do something, often of the form `command(input parameters)`.

**comments**: text following a `#` in Python. We use them to capture our pseudocode, and then after we have written some Python statements, we use them to explain what blocks of our code do.

**design pattern**: an approach to solving some programming problem that you'll see in many pieces of code.

**deterministic**: a process in which an input always produces the same output. See also **non-deterministic**.

**encoding**: taking a countable number of something and associating a number with each something, typically a dense manner (e.g., if we had 8 things, we would number them 0 to 7).

**error messages**: feedback we get from the Python interpreter when we asked it to do something it didn't understand.

**finite state machine**: a mathematical model for organizing a system that moves through a series of tasks. In these models, there is a finite number of tasks to perform.

**for-loop**: a type of looping statement in Python that works particularly well for iterating over a sequence. Contrast with **while-loop**.

**function**: other names for commands of the form `command(input parameters)`. An important set of functions are those that are built into Python (e.g., `help`). See also **method** and **procedure**.

**function composition**: every programming language has a set of rules for how it evaluates expressions built from a number of simple functions. Python evaluates nested functions (i.e., ones nested inside parentheses) innermost first, which should feel natural as it mimics what we do in basic algebra. You will also see a string of function applications (typically as method applications), and Python evaluates these in a left-to-right manner.

**infix operator**: another way to command the interpreter to do something for us, but the command sits *between* its input parameters, e.g., `2 + 3` where `+` is the command to perform addition.

**literal object**: what computer scientists call an explicit value that we write into our scripts (e.g., `'Cosmo the dog'` is a string object whose value is that string).

**method**: see **function** (until Act II).

**name**: what the assignment operator (`=`) allows us to do with the object that is the result of a command. We can also use this operator to give another name to an existing object.

**namespace**: a context that helps you keep track of what object a particular name resolves to. A name might have meaning in one namespace, but not another.

**non-deterministic**: a process where an input can produce a number of different outputs. See **deterministic**.

**object**: pretty much everything in Python is an object with which we can interact.

**off-by-1 error**: a very common error in programming, where you think you know where a sequence starts or ends, but you misjudge it by a single step.

**optional parameters**: some commands take lots of input parameters, but you need specify only a few of them and the Python interpreter assumes you meant the default for the other unspecified ones (e.g., `open` assumes you want to open a file for reading).

**overloading**: using the same operator (e.g., `+`) or method name (`find`) for distinctly different operations. The actual operation performed depends upon the type of operands you provide to the operator or method.

**procedure**: another term for **function**.

**result**: every Python function returns a result.

**sequence**: an ordered collection of items.

**state**: information that describes the current context. A machine keeps state to help it remember what it is doing.

**statement**: you can think of these as sentences in the language of Python. The Python interpreter will read and execute them one at at time.

**stream**: the abstraction we'll use for an open file (e.g., an open text file is read as a stream of characters).

**syntax**: rules we need to follow when saying something in any programming language.

**type**: what defines the kinds of attributes you'll find on a Python object.

**variable**: what we call names that refer to (i.e., name) different objects at different points in our script.

**while-loop**: a type of looping statement in Python that works particularly well for iterating until you hit some exit condition. Contrast with **for-loop**.
