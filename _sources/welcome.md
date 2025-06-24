# Welcome #

This book tells a story about computing, computational thinking, and computational approaches to problem solving. Computational thinking, paired with today's digital devices and the skill of computer programming that you'll learn in this book, will empower you with a new approach to solving the little problems you encounter in everyday life. 

Do you need to find the prime numbers between 1 and 100, convert a temperature in Fahrenheit to Celsius, or use bisection search to approximate the square root of a number? No? Well, neither do I. The calculator on my smartphone computes square roots just fine, and a quick Google search will find the answers to the other two problems. Yet if you pick up most any other book on learning to code, it will subject you to a lengthy procession of tiny programs that solve basic math challenges. Such programs helped me to learn the syntactic rules of a new programming language, but they were not how I learned to use computation to solve the problems in my life.

There is another way. In this book, you will learn to solve problems with computation by writing programs that grab data from the Internet, modify a digital picture you've taken, and help you understand how Google finds the answers to your questions from all the world's websites *in under a second*. Yes, that's amazing.

Learning to program is a powerful skill when you simultaneously learn how to problem-solve. This book will teach you how to solve the computational problems you encounter in everyday life. And if you feel inspired, it can also help you imagine ways in which we might collectively grapple with the big, thorny problems facing our society. For programming has never really been a solitary activity disconnected from the world. It is a tool of a thriving community full of increasingly diverse individuals building products and services that we each use every day of our lives.

This book will introduce you to this community, which will be an invaluable resource for you when you inevitably have questions. But you'll find that this community can be much more. It can also be a source of promising approaches to your problems and pointers to rich resources on which you can build. And if you decide to use your new literacy and capabilities to tackle problems beyond those in your own backyard, you will find colleagues in this community with whom you can partner.

## What you'll learn

A good story is not only fun to read, but it can expand your view of the world. I have tried hard to avoid making this book a tortuous march through a set of dry, disconnected topics. Every technical concept I cover is taught through a short story involving a problem-to-be-solved. I have then ordered and threaded these individual short stories together with an overarching storyline.

I'll speak about this storyline in a moment, but you are probably thinking, "Wonderful, but what specifically will I gain from reading this collection of stories?" Great question. I hope you learn the following:

1. To solve problems using computers. In general, problem solving involves thinking methodically, expressing your goal without ambiguity, decomposing your specific challenge into manageable subproblems, and solving these subproblems through a variety of proven approaches.
2. To address real-world problems and transform them so that they can be solved with the help of a computer. In general, this involves gaining experience in representing and processing information in digital formats.
3. To understand how computers, networks of computers, and their associated software systems operate and communicate.
4. To apply the power of algorithms, abstraction, decomposition, concurrency, and many other computer science concepts in problem solving.
5. To reason about the limits of computing machines and identify the limitations of specific software solutions. You'll also develop a toolbox of techniques for finding design faults and diagnosing erroneous outputs.
6. To write reasonably robust and efficient code in Python using procedural and object-oriented approaches, and to use and understand Python modules and packages written by others. You will gain the confidence to be a contributing member of the worldwide Python community.

Don't worry if the technical terms in this list are unfamiliar to you. You will soon understand what they mean and how they can help you to become an amazing problem-solver.

I want to emphasize that this book isn't trying to turn you into a computer scientist or a professional software engineer. You can use it to become an individual in these professions, but I've written this book for *anyone* who finds that they need to understand computational thinking and computer programming to accomplish their personal and professional aspirations. If that's you, keep reading!

## The Python programming language

The last of the listed learning goals mentions Python, which is the programming language that you'll learn in this book. It is a popular language, used by millions of people around the world, and it is a great starting point for learning computer programming. Let me try to explain why with as few technical terms as possible.

I chose to base this book on Python because it emphasizes *readability*. I believe that you'll read more code than you'll ever write. This isn't very different from our experiences with our native languages, if you think about your typical day. You will read a lot of code in this book, and this will make you a better writer of code. In fact, when I do ask you to write Python code, I'll encourage you to write in a manner that is easy to read and understand.

Readability is important because code is dense, with lots of things typically happening in each statement. This detail, as we will discuss, is necessary for the computer to understand what it is that you want it to do, but humans don't like too much detail all the time. Just as we sometimes skim dense documents to get the gist of what's going on, we're going to want to develop this same skill in computer programming. Python's emphasis on readability will help get us started toward this important skill of skimming code to understand its gist.

