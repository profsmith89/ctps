## Chapter 12 ##

## ALE 12.1: A shell game

In the chapter, we learned that the sort most of us use to order the playing cards when dealt a hand is called *insertion sort*. Despite the fact that it isn't that efficient---it has a worst-case performance of $O(n^2)$---it was easy to understand and code.

In this exercise, we'll look at a related sort called *Shellsort*, which is named after Donald Shell who published the first description of the method in 1959. I grabbed the pseudocode for this sort from [Wikipedia](https://en.wikipedia.org/wiki/Shellsort) and implemented a version of it in Python. How it works is much more cryptic.

**Step 1.** Re-familiarize yourself with insertion sort by reading through the following code. This version counts the number of swaps that the algorithm takes to sort the input list. If you want to see these swaps as they take place, uncomment the print-statements on lines 10 and 15. **Make sure to replace the comment characters before you proceed to the next step.**

```{code-block} python
---
lineno-start: 1
---
### chap12/ale01/insertion.py

def insertion_sort(s):
    '''Sorts the unsorted sequence s in place and
       returns the number of swaps performed.
    '''
    swaps = 0

    for i in range(1, len(s)):
        #print(f'Considering item at index {i}')
        for j in range(i, 0, -1):
            if s[j - 1] > s[j]:
                # Swap the two items
                s[j - 1], s[j] = s[j], s[j - 1]
                #print(f'Swapped {j-1},{j}: {s}')
                swaps += 1
            else:
                # When swaps are done, the ith element is where it belongs
                break
                
    return swaps
```

You can test `insertion_sort` using this code block:

```{code-block} python
---
lineno-start: 25
---
### chap12/ale01/insertion.py

# A simple test routine
s_test = [62, 83, 18, 53, 7, 17, 95, 86, 47, 69, 25, 28]
print(f'unsorted list: {s_test}\n')
    
swaps = insertion_sort(s_test)
print(f'sorted list: {s_test}\n')
print(f'total swaps (INSERTION sort) = {swaps}')
```

**Step 2.** Now let's look at the code for Shellsort and try to understand its behavior (i.e., don't yet run it). To be clear, I don't expect you to go through every line of code. Instead, we'll practice our skills at understanding the computational complexity of an algorithm.

Let's start with a simple question. In the function `shell_sort`, **how many loops do you see?** You can ignore the function `gen_gaps`, i.e., assume the call to that function takes a fixed amount of time.

```{code-block} python
---
lineno-start: 1
---
### chap12/ale01/shell.py
'''
Shellsort, translated to Python using pseudocode from: 

https://en.wikipedia.org/wiki/Shellsort
'''

def gen_gaps(n):
    '''Compute and return a shellsort gap sequence'''
    gaps = []
    k = 1
    g = n // (2**k)
    while g != 0:
        gaps.append(g)
        k += 1
        g = n // (2**k)
    return gaps
    

def shell_sort(s):
    '''Sorts the unsorted sequence s in place and
       returns the number of swaps performed.

       Comments below this line are unchanged from
       the wikipedia entry.
    '''
    swaps = 0

    gaps = gen_gaps(len(s))
    #print(f'gap sequence: {gaps}\n')
    
    # Start with the largest gap and work down to a gap of 1
    # similar to insertion sort but instead of 1, gap is being used in each step
    for gap in gaps:
        # Do a gapped insertion sort for every elements in gaps
        # Each loop leaves a[0..gap-1] in gapped order
        for i in range(gap, len(s), 1):
    
            # save s[i] in temp and make a hole at position i
            temp = s[i]
    
            # shift earlier gap-sorted elements up until the correct location
            # for s[i] is found
            j = i
            while j >= gap and s[j - gap] > temp:
                s[j] = s[j - gap]
                j -= gap
                swaps += 1
    
            # put temp (the original a[i]) in its correct location
            s[j] = temp

    return swaps
```

You should see three: a for-loop on line 34; another on line 37; and a while-loop on line 45.

