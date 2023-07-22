# Glossary

**abstraction:** a description of an object where we focus on a coherent set of its characteristics, ignoring the rest of its details. Seemingly different objects (e.g., a string and a list) can share the same abstraction. Abstraction is a fundamental idea in computer science.

**abstraction barrier:** the separation we maintain between operations that adhere to the abstraction we've built and operations that violate that abstraction. Abstractions work best when both the client using the abstraction and the implementor of the abstraction adhere to the abstraction barrier.

**actual parameters:** the names we provide as the inputs to a function at a call site. The value of the actual parameters gain new names (i.e., the **formal parameters**) inside the function.

**address:** a number that represents a way for you to specify which element in a large collection of elements you want. An index into a Python sequence is a type of address. Typically, computer scientists talk about a memory address, which is an index into the computer's memory when viewed as a large array of bytes.

**algorithm (Chapter 3):** a sequence of well-specified steps that a computer can execute.

**aliases:** what we call two names that refer to the same object.

**API:** an application programming interface is the definitions (i.e., variables, constants, classes, and functions) in a module, package, or library that they make available for programmers to use.

**array:** a sequence of items of the same type, which are stored compactly in the computer's memory, i.e., sequentially one after the other. A string is a 1-dimensional array of characters, and an image is a 2-dimensional array of pixels.

**attributes:** Python objects can be complex things with many different pieces to them. These object pieces are what computer scientists call attributes.

**background:** when you launch a program through the shell, it can be run in *foreground* or *background*. When run in the foreground, the program must terminate before the shell prompt reappears. When run in background, the shell prompt reappears as soon as the program launches. The program's output and shell's output will then be interleaved as each executes concurrently. Having the capacity to run a program in background allows us to launch a server in background and then use the shell to run the client.

**bit:** the fundamental unit of information in all computing and digital communications systems. The value of a bit is either 0 or 1, and as such, it corresponds to a binary digit (i.e., a digit in base-2).

**binary:** when people talk about binary, they either talking about base-2 notation or a collection of bits for which we have no other interpretation. See **interpretation**.

**binary data:** normally we think of data as representing something, like a string or an integer, but binary data are data that we think of only as what they're made of: **bits**.

**binary numbers:** a number written in bits (i.e., base-2).

**bind:** the term used when pairing an actual parameter with its formal parameter during a function call.

**bitwise operators:** the set of binary operator that don't act like arithmetic operators, which treat a string of *n* bits like a number, but instead treat a string of *n* bits as *n* independent operations to be perform at each bit location in the string. These operations are particularly useful in creating masks, which help you grab a portion of a string of bits.

**blocking** and **non-blocking calls:** in a blocking call, your script doesn't continue its execution until the work in the call is complete. This is the type of function call we have learned to build. A non-blocking function call is our introduction to concurrency in computation. The call doesn't finish its work before returning to the caller. It causes two processes to run concurrently!

**byte:** eight bits grouped together. This is typically the smallest unit of data that you can access given a memory (or other type of) **address**.

**class:** see **type** (until Act II).

**command:** common way to direct the Python interpreter to do something, often of the form `command(input parameters)`.

**comments:** text following a `#` in Python. We use them to capture our pseudocode, and then after we have written some Python statements, we use them to explain what blocks of our code do.

**computer architecture:** the structure of a computer as seen from the collection of its components and interconnection of those components. This includes components like the computer's processor and memory system, and how the processor is connected to the memory system.

**connection:** the abstraction that allows two software entities to communication. To move data through the connection, it might involve moving data around in a computer's memory as well as sending data across a communication link like wifi.

**data science:** a discipline that uses data collected about the world to extract meaningful insights and generate new knowledge.

**design pattern:** an approach to solving some programming problem that you'll see in many pieces of code.

**deterministic:** a process in which an input always produces the same output. See also **non-deterministic** and **stochastic program**.

**dictionary:** a data structure that maintains a mapping from keys to values. These data structures are also called associative arrays as they associate a value with a key. As a real-world example, think about the contact list on your cell phone, which associates your friend's name with their cell phone number.

**digital abstraction:** the bits we store and manipulate represent something in our physical world. They might be a voltage value, or the presence or absence of current flowing. They might be a burnt spot on a CD, or the lack of a burnt spot. They might be the presence or absence of light. When we talk about the digital abstraction, we're saying we don't care how you've represented a logical one or logical zero. We just want to know whether the physical phenomenon represents a one or a zero.

**docstrings:** strings that we add immediately after a function definition that provide more information about the purpose, inputs, outputs, and assumptions of the function.

**encoding (Chapter 2):** taking a countable number of something and associating a number with each something, typically a dense manner (e.g., if we had 8 things, we would number them 0 to 7).

**encoding (Chapter 3):** taking an abstract concept or complex phenomenon and giving it a specific representation. For example, we might take the concept of the letter `'b'` and write it as a black, 12-point, Courier letter `'b'` or represent its sound in English with the word "bee." Or as another example, the concept of "long" as a Spanish word (i.e., "largo"). Compare this definition of encoding with the previous one (from Chapter 2). Think about how they are actually the same!

