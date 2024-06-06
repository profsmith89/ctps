# Chapter 11: Discover Driving Directions #

We began this act with a search problem that asked us to create a listing of all web pages that mention a particular keyword or phrase. Search was effectively a question of matching. Are there problems that we think of as search that more than just matching against a pattern? Google definitely thinks about search much more broadly. It searches over many different sources of digital information seeking answers to our questions.

```{margin} Google Maps
It didn't take many years of Google's existence before it realized that maps and mapping should an important part of its web services. As a fun look at the history of Google Maps and how middle-aged entrepreneurs working civilized hours launched what has become Maps, you might read [this short Medium article](https://medium.com/@lewgus/the-untold-story-about-the-founding-of-google-maps-e4a5430aec92).
```

In this chapter, we expand our definition of a search problem to include the many processes we use in our daily lives to find solutions to questions for which we have a well-described goal. We will consider several different instances of this, but one of the most recognizable examples of *goal-directed search* is the finding of a path from one point on a map to another. You have almost certainly used this type of software in driving across the country or walking in an unfamiliar city, and how such software works is this chapter's problem-to-be-solved.

## A new approach to programming

In addition to introducing a new problem-solving technique (i.e., goal-directed search), this chapter's problem will require us to move beyond a *procedural approach* to programming. To this point, we decomposed each problem into functional components and then "wired" these components together in a sequence of steps that when executed produced a solution. This approach is not that much different than the recipes we follow when cooking. And with a recipe (i.e., an algorithm), we took its functional components and separately designed them. 

This approach uses the power of abstraction to create a public interface that hides each function's implementation. This is *functional abstraction*, and it allows us to think first about a component's outputs in terms of its inputs, and then later worry about the set of computational steps that would turn specific inputs into the desired outputs. And when the public interface created a useful abstraction, we found that we could reuse this function across our program and hopefully other programs we or others might write.

*Encapsulation* is another benefit of the use of a public interface and hidden implementation. When we hide a function's implementation, we can test, debug, and even change the function's implementation without affecting the sites that call our function. By now, you know that creating a correct program is hard work and that it's practically impossible to write a correct program in one go. But by using functional abstraction and encapsulation, we can build toward a correct program, one piece at at time. And when we miscode a component, we can fix the bugs in that component without causing a domino of changes in the rest of our program.

In sum, we use decomposition and functional abstraction to break complex problems into simpler pieces, and functional abstraction and encapsulation to code each piece separately.

While a procedural approach to programming works well, it has limits. When the objects that our scripts manipulate are ones built into Python (e.g., strings, ints, and lists) or ones provided in some module built by a Python programmer (e.g., network responses and digital images), focusing on the functions in the recipe works well. However, to solve this chapter's problem, we are going to need to think about more than its component tasks. We are going to need to build our own objects, i.e., some of the ingredients needed in our recipe. In other words, we are going to need to build our own *data abstractions*.

But have no fear (said the Cat in the Hat), you already have the tools you need to build data abstractions: decomposition, abstraction, and encapsulation! When applied to data (instead of just procedures), you enter the exciting world of *object-oriented programming (OOP).* Python is an object-oriented programming language, which means that it contains features designed to make data abstraction easy.

```{margin} Building Off Others
The challenge in designing algorithms and data structures is in discovering functional and data abstractions that are simultaneously: (1) easily understood by humans; (2) easily coded in an existing programming language; (3) easily executed by existing computers; and (4) easily mapped to real-world problems. Achieving all four isn't easy, and an important reason to be familiar with classical algorithms and data structures is because they have been shown to exhibit all these features. We avoid this challenge because problem solving with computation is more about selecting which classical algorithms and data structures are problem-appropriate than it is about discovering entirely new algorithms and data structures.
```

By the end of this chapter, you will have gained the ability to create recipes and ingredients, which will allow you to solve an unbelievably wide range of complex, real-world problems. Computer scientists think of this pairing as *algorithms and data structures*. There are many elegant ideas in this space, and through the rest of this act, you'll get a taste of the rich world of classical algorithms and data structures.

```{admonition} Learning Outcomes
Learn to write a script for goal-directed search and to simulate what you cannot analytically solve. You will understand the difference between procedural and object-oriented programming, and you'll build classes for data structures relevant to your problem. You'll learn the power and shortcomings of heuristics in algorithms. In particular, you will be able to:
*   Describe the components of a formal specification for a goal-directed search problem [design and CS concepts];
*   Discuss the parallel nature of goal-directed search and finite state machines [design and CS concepts];
*   Code a random walk across a graph-like data structure with nodes and edges [programming skills];
*   Argue that the random walk you coded will terminate [CS concepts];
*   Simulate real-world phenomenon and discuss what you can learn from these simulations [design and CS concepts];
*   Understand the benefits of object-oriented programming and how it differs from procedural programming [CS concepts];
*   State the differences between a class and an instance of that class [CS concepts];
*   Use classes to build your own data structure and inheritance to avoid duplicating code found in related classes [design and programming skills];
*   Employ Python's magic methods so that the objects you build look like they were built into Python [programming skills];
*   Describe the purpose of Python's `self` convention [programming skills];
*   Discuss the importance of the representational invariant for writing robust object-oriented code [CS concepts];
*   Understand how to build a data structure that represents a map and practice building your own classes [programming skills];
*   Write a script that finds solution to the driving directions problem and specialize it to perform searches with different algorithmic characteristics (i.e., breadth-first and depth-first search) [design and CS concepts];
*   Describe the difference between uninformed and informed searches, and state the power (and danger) of heuristics [CS concepts].
```

## Driving directions, a formal specification

Solving a goal-directed search problem, like the finding of directions from one point on a map to another, is fairly easy if you understand the key components that model any goal-directed search problem. To illustrate that these components describe more than this specific goal-directed search problem, I'll also use these components to formally specify the problem of cooking a delicious meal.

Each goal-directed search problem has *a starting (or initial) state* and *a goal state*. These states can be as simple as "I'm here" (starting state) and "I want to be there" (goal state), or "I have these ingredients available to me" (initial state) and "I want to serve a delicious dinner" (goal state).

Besides these two states, goal-directed search problems have *a set of valid actions* through which we can manipulate *the current state*. In the driving context, I have a set of roads (the valid actions) that move me from one location (the current state) to another. In the cooking context, I have a set of kitchen instruments and things I can do with them (the valid actions) that change the state of the ingredients. 

The many road locations and partially prepared mixtures of ingredients are states, along with our designated start and goal states, that exist in the problem's complete search space. We might be able to enumerate all these states for a particular goal-directed search problem, but in real applications, we don't often try. All that matters is that we can distinguish one unique state from another, which is, once again, an encoding problem that translates a collection of real-world things in a computational form.

*The solution* to a goal-directed search problem is a finite sequence of valid actions that when performed transform the start state into the goal state. If we cannot find such a sequence, there is no solution to the problem (e.g., there are no roads that take you from your starting location to your desired goal location).

## Parallels with finite state machines

Does this model description remind you of a finite state machine (FSM), which we discussed in Chapter 2? It should! We drew a FSM as a model with nodes, which captured their current state information, and directed edges between these nodes, which were the allowable actions that changed the state represented by the node at the edge's tail to that represented by the node at the edge's head. The same is true in a goal-directed search model.

While both models have a single start state, they may differ in their number of goal states. You'll recall that a FSM may have one or more goal states, which together cover the set of valid input strings. In goal-directed search, we typically have a single goal state.

To understand importance of this difference, you need to remember the purpose of each model. A FSM distinguishes valid strings from invalid ones. These strings are input to the FSM, and they drive the sequence of steps taken by it. At the end of the input string, the FSM reports whether the sequence ended in a goal state.

In goal-directed search, the sequence of steps is not being driven by some external input. Instead, it is the job of our script to search for some sequence of steps that takes us from the start state to the goal state. If in this search, our code ever hits the goal state, it stops and reports the sequence of steps it found that moved it from the start state to this goal state. 

