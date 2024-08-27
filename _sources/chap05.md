# Chapter 5: Play Guess-a-number #

You have learned to write client applications for the client-server model, and although we have seen how powerful this can be, we'd like to understand (and be able to code) both sides of this model. To bring in the server side, we are going to focus on networked scripts that allow us to connect a human on one computer with a human on another. In particular, we will demonstrate this capability through the development of a networked game.

Games are useful platforms for learning for two simple reasons: (1) they typically have a well-defined set of legal actions and a clear goal, which constrains the amount of functionality we need implement; and (2) they can be, simultaneously, fun to play and difficult to test.

The class of difficult-to-test games that we'll build are those involving some aspect of randomness. Learning to write a stochastic program allows us to solve an amazingly wide range of problems. Debugging and testing a script with nondeterminism, however, can be a challenge, and our network game example will teach you some ways to deal with that challenge.[^fn1]

Since we've learned to start with an easy instance of our problem-to-be-solved, we are going to build toward a networked game. We'll first build a simple game involving randomness that runs on our computers and requires only a single person to play. We will then add a networking component. Are you ready to build something educational and fun?

```{admonition} Learning Outcomes
This chapter moves away from our work with children's books and focuses on building children's games. Games typically have a well-defined set of legal actions and a clear goal, which simplifies the first two steps in our problem solving process (i.e., the need to precisely specify the problem and imagine specific instances of it). This allows you to spend more time on the last six steps, and using the knowledge you've gained in the previous chapters, you are now ready to practice these steps on your own. You may find it hard to apply what you've learned and you'll undoubtedly make mistakes, but with practice will come competency.

To successfully solve a complex problem, you need to learn to scaffold. This chapter begins with the design of a simple, single-person game that you can run on your machine, and it ends with a networked version, where you build both the client and server. After this journey, you will be able to:

*   Employ randomness in game design and test such unpredictable programs [design and programming skills];
*   Think about the turn and game-loop structure of game scripts [design];
*   Describe the purpose of type conversion and know when to use it [CS concepts and programming skills];
*   Understand the importance of input checking and failing gracefully, and recognize a couple of different error-handling patterns, including Python's try-except-statement [design and programming skills];
*   Split a non-networked script into a portion run on a client and the rest on a server [design];
*   Draw sequence diagrams and use them to map out the sequence of network messages that will allow the client and server to accomplish a task [design];
*   Evaluate the pros and cons of using libraries with similar functionality [design and programming skills];
*   Explain the purpose of sockets in network programming and how to use them to create networked applications [CS concepts and programming skills];
*   Write and run a client and server that can communicate by interprocess communication using sockets [design, CS concepts, and programming skills];
*   Understand what it means to talk about a hostname, software ports, IP addresses, and specialized addresses like the loopback address [CS concepts];
*   Discuss the purpose of a shim library [design and CS concepts].
```

## Guessing a number

Let's build an extremely simple game involving randomness. This game will have the computer "think" of a number between 1 and 100, and then we will try to guess it. 

The core of this game is generating a secret number, and if we can figure out how to do that in Python, we can probably build the rest of the game based on stuff we've already learned (i.e., using loops and conditionals). Given our recent mentions about randomness, you've probably guessed that having a computer "think" of a number means that it should randomly select one within a given range. As we have experienced lately, the Python library often has exactly the functionality we need, and it is true once again. We simply need to import [the `random` library](https://docs.python.org/3/library/random.html) and invoke its `randint` function, specifying the range we desire.

```{admonition} You Try It
Run the following code block and try changing the parameters to the `randint` function. You should eventually move the `import` and `randint` statements into `guess.py`, where you'll build our game.
```

```{code-block} python
---
lineno-start: 1
---
### chap05/guess.py
import random 

secret = random.randint(1, 100)
print(f'The secret number is {secret}')
```

Now that our computer can generate a secret number, we are ready to begin building the game. We can start *inside-out* or *outside-in*, which means deciding whether you think first about what takes place in a single game turn (i.e., inside-out) or by thinking about the overall structure of the game (i.e., outside-in).  For this particular example, let's begin with what takes place in a single game turn.

```{tip}
It doesn't matter whether you approach a problem inside-out or outside-in. Do whichever makes you most comfortable, or which seems to like the fastest way to get started.
```

In any single turn of this simple game, the player makes a guess and the computer checks this guess against its secret number. We might write down this idea using the following pseudocode:

```{code-block} python
---
lineno-start: 9
---
# Grab the player's guess

# Check guess against the secret
```

Is this all we need to do in a turn? No, this game wouldn't be much fun if we didn't let the player know the result of our comparison. Let's fix that as we convert the pseudocode into Python.

```{admonition} You Try It
Don't peek at the next code block, and try to write Python statements that implement line 11 in the previous code block. Make sure to print what you learn from your checks! You've seen, in previous chapters, all the Python that's required.
```

To push the answer down a bit, I'll take a moment to explain that I've tried to make as many of this chapter's code blocks runnable without errors as possible, but you should be aware of a few things:

1. Not every combination of pseudocode and Python code makes sense to run. For example, I sometimes use a variable name before I've converted the pseudocode that defines that variable name (e.g., in the next code block where it checks `guess` before I've written the code that defines it). Other times, a code block contains only part of a script. The text doesn't call out such cases as I assume you can recognize them.
2. More often, a code block fails because I'm demonstrating a common error (e.g., the need for a type conversion, as I illustrate in our first attempt to grab a player's guess). I explicitly call these instances out in the text.
3. Toward the middle of the chapter we begin writing client and server scripts, and you should know that it doesn't make much sense to run a client script without the server script also running---they need to communicate! Don't attempt to run any of the individual client and server scripts. This chapter's final section titled "Run it!" provides you with instructions on how to run client and server scripts together.

