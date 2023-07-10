# Glossary

**abstraction:** a description of an object where we focus on a coherent set of its characteristics, ignoring the rest of its details. Seemingly different objects (e.g., a string and a list) can share the same abstraction. Abstraction is a fundamental idea in computer science.

**abstraction barrier:** the separation we maintain between operations that adhere to the abstraction we've built and operations that violate that abstraction. Abstractions work best when both the client using the abstraction and the implementor of the abstraction adhere to the abstraction barrier.

**actual parameters:** the names we provide as the inputs to a function at a call site. The value of the actual parameters gain new names (i.e., the **formal parameters**) inside the function.

**algorithm (Chapter 3):** a sequence of well-specified steps that a computer can execute.

**aliases:** what we call two names that refer to the same object.

**API:** an application programming interface is the definitions (i.e., variables, constants, classes, and functions) in a module, package, or library that they make available for programmers to use.

**attributes:** Python objects can be complex things with many different pieces to them. These object pieces are what computer scientists call attributes.

**background:** when you launch a program through the shell, it can be run in *foreground* or *background*. When run in the foreground, the program must terminate before the shell prompt reappears. When run in background, the shell prompt reappears as soon as the program launches. The program's output and shell's output will then be interleaved as each executes concurrently. Having the capacity to run a program in background allows us to launch a server in background and then use the shell to run the client.

**binary data:** normally we think of data as representing something, like a string or an integer, but binary data are data that we think of only as what they're made of: **bits**.

**bind:** the term used when pairing an actual parameter with its formal parameter during a function call.

**blocking** and **non-blocking calls:** in a blocking call, your script doesn't continue its execution until the work in the call is complete. This is the type of function call we have learned to build. A non-blocking function call is our introduction to concurrency in computation. The call doesn't finish its work before returning to the caller. It causes two processes to run concurrently!

**class:** see **type** (until Act II).

**command:** common way to direct the Python interpreter to do something, often of the form `command(input parameters)`.

**comments:** text following a `#` in Python. We use them to capture our pseudocode, and then after we have written some Python statements, we use them to explain what blocks of our code do.

**connection:** the abstraction that allows two software entities to communication. To move data through the connection, it might involve moving data around in a computer's memory as well as sending data across a communication link like wifi.

**design pattern:** an approach to solving some programming problem that you'll see in many pieces of code.

**deterministic:** a process in which an input always produces the same output. See also **non-deterministic** and **stochastic program**.

**dictionary:** a data structure that maintains a mapping from keys to values. These data structures are also called associative arrays as they associate a value with a key. As a real-world example, think about the contact list on your cell phone, which associates your friend's name with their cell phone number.

**docstrings:** strings that we add immediately after a function definition that provide more information about the purpose, inputs, outputs, and assumptions of the function.

**encoding (Chapter 2):** taking a countable number of something and associating a number with each something, typically a dense manner (e.g., if we had 8 things, we would number them 0 to 7).

**encoding (Chapter 3):** taking an abstract concept or complex phenomenon and giving it a specific representation. For example, we might take the concept of the letter `'b'` and write it as a black, 12-point, Courier letter `'b'` or represent its sound in English with the word "bee." Or as another example, the concept of "long" as a Spanish word (i.e., "largo"). Compare this definition of encoding with the previous one (from Chapter 2). Think about how they are actually the same!

**error messages:** feedback we get from the Python interpreter when we asked it to do something it didn't understand.

**exception handling** or **catching an exception:** an exception occurs when our script tries to do something that isn't legal Python. Exceptions terminate our scripts unless we write code in our scripts to catch them while our script is running. This is the purpose of the Python try-except-statement. A try-block wraps the code in which an exception might occur, and the except-block specifies which exceptions we want to handle and contains the code that handles those caught exceptions. If no exception occurs in the try-block, the except-block isn't executed. Catching Python exceptions allows us to avoid writing code that checks for error conditions that the Python interpreter already contains code to check!