This fundamental difference in purpose is one of the reasons why we talk about the set of all states in a FSM and yet say that we're not often concerned with the full enumeration of states in a goal-directed search problem. In a FSM, when the input veers off a known path toward a goal state, our script can raise an error. In goal-directed search, we don't know what sequences of steps lead us to the goal state until we find one, and when we find one, we may be uninterested in finding another. 

## Solutions with particular characteristics

In goal-directed search, there is a lot we don't know. First, we don't know, given a particular set of nodes and edges, if it is possible to get from the start state to the goal state. And when there is a solution, we don't know if there is more than one.

Sometimes, all we care about is finding a solution (or reporting that there isn't one), and any one will do just fine. For example, any road that gets me home is a good road. Other times, we may decide to put some constraints on the search problem so that it yields solutions with particular characteristics. For instance, we may not feel that all roads are equally desirable: If we can get home without taking a toll road, that's what we'd like. A solution with the desired characteristics might not exist, but if one does, our script should return it.

## Let's walk before we drive

Let's stop talking about what a script that implements goal-directed search should do and start building one. The first thing we need for our problem-to-be-solved is a city map. We can build a nice regular one with the abstract data type `CitySqGrid`. We simply have to tell it how big we would like our square grid of city blocks to be.

```{margin} Running the Code
To run the code blocks in this chapter, you will need to upload `maze.py` and `city.py` from the book's GitHub repo into your IDE or interactive Python notebook session.
```

```{code-block} python
---
lineno-start: 1
---
from city import CitySqGrid

not_Boston = CitySqGrid(4)
print(not_Boston)
```

You'll notice that, by default, it assumes we are located at the city center, which is where it has put the starting point for our journey.

Now let's imagine that this city is surrounded by a countryside of open fields, and we'd like to spend our day relaxing in one of these fields and playing with my dog. We don't care which field, we simply want out of the city for the day.

```{margin} ASCII Art
Using an emoji for my dog shows the limitation of using ASCII art, which expects monospaced characters. Emoji don't abide by monospacing. Please excuse the poor spacing caused by the dog face. I find it more fun to look at this emoji than a bland 's' character.
```

The abstract data type `CitySqGrid` has an attribute that we can use to grab the coordinates of the starting location (`CitySqGrid.start`) and another with which we can move my dog around the city map (`CitySqGrid.move`). The form of the direction expected by `CitySqGrid.move` is one of the strings `'north'`, `'south'`, `'east'`, or `'west'`; capitalization doesn't matter for these strings. By the way, in the code block below, we tell `CitySqGrid` that we want our location on the map to look like my dog `Cosmo` rather than the boring `'s'` character.

```{code-block} python
---
lineno-start: 1
---
### chap11/walk.py
from city import CitySqGrid

# Our faithful dog
Cosmo = '\N{DOG FACE}'

nyc = CitySqGrid(4, Cosmo)
print(nyc)

loc = nyc.start
while True:
    if loc not in nyc:
        break

    direction = input('Where to? ')
    loc = nyc.move(loc, direction)
    print(nyc)

print('Enjoy your day outside the city!')
```

When we walk my dog around this virtual city, it doesn't let us walk into a building. Since we don't live in these buildings, we are not allowed in. What this means is that the `CitySqGrid` data type is smart enough to know that we can walk only on the streets and not off them. This is important if we want to successfully build a set of driving directions that won't have our car driving through buildings and fields.

## A random walk

Notice that the script `walk.py` solves one piece of this chapter's problem: it alerts us when we have reached the goal state (i.e., our walk took us out of the city). Now we need to solve the other half of the problem: get the script on its own, without our active help, to search the city for a route to the goal state.

How might we think about a procedure that would get us from the city center to the fields on the outskirts of the city? And for this question, let's assume that we don't know anything about the layout of the city except what we see immediately around us.

If you've spent any time solving mazes as a kid, you probably can think up a number of different techniques that might work. I'm going to suggest we have the script do the easiest thing possible: for each step the script takes, it randomly chooses a direction.

This is easy to code, as we'll do in a moment, but first let's ask: Will a random walk reliably get us out of a city? The answer to this question should be yes. In fact, for any of the techniques you were just imagining, we want the answer to be yes. If not, then there are some maps on which the technique won't work, and that's bad if we assured someone that we had a script that always solves this route-finding problem.

What we're talking about here is the proof of correctness we mentioned as one of the three formal components of an algorithm. Again, this book doesn't teach you how to create a formal proof of correctness, but we can make a stab at the form of the proof for a random walk.

## Will it work?

To argue that a random walk will get from the start state to the goal state if a path exists, we might begin by thinking about every type of city layout imaginable, and then test our approach against that infinite set of layouts. Yes, that sounds hard and is probably impossible to do.

So instead, good theoreticians and problem solvers transform these hard questions into their equivalent inverse. For us, this means we need to answer the question: What would need to be true to not allow a random walk to find the goal? If we can rigorously argue that there is nothing keeping us from finding a solution, then we can confidently state that a random walk will reliably get us out of any city.

The argument is fairly simple. We begin by eliminating the case where there is no path from the start to the goal state. Clearly a random walk would fail in this case; however, no procedure would find a path that doesn't exist. As such, the real question is: Is it possible for a random walk to never find a solution path when one exists? The answer to this question is no. If a solution path exists and our algorithm is capable of generating that path, there is some probability that it will randomly generate this sequence of steps and achieve our goal.

## A short walk, please

The kicker is, how long might we have to wait for a script that implements a random walk to find a sequence of steps that end in the goal state? Hard to say, right?

Let's again transform the question. What structure in a map might make it take an unbounded amount of time to find the solution? The answer might have popped into your mind. A loop!

In reality, we don't even need a loop in the roads to have a situation where our random walk takes forever. The algorithm might repeatedly generate a step west followed by a step east, and then west, then east, west, east, .... Even in a city with a single road that leads from the city center to a field on the outskirts, this possible sequence of steps would cause our algorithm to never return.

## No loops

We can eliminate solutions that take a really large amount of time (and space) to generate by eliminating loops from our search space. 

For example, a solution that heads straight east in our city with a square grid is a solution to our problem. One that heads straight east, but loops around the first block one time before continuing to proceed east, is another solution. However, the first 8 steps of the sequence `'eennwwsseeee'` put us directly where the sequence `'eeee'` also starts (i.e., the start state). If an algorithm has already explored a path that starts at the current state and returns to that state, repeating this path clearly won't advance us toward a solution. This means that we can eliminate loops in our maps and not worry that we will miss finding a solution.

## Only visit new spots

A necessary condition of looping is that we have to visit the same location more than once. If we create a random walk algorithm that is not allowed to visit a location more than once, we can guarantee that the algorithm will either find a (loop-free) solution or get caught in a "dead-end."

A walk ends in a dead-end if the algorithm cannot find a new location to visit from its current location. For example, in our previous example, the first 8 steps of the sequence (i.e., `'eennwwss'`) leaves us without a valid location to visit next that hasn't already been visited.

If you think about it, no run of this algorithm will take more steps than there are locations in the map. It may still take many random walks to find a solution, but no single random walk will take infinite time.

## A dog walk

Let's implement this particular type of random-walk algorithm, and since my dog Cosmo is much better at a random walk than I am, we will simulate him wandering around the city. His nose will tell him if he is about to repeat a location he's already visited, which will guarantee that our algorithm doesn't fall into any wasteful loops.

```{margin} Credit
This code was inspired by Program 1.4.4 in Computer Science: An Interdisciplinary Approach by Robert Sedgewick and Kevin Wayne (2017, p. 113).
```

The function `sim` in the code block below implements this behavior. It marks the path Cosmo takes through the input `my_city`, which `sim` displays once Cosmo has hit a dead-end or found himself in a field. In a moment we'll discuss why this function allows us to run multiple random walks one after the other.

```{code-block} python
---
lineno-start: 1
---
### chap11/dogwalk.py
from city import CitySqGrid
import random

# Our faithful dog
Cosmo = '\N{DOG FACE}'

def sim(my_city, trials=1):
    """Runs {trials} trials of a self-avoiding random walk in {my_city}.  It
       returns the number of times the dog hit a dead end."""
    dead_ends = 0
    cg = my_city.grid

    for t in range(trials):
        loc = my_city.start
        row, col = loc
        my_city.reset()

        while loc in my_city:
            # Leave a scent at current loc
            cg[row][col].content = '*'

            # Build list of available moves.  A move is available if there's not
            # a wall in that direction and no scent there.
            moves = []
            if not cg[row][col].northwall and cg[row-1][col].content != '*':
                moves.append('n')
            if not cg[row][col].southwall and cg[row+1][col].content != '*':
                moves.append('s')
            if not cg[row][col].eastwall and cg[row][col+1].content != '*':
                moves.append('e')
            if not cg[row][col].westwall and cg[row][col-1].content != '*':
                moves.append('w')

            if len(moves) == 0:
                dead_ends += 1
                break

            # Move to a random available choice
            m = random.choice(moves)
            if m == 'n':
                row -= 1
            if m == 's':
                row += 1
            if m == 'e':
                col += 1
            if m == 'w':
                col -= 1
            cg[row][col].content = Cosmo
            loc = (row, col)

        print(my_city)

    return dead_ends
```

```{margin} Terminology
It is worth reviewing the terminology I mentioned in Chapter 1. `my_city` is an object, and this object has a set of attributes associated with it. For example, the function `sim` references the `my_city` attributes `grid`, `start`, `reset`, and `print`, where the first two are data attributes (i.e., they simply name an object) and the latter two are function attributes (also called methods that can be called like a function).
```

In reading this routine, you'll notice that we take advantage of the fact that `CitySqGrid` has additional methods and data attributes that we need to use if the script itself is going to decide where to move and do so in an efficient manner (i.e., never make a move that would hit a wall). In particular, we can directly access the city's 2-dimensional grid, and for each grid location, a direction attribute (e.g., `northwall`) tells us if there's a wall in that direction. Finally, we take advantage of the fact that we can store a piece of content in the `content` attribute of a grid location.

Here's a code block that builds a small, regular city and then has Cosmo go for a walk.

```{code-block} python
---
lineno-start: 60
---
### chap11/dogwalk.py
print('\nBuilding a city with a square grid pattern')
not_Boston = CitySqGrid(4, Cosmo)
print(not_Boston)

# Cosmo walks himself
dead_ends = sim(not_Boston)
if dead_ends == 1:
    print(f'Cosmo hit a dead-end.')
else:
    print(f'Cosmo is frolicing in the fields!')
```

Go ahead and run it several times.

## Simulation

What we just did is the core of goal-directed search. A script --- in our case, `sim` --- begins an exploration at the start state (i.e., `loc`, defined in the first line of the *for-t* loop) and then using one of the allowable actions in that state (i.e., a random choice from the `moves` list), it moves forward a step (i.e., the update to `row` or `col` at the end of the *for-t* loop). It then checks the new state against the goal state (i.e., the while-loop check). If we're not at the goal state, it attempts another step of exploration. When we hit the goal state, the function returns the solution (i.e., prints the solution path just before the return statement in `sim`). If we hit a dead-end, the function returns indicating that it failed to find a solution (i.e., `dead_ends` is greater than 0).

What we also did was write a script that simulated an aspect of our world, and from that simulation, we can learn something fundamental about the world. In fact, simulations are an very useful computational tool. The particular type of simulation we built (i.e., [*a self-avoiding random walk*](https://en.wikipedia.org/wiki/Self-avoiding_walk)) is an important one in numerous areas of math, science, and engineering. This type of simulation is used, for example, to model and learn about the topological behavior of proteins; a problem no one knows how to model analytically. 

As an example of how we can use `sim` to learn something interesting, consider this question: How likely are we to find Cosmo wandering around in the fields versus stuck somewhere in the city? The answer to this question depends upon the size of the city, and this is where we use the `trials` input to our `sim` function. By considering the outcome of many random trials, we can get an idea of the likelihood in the limit. Try running the following code block, which uses a small city. 

```{code-block} python
---
lineno-start: 60
---
### chap11/dogwalk.py
print('\nBuilding a city with a square grid pattern')
not_Boston = CitySqGrid(4, Cosmo)
print(not_Boston)

# Run a self-avoiding random walk simulation
trials = 20
dead_ends = sim(not_Boston, trials)
print(f'{100 * dead_ends // trials}% dead ends')
```

```{admonition} You Try It
1.   Run `dogwalk.py` a few times with a `CitySqGrid` of 4, and you probably will get approximately 5-10% of Cosmo's walks ending at a dead-end.
2.   Now double the size of the city and run the simulation several times again. This time you'll probably see 35-40% of the walks end in a dead-end.
3.   Finally, double the city size once more. The likelihood of a walk ending in a dead-end has jumped again, to somewhere around 80-90%.

Our conclusion: In a small city, we should look for him in the fields; in a big city, he's probably still somewhere inside it. This simulation just taught us a fact about our world.
```

## Maps, not city grids

As useful as this simulator script is, it doesn't solve this chapter's problem. For example, we want an object that can model any roadmap, not just regular city grids. In other words, we want a data type that is more general than what we have with `CitySqGrid`. Where can we find such a data type? Well, where did this `CitySqGrid` data type come from?

The answer is that I built it. You won't find `CitySqGrid` in the Python language documentation or in a Python module that some generous soul published. Was it obvious to you at any point in using it that I wrote it? Probably not, and this is the beauty of a well-designed, object-oriented language. We've been using *instances* (e.g., `my_city`) of this entirely new data type, and as long as we understood the interface and behavior of this data type (i.e., its abstraction), we happily created and used instances of it to solve our problem as if the data type was a part of Python.

But now it is time to look at the definition and implementation of `CitySqGrid`, and from what we learn, discover how we can build a Python object that solves our driving directions problem.

The following code block lists the implementation of `CitySqGrid`.

```{code-block} python
---
lineno-start: 1
---
### chap11/city.py
import maze

class CitySqGrid(maze.Maze):
    # Some documentation temporarily omitted

    def __init__(self, size, character='s'):
        # Initializes the instance as a {size}-by-{size} maze object and sets
        # the start-point character to {character}

        # Sanity checks
        if size & 0x1 != 0:
            assert('CitySqGrid size must be an even number')
        if size < 2 or size > 20:
            assert('CitySqGrid size must be between 2 and 20 buildings')

        # Create the string that describes the configuration.  The actual
        # configuration written out is one of the test cases in maze.py.
        row1 = 'f5' * (size - 1) + 'f\n'
        row2 = 'a0' * (size - 1) + 'a\n'
        # Finally, put it all together with no trailing newline
        config = (row1 + row2) * (size - 1) + row1[0:-1]

        # Put start in city center, which is just the point (size,size)
        # when we double size to make maze rows and columns!  We don't
        # care about the goal point in this application.
        endpts = f'({size},{size}) (-1,-1)'
        
        maze.Maze.__init__(self, config, endpts)

        # Fill in the buildings.  Only seen on first build.  maze.reset
        # won't refill these buildings, which is the desired behavior
        # once we start the dog on its random walk.
        for i in range(1, size * 2, 2):
            for j in range(1, size * 2, 2):
                self.grid[i][j].content = '#'

        # Change 's' to a dog
        self.grid[size][size].content = character
```

This code block contains what is called a *class definition*, and classes are how we implement data abstraction in Python. In other words, the Python keyword `class` is how you define and implement your own data type. Here, we have defined a new data type called `CitySqGrid`. We'll talk in a moment about what goes in the parentheses when you define a new data type (i.e., a new `class`); just ignore that for now.

```{margin} Separating Interface from Implementation
I don't fault the designers of Python for not clearly separating interface from implementation. I've programmed in other languages that require a rigid interface specification, and I personally don't think it helps. The syntax of a programming language simply is not a good place to try to explain a data type's abstraction. Python's approach is saying, in effect, that the interface is in the code that follows, but you probably want to read something in English to best understand it.
```

Like a function definition, a class definition defines a data type's interface and makes explicit its implementation. When we learned about functions and their definitions in Chapter 3, it was easy to identify the public interface from the private implementation. The same separation exists with classes, but it is harder to see, especially in Python.

I have described the interface for `CitySqGrid` in a docstring at the start of this class definition. This information appears in the code block below and was replaced with the comment "Some documentation temporarily omitted" in the code block above. In the code block below, I show you where it goes in the actual definition by commenting out the code statements before and after this docstring.

A common convention among Python programmers is to use a docstring to describe (in English) the data type's abstraction and interface, as seen through its *data attributes* and *methods*. You may also find, as I have done, a comment following the docstring that is meant not to help users of the data type, but those who want to modify its implementation. If you just want to use the data type, as we did in `walk.py` and `dogwalk.py`, you need only read the docstring.

Please note that this is the convention I use. You may follow this convention or imagine your own. Just give those who will use your new data structure some way to understand what's in its interface and what are implementation details.

```{code-block} python
---
lineno-start: 1
---
### chap11/city.py
# class CitySqGrid(maze.Maze):
    """Abstraction: A CitySqGrid object represents a grid-like layout of a city
       with an even number of blocks in the north-south direction as it has in
       the east-west direction.  When an instance is created, your character is
       placed at the intersection in the exact center of the city.

       Interface attributes:

       instance.start: Starting location for your character in the your city.
       Locations are (row,col) tuples that specify a coordinate within the city
       grid.  You should only read this data attribute.

       instance.grid: A 2-dimensional array of cells containing information
       about the city grid and whatever you might find at each cell.  Locations
       index the cells in a grid.  See class `Cell` for more information about
       the city cells.

       instance.move(location, direction): Given a location and a direction to
       move, this method moves the contents of the input location to the new
       location in the specified direction, if the direction doesn't hit a wall.
       If direction is a wall, no move is made.  NOTE: It is up to the caller to
       guarantee that there's some character to move at location and that the
       input location is within the city grid.

       instance.reset(): Resets all the city's state to the state when this
       instance was first created.

       instance.__contains__(pt): Allows us to use the Python `in` syntax to
       check if a location `pt` is a location in your city grid.

       instance.__str__(): Converts your city object into a string, which when
       printed is an ASCII representation of what your city looks like.

       instance.print(): Same as `__str__`, except that it does the print too.
    """
    # Implementation details:  It only builds city's where there is an
    # intersection at the exact middle of the city.  This means that the
    # CitySqGrid size must be an even number.  For more implementation details,
    # see class `Maze`, on which we build this class.

    # def __init__(self, size, character='s'):
    # ...
```

## Object-oriented programming

You've now seen the power of *object-oriented (OO) programming*, in which we use data abstractions as well as procedural abstractions to decompose and structure our scripts. With a facility to create data abstractions, we are freed from using just the data types that Python or some Python library provides.

At first, OO programming may feel overwhelming as it comes with a boatload of syntax and jargon, but there's definitely nothing to fear here. You have been using the OO approach from the this book's start. Everything you manipulate in Python is an object, even what feels primitive and simple, like integers and strings.

Even though there's nothing to fear, even I find the OO terminology to be intimidating and confusing. If you have a strong desire to dive into this terminology and gain a picture of the incredibly wide range of things you can express and do, I encourage you to pick up an OO programming textbook. To get started, you might read Chapter 10 in John Guttag's book titled an *Introduction to Computation and Programming Using Python*. We won't cover the waterfront of OO programming and instead learn about its basic aspects as we use it in our problem solutions.

## The magic of magic methods

A particularly useful aspect of OO programming, which you saw in `walk.py` and `dogwalk.py,` is the capacity to create an abstraction barrier around a data structure. As with functions, the goal in data abstraction is to get the user of the abstraction to focus on the whole and it important attributes, not the implementation details. For example, we knew that an instance of the class `CitySqGrid` has an internal structure (e.g., it contained a 2-dimensional grid and a 2-element tuple called `start`), but in writing these scripts, we didn't care how this grid was exactly implemented or updated. Python's class facility helped me to hide these details from you.

But Python's OO mechanisms offered me more than this. It also allowed you to treat an instance of `CitySqGrid` just like Python's own built-in types. To understand this, consider a Python `list`. When we want to check whether some object (let's call it `my_direction`) is a member of a `list` object (let's call it `valid_moves`), we'd write: `my_direction in valid_moves`. If you look back at the while-loop in `dogwalk.py`, you'll see that we were able to write: `loc in my_city`. Instances of `CitySqGrid` work like instances of Python `list` when it comes to Python's built-in functions like `len` and `print`, and its infix operators like `in` and `+`.

The methods of `CitySqGrid` that help this data type connect into the nice syntactic features of Python are called *magic methods*, and the name of these methods begin and end with two underbar (`_`) characters. You see three of these magic methods listed in the docstring for class `CitySqGrid`, and another one in the code inside the `class` definition. Let's look at the first three.

The instance method `__str__` returns a string that describes this particular `CitySqGrid` object. As the data type's designer, I can return any string I like, and I've chosen to return the ASCII image that you've seen in our script's runs. This is a magic method because the interpreter looks for it when the object is used where the interpreter expects a string object. For example, the input parameters to `print` are expected to be strings, and when we pass a `CitySqGrid` object as a parameter to print, the Python interpreter looks for this specific magic method and, if it finds it, it uses it.

```{code-block} python
---
lineno-start: 60
---
### chap11/dogwalk.py
print('\nBuilding a city with a square grid pattern')
not_Boston = CitySqGrid(4, Cosmo)
print(not_Boston)
```

This is how we print a Python `list` by just passing a `list` object to `print`. Use the `dir` command to see what attributes are in the `list` namespace, and you'll find `__str__`. Now you know how we're able to print so many Python objects by just passing them as parameters to `print`!

```{code-block} python
dir(list)
```

The docstring on `CitySqGrid` also tells us that the instance method `__contains__` implements what we need in order to use the Python operator `in` with a `CitySqGrid`. This method requires a parameter, which is a point in the city grid that we're asking about. This point becomes the value to the left of the `in` operator, and the `CitySqGrid` instance we want to check is to the right. In other words, the Python interpreter changes the first code block below into the second one.

```{code-block} python
pt = (1, 1)
pt in not_Boston
```

```{code-block} python
pt = (1, 1)
not_Boston.__contains__(pt)
```

The key idea here is that when we define our own data type, we'd really like to use it like the data types that were built into Python. When this is true, a programmer who uses our new data type won't have to think about who built the data type they need. They just use it like every other data type they've used in the past for the common set of operations in Python (e.g., printing and testing for membership, assuming such a test makes sense in the context of the data type).

```{margin} How the move-method Works
As the docstring states, this method does minimal checking. It basically assumes that the person using the method knows that the location passed to this method is `in` the instance or has been checked by a call to `__contains__`.
```

## Actions specific to our data type

Of course, our data type can have data attributes and methods unlike any other data type in Python. Two examples of this in our `CitySqGrid` example are: (1) the `start` data attribute, which indicates the starting location of our character in the city; and (2) the `move` method, which we used to move from one point in the city grid to another by specifying our starting location and the direction we wish to move.

Notice that the name of these methods aren't surrounded by double underbar characters. There's nothing in Python stopping you from using, for example, the name `__move__` except convention in the Python programming community. Magic methods that hook into some nice Python syntax are named with surrounding double underbars, and the methods you design especially for your data type shouldn't have them. By following this simple convention, experienced Python programmers know what you're doing.

## Building on others

I've focused our discussion on the docstring, which describes the abstraction and public interface to our new data type. Let's now look at the implementation of the `class CitySqGrid`.

Whoa! Wait a minute. We see the class's implementation contains the definition and implementation of only one method, and the name of this method is not one of the ones we read about in the docstring! We'll get to this magic method (i.e., its name begins and ends with a double underbar) in a moment, but let's first understand what `CitySqGrid` really is.

```{margin} Terminology
Along with the term subclass, you will hear the complementary term superclass. `Maze` is the superclass of `CitySqGrid`, and it shares its attributes with all its subclasses. 
```

`CitySqGrid` is a new data type that I built, but I didn't build it from scratch. I built it as *a specialized kind of a more general data type*. This more general data type is what was specified in the parentheses of the class definition. Every instance of `CitySqGrid` is an instance of the data type called `Maze`, which is found in the `maze` module (i.e., in `maze.py`). Object-oriented enthusiasts call `CitySqGrid` a *subclass* of `Maze`. As a subclass, `CitySqGrid` inherits the attributes of `Maze`.

The `maze` class is quite long, and I haven't replicated it here. If you open `maze.py` in the Github repository associated with this book, you'll find implementations of the methods described in the `CitySqGrid` docstring (i.e., `move`, `reset`, `__contains__`, `__str__`, and `print`) and if you look at these implementations, you'll see that they do what the `CitySqGrid` docstring said they'd do.

What does it mean that these methods are in class `Maze` and not in class `CitySqGrid`? It means that nothing I did to specialize a `Maze` object into a `CitySqGrid` object changed how we print or test whether a point exists within one of these objects. Do you remember when we decided we needed only one implementation of the string-replace method back in Chapter 3? We wanted multiple invocations of this code, but we wanted to write that code only once. For all the same reasons we discussed there, object-oriented *inheritance* allows us to avoid repeating implementations in the context of data abstraction.

Speaking of building on others, you will notice that the `Maze` class builds on a class called `object`. This is a class at the root of the Python object hierarchy. Every built-in data type in Python is also an object of type `object`. By saying that `Maze` is derived from `object`, we're saying that we want `Maze` objects to be Python objects.

```{admonition} More Conventions
You will find the definition of methods in the `Maze` class with names that start with a double underscore, but do not end with a double underscore. These are helper functions used in some of the `Maze` methods. Object-oriented programming not only encapsulates the attributes of a data type in a `class` statement, but it allow you to use all of the helpful aspects of procedural programming within this statement.
For example, turning a `Maze` object into its ASCII image involves a row-by-row generation of ASCII characters. It is easier to implement this procedure by factoring out the work done for each row into a method called `__str_row` and then having `__str__` repeatedly call this helper function. However, this is an implementation detail, and we want it hidden from those using our data type and the attributes described in our interface docstring. If we could hide this particular method `__str_row`, we would be free to change its implementation and know that no client of our data type would break.
In Python, this hiding of attributes specific to the current implementation is done with a naming convention. While other programming languages have keyword mechanisms for making certain attributes of a class private to the implementation of the class, the closest we can come to this is to begin a data attribute or method with a double underbar (and no trailing double underbar). If a piece of code accesses such a named attribute at runtime, the Python3 interpreter will raise an `AttributeError` claiming that the object has no attribute with the double-underbar-leading name.
```

## Building an instance

We have talked about what we can do once we have an object of our new data type, but how do we create an instance of this data type? If you think about Python's built-in data types, like integers, strings, and lists, there is special syntax in the language to handle the creation of objects of these types. For example, when we typed a number without a decimal point, the Python interpreter knew that we wanted to create an integer object with that number as its value. For strings, we put the value in a matching pair of quote characters. For lists, we used square brackets.

```{code-block} python
---
lineno-start: 1
---
# Examples of creating an instance of a built-in object. We get to use special
# syntax in each of these cases.
an_int = 0
an_empty_string = ''
an_empty_list = []
print(an_int, an_empty_string, an_empty_list)
```

But we can also build instances of these built-in data types by directly invoking their *constructors*. The form of a constructor is the class name followed by a pair of parentheses. The result of a call to a constructor is an newly minted object of that class (i.e., type).

```{code-block} python
---
lineno-start: 1
---
# The same examples as the last code block, except now we're creating
# instance of built-in object using the type's constructor.
an_int = int()           # constructor defaults to an object of value 0
an_empty_string = str()  # constructor default is an empty string
an_empty_list = list()   # constructor default is an empty list
print(an_int, an_empty_string, an_empty_list)
```

Of course, it is possible to initialize these objects to values other than their defaults by passing in an initialization value in the constructor call.

```{code-block} python
---
lineno-start: 1
---
# With some initialization values
an_int = int(0)
a_string = str(an_int)
a_list = list(an_string)
print(an_int, a_string, a_list)
```

This is what we did when we wanted to create a 4-square-block instance of `CitySqGrid` with a `dog-face` character at its center and name it `nyc`, as we did in `walk.py`. Here's that part of the script again:

```{code-block} python
---
lineno-start: 7
---
nyc = CitySqGrid(4, Cosmo)
print(nyc)
```

What happens when we execute the first line above is that the Python interpreter will create a new object (i.e., a new instance) of class `CitySqGrid`. The instance is basically a raw piece of our computer's memory large enough to hold all the class's attributes. By raw, I mean that none of this memory is initialized in such a manner that object acts like an instance of `CitySqGrid`. This is the job of the `__init__` method.

The implementation of `__init__` method of `class CitySqGrid` make sure that the city we build is between 2 and 20 buildings on a side, and that this number is an even so that there is a single central location.  It also creates the configuration strings that are passed to the `__init__` method of `Maze`, the superclass of `CitySqGrid`. 

And toward the end of this initialization method, we find code that does a small bit of work to make the initial picture of the city prettier and sets the character at the city center, if the call to the constructor specified this optional parameter.

## Representational invariant

In general, the `__init__` method is responsible for creating an initial and valid representation of an object of the specified class. As we mentioned earlier, a powerful aspect of object-oriented programming is the pulling together of all the functions that manipulate an object's internal representation and publishing an interface that allows other programmers to use the abstract type without violating its representation. In other words, each class has a *representation invariant*, which the `__init__` method establishes and the other class methods are expected to maintain.

This is a non-trivial exercise when you, as the programmer of a class, are defining all of the class's methods and data attributes yourself. It is an even more interesting exercise when your class is derived from a more general abstract data type. In our example, the methods of `Maze` are operations you can perform on instances of `CitySqGrid`. It's fine for these superclass methods to operate on instances of the subclass if they maintain the representation invariant. If they don't, then you should *override* them by redefining those methods in the subclass.

In the case of `CitySqGrid`, none of the methods of `Maze` violate the representational invariant established by `CitySqGrid.__init__`; `CitySqGrid` just creates a special kind of `Maze`. We will see an example of method overriding soon, when we build yet another abstract data type to help us solve our driving-directions problem.

## Self

One last word on a piece of syntax you undoubtedly noticed in every method definition in `Maze` and `CitySqGrid`, and that is the name `self` as the first (and possibly only) formal parameter. Every method definition must contain one more formal parameter than the number of actual parameters when invoked, but this first formal parameter is not required in Python to be called `self`. While not required, it is highly recommended that you follow this common practice. If you do, other Python programmers will immediately know what you're doing.

This special first formal parameter allows you to access the data attributes of the actual instance on which the method was called. We see this in last two pieces of initialization in the `CitySqGrid.__init__` method. For example, we assign the `character` value to `self.grid[size][size].content` because we want to modify the grid in the namespace of the object we just created. Without this namespace specification, the Python interpreter would assume we were talking about some `grid` in the local namespace, like the `i` and `j` index variables in the nested for-loops that are in the local namespace.

```{tip}
Knowing whether you need to prepend `self` to a name is tricky, and it will take some time for you to naturally know when and when not to use it. Until you do, I suggest you pause as you first write a variable name in a class method and ask yourself, "Am I trying to access an attribute of a particular instance of this abstract data type (i.e., class), or is this just a local variable that is helping me to complete the work of this method?" You need `self` in the first case, and you don't in the second. 
```

## Instance and class variables

The docstring I use in `CitySqGrid` tries to help you answer the question in the previous Tip by prepending "instance" before the class attributes that require a `self`. These are all *instance variables*, i.e., data attributes associated with the instance.

Although we won't use them in this book, you can also create *class variables*. With a class variable, you don't have a copy of that variable for each instance, but a single variable that is shared by every instance of the class. Class variables create shared state, which brings its own representational invariant challenges. You'd access a class variable by replacing `self` with the class name as the namespace. If you'd like to learn more about class variables, I encourage you to read a book focused on the object-oriented properties of Python.

```{margin} Creating a Map
To create a map using the `Maze` class, you specify what the map looks like using two configuration strings. The first uses a specialized encoding to indicate where the edges of the roads exist, and the second indicates the start and goal locations. The comments in `maze.py` explain the structure of these strings.
```

## General maps

The `Maze` class, along with its associated `Cell` class, is the abstract data type we need to build a general map and on which we can design a goal-directed search algorithm that will produce driving directions. Feel free to look at the implementation of this class, but we don't need to understand these details to solve our driving-directions problem. We just need the interface, which we already know from using objects of type `CitySqGrid`.

```{margin} Credit
This map closely resembles the one used in Week 0 (Search) of [CS50's Introduction to Artificial Intelligence with Python](https://cs50.harvard.edu/ai).
```

The rest of this chapter uses a map specified by the two configuration strings: `MAZE_map` and `MAZE_map_endpts`. When printed, these strings produce the map in {numref}`Figure %s<c11_fig1_ref>`.

```{figure} images/c11_fig1.png
:name: c11_fig1_ref

The map produced by the configuration strings `MAZE_map` and `MAZE_map_endpts` in `maze.py`. We'll start at the location marked with an `s` and follow the roads which are the empty squares. Our goal is the square with the `g`.
```

## Keeping track

Now that we have a digital representation of a map and an interface that allows us to move around it, we can finally write the script that will find us a set of driving directions from our start location (`s`) to our goal location (`g`). This script has to do much of what we did in `dogwalk.py`, and so we'll start with its function `sim`. This function, however, did two things we don't want: (1) it ran lots of separate simulations; and (2) printed the results of each one.

When we ask for driving directions, we're not interested in seeing every path that the code explores and especially not the paths that end in dead-ends. Our problem specification says that it should execute until it finds a solution or determines that there is no path from start to goal. If you like solving printed mazes, you already know how to do this: our function will need to keep track of the parts of the map it has explored and those places on the map it can reach from start but hasn't yet explored. By tracking these unexplored places, our script can restart its search at one of them when its current search path ends in a dead-end. And it will know that the map does not contain a route from start to goal if it ever gets to a point when it hasn't found the goal and there are no unexplored places left to explore.

That reasoning explains how we'll squish lots of separate simulations into a single search exploration, but it will still leave us a printed map with all these search paths marked---one of which leads from start to goal (assuming there is such a path). Imagine a layering of all the `sim` outputs on top of each other, which is not what our problem specification stated was the desired output. The function should return the actual path the search found from start to goal. This is a solvable problem  if our code records the actions it took during the search, information we've been throwing away. With this additional information and a bit of processing, you'll see how it can print a sequence of actions that lead directly from start to goal.

We'll adapt `sim` to perform the first and then the second of these tracking actions. To begin this work, let's rename `sim` to `search`, rename its formal parameter `my_city` to `my_map`, and strip out the code for running numerous trials and the counting of dead-ends, as shown in the following code block. This code also generalizes the condition tested in the while-loop (line 8), which was specific to our goal in the dog-walk problem; it now tests whether we have reached the specified goal state.

```{code-block} python
---
lineno-start: 1
---
### Some useful code for finding driving directions on a map
def search(my_map):
    g = my_map.grid

    loc = my_map.start
    row, col = loc

    while loc != my_map.goal:
        # Leave a scent at current loc
        g[row][col].content = '*'

        # Build list of available moves.  A move is available if there's not
        # a wall in that direction and no scent there.
        moves = []
        if not g[row][col].northwall and g[row-1][col].content != '*':
            moves.append('n')
        if not g[row][col].southwall and g[row+1][col].content != '*':
            moves.append('s')
        if not g[row][col].eastwall and g[row][col+1].content != '*':
            moves.append('e')
        if not g[row][col].westwall and g[row][col-1].content != '*':
            moves.append('w')

        if len(moves) == 0:
            # hit a dead-end
            break

        # Move to a random available choice
        m = random.choice(moves)
        if m == 'n':
            row -= 1
        if m == 's':
            row += 1
        if m == 'e':
            col += 1
        if m == 'w':
            col -= 1
        g[row][col].content = Cosmo
        loc = (row, col)

    print(my_map)
    return
```

These changes aren't all we need. What's missing is the tracking we discussed, which like the last change is code common to every goal-directed search problem.

## Maintaining a frontier

Let's continue our fixes by keeping a list of all the places we haven't explored. Recall that each time through the current while-loop body, lines 12-22 generate a list of available `moves`, which are directions to new locations that we haven't explored. We then randomly makes one of these moves and discard the others. Discarding our other choices was fine when we were happy to hit a dead-end and end the current simulation round, but now we should exit the while-loop by the if-statement on line 24 only when there isn't *any* path in the map from start to goal. This means we need to lift the zeroing of the `moves` list (currently line 14) to a point before the while-loop so that the while-loop can maintain the list of *every* unexplored path.

In goal-directed search algorithms, this `moves` list is what is typically called the *frontier*, and we'll rename `moves` to `frontier` throughout. On this frontier list, we'll place unexplored locations (rather than directions) since we'll need to jump in our map from a `loc` which is a dead-end to one of these frontier-recorded, unexplored locations. Recording a direction is not enough information to make this jump.

Keeping a frontier list of every unexplored location also requires us to change the specific check in the if-statements on lines 15, 17, 19, and 21. Currently, each asks: (1) if a particular move is valid; and (2) if we haven't already explored this new location (i.e., is there a `'*'` there?). These two checks were sufficient when we regenerated our entire list of unexplored locations each time. Now we're trying to *maintain* a list of all unexplored locations, and since we might get to a location along multiple paths from the start location (see {numref}`Figure %s<c11_fig2_ref>` below for such an example using our map), we'll want to make sure we add each unexplored location to the frontier only once. This adds a new condition to each of these if-statements. For example, lines 15-16 become: 

```{code-block} python
---
lineno-start: 15
---
        if not g[row][col].northwall and g[row-1][col].content != '*' and \
            (row-1,col) not in frontier:
            frontier.append((row-1, col))
```

```{figure} images/c11_fig2.png
:name: c11_fig2_ref

Two paths from start that reach location (1,8). Our script should add this location to the frontier list only once.
```

When we put unexplored locations on the frontier list, choosing a next location to explore (lines 28-39) means that we just grab one of these locations off the list. The new code is much simpler, but notice that we no longer use `random.choice`, since that function doesn't remove an item from its input list. We replace it with `list.pop` for now, which you can think of as making a random choice because we aren't paying attention to what's at the end of list. Later in this chapter, I'll explain how the different choices you make here affect the running of the search.

```{code-block} python
---
lineno-start: 28
---
        # Choose a location from the frontier as next to explore
        loc = frontier.pop()
        row, col = loc
```

For completeness, the if-statement and its body on lines 24-26 become:

```{code-block} python
---
lineno-start: 24
---
        if len(frontier) == 0:
            print('No solution')
            return
```

We're very close to a version of `search` that achieves the first of our tracking actions (i.e., it searches for a path from start to goal and prints a marked map when it finds one). We just need to fix one small issue that arose when we changed from searching for a path on a very specific type of map (a city grid where start was at its center and the goal was to reach the fields outside the city) to any map on which we can place the start and goal locations anywhere.

Do you see the problem? It's quite subtle, and it reminds us to think systematically about edge cases when we try to write a script for the most general formulation of a problem. Here's the code that results from all the changes we just discussed:

```{code-block} python
---
lineno-start: 1
---
### Not quite chap11/directions0.py
def search(my_map):
    g = my_map.grid
    frontier = []

    loc = my_map.start
    row, col = loc

    while loc != my_map.goal:
        # Leave a scent at current loc
        g[row][col].content = '*'

        # Build list of unexplored locations. A move is possible if there's not
        # a wall in that direction. We also add unexplored locations only once.
        if not g[row][col].northwall and g[row-1][col].content != '*' and \
            (row-1,col) not in frontier:
            frontier.append((row-1, col))
        if not g[row][col].southwall and g[row+1][col].content != '*' and \
            (row+1,col) not in frontier:
            frontier.append((row+1, col))
        if not g[row][col].eastwall and g[row][col+1].content != '*' and \
            (row,col+1) not in frontier:
            frontier.append((row, col+1))
        if not g[row][col].westwall and g[row][col-1].content != '*' and \
            (row,col-1) not in frontier:
            frontier.append((row, col-1))

        if len(frontier) == 0:
            print('No solution')
            return

        # Choose a location from the frontier as next to explore
        loc = frontier.pop()
        row, col = loc

    print('Found a solution')
    print(my_map)
    return
```

Think about what is printed when the start location matches the goal location. Will the script place a star (`*`) on that shared location? It won't because the while-loop won't ever execute. Now think about what is printed when the start and goal locations are different (and a path exists). The script still won't mark the goal location. We can fix this by shifting when we put a star on a location: We'll mark a location when we *decide to explore it* instead of as we *start exploring it*. The script `directions0.py` does exactly this.

```{code-block} python
---
lineno-start: 1
---
### chap11/directions0.py -- Finding driving directions (first try)
import maze

# Marks for map, which use color codes for terminal printing
EXPLORED = '\033[34m*\033[0m' # blue *
FRONTIER = '\033[32mf\033[0m'      # green f

def search(my_map):
    g = my_map.grid
    frontier = []

    loc = my_map.start
    row, col = loc
    g[row][col].content = EXPLORED

    while loc != my_map.goal:
        # Build list of unexplored locations. A move is possible if there's not
        # a wall in that direction. We also add unexplored locations only once.
        if not g[row][col].northwall and \
            g[row-1][col].content != EXPLORED and \
            (row-1,col) not in frontier:
            frontier.append((row-1, col))
            g[row-1][col].content = FRONTIER
        if not g[row][col].southwall and \
            g[row+1][col].content != EXPLORED and \
            (row+1,col) not in frontier:
            frontier.append((row+1, col))
            g[row+1][col].content = FRONTIER
        if not g[row][col].eastwall and \
            g[row][col+1].content != EXPLORED and \
            (row,col+1) not in frontier:
            frontier.append((row, col+1))
            g[row][col+1].content = FRONTIER
        if not g[row][col].westwall and \
            g[row][col-1].content != EXPLORED and \
            (row,col-1) not in frontier:
            frontier.append((row, col-1))
            g[row][col-1].content = FRONTIER

        # my_map.print() # uncomment to see the frontier grow

        if len(frontier) == 0:
            print('No solution')
            return

        # Choose a location from the frontier as next to explore
        loc = frontier.pop()
        row, col = loc
        g[row][col].content = EXPLORED

    print('Found a solution')
    my_map.print()
    return
```

```{admonition} You Try It
Run `search` on the map from {numref}`Figure %s<c11_fig1_ref>`. You can do this by running the next code block if you're reading this chapter as an interactive Python notebook, or by executing `python3 directions0.py` in the shell, if you've grabbed the code distributed with this chapter.

You should see an output map with blue stars on all explored locations and green f-characters on locations that were added to the frontier list but never explored.
```

```{code-block} python
---
lineno-start: 54
---
### chap11/directions0.py -- Finding driving directions (first try)
import maze

print('\nBuilding our map')
my_map = maze.Maze(maze.MAZE_map, maze.MAZE_map_endpts)
my_map.print()

print('Starting the search')
search(my_map)
```

## Remembering how we got there

It's now time for us to add to `search` the other piece of tracking I mentioned, i.e., keeping extra information to record the actions taken. While we'd like to record only the actions on the path from start to goal, we won't know which they are until the search comes across the goal. Until that point, it must record the actions from all its explorations. Visually, `search` needs to record the actions it took along the five orange paths in {numref}`Figure %s<c11_fig3_ref>`, which didn't reach the goal, as well as the single blue path, which did.

```{figure} images/c11_fig3.png
:name: c11_fig3_ref

The six paths explored during the execution of `search` on our example map.
```

```{tip}
Recording paths like these (i.e., moving from one state to another by taking some action) is a pattern that you'll see in many computational problems. 
```

We record these paths by building a data structure that mimics the tree-like structure you see in {numref}`Figure %s<c11_fig3_ref>`. In this figure, all paths take the same first five actions (i.e., go east, east, east, then north and north again). At location `(5,4)`, two of the orange paths head west and the rest of the paths, including our desired blue one, head east. These first five actions represent the trunk of the imagined tree, and location `(5,4)` represents the first branching of the tree.

We can digitally represent this tree by connecting together a series of notes to ourselves, each of which we'll call a `TreeNode`. In these notes, the `search` function will record:

1. a state (in our problem this is a map location); and
2. the action (i.e., a driving direction) taken by our `search` function that is connected to the recorded state.

I cryptically described the stored action and that's because we have a choice: Do we want to record the action `search` took *from* the state in the note, or would it be better to record the action `search` took *to get to* that state? Well, which do we need to solve the problem in front of us? Which will help distinguish the blue path from the orange ones in {numref}`Figure %s<c11_fig3_ref>`? To answer this question, think about what you want to know when `search` finds the goal.

There's nowhere to go from the goal, since `search` has found where we want to go, but it needs to know how it got to there. So it needs to record the previous action that got it to the current location. And to walk all the way back from goal to start, it will want to chain the notes together. Using Python classes, the data structure I'm describing would look as follows:

```{code-block} python
---
lineno-start: 1
---
class TreeNode():
    def __init__(self, state, parent, action):
        self.state = state    # current location 
        self.parent = parent  # previous note in path
        self.action = action  # action that got us to this location
```

As an example of this chaining, the `search` function would start by creating a note for the start location, which it creates by calling the `TreeNode` constructor with parameters that say there is no parent note and no action that got us to start, i.e., `TreeNode((7,1), None, None)`. From start, the only move to make in our example map is east, and the constructor call for the next location is: `TreeNode((7,2), ???, 'east')`, where I've left the `parent` parameter unspecified. What is it supposed to be? Right! The note (i.e., the name of the `TreeNode`) created at start. With the name proceeding note in our current note, we'll be able to find the location that proceeded the goal. And from it the location before it, all the way back to the start.

Interestingly, branching points present no difficulties when proceeding in a tree from a leaf to the root. A branching location is the parent to all paths that emanate from this point! This means, if we start at the end of the blue path (i.e., the goal), `search` can easily follow the notes it created directly back to start, and to produce driving directions, it simply reverses the list of actions it encounters as it visits each `parent` from goal to start.

## The solution

We've now have all the components we need for our problem solution, and we just have to put them together. Let's start with the changes we need to make to the while-loop initialization and its body, which is the code that searches the map (lines 17-66 below). The trickiest part here is making sure that you have the data you need when you want to create a `TreeNode` and deciding what you want to store in the frontier list.

In `directions0.py`, we stored unexplored map locations in the frontier list, but now when we pop from the list one of the unexplored locations, we are going to want to have access to the note (i.e., `TreeNode`) that goes with it. It is needed to build the notes for the unexplored locations reachable from that `TreeNode`'s location. If you think about this for a bit, you'll realize that we'll want to store the notes on the frontier list, since a `TreeNode` contains the map location corresponding to it. A complete solution for `search` is shown in the next code block. 

```{code-block} python
---
lineno-start: 1
---
### chap11/directions-dfs.py
import maze

# Marks for map, which use color codes for terminal printing
EXPLORED = '\033[34m*\033[0m' # blue *
FRONTIER = '\033[32mf\033[0m' # green f

# Keep track of the tree of explored paths
class TreeNode():
    def __init__(self, state, parent, action):
        self.state = state    # current location 
        self.parent = parent  # previous note in path
        self.action = action  # action that got us to this location


def search(my_map):
    g = my_map.grid
    frontier = []

    # Initialize the variables used in the while-loop
    # with values from the start location
    loc = my_map.start
    row, col = loc
    g[row][col].content = EXPLORED
    cur_node = TreeNode(loc, None, None)

    # Search loop
    while loc != my_map.goal:
        # Build list of unexplored locations. A move is possible if there's not
        # a wall in that direction. We also add unexplored locations only once.
        if not g[row][col].northwall and g[row-1][col].content != EXPLORED and \
            g[row-1][col].content != FRONTIER:
            new_loc = (row-1, col)
            new_node = TreeNode(new_loc, cur_node, 'north')
            frontier.append(new_node)
            g[row-1][col].content = FRONTIER
        if not g[row][col].southwall and g[row+1][col].content != EXPLORED and \
            g[row+1][col].content != FRONTIER:
            new_loc = (row+1, col)
            new_node = TreeNode(new_loc, cur_node, 'south')
            frontier.append(new_node)
            g[row+1][col].content = FRONTIER
        if not g[row][col].eastwall and g[row][col+1].content != EXPLORED and \
            g[row][col+1].content != FRONTIER:
            new_loc = (row, col+1)
            new_node = TreeNode(new_loc, cur_node, 'east')
            frontier.append(new_node)
            g[row][col+1].content = FRONTIER
        if not g[row][col].westwall and g[row][col-1].content != EXPLORED and \
            g[row][col-1].content != FRONTIER:
            new_loc = (row, col-1)
            new_node = TreeNode(new_loc, cur_node, 'west')
            frontier.append(new_node)
            g[row][col-1].content = FRONTIER

        # my_map.print() # uncomment to see the frontier grow

        if len(frontier) == 0:
            print('No solution')
            return

        # Choose a location from the frontier as next to explore
        cur_node = frontier.pop()
        loc = cur_node.state
        row, col = loc
        g[row][col].content = EXPLORED

    # Follow the parent links from cur_node to create
    # the actual driving directions
    ddirections = [cur_node]
    while cur_node.parent:
        cur_node = cur_node.parent
        ddirections.insert(0, cur_node)

    # Print out the driving directions
    print('## Solution ##')
    print(f'Starting at {ddirections[0].state}')
    ddirections.pop(0)
    for n in ddirections:
        print(f'Go {n.action} then')
    print('Arrive at your goal')
    return
```

If you compare this code to the same while-loop initialization and body of `directions0.py`, you'll see that the only differences are the creation of notes and the maintenance of them on the frontier list. In this, pay careful attention to the difference between `cur_node` and `new_node`. The first is a loop-carried variable, which we update at the end of the while-loop and use throughout the early part of this loop body. The second (`new_node`) is just a temporary name that we don't want to overwrite the value in `cur_node` that we have to use multiple times.

```{tip}
I did change the form of the third condition in the if-statements on lines 32, 38, 44, and 50. I did this because we cannot easily search the frontier list for `new_loc`, which is buried in the elements of `frontier`. This is an example of finding a logically equivalent way to perform an operation that you need, but your programming language doesn't make easy. It is also an example of why programming is an art.
```

When `search` encounters the goal and the while-loop ends, the interpreter begins execution of the code on lines 68-81. These are the lines that use our notes to create the driving directions we desire. The first block (lines 68-73) starts with `cur_node`, which is the goal location when the while-loop exits, and builds a list (called `ddirections`) that is the reverse of the order in which we are visiting the notes on the path from goal to start. The second block (lines 75-81) uses this list to print the actions in human-readable form for the path from start to goal. The only trickiness is on line 78 where we have to remember to remove the note corresponding to the start location, which has no stored action.

You can run this complete `search` function using the same code block from earlier (i.e., the one we used to run the `search` function in `directions0.py`). Both pass in a map with designated start and goal locations; they simply differ in what they print!

## Depth-first search (DFS)

The search we built (in the script named `directions-dfs.py`) is what is called a *depth-first search*. If you uncomment line 56, `search` will print a sequence of maps corresponding to each step it makes. If you review these maps in order, you'll notice that `search` goes down a road as far as it can before backing up and trying a different road. When it encounters an intersection, it chooses a direction through that intersection and keeps going.

It's clearly not the only thing it could have done, and if we want to do something different, we need to understand what caused this behavior. The answer is amazingly simple: the way in which we add elements to and remove them from the frontier list. The current code does the following:

* When we find a new unexplored location, we *append it to the end* of the frontier list.
* When we go to explore a new location, we *pop one off the end* of the frontier list. In Python, `pop` with no arguments removes the last item from the list.

Computer scientists like to talk about this behavior as "last-in, first-out" and to imagine working with a *stack*, like a stack of plates. The last one added to the stack is the first one taken off.

What's good about this approach? Well, if we're lucky, the first path we explore might lead us to the goal state, and even if we're not, this approach can be very fast in finding a solution when there are many possible solutions. On the other hand, when there are many paths to the goal, this approach just takes the first it finds. There's no guarantee that our DFS solution is the shortest path.

## Breadth-first search (BFS)

But what if we're interested in finding the *shortest path* from start to goal (i.e., let's change our specification). Can we change the way `search` operates to guarantee that it returns the shortest path, if one exists? We can, and what's even more amazing is that it requires only a single character change to our current `search` function!

```{admonition} You Try It
Change line 63 so that you pass a `0` as the parameter to `pop`, or verify that this is the only difference between `directions-dfs.py` and `directions-bfs.py`. Now run the BFS script and watch how the maze is explored. How would you describe the two search approaches?
```

Both versions of `search` place new unexplored locations at the end of the frontier list; they differ only in how they take unexplored locations from the frontier list. DFS takes from the end, and BFS, or *breadth-first search*, takes from the front. Instead of "last-in, first-out" as under DFS, BFS manages the frontier in a "first-in, first-out" manner, which is what computer scientists called a *queue*. Imagine the line that queues up to check-out at a grocery store.

When you watched the BFS script operate, you saw the search expand in all directions at the same rate. In other words, no explored path was more than one step longer than any other path. This means that `search` will find the shortest path (or technically, one of the shortest) from start to goal before any longer path. This approach is guaranteed to find the shortest path, but at the cost of searching methodically along every path.

```{admonition} You Try It
If you take the time to figure out how the configuration strings work in `maze.py`, change the map we've been using or design your own, and then run them through the DFS and BFS scripts. You should find that these fairly short scripts are impressively robust!
```

## Informed searches

Both DFS and BFS are called *uninformed searches* because they don't use any information about the problem except what they gleam from their own explorations. This means that you can directly use these algorithms on any goal-directed search problem. You just need to encode your problem in a data structure comprising interconnected states, and then adapt the data-structure-specific parts of `search` to explore it!

But what if we were interested not in any goal-directed problem, but a specific one. Could we use information about the problem to create an algorithm that combines the direct-to-goal-nature of DFS with the good-answer-aspects of BFS? In other words, could problem-specific knowledge help us to identify and avoid paths that are unlikely to be a solution? As a concrete example, how might we adapt the goal-directed searches we've seen for the driving directions problem if we were given the "as-the-crow-flies" distance between each map location and the goal?

The as-the-crow-flies distance to the goal isn't a set of driving directions (i.e., we're not following the map!), but it does indicate which of two moves would bring us closer to the goal. Imagine using this distance to select the search algorithm's next location from the frontier list. We're using the distance to put the frontier list locations in a priority order. The higher the priority, the more likely that location will get us to the goal. 

In general, prioritizing our possible search moves is the job of a *heuristic function.* The heuristic we use, of course, might be wrong. For example, we might have to drive in a direction away from the goal to find the road that will get us to it. Heuristics don't guarantee that we'll do better, but good ones, on average, find a near-optimal solution more quickly than our two uninformed solutions.

There are many heuristics in the domain of search. I asked you to imagine a common one (called *greedy*), which always takes the move from the frontier list with the best heuristic score. Another, which often provides more consistently good results, combines the cost of the path to the current point with the value of that point's heuristic (called *A\* search*). Overall, there is a rich literature associated with search, and you are now prepared to dive into it to solve your own goal-directed search problems!

\[Version 20240523\]