We know that nested loops mean multiplication: we multiply together the expected iteration count of each loop. But the innermost of the three loops in `shell_sort` is a while-loop with a complex looping condition, it's not easy to tell how many iterations it will take.

If you're tempted to figure out the computational complexity of `shell_sort`, **don't**. It is an open question in computer science. Instead, we'll just check our intuition.

_Do you expect a sort with **three** nested loops to take more or less time than one with **two** nested loops?_

When you've decided on an answer to this question, move on.

**Step 3.** As a proxy for execution time, we'll count the number of swaps each sorting algorithm performs. Notice that `shell_sort` counts the number of swaps that it does on line 48, and like our implementation of `insertion_sort`, it returns this count (in addition to sorting the input array in place).

Suspense is over! Have `shell_sort` order the same test list we used for `insertion_sort`.

```{code-block} python
---
lineno-start: 57
---
### chap12/ale01/shell.py

# A simple test routine
s_test = [62, 83, 18, 53, 7, 17, 95, 86, 47, 69, 25, 28]
print(f'unsorted list: {s_test}\n')
    
swaps = shell_sort(s_test)
print(f'sorted list: {s_test}\n')
print(f'total swaps (SHELL sort) = {swaps}')
```

Are you surprised?

**Step 4.** Maybe you think I pulled a fast one on you. Let's compare these two sorting algorithms by generating a number of large arrays of random numbers.

In the following code block, the function `rnd_list` generates a list of length `n` of random numbers between `0` and `max_elem` inclusive. The function `aprint` avoids printing all the elements in a very long list, where "very long" is defined as more than 10 elements.

```{code-block} python
---
lineno-start: 7
---
### chap12/ale01/cmp.py
import random

def rnd_list(n, max_elem):
    '''Generate a random, unsorted list 'a' of n numbers 
       between 0 and max_elem
    '''
    a = []
    for i in range(n):
        a.append(random.choice(range(max_elem + 1)))
    return a

def aprint(header, a):
    '''Avoids printing huge arrays'''
    n = len(a)

    print(header, end='')
    if n > 10:
        print(f'[{a[0]}, {a[1]}, {a[2]}, {a[3]}, ... \
              {a[n-4]}, {a[n-3]}, {a[n-2]}, {a[n-1]}]')
    else:
        print(a)
```

We'll now use these helper functions to run three competitions between `insertion_sort` and `shell_sort`. Run the next code block several times.

```{code-block} python
---
lineno-start: 7
---
### chap12/ale01/cmp.py
for n in (10, 100, 1000):
    a = rnd_list(n, 999)
    b = a.copy()

    print(f'\n***{n} elements in array***')
    aprint('  unsorted list: ', a)
    print()

    swaps = insertion_sort(a)
    aprint('  sorted list: ', a)
    print(f'  total swaps (INSERTION sort) = {swaps}')

    print()

    swaps = shell_sort(b)
    aprint('  sorted list: ', a)
    print(f'  total swaps (SHELL sort) = {swaps}')
```

Wild, huh?

**Step 5.** Here's a brief explanation of why `shell_sort` wins, as described at the start of the Wikipedia article referenced above: *"The method starts by sorting pairs of elements far apart from each other, then progressively reducing the gap between elements to be compared. By starting with far apart elements, it can move some out-of-place elements into position faster than a simple nearest neighbor exchange."*

That's a wonderful example of "here's an idea for how we might do things more efficiently." Of course, an idea needs to be fleshed out. Donald Shell did that. And with some code, he showed that it worked more efficiently *in practice*. He also left us with a puzzle, and you can read the Wikipedia page for more on that.

## ALE 12.2: Taco cat

In this active learning exercise, our problem to solve is identifying palindromes! And you're going to do it using a divide-and-conquer approach and then code that approach using recursion.

A palindrome is a word or phrase that is unchanged whether you read it from left to right or right to left. The word 'kayak' is an example of a palindrome. Ignoring spaces, the title of this ALE is another palindrome, which might be a weirdly named pet.