**fail gracefully:** including error-checking code and code that catches runtime exceptions creates scripts that fail gracefully. By this, for instance, we mean that poorly structured inputs to our script don't cause the script to terminate in some hard-to-understand way and that the script doesn't abruptly terminate because a user mistakenly mistypes some input.

**finite state machine:** a mathematical model for organizing a system that moves through a series of tasks. In these models, there is a finite number of tasks to perform.

**for-loop:** a type of looping statement in Python that works particularly well for iterating over a sequence. Contrast with **while-loop**.

**formal parameters:** the names we give to the input values to a function. These are available in the function body. See **actual parameters**.

**function (Chapter 1):** other names for commands of the form `command(input parameters)`. An important set of functions are those that are built into Python (e.g., `help`). See also **method** and **procedure**.

**function (Chapter 3):** with **method** and **procedure**, ways to implement code abstraction, where we hide the implementation of some command behind a carefully designed interface, which tells you what inputs are required and what result will be produced.

**function composition:** every programming language has a set of rules for how it evaluates expressions built from a number of simple functions. Python evaluates nested functions (i.e., ones nested inside parentheses) innermost first, which should feel natural as it mimics what we do in basic algebra. You will also see a string of function applications (typically as method applications), and Python evaluates these in a left-to-right manner.

**header:** if we think of the main content of a file or message as the **body**, the header is data about the main content. It might, for example, contain the name of the file or the recipient of the message.

**immutable:** something that you cannot change after it is created (i.e., given its initial value or state). See **mutable**.

**infix operator:** another way to command the interpreter to do something for us, but the command sits *between* its input parameters, e.g., `2 + 3` where `+` is the command to perform addition.

**internationalization:** the process of making a piece of software work with multiple human languages. See **localization**.

**IP address:** an identifier comprised of four numbers between 0-255 separated by periods. The identifier is part of the IP protocol, and it both identifies a computer connected to the Internet and helps messages get routed to that computer. See **TCP/IP**.

**JSON:** another computing standard meant for exchanging structured data. The acronym stands for JavaScript Object Notation, and the data is structured like key-value pairs.

**library:** another name for a collection of modules. Programming languages like Python are typically distributed with what is called a *standard library*, which you can think of as the modules and packages available by default wherever the programming language is installed.

**literal object:** what computer scientists call an explicit value that we write into our scripts (e.g., `'Cosmo the dog'` is a string object whose value is that string).

**localhost:** the generic name given to the machine on which a program runs.

**localization:** specializing an internationalized piece of software for the user's native language. See **internationalization**.

**loopback address:** the **IP address** 127.0.0.1 that means the **localhost**.

**method:** see **function** (until Act II).

**module:** a Python script from which we want to pull some function or other definitions to use in our own script. See **library**.

**mutable:** something that can change its value or state. See **immutable**.

**name:** what the assignment operator (`=`) allows us to do with the object that is the result of a command. We can also use this operator to give another name to an existing object.

**namespace:** a context that helps you keep track of what object a particular name resolves to. A name might have meaning in one namespace, but not another.

**network buffer:** pieces of computer memory temporarily used as a holding spot for network data as these data are moved from one network endpoint to another.

**non-deterministic:** a process where an input can produce a number of different outputs. See **deterministic**.

**object:** pretty much everything in Python is an object with which we can interact.

**off-by-1 error:** a very common error in programming, where you think you know where a sequence starts or ends, but you misjudge it by a single step.

**optional parameters:** some commands take lots of input parameters, but you need specify only a few of them and the Python interpreter assumes you meant the default for the other unspecified ones (e.g., `open` assumes you want to open a file for reading).

**overloading:** using the same operator (e.g., `+`) or method name (`find`) for distinctly different operations. The actual operation performed depends upon the type of operands you provide to the operator or method.

