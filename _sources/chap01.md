# Chapter 1: Read a Children's Book #

I love a good story that opens with action and suspense. The action in this opening chapter will be to *solve our first computational problem* and *write our first Python script*. These two things are linked. The goal of a Python script is to get a computer to act in a certain way, to perform one or more specific computations. And if we correctly do the problem-solving work that sets up the writing of such a script, the computations that result will be a solution to our problem.

The suspense? Well, you won't just read about what it takes to write a script that solves the problem before us, but actively try many different short scripts that may move us closer to solving our problem. For each script, we will pay careful attention to how the computer reacts. I will help you to understand each response, and through this experience, you'll begin to build a mental model of what it takes to write a script that causes the computer to perform as we desire.

This trial-and-error approach is not that much different than what we did as young children as we learned how to communicate with those around us. There was a language that the adults were speaking, but we didn't yet understand it. We wanted to be part of the action, and so we would string together some words from this language and notice how the adults reacted. By paying careful attention to their reactions, we built *a mental model* that mapped our ideas to what statements we needed to say to express those ideas in that language.

## Problem solving, in general

But we want to do more than just develop this mental model and learn to express simple concepts in Python: We want to solve computational problems. To solve a problem, we will begin by imagining and then sketching out *a set of actions* that we think, if followed, would solve the problem before us. This set of actions is the content of the script that we want our computer to follow.

In the theater, scripts are useful when we can read them and follow their stage directions. If a script uses a language or a lot of terms that we do not understand, it is gibberish to us. The same is true for a computer script. We need to express the script in a language and with terms that the computer understands. Python is a language the computer understands, and this is where our mental model of what we can express in Python comes into play. The second part of problem solving with computation, then, is the translation of our imagined set of actions into Python.

If you keep in mind the elements involved in good storytelling, you can use them to become a good problem solver. Just as there are different narrative structures that help writers organize their stories, there are different problem solving strategies that we will use to structure our scripts. Just as storytellers need to decide what narrative details are necessary to include and which can be left out, we too will have to grapple with what details of our problem are pertinent in order to find a solution to it. And just as storytellers take advantage of cultural knowledge to streamline their narrative, we will learn to use prominent features of our computer and its programming language to produce efficient and easily understood scripts.

*Writing* a story or a script, however, is not the end for a storyteller or for us. Stories are meant to be read and scripts performed. Just as storytellers hope to elicit particular reactions from their readers, we hope that our scripts will produce particular results when *executed* by a computer. Unfortunately, the first draft of a story doesn't always elicit the reaction a storyteller hoped it would, and the first versions of our scripts often won't produce our desired outcome. But that's ok! We will figure out what failed, fix it, and begin again.

For every problem we aim to solve throughout this book, we will work through these three basic steps: sketch, translate, execute. {numref}`Figure %s <c01_fig1_ref>` summarizes how these steps take us from a problem to a solution, and it reminds us of what we need or need to know at each step. 

```{figure} images/c01_fig1.png
:name: c01_fig1_ref

The work we will do going from a problem-to-be-solved to a computational solution.
```

```{margin} Terminology
I'll talk about computer scripts as being _executed_ or _run_. Both terms simply mean that the computer performs the actions dictated in the script.
```

## Problem solving, in detail

While {numref}`Figure %s <c01_fig1_ref>` makes it look like problem solving is a *linear* process, our first attempt, like a first draft of a story, might miss its mark. For most of us, including me, problem solving is not a linear process, but an *iterative* one. A script when executed might not produce a correct solution or even run at all, and when this happens, we will need to figure out what went wrong and then restart our problem-solving process at some earlier step.

This need to back up and restart might occur at any step in our problem-solving process. For example, we might start translating our outlined set of actions into Python and then realize that we forgot a case that we need to handle. No problem. This simply means we need to back up and restart at an earlier step in our problem-solving process. At which step we restart depends upon the implications of what we had overlooked. 

Now I don't know about you, but I don't like to redo my work over and over again. We can avoid some of this back-up-and-restart action if we are more thoughtful before we begin sketching a set of computational actions. How thoughtful? Well, we will add an additional *five* steps to the front of the process in {numref}`Figure %s <c01_fig1_ref>`. 

Five steps before we even sketch out some computational actions might sound like a lot, but the thought put into these first five steps is what helps *experienced* problem solvers more quickly find working solutions. The resulting eight-step process is still iterative. No one solves a problem perfectly in their first attempt, and these new initial steps are meant to help us discover blindspots and oversights before we have done a lot of design and coding work. In other words, this early work will save us a lot of wasted work in the last, time-consuming stages of the process.

```{Margin} Warning!
What is the biggest trap into which many beginner programmers fall? They attempt to go directly from a problem description to the writing of a bunch of programming language statements. Learning to program is hard enough without skipping the important steps between the first and the seventh. As you tackle your first problems-to-be-solved, keep in mind that the actual writing of code is work done late in the process of problem solving with computation. It might look like an experienced programmer skips quickly to Step 7, but it is more likely that you are seeing them pull code from prior, similar problems (Step 5). As you practice, you too will begin to see such similarities between problems.
```

Putting this all together, the following is a brief description of each of the eight steps that will help us act like an experienced problem solver:

1. Precisely **specify** the problem you will solve.
2. **Imagine** one or more *specific instances* of the problem, identify the inputs you would use in those instances, and the answers you would like produced in each instance. What you learn in this step might cause you to change the specification you wrote in Step 1.
3. **Decompose** the problem into smaller tasks that together solve the overall problem. This decomposition might highlight *corner cases* you hadn't considered in Steps 1 and 2.
4. **Decide** which tasks identified in Step 3 are amenable to *computational approaches*. This decision might again cause you to change the problem or some aspect of the problem.
5. **Identify** what code you might have already written or that you know exists that might help perform each task you need done. Based on what exists, you might yet again adjust the problem-to-be-solved to minimize the work you need to do yourself.
6. **Sketch** the sequence of actions you want the computer to do in each task. What you discover as you do this work might change your mind about the decisions you've made in the previous steps.
7. **Translate** your sketch into *programming language statements*. If you run into any roadblocks in this translation, you might overcome these roadblocks by changing some prior decision.
8. **Execute** the resulting script to see if it functions correctly and meets all the problem's specifications. If it doesn't, you will need to ask yourself if the failure is an error in your code or a problem with your specifications.

Don't feel you need to memorize these eight steps or fully understand the italicized terms in them. Through repeated practice in the chapters of this book, these steps will soon become second nature to you. And before you know it, you won't just be acting like an experienced problem solver, you will be one.

