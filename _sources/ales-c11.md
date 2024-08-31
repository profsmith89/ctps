# Chapter 11 #

## ALE 11.1: Dictionary of dictionaries

\[Insert an edited version of Kitty's ALE 11.1.\]

## ALE 11.2: Making print work

\[Insert an edited version of Kitty's ALE 11.X titled "How the heck do you print a pin?" This will help the students to understand how they can add methods to an existing class and explain the difference between \`\_\_str\_\_\` and \`\_\_expr\_\_\` through a problem-to-be-solved.\]

## ALE 11.3: More power to the constructor

\[Insert an edited version of Kitty's ALE 11.X titled the same as this ALE. Change the task: support more icons by understanding the star rating and something from the note field.\]

## ALE 11.4: See the path

The driving-directions script built in Chapter 11 prints only the turn-by-turn directions that gets you from the start to the goal locations. Today's mapping applications provide both this and the path marked on a map. Your task is to mark the turn-by-turn driving directions on the searched map and print it.

**Step 1.** We'll start with the chapter's depth-first solution. Copy `directions-dfs.py` and name this copy `ale04.py`. Edit line 1 to change `directions-dfs.py` to `ale04.py`.

**Step 2.** We're going to make our changes inside the function `search`, since that's where the current turn-by-turn driving directions are printed out. Find where those directions are printed and make sure you understand them.

**Step 3.** We'll leave the code that produces the `ddirections` list of `TreeNote`s alone (lines 55-60) and start adding code around and inside the printing of the solution (lines 62-68). We'll reuse `my_map`, which `search` used to find the solution, and place `EXPLORED` marks on the map to indicate the solution route. The following pseudocode describes all the work we need to occur to mark up `my_map`:

```{code-block} python
---
lineno-start: 61
---
### chap11/ale04.py
    # Reset the map so we can plot the driving directions

    # Mark the start location as EXPLORED

    # Print out the driving directions and map them
    # ... HINT: the existing code goes here

    # Re-mark the start and goal locations and then print
```

You'll need to use the following attributes of a `Maze` object (i.e., `my_map`) to create the code that implements these pseudocode steps: `start`, `goal`, `reset`, `mark`, `move`, and `print`. Look at how we've used these attributes elsewhere in the `search` function and read through their docstrings in `maze.py`.

**Step 4.** When you've completed Step 3 and have your script printing the step-by-step driving directions and a map with the route marked, it's time to try a new map. In the `main` function of `ale04.py`, duplicate the line that reads:

```{code-block} python
    my_map = maze.Maze(maze.MAZE_map, maze.MAZE_map_endpts)
```

Comment out the original line and then change the duplicate into the following:

```{code-block} python
    my_map = maze.Maze(maze.MAZE_map_ale04, maze.MAZE_map_ale04_endpts)
```

Run your script and take a look at the solution. Do you like the route it found? If not, how might you change your script to have it find and print a more direct route from start to goal?

## ALE 11.5: Build your own class

\[Insert an edited version of Kitty's ALE 11.2 titled "Build your own Pin" that has the student build their own class. It would be nice if it tied into the map motif as \`Pin\` did.\]


\[Version 20240831\]