**package:** a whole set of helpful modules that a Python programmer wishes to distribute all together. There are many packages in Python. The [Python Package Index](https://pypi.org/) is an online repository of software for the Python programming language.

**procedure:** another term for **function**.

**process:** the name we give to a running script, which includes not only its computer code, but also the data it generates.

**protocol:** a fancy way of saying that two processes cannot communicate unless they agree on how they're going to do it (e.g., you speak first and then we'll alternate) and what language they're going to use (e.g., we'll speak in slang-free English).

**pseudorandom number:** computers use algorithms to produce the "random" numbers. To a human, pseudorandom numbers look random because the next number at any point in the sequence is hard for us to guess, unless you know the algorithm being used and use it to compute the next number. Computer games are fun to play because they use something "random" (e.g., the current time) to *seed* the pseudorandom number generator. The seed is used to drop you into the pseudorandom sequence at some hard-to-guess point. Of course, if you provide a pseudorandom number generator with the same seed each time, you'll get the same "random" sequence out.

**result:** every Python function returns a result.

**return-statement:** the statement that directs a program's control flow from the current function back to the function that called it (i.e., back to the current call site). A return-statement may include an expression, which becomes the value of the call-site expression. If no expression is provided, Python returns the special value `None`.

**sequence:** an ordered collection of items.

**sequence diagram:** a stylized diagram that helps us imagine the sequences of messages between the client and server necessary for the task before us.

**shim:** a term used for a library that modifies the API of another library for the purposes of simplifying that interface, extending that interface, or translating between APIs.

**socket:** a software representation of a communication endpoint. We bind sockets to a software port number.

**standard:** a documented and widely agreed-upon method for doing something.

**state:** information that describes the current context. A machine keeps state to help it remember what it is doing.

**statement:** you can think of these as sentences in the language of Python. The Python interpreter will read and execute them one at at time.

**stochastic program:** a program that explicitly uses randomness in its execution. Games often employ randomness so that the gameplay varies from one execution to the next, but randomness is employed much more broadly in computer science (e.g., in the design of simulations). A non-stochastic program (i.e., a deterministic program) will produce the same output for a given input on each and every execution.

**stream:** the abstraction we'll use for an open file (e.g., an open text file is read as a stream of characters).

**structured data:** data that are organized and fit into an easily described format. Contrast with **unstructured data**, which has no easily described format. A list of credit card numbers is an example of structured data; a dump of social media posts is an example of unstructured data.

**syntax:** rules we need to follow when saying something in any programming language.

**TCP/IP:** the protocol suite that underlies the Internet. The TCP acronym stands for Transmission Control Protocol and the IP acronym stands for Internet Protocol (IP).

**transaction:** the fundamental operation that takes place when two processes communicate with each other.

**tuple:** one of Python's built-in data types. It is like a list in that the data in a tuple are ordered, but unlike lists, tuples are immutable. We can create a tuple using parentheses, and the assignment operator can disassemble a tuple.

**type (Chapter 1):** what defines the kinds of attributes you'll find on a Python object.

**type information (Chapter 5):** information that tells us (or the Python interpreter) how to interpret the "bag of bits" before us (or it). For example, the quotes around a string in a Python script are not a part of the string, but tell us and the Python interpreter how we should interpret the characters and words in the string (i.e., as a literal string value and not as a variable name or Python reserved word). The quotes are acting like type information.

**type conversion:** it is sometimes possible for us to convert a value stored as one type of object into a conceptually-similar, but different-in-form type of object. The string `'42'` and the integer `42` both represent the number forty-two, but these are different Python objects with different operations you can perform on them.

**Unicode:** a standard that attempts to map every character in every language into a set of numbers.

**utf-8 and latin-1:** two of many character encoding systems, which assign characters to specific bit patterns. See **encoding**.

**variable:** what we call names that refer to (i.e., name) different objects at different points in our script.

**while-loop:** a type of looping statement in Python that works particularly well for iterating until you hit some exit condition. Contrast with **for-loop**.

\[Version 20230709 --- through Chapter 5\]