```{admonition} Learning Outcomes
While solving each chapter's problem, you will gain practice in design, new knowledge of computer science (CS) concepts, and specific skills in Python programming. To help you understand how you will grow in these three areas, I include a sidebar like this one at the start of each chapter, which lists the chapter's learning outcomes by area. By the end of this first chapter, you will be able to:

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

When my children were young, I read to them nightly in the hope that this special time together would help them as they struggled to learn how to read. As we embark on learning to read and write Python, let's see if we can *have our computer to read a children's book to us*.

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

In addition to what is the input to our script, we also have to specify what our script should output. Let's agree that our script output on our computer screen the text we see in {numref}`Figure %s <c01_fig2_ref>`.

```{tip}
These two choices illustrate an approach for which I'll repeatedly advocate: Start with a single and simple instance of your problem-to-be-solved. (For example, we have decided to start with a script that won't read any book in any format, but this one specific book in one particular format.) Then write a script that solves that instance. Only then should you consider how you might expand your script's functionality so that it solves multiple different instances of your problem (e.g., other books and other formats).
```

In summary, the script we will write will take the contents of the file named `CatInTheHat.txt` as input and display its contents on our computer screen, as if it were reading the book from start to finish. As a finishing touch, let's have the script add an extra line to the end of the book where it will say, as I always did for my children, "The end."

## Sketch using computational thinking

Given that this is our first computational problem, we will skip over Steps 3 through 5 of our problem-solving process. These three steps become important as you gain experience. For example, in subsequent chapters, you will start to see problems that you have already solved as pieces of the problem you are currently trying to solve. But since we can't yet plumb such experience, we will jump directly to Step 6 and sketch out the sequence of actions that will get the computer to read our book to us.

Remember that our goal in this step is a sketch. Although our script must eventually be written in a programming language, Step 6 is not about programming, but about *computational thinking*. This step is not programming because we are not trying to say something *in* a programming language like Python. This means that you don't need to know Python or any other programming language to create a sketch. You can use any language you would like to capture the sequence of actions in your sketch.

:::{margin} What is computational thinking?
As a published term, computational thinking was first mentioned by Seymour Papert in his 1980 book titled _Mindstorms: Children, computers, and powerful ideas_. As a mathematician, computer scientist, and educator, Papert was looking for concrete ways to have children better understand abstract concepts, and he felt computers could help children connect with powerful mathematical ideas in a manner beyond their formulae. To Papert, this was thinking with computation.

In a 2006 CACM article, Jeannette Wing argued for a much broader notion of computational thinking. This article provides numerous diverse examples of computational thinking, and it posits that such thinking is an important skill for everyone, not just computer scientists. Wing also emphasized that this type of thinking is more like conceptualizing and problem solving than the narrow act of programming a computer. She debunked the idea that computational thinking is how computers "think." In contrast, she stressed that it is how humans think, and this thinking draws together the many ways we think in other logical and mechanical domains.

Although Wing doesn't provide a single definition for computational thinking in her 2006 article, many in the community have contributed thoughts toward a universally agreed-upon definition. For example, Alfred Aho offered this definition in 2011: "computational thinking [is] the thought processes involved in formulating problems so their solutions can be represented as computational steps and algorithms. An important part of this process is finding appropriate models of computation with which to formulate the problem and derive its solutions."

More recently, the work by Wing, Aho, and others has been synthesized into the following definition: computational thinking is the "thought processes involved in formulating problems and their solutions so that the solutions are represented in a form that can be effectively carried out by an information-processing agent." In their 2021 _Science & Education_ article, Michael Lodi and Simone Martini call this the Aho-Cuny-Snyder-Wing definition, in recognition of the contributions from these four particularly influential individuals.
:::

So what is computational thinking if it is not programming? The margin note presents what some noteworthy people have said about this term, but for our purposes, we mean *the thought that we as humans do to structure a problem and its solution script so that the problem is amenable to being solved by a computational device*. 

This definition begins by emphasizing one of Jeanette Wing's important characteristics about computational thinking: It is "a way humans solve problems" and it is not about "trying to get humans to think like computers." It is *a way* because the definition constrains our creative, problem-solving ability: the problem specification must be amenable to a computational solution and the solution script that we eventually produce must be able to be run on a computer. This constraint is something that Alfred Aho emphasizes in his definition of computational thinking. In other words, computers and computation are the primary tools we will use in producing a solution to our problem. Beyond this constraint, we can use any of the diverse skills we often employ in everyday problem solving, from simple mathematical analyses to complex planning to the understanding of human behavior. You can see this if you read through Jeannette Wing's seminal paper on computational thinking and think about what is involved in each of her many examples.

## Capturing this thinking

Creating a sketch of a script using computational thinking is what we want to do, but you are probably still thinking, "This isn't clear. What exactly am I supposed to do?"

To help, let's return to our imagery of children learning. With time and practice, children are able to quickly turn ideas into coherent, grammatically correct statements, but are perfect clarity and correct grammar the most important issues when first learning to talk? Do you see parents interrupting their young child every time the child is unclear or makes a grammatical mistake? I don't. Instead, I see parents encouraging their children to *put words to their ideas*. The resulting broken English (or whatever your native tongue) isn't always coherent or grammatically correct, but it is often sufficient to communicate the child's thoughts.

When starting out, we have the same goal: *capture in a sketch (i.e., broken English) our ideas about how we might computationally solve the problem*.

Broken English for us is what computer scientists call *pseudocode*. In pseudocode, it is fair game to write down the set of actions you'd like the computer to take in any English phrase that makes sense to you. If you want the computer to open a window on the screen, you can type the phrase: `open a window on the screen`. If you want the computer to add 5 to 6, you can type: `add 5 + 6`. I don't care what you write as long as you --- and hopefully others --- have a strong sense of what you mean. 

## Choose your IDE

So lets's write down some pseudocode, i.e., some ideas about how to instruct a computer to read a book to us. But where should we write these ideas, this pseudocode? We could scribble on a piece of paper, but it would be better if we wrote this pseudocode where it would be easy for us to turn our ideas into actual Python code, which is the next step in our problem-solving process.

Fortunately, there are numerous applications available in which you can write, run, and inspect code. These tools are typically called an *editor*, an *interpreter*, and a *debugger*. The editor is where we will write our pseudocode and then our Python script. The interpreter is what allows us to run the script we have written, and the debugger is a tool that can help us understand the reasons why our script may not have run as we had hoped.

While you can use these as standalone tools, many programmers like to use what is called an *Integrated Development Environment (IDE)*. An IDE packages together all the tools you'll need in a single application. On the website that is a companion to this book \[see P01wp01\], you can find directions that will help you get started with several different, including some free, IDEs. It doesn't matter which of these you choose, but start using one as you work your way through this book.

These IDEs contain many features, but don't let yourself be overwhelmed with their extensive functionality. We will need only a small slice of it. Many professional programmers use these IDEs, which means that you can continue to use your selected tool long after you finish this book. 

## Our generic IDE

In whichever IDE you chose, pay special attention to two bars, three panels, and one button. {numref}`Figure %s <c01_fig3_ref>` illustrates where I locate these items in what this book calls a *generic IDE*. Many IDEs follow this layout. If you use one of the recommended IDEs, the setup instructions describe where you can find each of these things.

```{figure} images/c01_fig3.png
:name: c01_fig3_ref

