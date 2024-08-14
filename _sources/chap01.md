# Chapter 1: Read a Children's Book #

I love a good story that opens with action and suspense. The action in this opening chapter will be to *solve our first computational problem* and *write our first Python script*. These two things are linked. The goal of a Python script is to get a computer to act in a certain way, to perform one or more specific computations. And if we correctly do the problem-solving work that sets up the writing of such a script, the computations that result will be a solution to our problem.

The suspense? Well, you won't just read about what it takes to write a script that solves the problem before us, but actively try many different short scripts that may move us closer to solving our problem. For each script, we will pay careful attention to how the computer reacts. I will help you to understand each response, and through this experience, you'll begin to build a mental model of what it takes to write a script that causes the computer to perform as we desire.

This trial-and-error approach is not that much different than what we did as young children as we learned how to communicate with those around us. There was a language that the adults were speaking, but we didn't yet understand it. We wanted to be part of the action, and so we would string together some words from this language and notice how the adults reacted. By paying careful attention to their reactions, we built *a mental model* that mapped our ideas to what statements we needed to say to express those ideas in that language.

## Problem solving, in general

But we want to do more than just develop this mental model and learn to express simple concepts in Python: We want to solve computational problems. To solve a problem, we will begin by imagining and then sketching out *a set of actions* that we think, if followed, would solve the problem before us. This set of actions is the content of the script that we want our computer to follow.

In the theater, scripts are useful when we can read them and follow their stage directions. If a script uses a language or a lot of terms that we do not understand, it is gibberish to us. The same is true for a computer script. We need to express the script in a language and with terms that the computer understands. Python is a language the computer understands, and this is where our mental model of what we can express in Python comes into play. The second part of problem solving with computation, then, is the translation of our imagined set of actions into Python.

If you keep in mind the elements involved in good storytelling, you can use them to become a good problem solver. Just as there are different narrative structures that help writers organize their stories, there are different problem solving strategies that we will use to structure our scripts. Just as storytellers need to decide what narrative details are necessary to include and which can be left out, we too will have to grapple with what details of our problem are pertinent in order to find a solution to it. And just as storytellers take advantage of cultural knowledge to streamline their narrative, we will learn to use prominent features of our computer and its programming language to produce efficient and easily understood scripts.

*Writing* a story or a script, however, is not the end for a storyteller or for us. Stories are meant to be read and scripts performed. Just as storytellers hope to elicit particular reactions from their readers, we hope that our scripts will produce particular results when *executed* by a computer. Unfortunately, the first draft of a story doesn't always elicit the reaction a storyteller hoped it would, and the first versions of our scripts often won't produce our desired outcome. But that's ok! We will figure out what failed, fix it, and begin again.

```{admonition} Terminology
:class: tip
I'll talk about our scripts (i.e., our computer programs) as being _executed_ or _run_. Both terms simply mean that the computer performs the actions dictated in the script.
```

For every problem we aim to solve throughout this book, we will work through these three basic steps: sketch, translate, execute. {numref}`Figure %s <c01_fig1_ref>` summarizes how these steps take us from a problem to a solution, and it reminds us of what we need at each step. 

```{figure} images/c01_fig1.png
:name: c01_fig1_ref

The work we will do going from a problem-to-be-solved to a computational solution.
```

## Problem solving, in detail

While {numref}`Figure %s <c01_fig1_ref>` makes it look like problem solving is a *linear* process, our first attempt, like a first draft of a story, might miss its mark. For most of us, including me, problem solving is not a linear process, but an *iterative* one. A script when executed might not produce a correct solution or even run at all, and when this happens, we will need to figure out what went wrong and then restart our problem-solving process at some earlier step.

This need to back up and restart might occur at any step in our problem-solving process. For example, we might start translating our outlined set of actions into Python and then realize that we forgot a case we need to handle. No problem. This simply means we back up and restart at an earlier step in our problem-solving process. At which step we restart depends upon the implications of what we had overlooked. 

Now I don't know about you, but I don't like to redo my work over and over again. We can avoid some of this back-up-and-restart action if we are more thoughtful before we begin sketching a set of computational actions. How thoughtful? Well, we will add an additional *five* steps to the front of the process in {numref}`Figure %s <c01_fig1_ref>`. 

Five steps before we even sketch out some computational actions might sound like a lot, but the thought put into these first five steps is what helps *experienced* problem solvers more quickly find working solutions. The resulting eight-step process is still iterative. No one solves a problem perfectly in their first attempt, and these new initial steps are meant to help us discover blindspots and oversights before we have done a lot of design and coding work. In other words, this early work will save us a lot of wasted work in the last, time-consuming stages of the process.

```{Tip}
What is the biggest trap into which many beginner programmers fall? They attempt to go directly from a problem description to the writing of a bunch of programming language statements. Learning to program is hard enough without skipping the important steps between the first and the seventh. As you tackle your first problems-to-be-solved, keep in mind that the actual writing of code is work done late in the process of problem solving with computation. It might look like an experienced programmer skips quickly to Step 7, but it is more likely that you are seeing them pull code from prior, similar problems (Step 5). As you practice, you too will begin to see such similarities between problems.
```

Putting this all together, the following is a brief description of each of the eight steps that will help us act like an experienced problem solver:

1. Precisely **specify** the problem you will solve.
2. **Imagine** one or more *specific instances* of the problem, identify the inputs you would use in those instances, and the answers you would like produced in each instance. What you learn in this step may cause you to change the specification you wrote in Step 1.
3. **Decompose** the problem into smaller tasks that together solve the overall problem. This decomposition may highlight *corner cases* you hadn't considered in Steps 1 and 2.
4. **Decide** which tasks identified in Step 3 are amenable to *computational approaches*. This decision may cause you to change the problem or some aspect of the problem.
5. **Identify** what code you have already written or that you know exists that might help perform each task you need done. Based on what exists, you may yet again adjust the problem to minimize the work you need to do yourself.
6. **Sketch** the sequence of actions you want the computer to do in each task. What you discover as you do this work might change your mind about the decisions you've made in the previous steps.
7. **Translate** your sketch into *programming language statements*. If you run into any roadblocks in this translation, you might overcome these roadblocks by changing some prior decision.
8. **Execute** the resulting script to see if it functions correctly and meets all the problem's specifications. If it doesn't, you will need to ask yourself if the failure is an error in your code or a problem with your specifications.

Don't feel you need to memorize these eight steps or fully understand the italicized terms in them. Through repeated practice in this book's chapters, these steps will soon become second nature. And before you know it, you won't just be acting like an experienced problem solver, you will be one.

```{admonition} Learning Outcomes
While solving each chapter's problem, you will gain practice in design, new knowledge of computer science (CS) concepts, and specific skills in Python programming. To help you understand how you will grow in these three areas, each chapter begins with a list of its learning outcomes. By the end of this first chapter, you will be able to:

*   Describe the general process involved in problem solving with computation [design];
*   Take the first steps in this process [design and programming skills];
*   Use an Integrated Development Environment (IDE) as a place to sketch out and run a computational solution to a problem [programming skills];
*   Use the interactive Python interpreter to bootstrap your knowledge of Python [programming skills];
*   Understand the structure of the feedback you receive from the Python interpreter and start becoming comfortable with its error messages [programming skills];
*   Consult a number of different resources that can help you interpret the meaning of this feedback [programming skills];
*   Write a few commands and expressions in Python and name their results [programming skills];
*   Recognize the basic form and function of Python's if-, while-, and break-statements [programming skills];
*   Explain namespaces and straightforward aliasing of Python objects [CS concepts].
*   Read a text file and print its contents [programming skills].
```

## Our first computational problem

I employed the imagery of how we learn to communicate ideas and stories to help you imagine how you will learn to solve problems with computation. Let's stay close to this imagery with our first problem-to-be-solved.

When my children were young, I read to them nightly in the hope that this special time together would help them as they struggled to learn how to read. As we embark on learning to read and write Python, let's see if we can *get our computer to read a children's book to us*.

## Imagine a specific instance

We don't yet have enough experience in computation to know whether we have precisely specified this problem, but that's ok. Since problem solving is an iterative process, we can always come back and refine our simple specification later. Let's move on to Step 2 of the problem-solving process and consider a specific instance of our problem.

Step 2 talks about identifying our inputs, and for our problem this means choosing a book to read. One of the first books I remember having read to me when I was young was *The Cat in the Hat* by Dr. Seuss. I loved this rambunctious story, and it naturally was the first book I pulled out to read to my children. Let's have this be the first book we get the computer to read to us.

Ok, but what copy of this book? In the physical world, this book comes in many different shapes and sizes, and digitally, in many different file formats. We need a file format that our computers can access and interpret.