Python is also one of the fastest-growing languages in terms of popularity. What this means for us is that the growing community of Python users is constantly producing Python code that we can use in our projects. In other words, we don't have to write our code using only the primitive commands shipped in the core language. We can quickly begin tackling hard problems because others have written and shared Python code that we will use as building blocks. What do I mean, exactly? Well, think about how hard it would be to build a wood-frame house if you not only had to construct the wood frame for the floors, walls, and roof, but you also had to chop down the trees and slice the logs into boards. Constructing the frame is hard enough; adding the latter work makes the project something only a historical hobbyist would love. Working in a programming language with a growing and vibrant community matters, as we will discuss more in the chapters to come.

## Beyond Python

I want you to notice that the first five learning goals above don't mention Python or any other programming language. This is because we need a programming language to help us to practice thinking in a computational manner. Once you've developed that skill, it's not that difficult to convert your Python scripts (i.e., your programs written in Python) into a different language.

In fact, the new language might not even be another traditional programming language. In the last chapter, I will show you how to take what you've learned about directing a machine in Python and apply this knowledge to create effective prompts in English for directing a generative artificial intelligence (GAI) chatbot to do your bidding.

## Three acts

I have chosen to structure these lessons about computers and computation as a theatrical drama in three acts. It is a drama in the full sense of the word. Each story (i.e., chapter) begins with a problem that we'll take apart and then solve.

But I like the image of a drama for an additional reason. As some of you might already know, the word "drama" comes from the Greek word *dran*, which means to do, to act. And act you will.

I ask that you don't passively read each of the chapters that follow. As explained on the book's companion website, you can (and should) execute and edit every piece of code in this book. Doing so will deepen your understanding of the concepts we cover. It will allow you to answer the "I wonder" and "what if" questions that pop into your mind.

I'm serious. Don't just read. Experiment. Try to break things. Try to make the code in this book do things that interest you.

## Programmers and playwrights

As you work your way through the code examples in this book, I'd like you to think of yourself as a programmer and a playwright. Paul Graham of Y Combinator fame wrote in 2003 that hackers and painters, seemingly different professions, are quite similar in that both groups hone their skills by doing.[^fn1] While I have great respect for Paul, I think programmers and playwrights are a more apt twosome. Why? It has to do with the immediacy of the feedback they receive on what they write. 

Playwrights who test their early drafts with a theatrical troupe receive immediate feedback on the success of what they write. Those playwrights can compare what they had in mind while writing to the results when the scripts are performed. Paying close attention to what works and what doesn't, they can build a masterpiece. 

Likewise, the best computer scripts come not fully formed from a programmer's mind but blossom over time as the programmer tries things and adjusts to feedback, from both the machine and their peers. Programming, like playwriting, is a craft that improves with feedback, practice, and community. 

## The hump

The skills in any worthwhile craft take work to develop and master. By opening this book, you've taken the first step toward developing the craft of solving problems with computation, a skill of growing importance in our modern world.

But I need to be honest with you. You will work hard to develop this skill. Yet, just like learning how to read and write in any language, you'll be amazed when things finally click.

This book will help you to get over the hump we all experience. On one side of this hump, it feels as if you're drinking from a firehose and you have no idea what you're doing. But on the other side, you'll find yourself making connections between concepts, writing parts of your programs where you don't have to look up every piece of syntax, and running scripts that make you feel proud and powerful.

Besides helping you get started with programming, this book will teach you to interpret the immediate, often frustrating feedback you'll get as the computer performs the first versions of your scripts. Knowing what to do when things fail will give you the tools you need to succeed.

## Problem solving

I strongly believe that doing is the best path to learning, but what you're asked to do can be engaging and inspiring, or it might just be dreadfully boring. Alfred Hitchcock said that "drama is life with the dull bits cut out," and cutting out the dull bits is my goal in teaching you the art and skill of computer programming.

This book won't waste your time on a parade of programming language features, a dictionary of computer science concepts, and the small, typically mind-numbing code examples that demonstrate them. You can find ample examples online when you need them. 

Instead, we will spend our time together in each chapter developing solutions to widely encountered problems. For example, how does one grab data from a text file? How do two individuals on different digital devices communicate and cooperate with each other? How can you manipulate a digital image to portray something that never occurred? And can search, which is at the heart of so many tools, work quickly over a large collection of data?

Every computational concept and programming language feature that I cover is taught within the context of a problem-to-be-solved. I'll encourage you to work through each problem, and periodically, I'll give you the chance to practice what you've learned. This work will both deepen your understanding of the concepts covered and develop your skills as someone capable of using computation to solve real-world problems.

## Learning to problem-solve in three acts

Structurally, the book segments the problems into three acts. The acts correspond to the stages that I envision in your evolution from someone just learning to write your own scripts to someone that uses programming and computational thinking as skills in your everyday work.