The bars, panels, and button we need to get started, as illustrated in our generic IDE.
```

Running along the top of our generic IDE is the *title bar*, which is where you will find the name of the *project* you have open. You can think of a project like a folder on your computer. It is where you store all the scripts, other files, and specific configuration information associated with the problem you are trying to solve. You can name your projects anything you like.

The other major bar is the *menu bar*, which often resembles a tray of icons. By selecting one of these icons, you can inspect different aspects of your project. To begin, the menu bar in our generic IDE will contain only two icons, as illustrated in {numref}`Figure %s <c01_fig2_ref>`. The first looks like a small stack of papers, and clicking this icon changes the panel to the right of the menu bar into a *file explorer*. It allows you to manipulate the files in your project. The second looks like a small gear, and clicking it displays *configuration options* for the IDE.

```{tip}
The only two configuration options you must set at this point are the ones that define: (1) the way the editor performs indentation; and (2) the number of spaces in an indentation. Indentation holds meaning in the Python programming language, as we will discuss shortly, and so you will want to set your IDE to use the same indentation settings as I did in creating the scripts that come along with this book. If you do this, you will be able to seamlessly use my scripts. If you don't, you will generate unnecessary headaches for yourself as you edit my scripts and then try to run them. As explained in the setup for each recommended IDE, you want the IDE editor to instantiate indentation as a sequence of spaces (i.e., not tab characters), and set the number of spaces in a single indentation 4.
```

Besides the panel to the immediate right of the menu bar, our generic IDE also contains two other panels: the *code editor* and the *console*.

The code editor acts like a panel in a word processor. When we click our mouse inside this panel, we get a cursor at the location of our click and can start typing. And just like in a word processor, we can use the functionality of the editor to edit, search, and format the text we type.

```{tip}
You might be wondering if you could use your current word processor to write your Python scripts. You could, since a Python script is nothing more than a specially structured text file, but the features we want in our code editor are not the same ones useful in writing an email to grandma, a theatrical script, or a technical paper. For example, we won't need to insert images into our Python scripts, but we will want to include code modules written by others. We won't care if a word in our script is a valid English word, but we will want help knowing what the word means in the context of this script. Finally, we will want a different type of autocomplete (e.g., one that helps us remember the particulars of the syntax in our programming language). Hopefully, you're starting to see the benefit of adopting an editor purpose-built for programming. Like a good word processor, a good code editor will make the process of coding easier.
```

The last panel, and in our generic IDE it is the one to the far right, is what I will call the console. It is where we will interact with our script as it runs. You can think of this panel as the stage where the actor (the Python interpreter on our computer) will perform our script. We will also be able to do other things in this console pane, as we will soon see.

How do we ask the interpreter to run the script that appears in the editor pane? That's right! We click the *Run button*.

```{tip}
If you haven't selected an IDE and followed the directions on the book's website for getting started with that IDE, please do that now. No matter which IDE you choose, bookmark its "getting started" webpage in your browser, since we will refer to it several times in this chapter.
```

## Our first pseudocode

We are now ready to do some computational thinking and sketch out our thoughts in an IDE. If we want our computer to read *The Cat in the Hat* to us, what is the first action it should take? Think about the first action that you would take if you were to read this book to another person. Now write that down in the editor window.

You probably wrote something like the following, where the initial `1` is just the line number displayed by the IDE's editor:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
```

If you wrote something like, "Read the first line" then you have to get used to thinking about *every* step in the process of doing something. Unlike a human, the Python interpreter won't know that to read the first line, you must first open the book. This is important: When writing a computer script, you must tell it every detail of what you want it to do. We will see in a moment what happens when we don't provide the interpreter with specific instructions.

What we just wrote is pseudocode. It is a description of what we want this part of our script to tell the interpreter do. This description isn't Python code, and we want to make sure that the interpreter doesn't try to interpret it as such. 

## Comments

I made sure that the interpreter would ignore my line of pseudocode by placing a hash mark (`#`) at the start of the line. This character holds special meaning in Python. When the interpreter sees it, it knows to ignore everything from the hash mark to the end of the line. Programmers refer to things like this as *comments*. 

Comments are notes to ourselves, or we might use them to leave a note for anyone else reading our script. Let me emphasize that comments are for humans. The interpreter will ignore whatever is in a comment and so it cannot contain any actions that we want the computer to interpret or act out.

Despite the fact that comments don't tell the computer anything, they are important and useful. I will use them for two distinct purposes:

1. To capture our ideas during the problem-solving phase when we are sketching out the actions we would like the computer to take (Step 6).
2. To explain why the Python code we write in Step 7 looks like it does. According to Swaroop in [*A Byte of Python*](https://python.swaroopch.com/), "Code tells you how, comments should tell you why."

## Commands and input parameters

Normally, we would write more pseudocode to sketch out what we want the interpreter to do for us, but let's convert this pseudocode into Python code so that we can start to get a feel for what it is like to command the computer to do things.

Our pseudocode instructs the interpreter to open up our book, just as if we had a physical copy of *The Cat in the Hat*, and so let's write `open('CatInTheHat.txt')` on the line below our pseudocode. This new line is a valid Python statement. What does it command the interpreter to do? To open the file named `CatInTheHat.txt`.

Let's talk about this statement's syntax. The form of this statement is: *command*(*input parameters*). In our example, we are issuing an `open` command, which requires a single input parameter. The parameter tells the interpreter what we want opened. With commands like this, we follow the command with a possibly empty list of input parameters enclosed in parentheses.

How is it that the Python interpreter knows what to do when it encounters an `open` command? There are three answers to this question:

1. The command is built into the Python language;
2. It was added to the language by some other person; or
3. We added it to the language ourselves.

```{margin} Terminology
Think of the term _command_ and _function_ as interchangeable terms, as in every command is performed by some code that we think of as a function.
```

In the case of open, it is a built-in function. We will stick with what is built into the Python language in solving this chapter's problem; we'll see the other two categories of commands in the following chapters.

## Scripts versus execution

So far we have a script with a single statement written in it. This is like a playwright writing one line for an actor. But a theatrical script isn't just written, we also want the actor to perform this line so that we can see how it works. The equivalent of this handoff from playwright to an actor in programming is: Click your IDE's Run button. This instructs the Python interpreter to run your script.

Go ahead and try it with the script we've written so far, which is copied below:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
open('CatInTheHat.txt')
```

```{margin} Be an Active Learner
You will notice that I do not reproduce the result of running this script in the text. This is to encourage you to be an active learner. You should write the code along with me in your own IDE, and then hit the Run button and see what happens. You should also feel free to experiment by changing the code and hitting the Run button again. You can't break anything and so experiment. Be creative!
```

Oops, our code produced a whole lot of red text in the console panel (or just below the code block if you are reading this chapter as an interactive Python notebook). 

## Our first error

Computer scientists call this red text an *error*. I don't want you to think of this as an error, but as *feedback*. The Python interpreter is trying to tell us that it couldn't understand our script in some way. If it helps, think about the interpreter as an actor whose English isn't too great.

What can we learn from the broken English in this error message? `FileNotFoundError` looks like promising information, and the file the interpreter couldn't find was `CatInTheHat.txt`. Do you see this in the error message? The message also says that the interpreter had this trouble when it tried to execute our `open` command on line 2 of our script.

The interpreter can't find the input file because there is nothing but our Python script in the file explorer panel. How can we change this? We could add a new empty file to our project called `CatInTheHat.txt`, but then we'd have to type its contents and I'd rather not type in something that already exists. So where can you find this existing file? Return to the webpage on the companion site that helped you get started with your chosen IDE (or Jupyter notebook application), and follow the directions in the section titled "Loading files into your runtime." 

With that done, click the Run button on our short script again.

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
open('CatInTheHat.txt')
```

Success! I mean, at least no error this time.

If you are following along using an IDE, clicking the Run button caused nothing to appear in the console panel. If you are reading this chapter as an Interactive Python Notebook, you saw something strange printed after the code block. For those using an IDE, you will see this strange output in a moment.

Both the IDE and the Jupyter notebook application are using the same Python interpreter, but we experienced two different behaviors. Let's understand this difference because it will help us to build a mental model of the Python interpreter's operation. 

## The interactive interpreter

The console panel in an IDE isn't just a place where we see the results of the execution of our script, but it is also where we can feed pieces of Python code directly to the *interactive* Python interpreter. This is like asking the actor to read one line of your script so that you can see how it sounds, or in our case, what the line causes the interpreter to do.

Let's feed the two lines of our script to the interactive Python interpreter one at a time.

What happens when we feed our comment line to the interactive interpreter? For those running in an IDE, you should be copying the following line from the editor pane and pasting it into the console panel at the console's prompt. Now hit the return key (with the focus in the console panel). For those running in a Jupyter notebook application, just hit the Run button on the following code block.

```{code-block} python
# Open the book so we can read it
```

Nothing happened! Exactly what the interpreter should do with a comment line.

Now type the `open` line in the console panel, or run the following code block.

```{code-block} python
open('CatInTheHat.txt')
```

What happened this time? Something interesting. It's not red-colored text, and so it is not an error. The interpreter is trying to tell us something different. In technical jargon, the interactive Python interpreter prints for us *the value computed by the statement it executed*. To understand what this means, let's try something simpler.

```{margin} Note
At this point, I stop providing explicit directions for using the interactive Python interpreter in IDEs and Jupyter notebooks.
```

## The interpreter as a calculator

What happens if you write `2 + 3` as the input to the interactive Python interpreter? Before you run it, take a moment and think about what we are asking the interpreter to do.

```{code-block} python
2 + 3
```

Cool, we can use the interpreter like an infix calculator. It takes an arithmetic expression, and it prints what it calculates. More generally, the interpreter will print for us the value it computes, if any, for each statement we ask it to execute.

```{admonition} You Try It
Take a bit of time and play with the interpreter as a calculator. Keep in mind as you play that you are playing to learn. If you try something and it fails, try something slightly different. As you read this book, get into the habit of experimenting and forming theories from what you observe during these experiments.
```

From your experiments, I hope you have learned that the interactive Python interpreter prints the value of the statement we ask it to evaluate. So, what is this value that it printed when we asked it to open a file?

## Python help

Wouldn't it be nice to ask for some help? It would and so let's do that. Type `help(open)` in the interactive interpreter and run it.

```{code-block} python
help(open)
```

The `help` command is another Python built-in function, and it tells us about what we passed to this function. Running the statement, we learn that `open` takes a filename and a whole bunch of other input parameters. These other parameters, however, are *optional parameters*, which are given default values if you do not specify them. That's what the `=` syntax means.

For now, let's look only at `mode` and `encoding`. `mode` defaults to `'r'`, which means that we are going to read the file. Alternatively, we might want to write the file. `encoding` is something we will talk about in greater detail later, but for now you can think of it as telling the interpreter in what language the file is written (i.e., the computer equivalent of asking if it is written in English or French).

Finally, `help` tells us that this built-in function opens a file and returns a *stream*. Again, we will talk about file input and file output (i.e., file I/O) in greater detail later in the book, but think about it. When I open a book, I probably open the cover and put my finger on the first word on the first page. You can think of a stream as my virtual finger telling me where I'll read the next characters in the file.

Returning to the question about the value the interpreter printed, it was cryptically telling us about the position of its virtual finger in the file. Less cryptically, this value is the description of an object that does I/O on a text file whose name is `CatInTheHat.txt`. This object has only opened the file for reading, and it expects the file's content to be encoded in this UTF-8 language.

## Revisiting our first error

What happens if we ask the interactive Python interpreter to open a file that doesn't exist? We saw that this caused an error when we executed our whole script and the text file we wanted wasn't in the list of files. Will this error message look different when the problem occurs in the interactive interpreter? I don't know. Ok, I do, but let's pretend I don't.

Let's misspell the file name and execute that `open` statement. Type and execute `open('CatInTheHut.txt')` in the interactive interpreter.

```{code-block} python
open('CatInTheHut.txt')
```

We got a red error message again, as expected. This time it shouldn't look as scary, as we're already familiar with it. Yup, `FileNotFoundError` as we might have expected. This is a type of `OSError` that `help` told us would be raised upon failure to successfully complete the `open` operation.

The only difference this time is that the `Traceback` looks a bit different. It doesn't tell us where in our script the execution error occurred, but that there was a failure on line 1 of this cryptic file "`<stdin>`". We don't really need to understand this broken English, since this traceback information is only needed when we have an error buried somewhere in a multi-line script.

## Undefined names

Let's get back to our script. After `open`, the interpreter is sitting waiting for us to tell it what we want it to do with the open file (our virtual open book).

Well, how about we read the first line? If Python has a built-in file `open` command, maybe it has another built-in function called `readline`. Let's type `readline()` at the bottom of our script and run it. The empty parentheses at the end of `readline` simply say that we're not passing this command any input parameters.

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

Give your friend a book and then command them to `open('book_name')`, where `book_name` is the name of a book that they're holding. Next, command them to `readline()`.

Did they do what was commanded? They probably opened the book, but it's hit or miss if they read a line from that book. When you say `readline()`, you didn't specify what line you wanted read. What if our script had opened two files? How would the interpreter know which of the open files it should read?

This is an example of what's good and bad about computers. They are very literal. They will do exactly what you ask them to do and nothing more.

If you're lucky, when you tell a computer to do something ambiguous, it won't do anything and tell you that it is confused by your command. That's what the Python interpreter did when I asked it to execute a command name that it didn't know.

If you're unlucky, it will do something random, and you need to determine for yourself if the computer did what you wanted it to do. We will spend a good bit of time talking about how to avoid these situations and what to do when you find yourself in them.

## Naming a computation's result

We can solve this problem of ours by using the result of the `open` command. In particular, we want to tell the interpreter to use that thing it computed during `open` to read the first line in that file's stream. You know, that line under its virtual finger. 

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

Edit the `open` statement as follows, which assigns the result of the `open` command to the name `my_open_book`:

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
my_open_book = open('CatInTheHat.txt')
# Read the first line using that thing you computed
```

A single equal sign (`=`) is the *assignment operator* in Python. We'll talk more about naming and the intricacies of assignment, but for the moment, you can think of this single equal sign as saying, "Whatever `open` produced as a result, name it `my_open_book`."

With a name for our open book, we can now read from it. Append `my_open_book.readline()` to the end of your script, which is proper Python syntax for reading the next line from the open file. I'll explain the syntax of this statement in a bit, i.e., don't fret over the use of the period between `my_open_book` and `readline`.

```{code-block} python
---
lineno-start: 1
---
# Open the book so we can read it
my_open_book = open('CatInTheHat.txt')
# Read the first line using that thing that you computed
my_open_book.readline()
```

Click the Run button. No errors! But whether you see the first line of the book depends upon whether you ran this block of code using the interactive Python interpreter or not. 

## Debugging

It's one thing to have the interpreter report to you that something didn't work, but it can be even more frustrating when it runs to completion and the result isn't what you expect. How do you figure out where and what went wrong?

We will learn that there's no single, simple answer to this important question. The good news is that there are many effective strategies you can employ to determine what went wrong. Because our current script is so small, we'll start with the most straightforward, which you can think of as a brute-force approach. In this approach, we start at the beginning of the script and perform each operation in order using the interactive interpreter. This tool will show us the result of each Python statement and keep track of the prior statements' named results.

```{margin} My Conventions

I'll often add a contextual comment as the first line in a code block and use three hash marks (`###`) to distinguish it from the comments in the script. And for transcripts of interactions with the interactive interpreter like this code block, I'll use the greater-than character (`>`) as the interpreter's prompt. Your interactive interpreter's prompt might be different, and that's ok. Just type the text after this symbol at your prompt, and then hit the return key. The non-highlighted text is the interpreter's response, if any, that you should see.
```

It's perhaps easier to understand what this means if we do this with our script. The following is a transcript of what happened when I fed the highlighted lines, one at a time, to the interactive interpreter:

```{code-block} none
---
emphasize-lines: 2, 3, 5
---
### Run the highlighted statements in the interactive interpreter
> my_open_book = open('CatInTheHat.txt')
> my_open_book
<_io.TextIOWrapper name='CatInTheHat.txt' mode='r' encoding='UTF-8'>
> my_open_book.readline()
'The sun did not shine.\n'
```

You'll see that I typed the name of the result we computed in our `open` command (i.e., `my_open_book`) at the second prompt, and the interpreter printed the value of the object associated with that name (i.e., a `TextIOWrapper` object). The printed details match what we had earlier discussed. 

By typing this name at the interactive interpreter's prompt, we can see what it means for the interactive interpreter to remember named results (i.e., keep the state of our running computation). We can look at (and even modify) this state at any point in the execution of our script. 

Continuing with the transcript above, I then typed the second line of our script and hit return. In response, the interpreter printed the book's first line. Our script will read the first line of our book! And congratulations on successfully completing your first debugging session.

## Showtime

The interactive interpreter is a great way to test little pieces of code. It's like asking the actors in the theater to run through just a small piece of a theatrical script. We expect our actors to jump in and out of character while we run these tests on pieces of the script. When it comes to showtime, however, we want slightly different behavior. Actors should stay on script and do only what's specified.

Showtime for our Python script is when we click our IDE's Run button. The showtime run of our Python script prints nothing in the console panel because *we didn't tell the Python interpreter to print anything*. In particular, the last statement we told the interpreter to execute was to read a line from our file. And that's all it did. If we want the interpreter to print the line it read, we must tell it explicitly to do this. It's like the difference between asking a human actor to read a line in a book while onstage versus reading a line in a book and saying it out loud.

## Printing to the console pane

To have your script print something to the IDE's console panel, you must command the interpreter to `print`. The argument to the `print` command is what we want to print, which in our case is that thing the interpreter read in `readline`.

If you guessed that we had better name this thing we read so that we can refer to it in our `print` command, good for you! You are learning to code! Let's call the value we read `the_line`. The following is what our script looks like after these additions:

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

Do we dare try running our entire script now? Are you ready for showtime? Go ahead and run it. 

Success! We have started to create a digital agent that can read a book to us, or it could be a digital actor that could read a script to an audience.

## Statements, objects, attributes, types, and string literals

We've done a lot, and let's take a moment to focus on terminology and what's happening in what we've written.

We have been writing *statements*, one per line, that the Python interpreter reads. As the interpreter reads a line, it discards anything that is a comment, and if there's something left, it performs the requested computation.

When the Python interpreter executes a computation based on a statement in our script, it works with things called *objects*. As you'll come to learn, almost everything our script manipulates is an object. For now, think of an object as a blob with *attributes*. An object's attributes are defined by its *type*, also called its *class*.

As a real-world analogy of an object, its type, and its attributes, consider my dog Cosmo. Cosmo is an object of type dog. The characteristics of a dog (e.g., fur, tail, and wet nose) are the attributes of any dog object. These attributes will have specific values for any particular dog object (e.g., my dog Cosmo has yellow fur).

In the code block above, the result of the `open` command was an object, which we named `my_open_book`. The result of the `readline` command was also an object, which we named `the_line` and printed to the console. In fact, the second line in {numref}`Figure %s <c01_fig4_ref>` refers to three distinct objects:

1. `'CatInTheHat.txt'` is a *literal* object, which has a fixed value; 
2. `open` without its parentheses and arguments is an object that allows us to do things; and 
3. there is an object produced by the execution of `open` that we have named `my_open_book`. 

```{figure} images/c01_fig4.png
:name: c01_fig4_ref

Identifying what's what in the statements of our simple script.
```

```{margin} Valid Names in Python
You have seen several names for objects and commands in this chapter, and you are probably starting to wonder if there are any rules about what constitutes a valid (i.e., won't cause an error) name in Python. There are, and these are the simple rules:

*  _Valid characters_: A variable name may contain the underscore character (`_`), any upper or lowercase letters (`a-z` or `A-Z`), and the digits `0` through `9`.

*   _Valid starting character_: A variable name may start with any of these characters except the digits `0` through `9`.

*   _Case matters_: `the_line` and `The_line` are two different names.

*   _No spaces_: The space character is never allowed in a variable name. As you've seen, I like to replace spaces with the underscore character. Others prefer what is called CamelCase, where you run the words together and capitalize the start of each (as you might have noticed with the attribute `TextIOWrapper`).

*   _Length_: Python allows names of any length, but as we'll discuss in a later chapter, your names should adhere to the Goldilocks Principle. They should be long enough to express something meaningful to your script's reader, but not so long as to be distracting and hard to type.
```

You may hear people talk about the names of objects, like `the_line` and `open`, as *variables*. The idea behind this term is that a name like `the_line` can refer to one object at some point in the script and then to different object at another point. For instance, we have used the variable `the_line` to grab and print the first line of our book; later we'll use it to grab and print the book's second line. Same name, but its value (i.e., object it names) varies over time.

Python has a few built-in functions, like `type`, that can help you understand what kind of objects you have named in your script. Try out the following, highlighted Python statements in the interactive interpreter:

```{code-block} none
---
emphasize-lines: 2, 4, 6, 7
---
### Run the highlighted statements in the interactive interpreter
> type('CatInTheHat.txt')
<class 'str'>
> type(open)
<class 'builtin_function_or_method'>
> my_open_book = open('CatInTheHat.txt')
> type(my_open_book)
<class '_io.TextIOWrapper'>
```

In this transcript, I first asked the interactive interpreter to tell me the type of `'CatInTheHat.txt'` and it said `str`, which stands for *string*. Strings are what computer scientists call a sequence of characters like: *From this sentence's capital F to its final period is a string.* Filenames on our computers, like `CatInTheHat.txt`, are strings.

You create a particular string value in Python by enclosing your desired characters in either single or double quotes, as I did with `'CatInTheHat.txt'`. We need these delimiting characters so that we can include spaces in our strings. Because Python allows both a matching pair of single or double quote characters as the delimiter, I can easily create the string `"Mike's dog"`.

```{admonition} You Try It
Try typing `type("Mike's dog")` to the interactive interpreter. Don't forget to hit return. Try replacing `"Mike's dog"` with other strings. What happens if you leave off the starting or ending quote? What happens if the delimiter you use is a character in your string?
```

Both `'CatInTheHat.txt'` and `"Mike's dog"` are *string literals*, which means these objects have only one value. They are not variables that can change their value. Their value is literally what you type within the quotes.

If you type `CatInTheHat.txt` without the surrounding quotes, Python will think it should know about a variable called `CatInTheHat`. This is why you need the quotes if you want to specify a string literal and why quotes can't appear in a valid variable name. Don't worry if this is confusing. It will eventually seem second nature to you.

The `type` command reports that the object named `open` is a built-in function. I've already mentioned that I'll use the terms *command* and *function* interchangeably, and to that list I'll add *method*. They all roughly mean: "go do something complex based possibly on some input parameters I provide and return to me the result of your computation."

When we want a function object to do its work, we follow its name with a set of parentheses, as we did with `open`, `print`, and interestingly `readline` in script we wrote. Technically, `readline` is a method because it is a function attribute of the objects of type `_io.TextIOWrapper`.

This is a lot of jargon, but don't worry. We'll continue talking about types, attributes, and this dot syntax in a moment.

## Aliasing

The name `my_open_book` is a variable because it may name the first book I read to my children at night and then later that evening name the second book we read. In a Python script, this might look something like the following:

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
# Open the first book to be read
my_open_book = open('CatInTheHat.txt')
# ... do some work with my_open_book

# Switch to the next book
my_open_book = open('MikeAndHisSteamShovel.txt')
# ... do some more work with my_open_book
```

This situation illustrates that we may use one name for multiple different objects during the lifetime of a script. But we may also have multiple names for the same object. For instance, I can create another name for the object named `my_open_book` by placing this name on the right-hand side of the assignment operator and a new name on the left-hand side.

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
my_open_book = open('CatInTheHat.txt')
the_same_book = my_open_book
```

Let's verify this claim and learn another built-in function along the way. We begin by looking at what the `type` command tells us about `the_same_book`.

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
my_open_book = open('CatInTheHat.txt')
print(type(my_open_book))
the_same_book = my_open_book
print(type(the_same_book))
```

When run, we find that both variables have the same type. We would expect that if both named the same object, since each object has only one type. But `type` does not verify for us that both variables name the same object. We would get the same answer from `type` if the interpreter made a copy of the object named `my_open_book` during the assignment to `the_same_book`. We need a tool in our tool chest that will help us determine if: (1) Python's assignment operator creates a new name for an object; or (2) if it creates a copy of the object and gives the new name to the new copy.

This is the purpose of the built-in `id` function. It tells us the value of an object's identifier, which is an attribute common to every Python object. Object identifiers are unique. No two objects should have the same object identifier. It's like Social Security Numbers (SSNs) in the United States; no two people should have the same SSN. Let's print the object identifiers for both of our variables instead of their types.

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
my_open_book = open('CatInTheHat.txt')
print(id(my_open_book))
the_same_book = my_open_book
print(id(the_same_book))
```

You probably got back two large numbers, and if you look closely, they are identical. Therefore, `my_open_book` and `the_same_book` are two different names for the same object. This is equivalent to the fact that I respond to both Mike and Michael.

Why is this an important fact to know? Execute the following code block:

```{code-block} python
---
lineno-start: 1
---
### RUNNABLE, but NOT part of our script
my_open_book = open('CatInTheHat.txt')
the_same_book = my_open_book
my_open_book.readline()
the_same_book.readline()
```

What happened? My virtual finger moved along in the book no matter which name I used to read a line! This is called *aliasing* in computer science, which makes sense given our common understanding of the term.

Again, I raise this point for two reasons: (1) You should have the right mental model for what's happening in the Python interpreter; and (2) you will be surprised by aliasing at some point in your writing of Python scripts.

## Namespaces

Before completing our script, I want to explain the dot syntax in `my_open_book.readline()`. We saw earlier that `readline` by itself was not a name for a Python object because we received a `NameError` when we tried executing `readline()`. But by typing `my_open_book.readline()`, we successfully called a function. What's going on here?

The computer science term for this is *namespaces*. This concept isn't hard because we use namespaces in our daily lives all the time. For example, when I use the name "Christina" at the office, I mean my administrative assistant. When I think of "Christina" at home, I mean by wife. My office and my home are two different namespaces, and I replace one with the other as I move from home to work and back again.

As another example, when I think of "Jim" while at work, I'm thinking of Jim Waldo, my long-time co-instructor. If someone mentions "Jim" to me at home, I'll look confused. I don't have anyone at home named "Jim". This is what happened with the name `readline`. This name is known only within the namespace of an object of type `_io.TextIOWrapper`.

Python provides a built-in function, called `dir`, that lets you see what names are currently defined in a namespace. Type `dir()` at the interactive interpreter's prompt, and it will show you the names in the current, local namespace. If you ran the code blocks above in your interactive interpreter, you should find our two defined names, `my_open_book` and `the_same_book`. When we did an assignment to these names (via the `=` operator), Python put these names in our local namespace so that later uses of those names worked without error.

In the list names we got back from `dir`, you'll also see a number of special names with the double underscores at the start and end of the identifiers. I don't want to dwell on these special names at this point, but I will direct your attention to the name `__builtins__`. Type `dir(__builtins__)` and you can see that this is where the Python keeps the names for its built-in functions (e.g., `help`, `type`, `dir`, `NameError`, and `FileNotFoundError`).

Of particular note is that the name `readline` is not in the local namespace or the `__builtins__` namespace. What about the namespace associated with the object `my_open_book`? Let's type `dir(my_open_book)` and see. Bingo! There it is! These are the attributes of the object known to us as `my_open_book`.

Putting this all together. When we wrote `my_open_book.readline()`, you can read that as saying, "We expect that in the namespace of the Python object named `my_open_book` there is an attribute called `readline`. And because we followed that attribute with empty parentheses, we believe that this attribute is a function we can invoke without any input parameters. Assuming we're correct, execute this command and please return to me the result of that computation."

Give yourself a high-five. You have learned a lot about writing scripts that a program can interpret and how it accomplishes this.

## Reading more than one line

I don't know about you, but if someone told me that they were going to read to me and then stopped reading after the first line, I'd be unhappy. How do we get our script to read the next line in our book?

We saw earlier that multiple calls to the object representing my virtual finger in the file moves my virtual finger forward. So if we add another copy of the command `my_open_book.readline()` to the end of our script, we might read the next line and move our virtual finger to the third line. And if we want to see the line we just read, we should print again. Since our script is starting to get long, I'll also remove the comments we originally inserted. When your script looks like the following, hit that Run button.

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

You might be annoyed by the blank line that our script inserts between the first two lines of our book. To understand why this happens, we first need to talk about special characters, and specifically, the carriage return character.

What, might you ask, is a carriage return? It is a term that had meaning when people used [old-style typewriters](https://www.youtube.com/watch?v=FkUXn5bOwzk). Around 1:28 into the linked video, the person pushes the silver handle to reset the carriage to the start of the next line. This silver handle is a carriage return. On our computers when we're in a text editor, we simply hit [the button labeled "return" (Apple Mac) or "enter" (IBM PC)](https://en.wikipedia.org/wiki/Enter_key).

The question for us is how do we represent this carriage return in our text documents? The answer is that it becomes a special character sequence in our strings. We can find this sequence at the end of each line we read in using the `readline` method. Again, we can use the interactive interpreter to take a peek behind the scenes at our computation. Remember, in the interactive interpreter, typing an object's name tells the interactive interpreter that we want to see the value of that object. 

```{code-block} none
---
emphasize-lines: 2, 3, 4
---
### Run the highlighted statements in the interactive interpreter
> my_open_book = open('CatInTheHat.txt')
> the_line = my_open_book.readline()
> the_line
'The sun did not shine.\n'
```

The backslash followed by a lowercase n (`\n`) is the printed representation for a carriage return (also called a newline character). You can type `\n` in any string literal to say that you want a carriage return at that point in the string.

Now type `print(the_line)` and hit return in the interactive interpreter. Notice that when we print the value of this object, the interpreter knows that we want an actual return and not a character representation of the carriage return.

```{code-block} none
---
emphasize-lines: 2
---
### Run the highlighted statements in the interactive interpreter
> print(the_line)
The sun did not shine.
 
```

Wait a minute! How many carriage returns occurred in the execution of `print` command that ends the following script? Two! One at the end of "The sun did not shine." and another at the end of the blank line that follows it.

We can verify this by creating a new string object without any carriage return at the end of the string.

```{code-block} none
---
emphasize-lines: 2, 3
---
### Run the highlighted statements in the interactive interpreter
> a_plain_line = 'The sun did not shine.'
> print(a_plain_line)
The sun did not shine.
```

There are a lot of formatting options associated with `print`, and I encourage you to read about them. We won't cover them all. But to answer our mystery, look at `help(print)` and notice the input parameter called `end`. By default, a newline character is appended to the end of anything we print with `print`.

To fix our script, we can specify the `end` parameter we want, which is nothing (i.e., a string with no characters). A string with no characters is just '', which looks like a double quote but is actually two single quotes right next to each other (i.e., we type a single quote to indicate the start of a string, type no characters in the string, and then type another single quote to mark the end of the string).

Here's our updated script, which eliminates the unwanted blank lines in our printed output.

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

## Control-flow statements

We have a script that reads the first two lines of the story, and we could clearly copy the last two statements many more times if we wanted the entire story read. But that creates a script that is tailored to the length of this specific story. What if we wanted to build a digital actor that could read an entire story no matter what its length?

This is the purpose of *control-flow* statements in computer programs. The behavior of our script changes based on the testing of some condition in our code. In this case, the condition we want to test is if we have run out of lines in our story.

What would the pseudocode for such a script look like? Well, it wouldn't duplicate the `readline` and `print` statements, but reuse the original ones. This loop back needs to occur only when there is more story to be read, and so we need to insert some kind of test. A test that would work would be to ask if we just printed the last line of the story. Spend some time making sure you understand the following pseudocode that implements this set of ideas:

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
# Here
the_line = my_open_book.readline()
print(the_line, end='')
# Was that the last line? If yes, quit. If no, go to Here.
```

The pseudocode in the comment at the end of the script illustrates the basic structure of a *condition branch* in most programming languages. With it, the computer tests some condition, and if the condition is true, it executes one block of statements, otherwise it executes a different block of statements.

```{margin} Note
Notwithstanding this insight, I'll continue using my theater analogy.
```

The pseudocode also illustrates that we can add statements that (unconditionally) jump around in our script, both forward (toward statements later in our script) and backward (toward statements earlier in our script). With this new capability, we don't have to just execute a script one statement at a time from the first to the last. Our programs will now look more like musical scores than the script for a play. 

## Unit tests

Before we convert our new pseudocode to actual Python statements, let's ask ourselves if this logic is correct. This is not easy to do. As I designed this logic, I'm already inclined to believe it is correct. That's just human nature.

A good way to avoid this trap is to pick a set of test inputs from different parts of the input space, and then carefully work through our code thinking about what it will do on each input. When we have running code, we can perform these same tests by building actual test inputs (called *unit tests*) and running the code to see what happens, but for now, let's just analyze things by hand.

What describes the space of inputs for our script?

If we think about our script, we know that it reads an input file a line at a time. It might be useful, therefore, to test our pseudocode on input files of different lengths, as measured in lines. Let's try text files of three different lengths: a file with no lines; a file with one line; and a file with more than 1 line.

Starting with a file with no lines in it, what will our script do? It will first open the empty file and then attempt to read a line from this empty file. To continue checking our logic, we need to know what `readline` returns in this situation, and we can use the built-in `help` function to find the answer.

If you type `help(my_open_book)`, Python assumes we want to know what we can do with the object that our variable `my_open_book` names. What a nice feature of the Python help command!

```{code-block} python
### Run in interactive interpreter
help(my_open_book)
```

If you scroll down to where help talks about `readline`, you'll see that `readline` returns an empty string if EOF is hit immediately. EOF stands for *end of file*, and it tells you that there is nothing more to read (or in this case, nothing to read at all).

Is it ok to print an empty string? Let's try it.

```{code-block} python
### Run in interactive interpreter
print('')
```

Yup! No complaints from the interpreter; it happily printed nothing.

Next, we need to check if we can answer the looping question in our pseudocode: was the line we printed the last line in the file? Well, not really. We have stepped beyond the last line of the file. Yet, if we change the condition we test to ask, "Did `open` return EOF?", things should work.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
# Here
the_line = my_open_book.readline()
print(the_line, end='')
# Did open return EOF? If yes, quit. If no, go here.
```

## Be open to change

Take note of this simple change, and think about what we just did. It is an example of making small adjustments to the logic of our originally conceived script so that what we are designing fits better with what's easy in our programming language. It is an example of stepping back to an earlier step in our problem-solving process.

Be open to this. Playwrights, artists, engineers, and other designers do this all the time. They listen to the "backtalk" from their materials or, in the case of playwrights, from their actors. There are typically many ways to accomplish something, and it is often a Good Thing to listen and adjust your design accordingly.

```{admonition} You Try It
Now it is your turn to try checking our logic. Repeat what we just did for the other two input cases. Verify for yourself that our logic seems sound for these cases too.
```

```{margin} Terminology
By HLL, I mean a programming language that is close to something we humans can read and understand. The opposite of high level is the very simple statements that direct the hardware processor in our computers. In Act III, we will talk about how the HLL statements we write are "lowered" into instructions that our computer's processor understands.
```

## No spaghetti code

Let's now convert our pseudocode into actual Python code. This is a bit tricky because computer scientists are quite picky about the kind of control flow you should write in a *high-level language* (HLL) like Python.

Our commented pseudocode contains an unconstrained `goto` statement. What is a `goto`? With our original scripts, the interpreter finished a statement and then moved down a line to interpret the next statement. With a `goto` statement, we can tell the interpreter to jump to any point in our script. To indicate where the interpreter should go, computer scientists *label* a script's statements. Our pseudocode inserted a comment containing the word `Here`. Alternatively, we could have used the line number of a statement and written our comment this way: "Did open return EOF? If yes, quit. If no, goto line 2."

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
the_line = my_open_book.readline()
print(the_line, end='')
# Did open return EOF? If yes, quit. If no, goto line 2.
```

This tiny `goto` might not look dangerous, but its simplicity is what makes it dangerous: it gives you (the programmer) the power to go anywhere in your script by simply changing the line number. Used in an constrained way, this tiny `goto` leads, it has been argued (vehemently), to unmaintainable spaghetti code. You can read about [this historical debate](https://en.wikipedia.org/wiki/Goto) on Wikipedia.

The solution is to hide the `goto` in more constrained, control-flow statements. For our script, we'll use *if-* and *loop-statements*. Together, they help humans to understand and maintain their scripts.

## Testing a condition

We begin by learning how to test a condition in Python, which is an integral part of both if- and loop-statements. You'll remember that our pseudocode contains a condition we want tested: is the result of `readline` equal to the empty string (indicating EOF)? In Python, this is written as: `the_line == ''`. It asks, "Is the value of the object called `the_line` equal to the empty string?"

Notice the special way Python writes "is equal to?", which distinguishes this operation from Python's assignment operator. The double equal (`==`) operator compares the objects to its left and right, and it returns `True` if they are equal and `False` if they are not. `True` and `False` are built-in literals of type `bool`.

The Python if-statement expects an expression like this, and it uses this test result to dynamically determine which group of statements the interpreter should execute next. For example, the following if-statement prints "The End." only when the value of `the_line` is the empty string.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
the_line = my_open_book.readline()
print(the_line, end='')
if the_line == '':
    print("The End.")
# Did open return EOF? If yes, quit. If no, goto line 2.
```

You can put any expression that evaluates to a `bool` between the `if` and the colon (`:`). Only if this expression is `True` will the indented statements after the if-statement be executed. We'll see other comparison operators and other forms of Python's if-statement in later problems.

## Indentation

Indentation, or the whitespace at the start of each line in a script, holds meaning in Python. This shouldn't be a completely strange concept for we use whitespace in our writing to, for example, indicate the start of a new paragraph.

Other programming languages use explicit symbols to group statements together. The difference is more taste than anything, but Python believes that indentation without any extra special symbols makes scripts easier to read and understand.

## Spaces, not tabs

We will talk more about indentation, but know that it matters to the way that the Python interpreter understands your script. If you want to avoid problems, use spaces, not tabs, when indenting your Python statements. A tab is a special character, like the newline character, and you will want to set your IDE to convert tabs into spaces.

Furthermore, in Python, all the statements you want grouped together should be indented *by the same amount*. If they are not, you will have problems.

```{admonition} You Try It
Before reading on, choose which of the following will the script above print to the console when run with CatInTheHat.txt?

1. 	A.	Nothing
2. 	B.	The first line of CatInTheHat.txt
3. 	C.	The first line followed by The End. 
4. 	D.	Just The End. 
5. 	E.	All the lines followed by The End.

```

## Loops and breaks

We are almost done. We only need a Python statement that creates a loop, allowing us to repeat the execution of lines 2-4 until we hit EOF. There are two looping statements in Python, and we will use the first of them, a *while-loop*.

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')
    if the_line == '':
        print("The End.")
```

Like the if-statement, the while-statement tests a condition and only if that condition is `True` will it execute the indented statements. In the code block above, the indented statements include everything on lines 3-6. Line 6 is indented twice because it is part of the statements grouped under the `while` as well as dependent upon the execution of the `if` on line 5.

The test in this while-statement consists of the literal True, and that probably seems strange to you. Because this test can never be `False`, it creates an *infinite loop*. To avoid the interpreter being stuck in this loop forever, we should provide a way for the interpreter to jump out of it; and yet, our script doesn't. Shall we try executing our script anyway? Go ahead and run it.

Running this incorrect script prints an unending stream of lines containing "The End." It did start with the contents of `CatInTheHat.txt`, but the console output went by too fast for you to see it. Our computers are fast.

```{tip}
To stop a script stuck in an infinite loop, click the Run button, which now looks like a Stop button. If you don't have anything that looks like a Stop button, the keyboard combination control+c will end the runaway process on most systems.
```

To eliminate the infinite loop in our script, we'll use a *break-statement*. It is basically a `goto` that directs the interpreter to the first statement after the enclosing loop, which for us is the end of the script.

Where in our script should we add a break-statement? Think about it, and then make sure you can explain why the following is the correct placement:

```{code-block} python
---
lineno-start: 1
---
my_open_book = open('CatInTheHat.txt')
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')
    if the_line == '':
        print("The End.")
        break
```

Our infinite-loop problem is gone, and this script "reads out loud" one of my favorite children's books, *The Cat in the Hat* by Dr. Seuss. Problem solved!

```{margin} More Instructions
Use Project Gutenberg's advanced search option and set "Filetype" to "Plain Text (txt)" and "Language" to English.
```

## Any book

By the way, our script is flexible enough handle any book (i.e., plain text file) by simply changing the literal string in `open`. Download the plain text version of your favorite book from [Project Gutenberg](https://www.gutenberg.org/) and try it.

This works, but it isn't a great solution. We can make this easier if we slightly change our problem specification (and learn a new built-in function). Instead of using a string literal in `open`, our script should ask the user what book they'd like to read, store the response in a variable, and then use that variable as the input parameter to `open`.

To ask the user for input while your script is running, Python provides us with the built-in function `input`. Type `help(input)` at the console prompt to read a bit about this function. It does exactly what we need.

```{code-block} python
---
lineno-start: 1
---
my_book = input('What book would you like to read? ')
my_open_book = open(my_book)
while True:
    the_line = my_open_book.readline()
    print(the_line, end='')
    if the_line == '':
        print("The End.")
        break
```

## This problem, in general

Congratulations on solving your first computational problem! I hope you feel proud of your accomplishment. But you might also be asking, "Was this problem just a vehicle to get started in problem solving with computation, or was it something more?" The answer is something more.

While the context and storyline in which I wrapped this chapter's problem may not seem like something you'd ask your computer to do, the act of opening and accessing the contents of digital files is a skill that you will use over and over again in solving your own problems. You have now started to build that skill.

Like this first problem-to-be-solved, I chose the problems in each chapter of this book for two reasons: (1) each builds upon the knowledge you've gained in previous chapters to teach you new design techniques, CS concepts, and programming skills; and (2) because each problem is an instance of a more generally applicable problem. In other words, my selection of our problems-to-be-solved is not frivolous, but builds in you a competency to attack many similar problems.

\[Version 20240524\]