We might choose a copy of our book in PDF (Portable Document Format) or DOCX (Microsoft's file format for word processing), as these formats allow us to include text and pictures. Or we might choose a file format that stores only the book's text. Notice that, in making this choice, we are already refining our problem specification!

Let's decide that our script will expect the book to be a plain text file format (`.txt`), since I have such a plain text file containing the beginning of *The Cat in the Hat* available for you to use as an input. {numref}`Figure %s <c01_fig2_ref>` shows what we would see if we opened this plain text file in a typical word processor (e.g., Microsoft Word or Google Docs).

```{figure} images/c01_fig2.png
:name: c01_fig2_ref

The contents at the start of the file named `CatInTheHat.txt`.
```

In addition to our script's input, we also have to specify its output. Let's agree that our script output on our computer screen the text we see in {numref}`Figure %s <c01_fig2_ref>`.

```{tip}
These two choices illustrate an approach for which I'll repeatedly advocate: Start with a single and simple instance of your problem-to-be-solved. Here we have decided to start with a script that won't read _any book in any format_, but this _one specific book in one particular format_. Then write a script that solves that instance. Only then should you consider how you might expand your script's functionality so that it solves multiple different instances of your problem (i.e., other books and other formats).
```

In summary, the script we will write will take the contents of the file named `CatInTheHat.txt` as input and display its contents on our computer screen, as if it were reading the book from start to finish. As a finishing touch, let's have the script add an extra line to the end of the book where it will say, as I always did for my children, "The End."

## Sketch using computational thinking

Given that this is our first computational problem, we will skip over Steps 3 through 5 of our problem-solving process. These three steps become important as you gain experience. For example, in subsequent chapters, you will start to see problems that you have already solved as pieces of the problem you are currently trying to solve. But since we can't yet plumb such experience, we will jump directly to Step 6 and sketch out the sequence of actions that will get the computer to read our book to us.

Remember that our goal in this step is a sketch. Although our script must eventually be written in a programming language, Step 6 is not about programming, but about *computational thinking*. This step is not programming because we are not trying to say something *in* a programming language like Python. This means that you don't need to know Python or any other programming language to create a sketch. You can use any language you would like to capture the sequence of actions in your sketch.

So what is computational thinking if it is not programming? The historical note at the end of this chapter presents what some noteworthy people have said about this term, but for our purposes, I mean *the thought that we as humans do to structure a problem and its solution script so that the problem is amenable to being solved by a computational device*. 

This definition begins by emphasizing one of Jeanette Wing's important characteristics about computational thinking: It is "a way humans solve problems" and it is not about "trying to get humans to think like computers."[^fn1] It is *a way* because the definition constrains our creative, problem-solving ability: the problem specification must be amenable to a computational solution and the solution script that we eventually produce must be able to be run on a computer. This constraint is something that Alfred Aho emphasizes in his definition of computational thinking: computers and computation are the primary tools we use in producing a solution to our problem.[^fn2] Beyond this constraint, we can use any of the diverse skills we often employ in everyday problem solving, from simple mathematical analyses to complex planning to the understanding of human behavior. You can see this if you read through Jeannette Wing's seminal paper on computational thinking and think about what is involved in each of her many examples.

## Capturing this thinking

Creating a sketch of a script using computational thinking is what we want to do, but you are probably still thinking, "This isn't clear. What exactly am I supposed to do?"

To help, let's return to the imagery of children learning. With time and practice, children are able to quickly turn ideas into coherent, grammatically correct statements, but are perfect clarity and correct grammar the most important issues when first learning to talk? Do you see parents interrupting their young child every time the child is unclear or makes a grammatical mistake? I don't. Instead, I see parents encouraging their children to *put words to their ideas*. The resulting broken English (or whatever your native tongue) isn't always coherent or grammatically correct, but it is often sufficient to communicate the child's thoughts.

When starting out, we have the same goal: *capture in a sketch (i.e., broken English) our ideas about how we might computationally solve the problem*.

Broken English for us is what computer scientists call *pseudocode*. In pseudocode, it is fair game to write down the set of actions you'd like the computer to take in any English phrase that makes sense to you. If you want the computer to open a window on the screen, you can type the phrase: `open a window on the screen`. If you want the computer to add 5 and 6, you can type: `add 5 + 6`. I don't care what you write as long as you --- and hopefully others --- have a strong sense of what you mean. 

## An environment for coding

So lets's write down some pseudocode, i.e., some ideas about how to instruct a computer to read a book to us. But where should we write these ideas, this pseudocode? We could scribble on a piece of paper, but it would be better if we wrote this pseudocode where it would be easy for us to turn our ideas into actual Python code, which is the next step in our problem-solving process.

Fortunately, there are numerous applications available in which you can write, run, and inspect code. These tools are typically called an *editor*, an *interpreter*, and a *debugger*. The editor is where we will write our pseudocode and then our Python script. The interpreter is what allows us to run the script we have written, and the debugger is a tool that can help us understand the reasons why our script may not have run as we had hoped.

While you can use these as standalone tools, many programmers like to use what is called an *Integrated Development Environment (IDE)*. An IDE packages together all the tools you'll need in a single application. In a moment, I'll help you get started with an actual IDE in which you'll build the computational solutions I discuss in this book's chapters. There are many different, including some free, IDEs, and it won't matter which you use. The most important thing is that you use an IDE as you work your way through this book.

## Our generic IDE

Most IDEs contain numerous features, and it is easy to feel overwhelmed with their extensive functionality. Don't worry. You'll need only a small slice of it in this book.[^fn3] In particular, I want you to become familiar with six things found in every IDE, which are typically instantiated as two bars, three panels, and one button. {numref}`Figure %s <c01_fig3_ref>` illustrates where I locate these six items in what this book calls a *generic IDE*.

```{figure} images/c01_fig3.png
:name: c01_fig3_ref

The bars, panels, and button we need to get started, as illustrated in our generic IDE.
```

Running along the top of our generic IDE is the *title bar*, which is where you will find the name of the *project* you have open. You can think of a project like a folder on your computer. It is where you store all the scripts, other files, and specific configuration information associated with the problem you are trying to solve. You can name your projects anything you like.

The other major bar is the *menu bar*, which often resembles a tray of icons. By selecting one of these icons, you can inspect different aspects of your project. To begin, the menu bar in our generic IDE will contain only two icons, as illustrated in {numref}`Figure %s <c01_fig3_ref>`. The first looks like a small stack of papers, and clicking this icon changes the panel to the right of the menu bar into a *file explorer*. It allows you to manipulate the files in your project. The second looks like a small gear, and clicking it displays *configuration options* for the IDE.

```{tip}
There are two IDE configuration options you must set if you want to work with the code in this book. They are: (1) the editor should perform indentation using spaces, not tabs; and (2) a single level of indentation is four spaces. As we will discuss shortly, indentation holds meaning in the Python programming language. By adopting the same indentation settings as I did in creating this book's scripts, you will be able to seamlessly use my code. If you don't adopt these settings, you will generate many unnecessary headaches for yourself.
```

Besides the panel to the immediate right of the menu bar, our generic IDE also contains two other panels: the *code editor* and the *console*.

The code editor acts like a panel in a word processor. When we click our mouse inside this panel, we get a cursor at the location of our click and can start typing. And just like in a word processor, we can use the functionality of the editor to edit, search, and format the text we type.

```{tip}
You might be wondering if you could use your current word processor to write your Python scripts. Although you could---a Python script is nothing more than a specially structured text file---the features we want in our code editor are not the same ones useful in writing an email to grandma, a theatrical script, or a technical paper. For example, we won't need to insert images into our Python scripts, but we will want to include code modules written by others. We won't care if a word in our script is a valid English word, but we will want help knowing what the word means in the context of this script. Finally, we will want a different type of autocomplete (e.g., one that helps us remember the particulars of the syntax in our programming language). Hopefully, you're starting to see the benefit of adopting an editor purpose-built for programming. A good code editor will make the process of coding easier. Don't use a word processor to write or edit your code.
```

The last panel, and in our generic IDE it is the one to the far right, is the console. It is where we can interact with our script as it runs. You can think of this panel as the stage where the actor (the Python interpreter on our computer) performs our script.

How do we ask the interpreter to run the script that appears in the editor pane? That's right! We click the *Run button*.

```{admonition} You Try It
It's time to select the IDE you'll use as you work through the problems-to-be-solved in this book. On the companion website to this book, you can find directions that will help you get started with one of several different, including some free, IDEs. It doesn't matter which you choose. Only once you're set up with an IDE should you read on.
```

## Our first pseudocode

We are now ready to do some computational thinking and sketch out our thoughts in an IDE. If we want our computer to read *The Cat in the Hat* to us, what is the first action it should take? Think about the first action that you would take if you were to read this book to another person. Now write that down in the editor window in a file called `seuss.py`.

You probably wrote something like the following, where the initial `1` is just the line number displayed by the IDE's editor:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
```

If you wrote something like, "Read the first line" then you have to get used to thinking about *every* step in the process of doing something. Unlike a human, the Python interpreter won't know that to read the first line, you must first open the book. This is important: When writing a computer script, you must tell it every detail of what you want it to do. We will see in a moment what happens when we don't provide the interpreter with specific instructions.

What we just wrote is pseudocode. It is a description of what we want the interpreter to do. This description isn't Python code, and we want to make sure that the interpreter doesn't try to interpret it as such. 

## Comments

I made sure that the interpreter would ignore my line of pseudocode by placing a hash mark (`#`) at the start of the line. This character holds special meaning in Python. When the interpreter sees it, it knows to ignore everything from the hash mark to the end of the line. Programmers refer to text like this as *comments*. 

Comments are notes to ourselves, or we might use them to leave a note for anyone else reading our script. Let me emphasize that comments are for humans. The interpreter will ignore whatever is in a comment and so it cannot contain any actions that we want the computer to interpret or act out.

Despite the fact that comments don't tell the computer anything, they are important and useful. I will use them for two distinct purposes:

1. To capture our ideas during the problem-solving phase when we are sketching out the actions we would like the computer to take (Step 6).
2. To explain why the Python code we write in Step 7 looks like it does. According to Swaroop in [*A Byte of Python*](https://python.swaroopch.com/), "Code tells you how, comments should tell you why."

## Commands and input parameters

Normally, we would write more pseudocode to sketch out what we want the interpreter to do for us, but let's convert this pseudocode into Python code so that we can get a feel for what it is like to command the computer to do things.

Our pseudocode instructs the interpreter to open up our book, just as if we had a physical copy of *The Cat in the Hat*, and so let's write `open('CatInTheHat.txt')` on the line below our pseudocode. This new line is valid Python. What does it command the interpreter to do? To open the file named `CatInTheHat.txt`.

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
open('CatInTheHat.txt')
```

Let's talk about the *syntax* (i.e., the form and ordering of the elements) in this line that makes it valid Python. Its form is: *command*(*input parameters*). In our example, we are issuing an `open` command, which requires a piece of information to do its work (i.e., a single input parameter). In this case, the parameter tells the interpreter what file we want opened. The single quotes around the filename are necessary, and I'll explain why in a bit.

In general, a command may require zero, one, or many input parameters. When there are multiple input parameters, each is separated by a comma. A command that takes no parameters has this form: *command()*. In other words, you always need the parentheses.

Returning to our open command, how is it that the Python interpreter knows what to do when it encounters an `open` command? There are three answers to this question:

1. The command is built into the Python language;
2. It was added to the language by some other person; or
3. We added it to the language ourselves.

In the case of `open`, it is a *built-in function*. We will stick with commands built into the Python language in solving this chapter's problem; we'll see the other two categories of commands in the following chapters.

```{admonition} Terminology
:class: tip
Think of the term _command_ and _function_ as interchangeable terms. Every command is performed by some code that we think of as a function. We'll learn to build functions in Chapter 3.
```

## Scripts versus execution

So far we have a script with a comment and a single command, which is like a playwright writing a short note to themselves and a single line for an actor. Once written, the next step is to have an actor to perform this line so that the playwright can see how it works. In your IDE, the equivalent of this handoff from playwright to actor is: click the IDE's Run button. It instructs the Python interpreter to run your script.

Go ahead and try it with the script we've written so far, which is copied below:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
open('CatInTheHat.txt')
```

```{admonition} You Try It
As you read on, you'll notice that I do not include the result of running this script in the text. This is to encourage you to be an active learner. Write the two lines above in your own IDE, and then hit the Run button and see what happens. Feel free to experiment by changing the code and hitting the Run button again. You can't break anything and so experiment. Be creative!
```

Oops, our code produced a whole lot of text in the console panel that's telling us we did something wrong. 

## Our first error

Computer scientists call this text an *error message*. I don't want you to think of this as an error, but as *feedback*. The Python interpreter is trying to tell us that it couldn't perform a part of our script. Unfortunately, its feedback isn't always easy to understand. If it helps, think about the interpreter as an actor whose English isn't too great.

What can we learn from the broken English in this error message? `FileNotFoundError` looks like promising information, and the file the interpreter couldn't find was `CatInTheHat.txt`. Do you see this in the error message? The message also says that the interpreter had this trouble when it tried to execute the `open` command on line 2 of our script.

The interpreter can't find the input file because this file isn't one of those listed in the file explorer panel. How can we change this? We could add a new empty file to our project called `CatInTheHat.txt`, but then we'd have to type its contents and I'd rather not type in something that already exists. So where can you find this existing file? It's in the `txts` folder in your IDE's file explorer.

```{admonition} You Try It
Use your IDE's file explorer to make a copy `CatInTheHat.txt` so that this copy is at the same level of the file explorer as your `seuss.py` script. Your IDE probably supports the same kind of copy, paste, move, and rename actions you've used with your computer's operating system.
```

With `CatInTheHat.txt` where the script `seuss.py` expects it, click the Run button again. Success! I mean, at least no error message this time.

But more than no error message, it looks clicking the Run button did nothing. The interpreter was completely silent while executing our script. Is that what you expected to happen? If we're going to command the interpreter to do things for us, we had better develop an understanding of how it operates. Only with a good mental model of its operation can we successfully command it.

## The interactive interpreter

Giving the Python interpreter a script (like `seuss.py` that we've started writing) is one way to command it, but it isn't the only way. We can also ask the interpreter to *interact* with us. You should think about this as having a conversation with the interpreter. In this conversation, we can feed it different parts of our Python script and see how it reacts, and like an attentive friend, it will remember what we've previously said to it.

In the directions that got you started with your selected IDE, I include a section that tells you which panel in your IDE you should use to start the Python interpreter in its interactive mode, i.e., what people call the *interactive Python interpreter*. This IDE panel contains a shell prompt at which you type `python3`, and then press the return key on a Mac or the enter key on a PC.

```{admonition} Terminology
:class: tip
You'll learn a lot more about the shell in Chapter 13, but briefly, the shell is a mechanism for you to run programs on your computer and manipulate your computer's files and folders.
```

The block below illustrates this, where "shell\_prompt$" is the shell prompt (your IDE's shell prompt will look different). The next two lines are information about the version of the Python interpreter you're running and a few commands you can send to it; the interpreter prints these lines as it starts running (your lines will look slightly different). The final line, starting with "\>\>\>", is the interactive Python interpreter's prompt (yours will look exactly like this).

```{code-block} none
---
emphasize-lines: 1, 4
---
shell_prompt$ python3
Python 3.12.3 (main, Apr  9 2024, 08:09:14) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

To exit the interactive Python interpreter, you simultaneously press your keyboard's control key and the letter-D key.[^fn4] Going forward, I'll abbreviate this key combination as *Ctrl+D*.

```{admonition} You Try It
Use Ctrl+D to exit the interactive Python interpreter and then type `python3` at the shell prompt to re-enter the interactive interpreter.
```

## Code and transcript blocks

I want to pause here and make sure you recognize that the difference between a *code block* (e.g., the earlier block that contained a comment and an open command) and a *transcript block* (e.g., the previous block that illustrates how you can launch the interactive Python interpreter). Code blocks are the scripts we'll feed to the Python interpreter while transcript blocks are recordings of the conversations we have with the Python interpreter or our computer's operating system.

The previous block is a recording of my interaction with the shell on my machine, and it ends with a prompt that allows us to converse with the interactive Python interpreter. You use Ctrl+D to end this conversation and return to talking to the shell.

## Interacting with the interpreter

Ok, let's chat with the interactive Python interpreter and see how it reacts to the two lines in our `suess.py` script. In other words, let's feed the two lines of our script to the interactive Python interpreter one at a time.

Starting with the comment line, type it at the interactive interpreter's prompt and hit enter. What happened?

```{code-block} python
>>> # Open the book so we can read it
>>> 
```

The transcript shows that nothing happened! Exactly what the interpreter should do with a comment line.

Now type the `open` line at the interactive Python interpreter's prompt.

```{code-block} python
>>> open('CatInTheHat.txt')
<_io.TextIOWrapper name='CatInTheHat.txt' mode='r' encoding='UTF-8'>
>>> 
```

The interpreter responded with something interesting. It doesn't read like the error message we saw earlier. The interpreter is trying to tell us something different. In technical jargon, the Python interpreter, in interactive mode, prints *the value it computed*. To understand what this means, let's try something simpler.

## The interpreter as a calculator

What happens if you type `2 + 3` at the interactive Python interpreter's prompt?[^fn5] Before you hit enter, take a moment and think about what we are asking the interpreter to do.

```{code-block} python
2 + 3
```

Press the enter key. Cool, we can use the interpreter like a calculator to evaluate an arithmetic expression. Given an expression, the interpreter prints what it calculates.

```{admonition} You Try It
Take a bit of time and play with the interpreter as a calculator. Keep in mind as you play that you are playing to learn. If you try something and it fails, try something slightly different. As you read this book, get into the habit of experimenting and forming theories from what you observe during these experiments. By the way, a simple Google search with the phrase "Python arithmetic operators" will connect the mathematical operations you know with the symbols that perform these operations in the Python programming language.
```

From your experiments, I hope you have learned that the interactive Python interpreter prints the value of the expression we asked it to evaluate. So, what is this value that it printed when we asked it to open a file?

## Python help

Wouldn't it be nice to ask for some help? It would and so let's do that. Type `help(open)` in the interactive interpreter and run it.

```{code-block} python
help(open)
```

The `help` command is another Python built-in function, and it tells us about what we provided as a parameter to this function. The `help` command says that `open` takes a filename and a whole bunch of other input parameters. These other parameters, however, are *optional parameters*, which are given default values if you do not specify them. That's what the `=` syntax means.

For now, let's look only at `mode` and `encoding`. `mode` defaults to `'r'`, which means that we are going to read the file. Alternatively, we might want to write the file. `encoding` is something we will talk about in greater detail later, but for now you can think of it as telling the interpreter in what language the file is written (i.e., the computer equivalent of asking if it is written in English or French).

Finally, `help` tells us that this built-in function opens a file and returns a *stream*. Again, we will talk about file input and file output (i.e., file I/O) in greater detail later in the book, but think about it. When I open a book, I probably open the cover and put my finger on the first word on the first page. You can think of a stream as my virtual finger telling me where I'll read the next characters in the file.

Returning to the question about the value the interpreter printed, it was cryptically telling us about the position of its virtual finger in the file. Less cryptically, this value is the description of an *object* that reads a text file whose name is `CatInTheHat.txt` and whose content is encoded in the UTF-8 language.

```{admonition} Terminology
:class: tip
I'll talk more about objects later in this chapter. For now, think of a Python object as the physical representation of an idea. Python records something when it computes the answer to the expression `2 + 3`. Similarly, it records some information when it opens `CatInTheHat.txt` and puts it virtual finger at the first character in that file.
```

## Revisiting our first error

What happens if we ask the interactive Python interpreter to open a file that doesn't exist? We saw that this caused an error when we executed our whole script and the text file we wanted wasn't in the list of files. Will this error message look different when the problem occurs in the interactive interpreter? I don't know. Ok, I do, but let's pretend I don't.

Let's misspell the filename and execute that `open` statement. Type the following in the interactive interpreter:

```{code-block} python
open('CatInTheHut.txt')
```

We got an error message again, as expected. This time it shouldn't look as scary, as we're already familiar with it. Yup, `FileNotFoundError` as we might have expected. This is a type of `OSError` that `help` told us would be raised upon failure to successfully complete the `open` operation.

The only difference this time is that the `Traceback` looks a bit different. It doesn't tell us where in our script the execution error occurred, but that there was a failure on line 1 of this cryptic file "`<stdin>`". We don't really need to understand this broken English, since this traceback information is only needed when we have an error buried somewhere in a multi-line script.

## Undefined names

Let's return to our script `seuss.py`. We now know that the interpreter does nothing with our comment line and then computes a stream object (i.e., a virtual open book) from our `open` command. At the end of `seuss.py`, the interpreter is ready for us to tell it what we want it to do with this stream object, this open book.

Well, how about we read the first line? If Python has a built-in file `open` command, maybe it has another built-in function called `readline`. Let's type `readline()` at the bottom of our script and run it. The empty parentheses at the end of `readline` simply say that we're not providing this command with any input parameters.

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
open('CatInTheHat.txt')
readline()
```

Oops, another error that says, `NameError: name 'readline' is not defined`. This seems to be telling us that Python doesn't know what to do when we tell it to execute a command called `readline`.

Let's look at the online Python documentation and in particular at the list of [built-in functions](https://docs.python.org/3/library/functions.html#built-in-funcs). The command `open` is there, but not `readline`. In fact, there are no read functions at all. There must be something wrong with our thinking. What good is it to open a file if you cannot read it?

## Talking through your confusion

Let's try these commands on a person to see if we can glean what might have gone wrong. Yes, you should try the following with a friend or family member.

Give your friend a book and then command them to `open('book_name')`, where `book_name` is a book that they're holding. Next, command them to `readline()`.

Did they do what was commanded? While they probably opened the book in their hands, it's hit or miss if they read a line from it. The command `readline()` doesn't specify what line where you want read. What if our script had opened two files or your friend two open books? How would each know which to read?

This is an example of what's good and bad about computers. They are very literal. They will do exactly what you ask them to do and nothing more.

If you're lucky, when you tell a computer to do something ambiguous, it won't do anything and tell you that it is confused by your command. That's what the Python interpreter did when I asked it to execute a command name that it didn't know.

If you're unlucky, it will do something random, and you'll need to determine for yourself if the computer did what you wanted it to do. We will spend a good bit of time talking about how to avoid these situations and what to do when you find yourself in them.

## Naming a computation's result

We can solve this problem of ours by using the result of the `open` command. In particular, we want to tell the interpreter to use that thing it computed during `open` and read the first line in that file's stream. You know, that line under its virtual finger. This context removes any ambiguity about what line to read.

Let's delete `readline()` and type a new piece of pseudocode:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
open('CatInTheHat.txt')
# Read the first line using that thing you computed
```

This pseudocode highlights a naming problem. How do we tell the interpreter what "thing" I mean? Luckily, Python and all other programming languages allow us to name the results of a computation.

Edit the `open` statement as follows, which will assign the result of the `open` command to the name `my_open_book`:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
my_open_book = open('CatInTheHat.txt')
# Read the first line using that thing you computed
```

A single equal sign (`=`) is the *assignment operator* in Python. We'll talk more about naming and the intricacies of assignment, but for the moment, you can think of this single equal sign as saying, "Whatever `open` produced as a result, name it `my_open_book`."

With a name for our open book, we can now read specifically from it. Append `my_open_book.readline()` to the end of your script, which is proper Python syntax for reading the next line from the open file. I'll explain the syntax of this statement in a bit, i.e., don't fret over the use of the period between `my_open_book` and `readline`.

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
my_open_book = open('CatInTheHat.txt')
# Read the first line using that thing that you computed
my_open_book.readline()
```

Click the Run button. No errors! But also no read line in our console.

## Debugging

It's one thing to have the interpreter report to you that something didn't work, but it can be even more frustrating when a script runs to completion and the result isn't what you expect. How do you figure out where and what went wrong?

We will learn that there's no single, simple answer to this important question. The good news is that there are many effective strategies you can employ to determine what went wrong. Because our current script is so small, we'll start with the most straightforward, which you can think of as a brute-force approach. In this approach, we start at the beginning of the script and fed each line that isn't a comment to the interactive interpreter. The following is a transcript of what happened when I fed the highlighted lines, one at a time, to the interactive interpreter:

```{code-block} none
---
emphasize-lines: 1, 2, 4
---
>>> my_open_book = open('CatInTheHat.txt')
>>> my_open_book
<_io.TextIOWrapper name='CatInTheHat.txt' mode='r' encoding='UTF-8'>
>>> my_open_book.readline()
'The sun did not shine.\n'
```

Notice that I typed the name of the result we computed in our `open` command (i.e., `my_open_book`) at the second prompt, and the interpreter printed the value of that name (i.e., a `TextIOWrapper` object). This matches what we saw earlier that `open` computed. This also makes clear what it means for the interactive interpreter to remember named results (i.e., keep the *state of our running computation*). We can look at (and even modify the values of) the names we know at any point in the execution of a script. 

Continuing with the transcript above, I then typed the last line of our `seuss.py` script and hit enter. In response, the interpreter printed the book's first line. Our script does successfully read the first line of our book, but why didn't the book's first line appear when we ran `seuss.py`?

## Showtime

To answer this question, you need to understand what it means to have the Python interpreter in interactive mode. In this mode, the interpreter becomes a great way to test little pieces of code. It's like asking the actors in the theater to run through just a small piece of a theatrical script, during which we expect our actors to jump into and then out of character. When the interpreter jumps "out of character" (i.e., after it finishes whatever we asked it to do), it will print the result of its last computation. This is why it printed the first line after we asked it to execute `my_open_book.readline()`; it's the result of executing `readline` in the context of `my_open_book`.

When it comes to showtime, however, we want a different behavior: the actors should stay on script and do only what's in the script. Showtime begins when we click our IDE's Run button. The showtime run of `seuss.py` printed nothing because *we didn't tell the Python interpreter to print anything*. In particular, the script's last command told the interpreter to read a line from a file; it was to do nothing else. If we want the interpreter to print the line it read, we must tell it explicitly to do this. It's like the difference between asking a human actor to read a book versus read a book out loud.

## Printing to the console pane

To have your script print something to the IDE's console panel, you must command the interpreter to `print`. The argument to the `print` command is what we want to print, which in our case is that thing the interpreter read in `readline`.

If you guessed that we had better name this thing we read so that we can refer to it in our `print` command, good for you! You are learning to code. Let's name the value we read `the_line`. The following is what `seuss.py` should look like after these additions:

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

In pseudocode, `print(the_line)` says something like "please print the value of the object named `the_line` to the console."

Do we dare try running our entire script now? Are you ready for showtime? 

```{admonition} You Try It
Run the `seuss.py` script in your IDE.
```

Success! We have started to create a digital agent that can read a book to us.

## Statements, objects, attributes, and types

We've done a lot, and let's take a few minutes to focus on terminology and how the Python interpreter views what we've written.

We have been writing *statements*, one per line, that the Python interpreter reads. As the interpreter reads a line, it discards anything that is a comment, and if there's something left, it performs the requested computation.

When the Python interpreter executes a statement in our script, it works with *objects*. As you'll come to learn, almost everything that a Python script manipulates is an object. An object is how Python represents simple things like the number `5` and complex things like the `CatInTheHat.txt` file we opened. Even functions are objects.

Objects have *attributes*, and objects of the same *type* (also called its *class*) have the same collection of attributes. For example, the numbers `5` and `6` are of the same type (i.e., both are integers) while our open file object `my_open_book` has a different type and thus different attributes.

As a real-world analogy of an object, its type, and its attributes, consider my dog Cosmo. Cosmo is an object of type dog. The characteristics of a dog (e.g., fur, tail, and wet nose) are the attributes common to all dogs. These attributes will have specific values for any particular dog (e.g., my dog Cosmo has yellow fur). In contrast, a table is not a dog and thus has different attributes. It may not seem important to know the type of an object, but you'll soon learn how it helps the interpreter understand what it is we want it to do.

## Namespaces

How does Python allow you to refer to the attributes of an object? Using the dot syntax we used in `my_open_book.readline()`. We saw earlier that `readline` by itself was not a name for a Python object because we received a `NameError` when we tried executing `readline()`. But by typing `my_open_book.readline()`, we successfully read a line from our text file. The function `readline` is an attribute of all open-file objects, and given one, we can read a line from it.

The open-file object provides a context for the name that comes after the dot. The computer science term for the name to the left of a dot is *namespace*. This idea of namespaces is something we all use everyday. For example, when I use the name Christina at the office, I mean my administrative assistant. When I think of Christina at home, I mean by wife. My office and my home are two different namespaces, and I replace one with the other as I move from home to work and back again.

As another example, when I think of Jim while at work, I'm thinking of Jim Waldo, my long-time co-instructor. If someone mentions Jim to me at home, I'll look confused. There's no one named Jim in my immediate family. This is what happened with the name `readline`. This name is known only within the namespace of an open-file object.

Python provides a built-in function, called `dir`, that lets you see what names are currently defined in a namespace. Type `dir()` at the interactive interpreter's prompt, and it will show you the names in the current, local namespace. If you ran the statements in `seuss.py` before you ran `dir()`, you should find the names `my_open_book` and `the_line` in the printed list. When we assign objects to these names (via the `=` operator), Python put them in our local namespace so that later uses of those names worked without error.

In the list of names you got back from `dir`, you'll also see a number of special names with double underscores at the start and end of the identifiers. I don't want to dwell on these special names at this point, but I will direct your attention to the name `__builtins__`. Type `dir(__builtins__)` and you can see that this is where Python keeps the names for its built-in functions (e.g., `help`, `open`, `print`, `dir`, `NameError`, and `FileNotFoundError`).

Notice that the name `readline` is not in the local namespace or the `__builtins__` namespace. What about the namespace associated with the object `my_open_book`? Let's type `dir(my_open_book)` and see. Bingo! There it is. This is the full list of attributes of the object known to us as `my_open_book`.

Putting this all together. When we wrote `my_open_book.readline()`, you can read that as saying, "We expect that in the namespace of the Python object named `my_open_book` there is an attribute called `readline`." And because we followed it with empty parentheses, this attribute is a function we can invoke without any input parameters.

## Strings and string literals

Let's continue to investigate what `dir` tells us about the names in `seuss.py`.

```{admonition} You Try It
Type `dir('CatInTheHat.txt')` and then `dir(the_line)` in the interactive Python interpreter. Make sure you have fed the lines from `seuss.py` to the interactive interpreter before running these two `dir` commands.
```

Notice that the list of attributes for `'CatInTheHat.txt'` and `the_line` are the same, and these lists are different than those in the namespace of `my_open_book`. Remember that I said objects of the same type have the same attributes? This must mean that `'CatInTheHat.txt'` and `the_line` are objects of the same type (and different from an open-file object).

The Python built-in function `type` will tell you the type of an object. In the interactive interpreter, run the following list of highlighted Python statements:

```{code-block} none
---
emphasize-lines: 1, 3, 5, 6, 8, 9
---
>>> type('CatInTheHat.txt')
<class 'str'>
>>> type(open)
<class 'builtin_function_or_method'>
>>> my_open_book = open('CatInTheHat.txt')
>>> type(my_open_book)
<class '_io.TextIOWrapper'>
>>> the_line = my_open_book.readline()
>>> type(the_line)
<class 'str'>
```

When we ask the interactive interpreter for the type of `'CatInTheHat.txt'` and `the_line`, it says `str`, which stands for *string*. Strings are what computer scientists call a sequence of characters like: *From this sentence's capital F to its final period is a string.* Filenames on our computers, such as `CatInTheHat.txt`, are strings (i.e., the characters from the capital `C` until the second `t` in `txt` and including the period before `txt`). Since the ordering of the characters in a string matters, we think of them as strung together in a line.

You create a particular string value in Python, called a *literal string value*, by enclosing your desired characters in either single or double quotes. I used single quotes in `'CatInTheHat.txt'`, but I could also have used double quotes. We need these delimiting characters so that we can include spaces in our strings. Because Python allows both a matching pair of single or double quote characters as the delimiter, I can easily create the string `"Mike's dog"`.

```{admonition} You Try It
Feed `type("Mike's dog")` to the interactive interpreter. Try replacing `" Mike's dog"` with other strings. What happens if you leave off the starting or ending delimiter? What happens if the delimiter you use is a character in your string? What happens if your starting quote character doesn't match the ending one?
```

Both `'CatInTheHat.txt'` and `"Mike's dog"` are string *literals* because they represent a single, specific value. Their value is literally what you type within the quotes.

String literals are not the only kind of literal values. The number `5` is also a literal, i.e., it represents a single specific value. Of course, it's not string. If you ask `type` about it, you'll find that it is of type `int`. The interpreter is telling you that it thinks of `5` as a literal integer value. We'll see other types of literals as we write other scripts.

## Variables

Did you try removing both the opening and closing quotes on `'CatInTheHat.txt'`? Without the surrounding quotes, the interpreter responds with a `NameError: name 'CatInTheHat' is not defined`. Python thinks that `CatInTheHat` is a name[^fn6] and not a string literal.

You'll eventually hear people talk about the names of objects we define with the assignment operation (`=`), like `the_line`, as *variables*. The idea behind this term is that a name can refer to one object at one point in a script and then to different object at another point. For instance, we have used the variable `the_line` to grab and print the first line of our book, and in a minute, we'll extend `seuss.py` so that it uses the same name to grab and print the book's second line. Same name, but its value (i.e., object it names) varies over time.

Here's the same idea without the complexity of having to read the two strings from the file `CatInTheHat.txt`:

```{code-block} python
---
lineno-start: 1
---
the_line = 'The sun did not shine'
print(the_line)
the_line = 'It was too wet to play'
print(the_line)
```

In summary, `the_line` names two different string objects in the script above; its value varies as we execute the script. `'The sun did not shine'` is a string literal; its value never changes during the script's execution.

## Valid names in Python

You have seen several names (variables) in this chapter, and you are probably starting to wonder if there are any rules about what constitutes a valid (i.e., won't cause an error) name in Python. There are, and these are them:

* *Valid characters*: A name may contain the underscore character (`_`), any upper or lowercase letters (`a-z` or `A-Z`), and the digits `0` through `9`.
* *Valid starting character*: A name may start with any of these characters except the digits `0` through `9`.
* *Case matters*: `the_line` and `The_line` are two different names.
* *No spaces*: The space character is never allowed in a name. As you've seen, I like to replace spaces with the underscore character. Others prefer what is called *CamelCase*, where you run the words together and capitalize the start of each (as you might have noticed with the type of an open-file `_io.TextIOWrapper`).
* *Length*: Python allows names of any length, but as we'll discuss in a later chapter, your names should adhere to the Goldilocks Principle. They should be long enough to express something meaningful to your script's reader, but not so long as to be distracting and hard to type.

## Terminology illustrated

{numref}`Figure %s <c01_fig4_ref>` labels the lines we've written in `seuss.py` with the terms I've introduced in the last several sections.

```{figure} images/c01_fig4.png
:name: c01_fig4_ref

Identifying what's what in the statements of our simple `seuss.py` script.
```

The first line is a comment. The second line is a statement that causes the Python interpreter to open a file and create an open-file object that we name `my_open_book`. This line refers to three distinct objects:

1. A function object named `open`. Function objects contain the instructions that tell the interpreter how to execute the command this function represents.
2. A literal string object with the value `'CatInTheHat.txt'`, which is the parameter that `open` needs to do its work.
3. An object of type `_io.TextIOWrapper` that we give the name `my_open_book`.

The fourth line accesses an attribute of the object named `my_open_book`, which reads a line from the open file and then names the resulting string object `the_line`. In the fifth and final line, the Python interpreter uses the built-in function object `print` to print value of `the_line` on the console.

Give yourself a high-five. You have learned a lot about writing scripts and how the Python interpreter determines what you want it to do.

## Aliasing

I mentioned that a name like `my_open_book` is a variable because it may change its value over the runtime of a script. For example, a single script may read one book and then another, using `my_open_book` for each. In a Python script, this might look something like the following:

```{code-block} python
---
lineno-start: 1
---
# Open the first book to be read
my_open_book = open('CatInTheHat.txt')
# ... do some work with my_open_book

# Switch to the next book
my_open_book = open('MikeAndHisSteamShovel.txt')
# ... do some more work with my_open_book
```

While a script may use *one name for multiple different objects*, it may also have *multiple names for a single object*. For instance, I can create another name for the object named `my_open_book` by placing this name on the right-hand side of the assignment operator and a new name on the left-hand side.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
the_same_book = my_open_book
```

Let's verify this claim and learn another useful built-in function along the way. We begin with the following script, which prints what the `type` command tells us about `my_open_book` and `the_same_book`. Placing the command `type(my_open_book)` inside the parentheses for `print` simply says that I'd like the result of the `type` command used as the parameter to the `print` command. Just like you learned to evaluate arithmetic expressions in math class, the Python interpreter evaluates the expressions inside the parentheses first.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
print(type(my_open_book))
the_same_book = my_open_book
print(type(the_same_book))
```

When you run this script, you'll find that both variables have the same type. We would expect that if both named the same object, since each object has only one type. But `type` does not verify for us that both variables name the same object. We would get the same answer from `type` if the interpreter made a copy of the object named `my_open_book` during the assignment on line 3. We need a tool in our tool chest that will help us determine if: (1) Python's assignment operator creates a new name for an object; or (2) if it creates a copy of the object and gives the new name to the new copy.

This distinction is the purpose of the built-in `id` function. It tells us the value of an object's identifier, which is an attribute common to every Python object. Object identifiers are unique. No two objects have the same object identifier. It's like Social Security Numbers (SSNs) in the United States; no two people should have the same SSN. 

```{admonition} You Try It
Change the lines in the previous code block to print the object identifiers of both variables instead of their types. When your script looks like the one below, run it.
```

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
print(id(my_open_book))
the_same_book = my_open_book
print(id(the_same_book))
```

You probably got back two large numbers, and if you look closely, they are identical. Therefore, `my_open_book` and `the_same_book` are two different names for the same object. This is equivalent to the fact that I respond to both Mike and Michael. But why, you might wonder, is this important to know?

```{admonition} You Try It
Change your code block once again so that it now matches the next. Run this script and think about what the Python interpreter did.
```

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
the_same_book = my_open_book
the_line = my_open_book.readline()
print(the_line)
the_line = the_same_book.readline()
print(the_line)
```

In this script, the virtual finger moved along in the book no matter which name was used. This is called *aliasing* in computer science, which makes sense given our common understanding of the term.

I raise this point for two reasons: (1) You should have the right mental model for what's happening in the Python interpreter; and (2) you will probably be surprised by aliasing at some point in your writing of Python scripts.

```{tip}
It is easy to forget whether an assignment in Python (or any programming language for that matter) makes a copy of an object or gives that object a new name. I often forget in languages in which I've programmed for years. If you ever don't remember, don't guess. Look it up or try a simple experiment like we just did. Guessing is the fastest way to creating a hard to find error in your script.
```

## Reading two lines

We just saw multiple calls to `readline` grabs successive lines from our book, and by naming each line we read, we can `print` them one at a time. Let's clean up our script---removing comments and any statements from our previous experiments---so that it reads the first two lines of our book. When your `seuss.py` script looks like the following, hit that Run button.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
the_line = my_open_book.readline()
print(the_line)
the_line = my_open_book.readline()
print(the_line)
```

## Carriage returns

You might be annoyed by the blank line that our script inserts between the book's first two lines. To understand why this happens, we need to move beyond printable characters and talk about special characters, and specifically, the character that represents a *carriage return*.

What, might you ask, is a carriage return? It is a term that had meaning when people used [old-style typewriters](https://www.youtube.com/watch?v=FkUXn5bOwzk). Around 1:28 into the linked video, the person pushes a silver handle to reset the typewriter's carriage to the start of the next line. This silver handle is called a carriage return.

When we're in a text editor on our computers, we simply hit [the button labeled "return" (Apple Mac) or "enter" (IBM PC)](https://en.wikipedia.org/wiki/Enter_key), which moves the cursor to the start of the next line in our text document. While this key press doesn't look like it inserts anything into our text document, it actually does. Run the following three statements in the interactive Python interpreter, and it will show you all the characters in the first line of our text file.[^fn7]

```{code-block} none
---
emphasize-lines: 1, 2, 3
---
>>> my_open_book = open('CatInTheHat.txt')
>>> the_line = my_open_book.readline()
>>> the_line
'The sun did not shine.\n'
```

A backslash followed by a lowercase n (`\n`) is the printed representation for the mechanical action of a typewriter's carriage return. It is called a *newline character*. While you should think of `\n` as a single character, to type it in a string literal you must type a backslash \\ followed by a lowercase n. Where you put a newline character in a string literal indicates where you want the computer's cursor to move to the beginning of the next line when the string is printed.[^fn8]

Back in the interactive interpreter, type `print(the_line)` and hit enter. Notice that when we print the value of this string, the interpreter knows that we want the action (i.e., move the cursor to the start of the next line) and not a character representation of this special character.

```{code-block} none
---
emphasize-lines: 1
---
>>> print(the_line)
The sun did not shine.
 
```

Wait a minute! How many carriage returns did `print` just execute? Two! The one we saw at the end of value of `the_line` and another that caused the next blank line. To verify this, create a new string object without a newline at its end.

```{code-block} none
---
emphasize-lines: 1, 2
---
>>> a_plain_line = 'The sun did not shine.'
>>> print(a_plain_line)
The sun did not shine.
```

To answer this mystery, we have to dig into the formatting options associated with `print`. There are a lot options, and we'll talk here about just one. I encourage you to read the documentation for `print` when you need to do something new.

```{admonition} You Try It
Run `help(print)` and notice the input parameter called `end`. By default, `print` appends a newline character to the end of what it prints. In other words, the statement `print()` prints a blank line. Try it.
```

To eliminate the blank lines in our script's printed output, we set the `end` parameter to a string with no characters (i.e., the literal string '' that starts with a single quote, contains no characters, and ends with a single quote). This may look like a double quote character, but make sure you type two single quotes right next to each other. You can also create an empty string by typing two double quote characters right next to each other.

Here's our updated `seuss.py` script, which prints the book's first two lines as we expected to see them.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
the_line = my_open_book.readline()
print(the_line, end='')
the_line = my_open_book.readline()
print(the_line, end='')
```

## Reading an entire story

I don't know about you, but if someone said that they were going to read a story to me and then stopped after the first two lines, I'd be unhappy. Let's figure out how to get `seuss.py` to read the rest of our book.

A bad idea is to continue copying the pair of `readline` and `print` statements in our current `suess.py` script until there are as many copies of this pair as there are lines in our input file. We'd have a working solution, but it would be tied to the length of this particular story. To read another book, we'd have to write a script to match its length.

A better approach is to build a script that can read a story of any length, like a human actor would. The key to this approach is to write statements that *discover runtime facts* (i.e., facts we don't know when we write the script, such as the length of the current story) and then *act on those facts in such a way that it tailors the script's behavior*. Acting on runtime facts to change a script's behavior requires us to learn to use *control-flow statements*.

To this point, we have directed the Python interpreter with a linear sequence of statements, which it executes one after another from the first to the last. Control-flow statements allow us to specify: 

1. when a statement or block of statements should be executed; and
2. whether a statement or block of statements should be repeated.

The first involves *conditional branching* and the second *looping*. Both are the tailoring we'll need to solve our problem.

## Creating a loop

The `readline` and `print` commands we repeated in `seuss.py` are statements that we'd like to write once in our script and then, instead of copying this pair, indicate through other information in our script that the pair should be repeated as many times as there are lines in `my_open_book`. The following code block illustrates what this might look like, where I've indented the instructions I want repeated under a comment that says how many times to execute the pair.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
# Do the indented work as many times as there are lines in my_open_book
    the_line = my_open_book.readline()
    print(the_line, end='')
```

Lines 2-4 are what computer scientists call *a loop*, and the indented statements are called *the loop body*. Loops consist of what you should view as two distinct sets of instructions: those that define how many times the loop's repeating work is done; and those that define the work to be repeatedly done. In the script above, the pseudocode comment states how many times the loop repeats, and the loop body is the work that's repeatedly done.

In some problems, we'll know the number of times a loop needs to execute, but in this problem, we don't. We've opened the book and put our virtual finger on the story's first line, but we don't know how many lines are in it. Between lines 1 and 2, we'd need to add statements that instruct the interpreter to compute the number of lines in `my_open_book`.[^fn9]

While we could make this approach work, it isn't the only way to specify how many times the loop body should execute. Consider this alternative that doesn't use *a count* but *an ending condition*:

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
# Do the indented work until all the lines have been read
    the_line = my_open_book.readline()
    print(the_line, end='')
```

The two designs are functionally equivalent (i.e., both will solve our problem). Which one you pick depends on a whole host of questions you'll learn to ask throughout this book. For now, we'll choose the second approach, which requires less code (i.e., we don't have to count the number of lines in the input file before starting to read it). It also gets you to think about when and how a Python statement---and therefore your script---might *fail to operate as intended*.

```{tip}
Discovering when a script fails to operate as intended is the domain of _testing_, and despite being extremely important, testing a script is neither fun nor easy to do. For most of us, testing is akin to admitting that it's possible the script we wrote is wrong. When we write a script, it is human nature to believe it is going to work and work correctly. The truth is, however, you can't know that for certain until you test it.
```

## End of File (EOF)

The second approach hinges on knowing when `readline` successfully reads a line from the input file and when it doesn't. To get a feel for what this means, let's return to our `seuss.py` script,[^fn10] which always reads the first two lines from the input file, and think about an input for which this is a seemingly bad idea. Once you've taken a moment to decide upon a problematic input file, work through the next "You Try It" block.

```{admonition} You Try It
Use your IDE to open `CatInTheHat.txt` and delete all but the first line. It should now look like the contents of `Cat1.txt` in the `txts` folder. It might be problematic to ask our script, which expects a file with at least two lines, to read a file with just one. But let's try it anyway: Once you've changed `CatInTheHat.txt`, click the Run button.
```

Despite the fact that `seuss.py` tries to read more lines than are in the file, it correctly prints the input text file.[^fn11] Why? What does `readline` do when it attempts to read beyond the end of the input file?

Let's head back to the interactive Python interpreter. In it, run the first line of our script, i.e., call `open` and name the object it computes `my_open_book`. Now type `help(my_open_book)` and hit enter. The interpreter answers assuming we want to know what we can do with the object that our variable `my_open_book` names. What a nice feature of the Python `help` command!

Scroll down to where `help` talks about `readline`. There it says that `readline` returns "an empty string if EOF is hit immediately." EOF stands for *end of file*, and it tells you that there is nothing left to read.

Our script names this empty string `the_line` and then prints it. Is it ok to print an empty string? Try it by typing `print('')` in the interactive Python interpreter. No complaints as it happily prints nothing. By taking advantage of the resilient way `readline` handles EOF (i.e., we can direct the Python interpreter to read past the end of the input file), our script doesn't have to pre-determine the number of lines in the input file. Our new approach saves us work!

```{tip}
You won't always see these sorts of shortcuts ahead of time. Sometimes you discover them in the middle of your problem-solving process. When this happens, consider stepping back to an earlier step in the process. Playwrights, artists, engineers, and other designers do this all the time. They listen to the "backtalk" from their materials, tools, and in the case of playwrights actors. There are typically many ways to solve a problem, and it is often a Good Thing to listen and adjust your design accordingly.
```

## Three major tasks

Let's now implement this work-saving approach to write a version of `seuss.py` that reads an entire book and finishes with "The End," which you'll recall we added to our problem specification while imagining a specific instance. Update your `seuss.py` script to look like the following:

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')

# Do the indented work until all the lines have been read
    the_line = my_open_book.readline()
    print(the_line, end='')

print("The End.")
```

Notice that this script comprises three big pieces of work, which I have separated with blank lines:

1. It starts by opening the input file.
2. It then uses a loop to print all the lines in the input file.
3. When the loop is done, it prints "The End." Notice that this final print is not indented. It's not part of the loop but something that occurs after the loop is complete.

```{tip}
I encourage you to use blank lines to separate the major tasks in your script. This accomplishes two things: (1) it makes it easier to skim your script for a high-level understanding of what it does; and (2) it pulls together the Python statements that accomplish a high-level task so that you can focus, when necessary, on that task's code.
```

## Testing a condition

It's time to focus on our loop and translate its pseudocode into Python. We'll begin with our loop's *ending condition*, which is when value of `the_line` becomes the empty string. In other words, we need to compare that variable's string value against a string literal, and we need to know when they're equal. This is a *test for equality*, and it is one of many different comparison operators that Python provides.[^fn12] When comparing the equality of two strings or two numbers in Python, you use the `==` operator.[^fn13]

```{code-block} python
---
lineno-start: 1
---
the_line == ''
```

Whenever you test a condition in Python, the result is one of two literals: `True` or `False`. The `==` operator produces `True` when the value of the expression to the operator's left is equal to the value of the expression to its right. If they're not equal, `==` evaluates to `False`. You can read the condition above as asking the Python interpreter to determine whether the statement "the value of variable `the_line` is equal to the empty string" is logically true or false.

```{admonition} Terminology
:class: tip
If you evaluate `type(True)` or `type(False)` in the interactive Python interpreter, you'll learn that these literals are of type `bool`, which stands for _Boolean_. The name is a nod to the work of George Boole. `True` and `False` are the only possible values of type `bool`.
```

## Exiting the loop

We can now use this condition to control which statements the Python interpreter executes. For our problem, we want the following: when the condition we specified above is true, break out of the loop and execute the statements after the loop. In Python, lines 8-9 illustrate how we'd write this:

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')

# Do the indented work until all the lines have been read
    the_line = my_open_book.readline()
    print(the_line, end='')

    # Check for EOF
    if the_line == '':
        break

print("The End.")
```

A Python *break-statement* exits the loop in which it sits. Because we only want to perform this action when `the_line` is the empty string (i.e., there are no more lines in `my_open_book` to read), we place the `break` inside a Python *if-statement*. An if-statement controls when the instructions indented under it execute. In other words, the `break` executes only when the condition in the if-statement is true. When this condition evaluates to `False`, the statements indented under it are skipped. You can put any expression that evaluates to a `bool` between the keyword `if` and the colon (`:`).

## Indentation

Indentation, or the whitespace at the start of each line in a script, holds meaning in Python. It matters to the way that the Python interpreter understands your script, as I explained when discussing the operation of Python's if-statement and hinted in illustrating a Python loop. This shouldn't be a completely strange concept for we use whitespace in our writing to, for example, indicate the start of a new paragraph.[^fn14]

Not only does indentation matter, but how you create this indentation matters. If you want to avoid problems, indent using spaces, not tabs. A tab is a special character, like the newline character, and you will want to set your IDE to convert tabs into a specific number of spaces.[^fn15] Whitespace containing a tab character might look the same to you and me as one consisting of only spaces, but they don't look the same to the Python interpreter. 

Finally, neatness and consistency are a virtue in problem solving with computation. Just as it is hard to read an essay that's inconsistently indented, code that is inconsistently indented is hard for humans and the Python interpreter to understand. All the Python statements you want grouped together should be indented *by the same amount*. If they are not, you will have problems.

## Loop until

We nearly have a working Python script that solves our problem. We need only a Python loop statement to replace our remaining pseudocode, allowing us to repeatedly execute lines 5-10 (i.e., the loop body). There are two looping statements in Python, and we will use the one of them, the *while-loop*.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')

# Print every line in the book
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')

    # Check for EOF
    if the_line == '':
        break

print("The End.")
```

Like the if-statement, the while-loop tests a condition that determines the execution of the statements indented under it (i.e., lines 5-10 in our loop body). On line 4, between the keyword `while` and the colon, is the while loop's condition. I typed the literal `True` as this particular loop's condition. During execution, this condition is checked when the interpreter first encounters the while-statement (i.e., when it reads line 4 after processing the comment on line 3) and after each iteration of the while-loop's body. Only when the condition evaluates to `True` will the loop initiate another iteration of its loop body.

What does it mean that I've put the literal `True` in for our while-loop's condition? When it's evaluated, the Python interpreter will (trivially) determine that the expression `True` is true and move forward to execute the loop body. And because this condition can never evaluate to `False`, line 4 an example of an *infinite loop*: Without any other way to break out, the loop runs forever.

```{admonition} You Try It
At some point, you'll mistakenly create an infinite loop. To show that they're nothing scary, let's run one. Look at `seuss-infinite.py` in this chapter's code distribution and notice that I've commented out the if-statement and the break-statement it protects (lines 9-10 in the code block above). Comment out these same two lines in your `seuss.py` script. When you click the Run button, the script prints the story but not "The End." It's stuck continually trying to read another line in a file without any more lines and never gets to the print-statement on line 12). To stop this script, notice that the Run button has turned into a Stop button. Click it and the script will stop. If you're running in the shell, as we'll do in later chapters, the keyboard combination Ctrl+C (i.e., simultaneously press your keyboard's control key and the letter-C key) will end an infinite loop's execution. Don't forget to uncomment lines 9-10 in your `seuss.py` script before continuing.
```

Our loop doesn't run forever because it checks for the exit condition inside the loop body and breaks out of the loop when `readline` indicates that we hit the EOF. It's more convenient to put this condition inside the loop body than between the keyword `while` and the colon because we cannot check for EOF until we read a line. As I mentioned earlier, there are other ways to write this loop, and as you move through the upcoming chapters, you'll learn to evaluate for yourself which approach feels natural for your problem.

Returning to my emphasis on indentation in Python, notice that line 10 is indented twice. This double indentation indicates that the break-statement's execution depends upon the conditions in both the while-statement on line 4 and the if-statement on line 9. Only when both conditions are true will the `break` execute.

```{admonition} You Try It
Run the previous code block, which is called `seuss-final.py` in the code repository. You'll find that this script "reads out loud" one of my favorite children's books, The Cat in the Hat by Dr. Seuss. Problem solved!
```

## Any book

While our script's loop is flexible enough handle any book (i.e., plain text file), `seuss.py` opens a particular book, i.e., the one specified by the literal string in `open`. To read a different book, we need to change this string literal. While we could do this, a better approach has us go back and change our problem specification. Let's have our script to begin by asking the user what book they'd like to read. In our new script (called `anybook.py`[^fn16]), we'll store the user's response in a variable, and then use that variable as the input parameter to `open`.

To ask the user for input while your script is running, Python provides us with the built-in function `input`. If you type `help(input)` in the interactive Python interpreter, you learn how this function does exactly what we need as long as we type just the name of the file we want to read without any spaces before or after the filename (i.e., `input` captures everything we type).

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

```{admonition} You Try It
To try running `anybook.py` with a new story, download the plain text version of your favorite book from Project Gutenberg (https://www.gutenberg.org/) and upload it to your IDE's `chap01` directory. To find a plain text file, use Project Gutenberg's advanced search option and set "Filetype" to "Plain Text (txt)" and "Language" to English. We'll play with other human languages in Chapter 3.
```

## This problem, in general

Congratulations on solving your first computational problem! I hope you feel proud of your accomplishment. But you might also be asking, "Was this problem just a vehicle to get started in problem solving with computation, or was it something more?" The answer is something more.

While the context and storyline in which I wrapped this chapter's problem may not seem like something you'd ask your computer to do, the act of opening and accessing the contents of digital files is a skill that you will use over and over again in solving your own problems. You have now started to build that skill.

Like this first problem-to-be-solved, I chose the problems in each chapter of this book for two reasons: (1) each builds upon the knowledge you've gained in previous chapters to teach you new design techniques, CS concepts, and programming skills; and (2) because each problem is an instance of a more generally applicable problem. In other words, my selection of our problems-to-be-solved is not frivolous, but builds in you a competency to attack many similar problems.

## Historical references to computational thinking

As a published term, computational thinking was first mentioned by Seymour Papert in his 1980 book titled *Mindstorms: Children, computers, and powerful ideas*.[^fn17] As a mathematician, computer scientist, and educator, Papert was looking for concrete ways to have children better understand abstract concepts, and he felt computers could help children connect with powerful mathematical ideas in a manner beyond their formulae. To Papert, this was thinking with computation.

In a 2006 CACM article, Jeannette Wing argued for a much broader notion of computational thinking. This article provides numerous diverse examples of computational thinking, and it posits that such thinking is an important skill for everyone, not just computer scientists.[^fn18] Wing also emphasized that this type of thinking is more like conceptualizing and problem solving than the narrow act of programming a computer. She debunked the idea that computational thinking is how computers "think." In contrast, she stressed that it is how humans think, and this thinking draws together the many ways we think in other logical and mechanical domains.

Although Wing doesn't provide a single definition for computational thinking in her 2006 article, many in the community have contributed thoughts toward a universally agreed-upon definition. For example, Alfred Aho offered this definition in 2011: "computational thinking \[is\] the thought processes involved in formulating problems so their solutions can be represented as computational steps and algorithms. An important part of this process is finding appropriate models of computation with which to formulate the problem and derive its solutions."[^fn19]

More recently, the work by Wing, Aho, and others has been synthesized into the following definition: computational thinking is the "thought processes involved in formulating problems and their solutions so that the solutions are represented in a form that can be effectively carried out by an information-processing agent." In their 2021 *Science & Education* article, Michael Lodi and Simone Martini call this the Aho-Cuny-Snyder-Wing definition, in recognition of the contributions from these four particularly influential individuals.[^fn20]

\[Version 20240806\]

[^fn1]: Jeannette M. Wing. 2006. Computational thinking. Commun. ACM 49, 3 (March 2006), 33--35. https://doi.org/10.1145/1118178.1118215

[^fn2]: Alfred V. Aho. 2011. Ubiquity symposium: Computation and Computational Thinking. Ubiquity 2011, January, Article 1 (January 2011), 8 pages. https://doi.org/10.1145/1922681.1922682

[^fn3]: The IDEs I'll suggest to you are ones used by professional programmers. Because of this, you can continue with your selected tool long after you finish this book.

[^fn4]: If you're a MacOS user, I do mean the control and not command key.

[^fn5]: Note that I'm switching back from transcript blocks to code blocks. This allows you to copy the content of the code block and paste it at your interactive Python interpreter's prompt. Don't just read. Try!

[^fn6]: And in particular, a namespace for the name \`txt\`.

[^fn7]: Remember, in the interactive interpreter, typing an object's name tells the interactive interpreter that we want to see the value of that object.

[^fn8]: There are old computer systems that required you to use two special characters to move its cursor to the start of the next line. You typed a carriage return character (\`\\r\`), which moved the cursor to the beginning of the current line, and then a newline character (\`\\n\`), which moved the cursor to the next line. I'll assume you're using a modern computer that requires only the single \`\\n\` special character to perform both actions.

[^fn9]: Computing the number of lines in a file is something that ALE 1.2 asks you to do. Please see the ALEs (i.e., Active Learning Exercises) on the website that is the companion to this book.

[^fn10]: The version of \`seuss.py\` that I mean is the one that looks like the script \`seuss-2lines.py\` in your IDE's file browser.

[^fn11]: Your IDE's file editor might show \`Cat1.txt\` as a file with two numbered lines, but that last blank line contains nothing on it. It exists because of the newline character at the end of the first line! If you don't believe me, delete all the lines in \`CatInTheHat.txt\` and rerun \`seuss.py\`. It definitely didn't read two lines that time, and it still functioned correctly.

[^fn12]: A web search for "python comparison operators" will tell you about the others.

[^fn13]: Notice that Python's test for equality is written with two equal signs, which distinguishes it from Python's assignment operator, which uses a single equal sign. Even those of us experienced in coding will mistakenly type one equal sign when we meant two. When debugging your code, check for this type of mistake.

[^fn14]: Other programming languages use explicit symbols to group statements together. The difference is more taste than anything. Python believes that indentation without any extra special symbols makes scripts easier to read and understand.

[^fn15]: I use four spaces per indentation in the code throughout this book.

[^fn16]: I've started a convention in \`anybook.py\` that I'll continue with the rest of the book's scripts: it begins with three hash (or number sign) symbols. On this line, I tell you where you can find this code in the book's GitHub repository.

[^fn17]: Seymour Papert. 1980. Mindstorms: children, computers, and powerful ideas. Basic Books, Inc., USA.

[^fn18]: Jeannette M. Wing. 2006. Computational thinking. Commun. ACM 49, 3 (March 2006), 33--35. https://doi.org/10.1145/1118178.1118215

[^fn19]: Alfred V. Aho. 2011. Ubiquity symposium: Computation and Computational Thinking. Ubiquity 2011, January, Article 1 (January 2011), 8 pages. https://doi.org/10.1145/1922681.1922682

[^fn20]: Michael Lodi and Simone Martini. 2021. Computational Thinking, Between Papert and Wing. Science & Education 30 (April 28, 2021), 883--908. https://doi.org/10.1007/s11191-021-00202-5