**error messages:** feedback we get from the Python interpreter when we asked it to do something it didn't understand.

**exception handling** or **catching an exception:** an exception occurs when our script tries to do something that isn't legal Python. Exceptions terminate our scripts unless we write code in our scripts to catch them while our script is running. This is the purpose of the Python try-except-statement. A try-block wraps the code in which an exception might occur, and the except-block specifies which exceptions we want to handle and contains the code that handles those caught exceptions. If no exception occurs in the try-block, the except-block isn't executed. Catching Python exceptions allows us to avoid writing code that checks for error conditions that the Python interpreter already contains code to check!

**fail gracefully:** including error-checking code and code that catches runtime exceptions creates scripts that fail gracefully. By this, for instance, we mean that poorly structured inputs to our script don't cause the script to terminate in some hard-to-understand way and that the script doesn't abruptly terminate because a user mistakenly mistypes some input.

**file extension:** the letters that come after the last period in a filename (e.g., the `.docx` on a Microsoft Word file). We can use the extension as a hint for how we should **interpret** the bits in a file.

**finite state machine:** a mathematical model for organizing a system that moves through a series of tasks. In these models, there is a finite number of tasks to perform.

**for-loop:** a type of looping statement in Python that works particularly well for iterating over a sequence. Contrast with **while-loop**.

**formal parameters:** the names we give to the input values to a function. These are available in the function body. See **actual parameters**.

**function (Chapter 1):** other names for commands of the form `command(input parameters)`. An important set of functions are those that are built into Python (e.g., `help`). See also **method** and **procedure**.

**function (Chapter 3):** with **method** and **procedure**, ways to implement code abstraction, where we hide the implementation of some command behind a carefully designed interface, which tells you what inputs are required and what result will be produced.

**function composition:** every programming language has a set of rules for how it evaluates expressions built from a number of simple functions. Python evaluates nested functions (i.e., ones nested inside parentheses) innermost first, which should feel natural as it mimics what we do in basic algebra. You will also see a string of function applications (typically as method applications), and Python evaluates these in a left-to-right manner.

**header:** if we think of the main content of a file or message as the **body**, the header is data about the main content. It might, for example, contain the name of the file or the recipient of the message.

**hexadecimal numbers:** a hexadecimal number is one specified in base-16. Each hexadecimal digit can take on one of sixteen values: 0, 1, 2, ..., 9, A, B, C, D, E, F.

**hexdump:** a program or IDE extension that allows you to view a file as a collection of **hexadecimal numbers** (i.e., its raw bits). Without this tool, a computer will assume that you want to view a file with its default **interpretation**. In other words, you would use hexdump to view the bits in a JPEG file rather than have it displayed as an image.

**immutable:** something that you cannot change after it is created (i.e., given its initial value or state). See **mutable**.

**infix operator:** another way to command the interpreter to do something for us, but the command sits *between* its input parameters, e.g., `2 + 3` where `+` is the command to perform addition.

**internationalization:** the process of making a piece of software work with multiple human languages. See **localization**.

**interpretation:** given a collection of bits, we can interpret that collection of bits in many ways. One interpretation might be that the collection is a sequence of instructions. Another interpretation might be that it is an image file. Another might be that it is a Microsoft Word document. The same collection of bits might be a legal set of instructions and a recognizable image!

**IP address:** an identifier comprised of four numbers between 0-255 separated by periods. The identifier is part of the IP protocol, and it both identifies a computer connected to the Internet and helps messages get routed to that computer. See **TCP/IP**.

**JSON:** another computing standard meant for exchanging structured data. The acronym stands for JavaScript Object Notation, and the data is structured like key-value pairs.

**library:** another name for a collection of modules. Programming languages like Python are typically distributed with what is called a *standard library*, which you can think of as the modules and packages available by default wherever the programming language is installed.

**literal object:** what computer scientists call an explicit value that we write into our scripts (e.g., `'Cosmo the dog'` is a string object whose value is that string).

**localhost:** the generic name given to the machine on which a program runs.

**localization:** specializing an internationalized piece of software for the user's native language. See **internationalization**.

**loopback address:** the **IP address** 127.0.0.1 that means the **localhost**.

**machine learning (ML):** a field in computer science in which we build algorithms that can be trained on existing data (e.g., this is a picture of a cat) and then make predictions or classifications when given previously unseen data (e.g., is this a picture of a cat?). There are several different broad techniques for machine learning.

**method:** see **function** (until Act II).

**module:** a Python script from which we want to pull some function or other definitions to use in our own script. See **library**.

**mutable:** something that can change its value or state. See **immutable**.

**name:** what the assignment operator (`=`) allows us to do with the object that is the result of a command. We can also use this operator to give another name to an existing object.