* **Act I** (the setup) assumes a novice status. It introduces you to the steps involved in problem solving with computers, and it illustrates these steps by asking you to solve eight easy-to-understand challenges. Each challenge highlights skills that you'll use often in working with computers. 
    * The problems in the chapters 1-8 are ordered so that each builds on the previous chapter, introducing you to new computational ideas and concepts.
    * These chapters encourage you to be curious, try things, make lots of mistakes, and ask lots of questions. They also equip you with simple approaches to finding and fixing faults and errors in your scripts.
    * By the end of act I, you will have gained significant practice in translating a rough problem statement into a sequence---a *worklist*---of computational tasks that, when performed, will solve your problem. Worklist processing is the first of several problem-solving-techniques that you'll learn.
    * In terms of computational thinking, this first act focuses on two fundamental ideas: *decomposition*, which is about breaking of a daunting challenge into small, manageable tasks; and *abstraction*, which allows us to build simplified representations of the problem's essential elements. These ideas are two of the five cornerstones in computational thinking.

* **Act II** (the confrontation) is when you graduate from novice to apprentice of computational thinking and problem solving. This act teaches you to tackle real-world problems that are more complex than the easy-to-understand challenges of the first act.
    * Chapters 9-12 cover four problems related to search, a task that is at the heart of many problem-solving approaches. Through these challenges, you'll experience the power of abstraction and move beyond solutions consisting of simple worklists.
    * A key aspect of real-world problems is that we can no longer be satisfied with *any* solution to a problem; we need to create solutions that adhere to specific constraints, such as how long we are willing to wait for a program to run. This act is where you'll learn about a range of problem-solving techniques and the algorithms and data structures behind these techniques. *Algorithm design* and *data representation* are two more cornerstones of computational thinking.

* **Act III** (the resolution) moves from a focus on computational *techniques* to an emphasis on computational *tools*. As you become a skilled practitioner, you'll use many different tools and correspondingly different languages to direct these tools. Common across all computational tools is our desire to write short, unambiguous scripts that instruct these tools to solve the problems that interest us. And what keeps us from this goal? Two things: (1) identifying the right computational tool and (2) understanding why our set of instructions to this tool didn't work as expected.
    * Finding the right computation tool is a matter of understanding and exploiting patterns, as you'll see in chapters 13-18. *Pattern recognition* is the final fundamental idea in computational thinking, and it is at the core of machine learning and generative AI. If you can recognize the patterns in your problem, you can identify a computational tool to exploit them.
    * While the right tool will make a seemingly hard problem simple to solve, all computational tools have limitations. Skilled practitioners know the limitations of their tools and how they can fail to achieve desired results. Throughout this final act, you'll learn to recognize and, when possible, overcome these limitations.

## Layering and exploiting connections

Nowhere in this book will you find a single section on a programming structure like looping or an important computer science concept like abstraction. Instead, the text will regularly return to previously introduced topics, and when it does, I will layer on new nuances and new connections. This layering will help you to grow from novice to skilled practitioner.

This is a problem-solving-first approach to learning concepts in computer science and becoming proficient with the syntax of a programming language like Python. Starting simple and building toward complexity through a carefully ordered range of problem contexts is a proven way to learn and retain information.

Furthermore, experts become experts because they see the similarities between problems, and this ability helps them get quickly started on a new problem. Experts almost never start from a blank page. They start with an approach that solved a similar problem and then adapt it for the differences they see in their new problem. We will do the same thing in coding by often starting from something we have previously written.

## Getting started

The one exception to this never-start-from-a-blank-page approach is the first chapter, where I assume that you have no prior background in coding or computational thinking. It introduces you to the general problem-solving process, gets you started in an environment where you'll practice problem solving with computation, and explains some basic statements in programming and the Python programming language. This is important foundational material that could have comprised a complete chapter, but I wrapped it in a problem-to-be-solved, just like all the other chapters. That's because a problem-solving approach to learning this material works as well for it as for the rest of the book's material.

There is, however, a cost to organizing the introductory chapter with a problem-to-be-solved: it necessarily takes you on a longer journey (i.e., it is nearly twice the length of the average chapter). Don't let this discourage you. Once you've solved your first problem, the rest will fall into place quickly.

This is what awaits you in this book. It is an approach that has engaged students intrinsically motivated to learn programming and those who thought they could never do it. No matter which you are, I'm glad that you picked up this book. You are about to experience the beauty and power of computational thinking to solve real-world problems, especially those that excite you. And as you learn to write your own scripts, I hope you will, like the best playwrights, develop your own voice and your own unique style. Then what you do with this new and wondrous skill is up to you.

[^fn1]: Paul Graham, "Hackers and Painters," May 2003. https://www.paulgraham.com/hp.html