Your task is to write a function called `is_palindrome` that takes a single formal parameter `s`, which it expects to be a string, and returns `True` if the input string is a palindrome and `False` otherwise.

**Step 1.** Recursion expects that the parameter to the recursive call inside the recursive function is somehow a smaller version of the problem. What might we do to a palindrome to create a smaller version of the problem?

Well, consider the example word I gave above: kayak. This word is a palindrome if:

1. the first and last letters satisfy what we expect in a palindrome; and
2. the remaining middle three letters are also a palindrome.

To answer the second statement, we'd call is\_palindrome on 'aya'. If `s` is the name given to the string `'kayak'`, write a check for the first statement and the recursive call for the second.

**Step 2.** Now that we know the form of the recursion, let's identify the base cases. In this problem, there are two. Why? Well, we're stripping off two letters at a time and strings can contain either an even or odd number of letters.

Continuing with `s` as the name of the input string, write down when you know a string is a palindrome by checking nothing more than its length.

**Step 3.** Let's start putting our recursive function together. I've helped you get started with the function definition and a statement that strips anything other than the letters `a-z` and the digits `0-9`. Yes, your routine will check numbers and phrases with numbers to see if they're palindromes.

```{code-block} python
---
lineno-start: 1
---
### chap12/ale02.py

def is_palindrome(s):
    '''Returns True if s is a palindrome'''

    # Strip punctuation and spacing
    s = s.strip(' \t\n\'"~!@#$%^&*()-_+={}[]\\|;:<>,.?/')

    # REPLACE THIS COMMENT AND THE NEXT RETURN WITH YOUR CODE
    return False
```

Let's run a few simple tests.

```{code-block} python
---
lineno-start: 42
---
### chap12/ale02.py

def check(s):
    print(f'"{s}" is', end=' ')
    if not is_palindrome(s):
        print('not', end=' ')
    print('a palindrome\n')


check('mom')
check('Mom')
check('121')
check('23cs32')
check('not')
check('notion')
```

**Step 4.** Is it really right to say that `'M'` doesn't match `'m'`? Have your version of `is_palindrome` ignore capitalization and then rerun the tests above.

**Step 5.** Here are a few tougher tests. Try adding some of your own.

```{code-block} python
---
lineno-start: 18
---
### chap12/ale02.py

# Demetri Martin's palindrome poem
poem = '''Dammit I'm mad.
Evil is a deed as I live.
God, am I reviled? I rise, my bed on a sun, I melt.
To be not one man emanating is sad. I piss.
Alas, it is so late. Who stops to help?
Man, it is hot. I'm in it. I tell.
I am not a devil. I level "Mad Dog".
Ah, say burning is, as a deified gulp,
In my halo of a mired rum tin.
I erase many men. Oh, to be man, a sin.
Is evil in a clam? In a trap?
No. It is open. On it I was stuck.
Rats peed on hope. Elsewhere dips a web.
Be still if I fill its ebb.
Ew, a spider... eh?
We sleep. Oh no!
Deep, stark cuts saw it in one position.
Part animal, can I live? Sin is a name.
Both, one... my names are in it.
Murder? I'm a fool.
A hymn I plug, deified as a sign in ruby ash.
A Goddam level I lived at.
On mail let it in. I'm it.
Oh, sit in ample hot spots. Oh wet!
A loss it is alas (sip). I'd assign it a name.
Name not one bottle minus an ode by me:
"Sir, I deliver. I'm a dog"
Evil is a deed as I live.
Dammit I'm mad.'''

check('taco cat')
check('Was it a car or a cat I saw?')
check('A man, a plan, a canal: Panama!')
check('Alli trota la tortilla.')
check(poem)
```

## ALE 12.3: Fib forever

INSERT: Fibonacci example to show how a short, simple program can take infinitely long to execute.

## ALE 12.4: Computing to the end of time

INSERT: Pitfalls of recursion. A seemingly simple problem (Towers of Hanoi) that can take till the end of the world to complete. Raises the issue of tractable and intractable problems; intractable ones are ones where we only know long-running scripts for large N problems.

\[Version 20240403\]