**namespace:** a context that helps you keep track of what object a particular name resolves to. A name might have meaning in one namespace, but not another.

**network buffer:** pieces of computer memory temporarily used as a holding spot for network data as these data are moved from one network endpoint to another.

**nibble:** half a **byte** or 4 bits. A nibble can be written as a single hexadecimal character. See **hexadecimal numbers**.

**noise:** any degradation in the fidelity of our data.

**non-deterministic:** a process where an input can produce a number of different outputs. See **deterministic**.

**object:** pretty much everything in Python is an object with which we can interact.

**off-by-1 error:** a very common error in programming, where you think you know where a sequence starts or ends, but you misjudge it by a single step.

**optional parameters:** some commands take lots of input parameters, but you need specify only a few of them and the Python interpreter assumes you meant the default for the other unspecified ones (e.g., `open` assumes you want to open a file for reading).

**overflow** and **underflow**: while we can imagine a number of any size, computers are built from physical devices that have a fixed size. The computer I'm typing on right now contains 64-bit registers into which I can put an integer. While $2^{64}$ is a lot of values, I can still imagine more. As such, we define what bit patterns represent which numbers when stored in one of these registers. Typically, the range of representable integers is $-2^{63}$ to $2^{63} - 1$. If you perform a calculation that produces a positive integer greater than $2^{63} - 1$, the computation is said to have overflowed the computer's integer representation. Similarly for underflow.

**overloading:** using the same operator (e.g., `+`) or method name (`find`) for distinctly different operations. The actual operation performed depends upon the type of operands you provide to the operator or method.

**overtraining:** data-science models are trained to recognize certain generalities in our world, e.g., we might train a model to recognize swans. We've overtrained this model when it starts to say that all swans are white.

**package:** a whole set of helpful modules that a Python programmer wishes to distribute all together. There are many packages in Python. The [Python Package Index](https://pypi.org/) is an online repository of software for the Python programming language.

**pixel:** the smallest display element (and therefore addressable element) in an image. It is short for picture element.

**procedure:** another term for **function**.

**process:** the name we give to a running script, which includes not only its computer code, but also the data it generates.

**protocol:** a fancy way of saying that two processes cannot communicate unless they agree on how they're going to do it (e.g., you speak first and then we'll alternate) and what language they're going to use (e.g., we'll speak in slang-free English).

**pseudorandom number:** computers use algorithms to produce the "random" numbers. To a human, pseudorandom numbers look random because the next number at any point in the sequence is hard for us to guess, unless you know the algorithm being used and use it to compute the next number. Computer games are fun to play because they use something "random" (e.g., the current time) to *seed* the pseudorandom number generator. The seed is used to drop you into the pseudorandom sequence at some hard-to-guess point. Of course, if you provide a pseudorandom number generator with the same seed each time, you'll get the same "random" sequence out.

**result:** every Python function returns a result.

**return-statement:** the statement that directs a program's control flow from the current function back to the function that called it (i.e., back to the current call site). A return-statement may include an expression, which becomes the value of the call-site expression. If no expression is provided, Python returns the special value `None`.

**RGB:** one encoding for the range of color values that a pixel can display. The RGB color model is made up of three values representing the amount of red, green, and blue that exists in the color you want to display.

**saturation:** a method for handling **overflow** and **underflow**. If you perform a calculation that produces a positive integer greater than $2^{63} - 1$ and you expect overflow to saturate, the result will be $2^{63} - 1$. Saturating underflows on 64-bit integers pin the results to $-2^{63}$.

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

**traversal:** given an **array**, a traversal defines the order in which you'll visit the elements in the array. For example, a row-major traversal of a 2-dimensional array would visit each element in a particular row before visiting any element in the next row.

**tuple:** one of Python's built-in data types. It is like a list in that the data in a tuple are ordered, but unlike lists, tuples are immutable. We can create a tuple using parentheses, and the assignment operator can disassemble a tuple.

**type (Chapter 1):** what defines the kinds of attributes you'll find on a Python object.

**type information (Chapter 5):** information that tells us (or the Python interpreter) how to interpret the "bag of bits" before us (or it). For example, the quotes around a string in a Python script are not a part of the string, but tell us and the Python interpreter how we should interpret the characters and words in the string (i.e., as a literal string value and not as a variable name or Python reserved word). The quotes are acting like type information.

**type conversion:** it is sometimes possible for us to convert a value stored as one type of object into a conceptually-similar, but different-in-form type of object. The string `'42'` and the integer `42` both represent the number forty-two, but these are different Python objects with different operations you can perform on them.

**Unicode:** a standard that attempts to map every character in every language into a set of numbers.

**utf-8 and latin-1:** two of many character encoding systems, which assign characters to specific bit patterns. See **encoding**.

**variable:** what we call names that refer to (i.e., name) different objects at different points in our script.

**while-loop:** a type of looping statement in Python that works particularly well for iterating until you hit some exit condition. Contrast with **for-loop**.

\[Version 20230710 --- through Chapter 6\]