And now a set of Python statements that implement the pseudocode in line 11 above. It doesn't matter which two conditions you check and in what order as long as you handle all three.

```{code-block} python
---
lineno-start: 9
---
# Grab the player's guess

# Check guess against the secret
if guess < secret:
    print('Too small!')
elif guess == secret:
    print('Exactly! You win!')
else:
    print('Too big!')
```

## The player's guess

Good work! Now, how do we grab the player's guess? To this point, we've learned how to take input directly from a user, from a file, from the script's command line, and from a network resource. Which is appropriate for this problem?

Given that we'd like to build an interactive game, the first option is probably the most appropriate, and this means we'll use Python's built-in function `input` to grab something directly from the user. Like this:

```{code-block} python
---
lineno-start: 9
---
# Grab the player's guess
guess = input('Please input your guess: ')

# Check guess against the secret
if guess < secret:
    print('Too small!')
elif guess == secret:
    print('Exactly! You win!')
else:
    print('Too big!')
```

Run the earlier code block that defined `secret` and then the code block above (or run `guess.py` where you've placed these two code blocks). You'll find that the comparison between `guess` and `secret` fails with a `TypeError`. 

If you review [the documentation for `input`](https://docs.python.org/3/library/functions.html#input), you'll read that this function takes everything on the line typed by the player, except for the trailing newline character, and converts it to a string object. This means that our if-statements asked the Python interpreter to compare a string object (`guess`) against an integer object (`secret`). Such a comparison raises a `TypeError` exception because it makes no sense to compare these two things. The Python interpreter has no idea what you want it to do.

No big deal. We simply need to convert something like `'42'` into `42`. The first is a string representation of a number, but the interpreter doesn't know that. It simply knows that `'42'` is a string of characters.

We can convert a string representing a number into an integer using Python's built-in function `int`. We don't need the string returned by `input` for any reason other than passing it to `int`, and so let's pass it directly. The following code block does this, and it executes without an error.

```{code-block} python
---
lineno-start: 9
---
# Grab the player's guess
guess = int(input('Please input your guess: '))

# Check guess against the secret
if guess < secret:
    print('Too small!')
elif guess == secret:
    print('Exactly! You win!')
else:
    print('Too big!')
```

## Type conversion

What we have just done is known as *type conversion*. Type information helps the interpreter to know how it should interpret the bag of stuff you gave it and, if you instructed the interpreter to do something with this bag of stuff, manipulate it. What do I mean by "bag of stuff"? In the next chapter, we'll learn how computers exactly represent an object like an `int` and a `str`, but for now, we can use an analogy. 

My dog and I are both living beings that can be thought of as a "bag of stuff," i.e., collection of biological cells or molecules or atoms). We'll soon learn that `'42'` and `42` are both a collection of *bits*. This is what people mean when they describe the physical world as the world of atoms and the digital world as the world of bits.

While my dog and I are both a bag of cells, interesting things can happen in our world if my bag of cells is labeled with the type `man` and my dog's bag of cells is labeled with the type `dog`. With my type label, I can, for example, enter a restaurant and order some food because the restaurant serves bags of cells with my type. The restaurant, on the other hand, would shoo the bag of cells that is my dog out the door because of its type label.

Why doesn't the Python interpreter allow us to compare a string to an integer despite the fact that both are a bag of bits? The interpreter looks at the type information and uses this information to decide if the two objects are comparable. Two integers are comparable, as we learned in math. We can also compare two strings to determine their alphabetical order. But the interpreter doesn't know what to do if you ask it to compare a string and an integer because no one has defined what it means to compare objects of these different types.

This brings us to the concept of type conversion. Sometimes two bags of stuff have different types, but they represent the same idea. When this is true, we can successfully perform a type conversion. A dog and a man in a dog suit have two different types (`dog` and `man`), but they represent the same idea (dog). The same thing happens with `42` and `'42'`. The string `'42'` is a string in an integer suit. It's not the integer `42`, but we know that it represents the idea of the number 42.

## Try and recover

Of course, not every string value is a string in an integer suit. What happens if the player types `garbage` at our input prompt? Well, `input` will return a string object with the value `'garbage'` and this object will be passed to `int`, which will try unsuccessfully to convert it into an integer value. This unsuccessful attempt will generate a `ValueError` exception and end our script.

Perhaps that's a fine action for a player silly enough to type `garbage` when we asked them for a number, but is that what we want to happen if the player simply mistypes their input? Perhaps I mean to type `42`, but on my way to the `return` key, I mistakenly swipe the apostrophe key and actually input `42'`. Could we make our script a bit more resilient?

This is the purpose of Python's `try-except` compound statement. If we wrap our type conversion inside one of these statements, our script can catch the `ValueError` exception before it terminates our running script. Once caught, we can try and recover from the unsuccessful action. A good way to recover in this particular case is to wrap the try-except-statement in an infinite loop, which continually asks the player for a guess until they submit a guess that is an actual integer. Here then is the final code for our line of pseudocode saying we should grab the player's guess.

```{code-block} python
---
lineno-start: 9
---
# Grab the player's guess
while True:
    try:
        guess = int(input('Please input your guess: '))
        break
    except ValueError:
        print('Guesses must be an integer. Try again...')
```

How does this new statement work? There are two cases to consider:

1. If `int` is successful in converting the result of `input`, the interpreter operates as if the try-except-statement was never there. It makes it to the `break` statement and exits the infinite loop.
2. If `int` is unsuccessful and raises a `ValueError` exception, the exception doesn't terminate our script but redirects the interpreter to the code inside the except-clause, which prints a helpful message. Once this is done, we fall out of the try-except-statement and continues with the next iteration of the while-loop.

Any statement within the try-block that raises an exception listed in the except-block will redirect the interpreter to the except-block. In later problems, we'll change the kind of exception that our except-block catches.

By catching exceptions like this, we save ourselves some difficult coding work[^fn2] and create a program that *fails gracefully*. This phrase means that our script is: (1) resilient to poorly structured inputs; (2) able to give the user more than one attempt at making the input well-structured; and (3) more helpful than a typical Python exception message in telling the user what went wrong.

```{tip}
You _never_ want to trust that the user provided your script with well-structured input. You should _always_ have your script check that the user input is what you expect it to be. This is the first step in writing scripts with strong security guarantees. A try-except-statement wrapped in an infinite loop is just one design pattern that accomplishes this.
```

## The game loop

We started building our game from the inside-out, and now we have to think about the relationship between a single turn of the game to the entire game. Well, the game is not over until the player guesses the secret number and the computer prints: `Exactly! You win!` If we want the player to make multiple guesses, we probably have to wrap our logic for a single turn inside another loop. Let's make it an infinite loop, since we don't know how many attempts the player will need to finally guess the secret number, and `break` out of this infinite loop when the guess is correct. Here is the game all together, with a few debugging `print` statements currently commented out.

```{code-block} python
---
lineno-start: 1
---
### chap5/guess32.py
import random

def main():
    print('## Welcome to GUESS THE NUMBER! ##')
    
    secret = random.randint(1, 100)
    # print(f'DEBUG: The secret number is {secret}')
    
    while True:   # our game loop
        
        # Grab a guess from the player
        while True:
            try:
                guess = int(input('Please input your guess: '))
                break
            except ValueError:
                print('Guesses must be an integer. Try again...')
        # print(f'DEBUG: You guessed {guess}')
        
        # Check guess against the secret
        if guess < secret:
            print('Too small!')
        elif guess == secret:
            print('Exactly! You win!')
            break
        else:
            print('Too big!')

if __name__ == '__main__':
    main()
```

## Testing our proposed solution

We have a solution, but how do we know if it works correctly? As we learned in earlier problems, we could run it with a range of inputs that together test the different sequences of statements (also called *code paths*) in our script. In particular, we'll want to input integers and some garbage to test our grab-a-player's-guess loop, and we'll want to exercise each of the branches of the if-statement that compares the `guess` and the `secret`.

But how, for example, can we input a guess that's bigger than the secret if we don't know the secret? The answer is simple: We remove the randomness while testing. This is the purpose of the print-statements in the script that are currently commented out. Removing the comment character allows us to see the secret and input an appropriate guess to test a selected code path. We may also want to print `guess` to reassure ourselves that the script is working with the value it should have grabbed.

```{admonition} You Try It
Run some tests on `guess32.py`. Make sure you believe it works as expected.
```

## A networked architecture

Now that we understand the logic involved in a single-player game and have a working script for it, we can think about splitting this script's operation across a client script and a server script.

What does that exactly mean? It means that the client and server scripts will work together to generate the exact same user experience as we achieved with `guess32.py`. However, some part of the game will be running not on our machine, but some other machine elsewhere on the Internet.

Typically, we think of the player as sitting at a client, and if that's true, what might make sense to have run elsewhere? How about the generation of the secret number and the comparison of our guess against that secret. When those things are happening directly on our machine, it might be tempting to cheat and dig through our computer's memory looking for the secret. If we put it physically elsewhere, it would make it much harder for us to cheat.

Let's consider each statement in `guess32.py` and decide where to place them: in the client (C) script; or in the server (S) script. Extracting the non-comment statements in the `main` function, I've begun this exercise by placing an `S` in front of the line numbers corresponding to the generation of the secret (line 3) and the checking of the guess against the secret (lines 14-20). Any statements involving or dependent upon the secret should execute on the server.

```{code-block} python
   1  print('## Welcome to GUESS THE NUMBER! ##')
   2
S  3  secret = random.randint(1, 100)
   4
   5  while True:   # our game loop
   6
   7      while True:
   8          try:
   9              guess = int(input('Please input your guess: '))
  10              break
  11          except ValueError:
  12              print('Guesses must be an integer. Try again...')
  13
S 14      if guess < secret:
S 15          print('Too small!')
S 16      elif guess == secret:
S 17          print('Exactly! You win!')
  18          break
S 19      else:
S 20          print('Too big!')
```

I left an `S` off line 18 because that's a piece of functionality we want to make sure that we run on the client. The `break` effectively says that the game is over, and when this is true, we want the client script to exit.

Let's look at the rest of the unmarked statements. Should any of these run *on the server* for added security or for some other reason? It doesn't seem so. 

Ok, so let's flip our question around and ask, "Which of the remaining statements must run *on the client*?" It seems like it would be easiest if the while-loop that grabs and validates the player's guess ran where the player sits. This means we'd mark lines 7-12 with a `C`. 

```{code-block} python
   1  print('## Welcome to GUESS THE NUMBER! ##')
   2
S  3  secret = random.randint(1, 100)
   4
   5  while True:   # our game loop
   6      
C  7      while True:
C  8          try:
C  9              guess = int(input('Please input your guess: '))
C 10              break
C 11          except ValueError:
C 12              print('Guesses must be an integer. Try again...')
  13
S 14      if guess < secret:
S 15          print('Too small!')
S 16      elif guess == secret:
S 17          print('Exactly! You win!')
  18          break
S 19      else:
S 20          print('Too big!')
```

Wow, that's most of our statements. We're left with the `print` on line 1 and the `while` on line 5. It doesn't seem like the server needs to be involved in the initial print statement (line 1). Servers react to clients, as we saw in the last chapter, and line 1 is setup to the actual game play. 

So what about the loop that begins on line 5 (i.e., the game loop)? Let's decide to run that loop on the client. This continues the view that servers react to clients, that they simply manage access to resources. Our resource is the secret number.

Here's our final classification of statements across the client and server:

```{code-block} python
C  1  print('## Welcome to GUESS THE NUMBER! ##')
   2
S  3  secret = random.randint(1, 100)
   4
C  5  while True:   # our game loop
   6      
C  7      while True:
C  8          try:
C  9              guess = int(input('Please input your guess: '))
C 10              break
C 11          except ValueError:
C 12              print('Guesses must be an integer. Try again...')
  13
S 14      if guess < secret:
S 15          print('Too small!')
S 16      elif guess == secret:
S 17          print('Exactly! You win!')
C 18          break
S 19      else:
S 20          print('Too big!')
```

## Its sequence diagram

Now that we have decomposed our game into a client portion and a separate server portion, we next have to determine: (1) the messages that will be sent back and forth; and (2) what Python libraries exist that will help us transmit and receive these messages (e.g., as the Python `requests` library did for our `qweb` examples).

In determining the messages we want sent, it helps to draw what's called a *sequence diagram*. The executions of the client and server are shown in these diagrams as parallel, vertical lines, where time runs from the top to the bottom of the page. An event taking place at time *t1* in the client will generate a message that is sent to the server, and this message is shown as an arrow from the client to the server. Typically, we label these arrows with the content of the message. At a later time *t2*, the server might send a response message to the client. And so forth. Think of it as a conversation between two individuals.

Let's build a sequence diagram for our simple guessing game based on the decomposition of functionality we just described between the client and server programs. In that decomposition, the first message we need to send is one from the client to the server, as illustrated in {numref}`Figure %s<c05_fig1_ref>`. It results from our decision to start the game loop in the client, where we capture the user's first guess. The client, therefore, needs to share this guess with the server, and it sends a message to do so.

```{figure} images/c05_fig1.png
:name: c05_fig1_ref

A sequence diagram for our simple guessing game.
```

The server would compare this guess against its secret number and respond with a message to the client that contains the appropriate one of three answers originally printed by `guess32.py`. Upon receipt of this server message, the client would print the message contents for the player. Notice that this description splits the print statements in `guess32.py` across the two scripts: the server generates what to print and the client does the actual printing.

Finally, unless the player guessed the secret number, the client grabs a new guess from the player, and the message loop begins again.

## When to use a new library

We have a complete design for our networked game, and our next task is to start coding the client and server scripts. In this, we need to use a library that allows us to send messages between separately running applications. Can we use the `requests` library, as we did in our last problem-to-be-solved?

When we previously used the `requests` library, we didn't think much about the network messages that were sent between the machine on which we ran our script and the server to which we were communicating. We simply specified a protocol (i.e., HTTP), the hostname of the server we wanted to contact, a path to a resource on that server (which we had to look up), and some parameters to help the server know what we wanted back. 

It might help if you think of our use of the `requests` library like a phone call to a business's customer support line. This company publishes the number of its support line, and when we call it, we tell the company representative about the product we own and the questions we have. There's a well-defined form to this conversation: ask a question; get a response.

When we choose to write both the client and server scripts, we might like to use a networking library that gives us more freedom in the form of the conversation that takes place. Instead of a call to a company's customer support line, think about a call to a friend where one party might send a variable number of messages before the other party responds. The Python `socket` library gives us such freedom.

An important functional difference between our use of the `requests` library and this new `socket` library is that the latter separates the act of sending a message from the act of receiving one, which provides us with the freedom to have a less-scripted conversation.[^fn3] In fact, this separation of send and receive is exactly what a sequence diagram illustrates. We can draw multiple send lines from the client to the server before any response comes back from the server to the client.

Now, if you look at the structure of the conversation in our particular sequence diagram, you'll notice that we don't need this extra freedom. Should we use the `requests` library for our guess-the-number application? Well, this leads us to another distinction between the two libraries. If we used the `requests` library, we would have to write a server script that can parse HTTP messages, and if you look at what we wrote on the message lines in our sequence diagram, you'll see that our messages are quite simple. We don't need the complexity of HTTP messages. When we use the `socket` library, it makes no assumption about the form of the messages. To the `socket` library, our messages can be treated like short strings. This means that if we learn a little about this new networking library, we can send very simple text messages between our client and our server.

```{tip}
Python, like many programming languages, has lots of libraries with overlapping functionality. In problem solving, it is worth your time to find the right library for your task at hand.
```

## Sockets in action

To understand the `socket` library, it helps to have the right image in your mind. The image we used with the `requests` library was a verbal request. For the `socket` library, you should think of a thing, not an action, and the thing you should have in mind is an endpoint through which you can send and receive information, like a smartphone. As such, we need to create an abstract object in our script corresponding to this endpoint.

We will need one in both our client and server scripts (you can't communicate through your phone to another person unless they have a phone too), but let's start by focusing on the client. The following script grabs the statements from `guess32.py` that we marked as belonging in the client, and it inserts pseudocode corresponding to the messages in our sequence diagram.

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-client-pseudocode.py
import socket

# Addressing information for the server

print('## Welcome to GUESS THE NUMBER! ##')

# Create a socket and call it s
    # Connect s to the server
    
    while True:   # our game loop
        # Grab a guess from the player
        while True:
          try:
              guess = int(input('Please input your guess: '))
              break
          except ValueError:
              print('Guesses must be an integer. Try again...')
        
        # Use s to send guess to server
        # Wait for server to respond on s with answer to comparison
        # Print response
        
        # Is game over?
```

To think about what needs to happen as the script starts, it helps to think about what happens when we communicate using a smartphone. The comment about creating a socket object is equivalent to picking up our smartphone and selecting a communication app on it, like Messages on an iPhone where we can send text messages to another person. If we know how to reach this person (i.e., the addressing information in the pseudocode), then we can use this app (called `s` in the pseudocode) to send and receive messages.

The actual Python syntax we use to create a socket will remind you of the work we did in opening a file. Recall that we opened a file and gave a name to the object returned from the `open` command. We operated with that object for a while, and then we eventually told the system that we were done with it. The with-as-statement nicely bundled the open and close functionality into a single statement and an associated block within which the file was open. We'll do the same with socket objects.

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-client-noabstractions.py (INCOMPLETE)
import socket

# Addressing information for the server

def main():
    print('## Welcome to GUESS THE NUMBER! ##')
    
    # Create a socket and call it s
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        # Connect s to the server
        
        # REST OF SCRIPT
```

Just as we had to tell `open` some configuration parameters (e.g., are we opening a file for reading or writing?), we have to pass `socket.socket` some configuration parameters so that it creates for us a particular `socket` object. We won't dive into the many different types of sockets that are possible. It is sufficient for us to know that the parameters in our partial script will create a socket that uses TCP/IPv4, which is a common protocol used for transporting messages around the Internet.[^fn4] Using different parameters to create another kind of socket object is like choosing to use a different messaging app (e.g., Slack versus Message) in our smartphone analogy.

## Specifying the other party

So far, we've selected the channel that we will use to communicate, and the next step is to specify to whom we want to communicate. A client script does this with a call to `socket.connect`, in which we specify a tuple consisting of a *hostname* (or the IP address corresponding to that hostname) and a *port* on that host.

It probably makes sense to you that we need to specify the name of the entity to which we want to communicate, but what need are we satisfying by specifying a port? Well, networked computers have a number of software ports, and this is like a friend that has multiple phone numbers. You can send a text message to this friend on any of their numbers, but you have to pick one.[^fn5] 

In computer networking, some port numbers have well-defined and widely agreed-upon uses. For example, HTTP uses port 80 to send and receive messages; HTTPS uses port 443; and the older File Transfer Protocol (FTP) uses ports 20 and 21. As another example, there are numerous ways to send email across the network: the Simple Mail Transfer Protocol (SMTP) uses port 25;  the Post Office Protocol 3 (POP3) uses port 110; and the Internet Message Access Protocol (IMAP4) uses port 143.

In the client script we're building, we're going to use a big number[^fn6] for the port, which will place it far away from the port numbers used by the common services on our machine. We would rather not disrupt these important services with our messages.

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-client-noabstractions.py (INCOMPLETE)
import socket

HOST = '127.0.0.1'  # The server's hostname or IP address
PORT = 65432        # The port used by the server

def main():
    print('## Welcome to GUESS THE NUMBER! ##')
    
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((HOST, PORT))
        
        # REST OF SCRIPT
```

Our script specifies the hostname (`HOST`) as an IP address. Every computer connected to the Internet has an IP address, and to get started with our client and server scripts, we're going to use a special one. The IP address `127.0.0.1` is recognized by most networked machines as a way of saying you're talking about the machine on which you're logged in, which in computer parlance is called the *localhost*. Just as each of us has a name, we also know we're talking about ourselves when we say "self."

You'll also hear people talking about `127.0.0.1` as the *loopback address*. The idea is that we send out a message and it loops right back to us. By using this loopback address, we can run our client and server scripts on the same machine.

## Sending and receiving messages

With a socket created and configured, we are ready to start sending and receiving messages. There are numerous different methods for sending and receiving messages in the `socket` library, and we're going to use two fairly simple ones: `socket.sendall` and `socket.recv`.[^fn7]

Roughly speaking, `socket.sendall` takes the string we want to send and transmits it over the network, and `socket.recv` returns a message that was sent to us. But because the `socket` library is closer to the primitives in the machine than the `requests` library, these functions ask us to specify the size of the buffers containing our messages and the character encodings used in these messages. So many details!

Perhaps you're starting to see why the filename I've used in the opening comment of our current script is `guess-client-noabstractions.py`. In directly using the socket library, we have to deal with a lot of networking details that we mostly ignored in our use of the `requests` library. In a moment, we'll start using our own `socket32` library, which is a *shim* between our script and the `socket` library. This shim library simplifies the `socket` API and creates easier-to-use abstractions for simple networking applications like ours.[^fn8]

While the following code block contains the entire "no abstractions" client script, the next two sections discuss two details needed by the `socket` library that that we'll hide behind our `socket32` shim.

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-client-noabstractions.py
import socket

HOST = '127.0.0.1'  # The server's hostname or IP address
PORT = 65432        # The port used by the server

def main():
    print('## Welcome to GUESS THE NUMBER! ##')
    
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((HOST, PORT))
        
        while True:   # our game loop
            # Grab a guess from the player
            while True:
                try:
                    guess = int(input('Please input your guess: '))
                    break
                except ValueError:
                    print('Guesses must be an integer. Try again...')
            
            s.sendall(str(guess).encode('utf-8'))
            response = s.recv(1024).decode('utf-8')
            print(response)
            
            if response == 'Exactly! You win!':
                break

if __name__ == '__main__':
    main()
```

## Size matters

The first helpful abstraction in our `socket32` shim is to ignore message size. In the `socket` library, send and receive methods operate on network buffers, and these buffers are of a fixed size. This means that we need to pay attention to how the sizes of our messages interact with the fixed size of the network buffers.

In general, this is a messy problem that can result in some complicated code. You can read about some of the complicated situations in [the Python documentation titled "Socket Programming HOWTO."](https://docs.python.org/3/howto/sockets.html#socket-howto) This is an important issue for a production piece of code to handle, but we ignore it inside our `socket32` shim since the messages in our current problem-to-be-solved are small in size and neither the client nor the server will send multiple messages before performing a receive.

In particular, our `socket32` shim specifies that we'll use a buffer size of `1024`, which is several times larger than the number of characters in our longest message. You can see this value as the parameter to the `recv` call in the code block above.[^fn9] {numref}`Figure %s<c05_fig2_ref>` illustrates how our `socket32` shim will hide the need to specify the receive buffer size when we write our `guess-client.py` script (coming below).

```{figure} images/c05_fig2.png
:name: c05_fig2_ref

An illustration of how our `socket32.py` shim library slips between our client script and the Python `socket` library to simplify the interface on the networking functions we use.
```

## Encoding again!

The second detail of the `sendall` and `recv` calls that we'll abstract away deals with encoding. By having our client and the server use the same shim, we can guarantee that they speak the same language, or more precisely in the land of computation, they encode their string objects in the same way. We began worrying about this in Chapter 3, when we talked about the many different ways the characters in a file might be encoded.

In some applications, clients and servers are written by different programmers. When this happens, we have to know the character encoding that the server will use and force our strings into that encoding. This is the work being done by the `str.encode` and `str.decode` methods in the code block above. In particular, the script encodes into a Python `bytes` object, which is useful for holding and manipulating *binary data*. Binary data is a topic we'll discuss soon, and for now, just think of this encoding work as transforming our string message into an agreed-upon encoding.

## A simplified networking interface

At this point, you shouldn't worry about understanding all the reasons why `sendall` and `recv` work the way that they do. Taking advantage of our shim library (in `socket32.py`), we can write a simpler client (`guess-client.py`). Carefully compare each statement in `guess-client.py` against its equivalent in `guess-client-noabstractions.py` and you'll see what I mean by hiding details.

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-client.py
from socket32 import create_new_socket

HOST = '127.0.0.1'  # The server's hostname or IP address
PORT = 65432        # The port used by the server

def main():
    print('## Welcome to GUESS THE NUMBER! ##')
    
    with create_new_socket() as s:
        s.connect(HOST, PORT)
        
        while True:   # our game loop
            # Grab a guess from the player
            while True:
                try:
                    guess = int(input('Please input your guess: '))
                    break
                except ValueError:
                    print('Guesses must be an integer. Try again...')
            
            s.sendall(str(guess))
            response = s.recv()
            print(response)
            
            if response == 'Exactly! You win!':
                break

if __name__ == '__main__':
    main()
```

Given the simplified networking primitives, it is easier to see that we must, on line 22, convert the user's integer guess into a string so that the client can send it in a message to the server. This type conversion is required because we're sending strings, not integers in our messages!

## The server

With our client complete, we can write pseudocode for the server script, taking the code parts of `guess32.py` we marked as belonging in the server. You'll notice that the code block below pre-populates the server script with a call to create the server's socket (line 8) and the address information the client uses to connect to the server (lines 5-6).

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-server-pseudocode.py
import random
from socket32 import create_new_socket

HOST = '127.0.0.1'  # Standard loopback interface address (localhost)
PORT = 65432  # Port to listen on (non-privileged ports are > 1023)

with create_new_socket() as s:
    # Bind socket to address and publish contact info
    
    # Answer incoming connection
    print('Connected by <client>')
    
    # Create a secret for this connection    
    secret = random.randint(1, 100)
    
    # Send and receive messages through the connection
    while True:   # message processing loop
        msg = # recv guess from client
        guess = int(msg)
        
        # Check guess against secret and respond
        if guess < secret:
            # sendall('Too small!')
        elif guess == secret:
            # sendall('Exactly! You win!')
        else:
            # sendall('Too big!')
    
    # If we get here, client broke connection
```

## A connection

Inside the body of the with-statement, you'll notice that this script doesn't just create a socket. It introduces the idea of a *connection*. To understand this, we need to move away from our image of two people texting and think about a phone call. In a phone call, there's an open line between the two parties for the entire length of the call. Even when both parties are silent (i.e., no one is sending a message), the phone line keeps the two parties connected. This is how a server works with messages sent to its sockets.

Let's see how we create such a connection. The next code block turns the pseudocode on line 9 of the previous code block into Python. It takes the socket the server created and manipulates it so that a client can message the server on this socket. In particular, the server script must `bind` the socket object to a host and port (line 11 below). This is equivalent to associating a cell phone (i.e., the socket) with a particular phone number (i.e., the host and port).

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-server.py (INCOMPLETE)
import random
from socket32 import create_new_socket

HOST = '127.0.0.1'  # Standard loopback interface address (localhost)
PORT = 65432  # Port to listen on (non-privileged ports are > 1023)

def main():
    with create_new_socket() as s:
        # Bind socket to address and publish contact info
        s.bind(HOST, PORT)
        s.listen()
        print("GUESS-THE-NUMBER server started. Listening on", (HOST, PORT))
        
        # REST OF SCRIPT
```

To receive phone calls on that phone, we also have to publish the phone number, which lets people know we'll answer it if they call it. This is the purpose of `listen` method (line 12 above), which we will always group with the call to `bind`.

## Picking up a call

With that configuration complete, the server can now await a `socket.connect` call from a client. The server does this with a call to `socket.accept` (line 16), which blocks the server script until the operating system notifies it that some client has asked for a connection. When this happens, the `accept` method returns a tuple consisting of a new `socket` object (`conn2client`) and an address tuple. This address tuple contains the client's address information, which is in the form of host and port. The address information is like caller-id on your phone!

```{code-block} python
---
lineno-start: 6
---
### chap05/guess-server.py (INCOMPLETE)

def main():
    with create_new_socket() as s:
        # Bind socket to address and publish contact info
        s.bind(HOST, PORT)
        s.listen()
        print("GUESS-THE-NUMBER server started. Listening on", (HOST, PORT))
        
        # Answer incoming connection
        conn2client, addr = s.accept()
        print('Connected by', addr)
        
        # REST OF SCRIPT
```

## The conversation

The returned socket (`conn2client`) is the one that the server will use to communicate with the client. By creating a new socket for each accepted connection, the server can serve multiple clients simultaneously. The original socket (i.e., `s` in our script) is how all clients reach the server to request a connection. When the server accepts a client's connection, it converses with it through a separate socket (our `conn2client`), which is dedicated to that client's conversation. This separation of tasks is a very powerful problem-solving technique!

In our completed server script, we'll operate on this new socket inside a with-statement (line 19) so that it is properly closed when the server is done with it.[^fn10] Inside this with-statement's body, we generate a secret for this client connection and slip into an infinite loop, in which the server waits for the client to send a message containing a guess.[^fn11] Once received, the server compares the guess against its secret. Depending on the result of this comparison, the server sends an appropriate response using `conn2client.sendall`. It then awaits the next guess.

```{code-block} python
---
lineno-start: 1
---
### chap05/guess-server.py
import random
from socket32 import create_new_socket

HOST = '127.0.0.1'  # Standard loopback interface address (localhost)
PORT = 65432  # Port to listen on (non-privileged ports are > 1023)

def main():
    with create_new_socket() as s:
        # Bind socket to address and publish contact info
        s.bind(HOST, PORT)
        s.listen()
        print("GUESS-THE-NUMBER server started. Listening on", (HOST, PORT))
        
        # Answer incoming connection
        conn2client, addr = s.accept()
        print('Connected by', addr)
        
        with conn2client:
            # Create a secret for this connection    
            secret = random.randint(1, 100)
            
            while True:   # message processing loop
                msg = conn2client.recv()
                if msg == '':
                    break
                guess = int(msg)
                
                # Check guess against secret and respond
                if guess < secret:
                    conn2client.sendall('Too small!')
                elif guess == secret:
                    conn2client.sendall('Exactly! You win!')
                else:
                    conn2client.sendall('Too big!')
            
            print('Disconnected')

if __name__ == '__main__':
    main()
```

When is the server done with the `conn2client` socket? You might think that the server can sever the connection once the client guesses the secret number (i.e., at line 33), but that would lead to a networking problem.[^fn12] Stay true to the server-managing-a-resource imagery and have the server wait until the client disconnects, as I describe next.

## Programmer beware

The steps we took to establish a connection between the client and server can fail in many ways, and if we were writing production scripts, we'd want to write them to catch and gracefully handle all these errors. When we bypass this important work and assume everything proceeds smoothly, our server script needs to handle only one networking condition: client disconnects.

The if-statement on line 25 is the code that recognizes when the client disconnects. It doesn't look like it does this, but here's how it works: This if-statement checks the value of `guess` from the `recv` method call, and if this value is the empty string, it breaks us out of the infinite loop. It doesn't matter whether the client finished its work (i.e., terminated cleanly) or it terminated prematurely. The networking protocol running below the socket library's API knows when the client breaks the connection and it alerts the server's operating system (OS). The OS then unblocks the server's `recv` call, and since there was no client message, the returned result of the `recv` is the empty string. Our script sees this empty string and knows that the client has terminated.

You might feel that there is a lot of networking setup for the server in our simple application. There is definitely more than in the client, but this setup supports the generation of very powerful servers that converse with many clients simultaneously. In fact, you use such servers everyday! As you try your own version of a networked game, your server script should copy lines 9-19 and 23-26 unchanged. You can change the name used for the result of the `conn2client.recv()` call on lines 24-25, but don't change the order of the networking calls or move this cryptic if-statement. Networking is a complicated and tricky subject. Our goal in this chapter and its exercises is understand the basics of network programming and play with the power of it without digging into the complexities.

## Run it!

Are you ready to try running our client and server scripts? It's a bit more complicated than running a single script but not terribly much. If you're running with an IDE, follow either the first or second option below:

* IDE Option 1: The simplest (and my recommended) approach is to run the Python interpreter on the server and client scripts using two different shell windows. Start the server script in one shell *before* the client script in the other. When the client script terminates, the server script should automatically terminate too.
* IDE Option 2: If you are comfortable with a Unix-like command line interface, you can use one shell window and start the server script in background (i.e., by appending an ampersand to the command). Then run the client script. It would look something like following transcript. Line 2 is the *job number* and *process ID* given to the running `guess-server` by the shell; yours will be different. Line 3 is the shell giving us the next prompt mashed together with the status print I included in the server script; the shell and the server are both writing to the terminal. You could type `python3 guess-client.py` right on line 4, but I hit return to get an unobscured shell prompt and then start the client. The client is now running, but so is the server, and both of these processes write something to the terminal.[^fn13] Since our code to grab a player's guess is resilient, I again hit return to get a clean prompt from the client. You can now see why I recommend Option 1, which gives the server and client their own terminal windows in which they output text.

```{code-block} none
---
lineno-start: 1
---
chap05$ python3 guess-server.py &
[1] 10440
chap05$ GUESS-THE-NUMBER server started. Listening on ('127.0.0.1', 65432)

chap05$ python3 guess-client.py 
## Welcome to GUESS THE NUMBER! ##
Please input your guess: Connected by ('127.0.0.1', 51049)

Guesses must be an integer. Try again...
Please input your guess:
```

If you're running this chapter's scripts as code blocks in an interactive Python notebook, you'll want to use the magic of the exclamation-point escape and the Unix `nohup` command.[^fn14] Here's what you do:

1. Upload `guess-server.py` and `socket32.py` into your notebook session.
2. Create a code block like the one below and run it. It'll start `guess-server.py` in background. An exclamation point (`!`) in the first column of a code block tells the Python interpreter to pass the rest of the line to the shell.
3. Finally, copy the `main` function from `guess-client.py` into a code block and run it. You've effectively done what I called "IDE Option 2" above but inside an interactive Python notebook. Since separate code blocks don't share where they put their output, you won't see the interleaving mess I described above.

```{code-block} none
!nohup python3 guess-server.py &
```

In each of these approaches, we're not really using the network. Remember that we're running these two scripts with the loopback interface, but we can't easily tell that without looking at the code. Again, abstraction at work!

\[Version 20240827\]

[^fn1]: Using randomly generated numbers is only one type of nondeterminism. Working with networked programs will introduce you to another type, and the techniques in this chapter will help you handle both of these forms.

[^fn2]: Think about what it takes to check if a string is an integer. We need to visit and check each character, verifying that it is a number. Except that we probably don't want leading zeros. And except that we should allow for a leading \`+\` or \`-\` character, which may or may not be there. This is tricky code to write correctly, and why should we bother? Someone already did this work in \`int\`!

[^fn3]: In case you're curious, the \`socket\` library is similar to the networking primitives in most operating systems. The \`requests\` library, in fact, uses the \`socket\` library in its implementation!

[^fn4]: There are many resources available on the Internet where you can learn about the TCP/IPv4 protocol. You won't need to know any details to code and run our scripts.

[^fn5]: Although the text talks about a friend with multiple cell phones, the ports I'm talking about are not the hardware ports that you can find, for example, on the side of your laptop. Software ports are a way to separate the stream of messages coming in on your laptop's network connection into several different logical streams. It's like what you probably do when you separate your real mail from your junk mail.

[^fn6]: You can have at most 65,536 ports, and so our "big number" will be somewhere close to the upper limit.

[^fn7]: Both these methods are blocking calls, e.g., \`socket.sendall\` blocks until the entire message is sent (or an error occurs). For short messages in an application like ours, where the server serves only one client, this is a reasonable thing to do.

[^fn8]: While we use a shim to simplify the interface to a library with more functionality than we need, shim libraries solve numerous software engineering problems. For example, you could use a shim library to keep an application running while the software libraries on which it depends are slowly upgraded. The shim translates between the new and old versions of an API. You can think of this as keeping a plane aloft while you sequentially upgrade each of its systems!

[^fn9]: It's not always possible to know the maximum size of the messages that will be sent nor be able to allocate a buffer as large as your application's largest message size, assuming it is known. As you can imagine in these instances, you'd have to turn a simple receive call into a loop of receive calls.

[^fn10]: There is no as-clause in the with-statement on line 19 because we previously created and named the object whose lifetime ends when the with-block is exited.

[^fn11]: Line 24 of \`guess-server.py\` names the object returned from \`conn2client.recv\` call \`msg\`. This object is a string, and if it is the empty string, the server knows that the client has hung up and it ends the message processing loop. If the message isn't the empty string, the server converts the message, which should be a string in an integer suit in this game, into an integer to compare it against the secret number. In other server scripts, line 27 might be more complicated depending on the message structure.

[^fn12]: If the server breaks the connection before the client, the connection won't be shut down properly and you'll have to wait for the operating system to timeout a bunch of networking resources. No big deal, but it is annoying when you can't immediately run your next test.

[^fn13]: The interleaving of the output that you see might look different since we cannot predict the way a machine will run two independent processes. Our machines are constantly running a small part of one program and then switching to run a small part of another, which makes it look like the machine is running them all at the same time---look up *timesharing* to learn more. To avoid this unpredictability, I carefully constructed the sequence diagram for this game to have the client and server alternate in sending messages (i.e., there's never any confusion about which should be talking and which should be listening).

[^fn14]: The nohup command is an infrequently used command that stands for "no hang up." Hang up is a term that Unix-like systems use to indicate that the user has logged out. At logout, all of a user's running programs are sent a hangup (HUP) signal, and typically programs end when they receive it. The nohup command says to ignore the HUP signal. The combination of nohup with a script launched in the background guarantees that the command won't block us from running other code cells.