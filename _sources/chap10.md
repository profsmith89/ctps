# Chapter 10: Build an Index #

We discovered in the last chapter that the choice of algorithm can make a huge difference in the running time of our script as the size of our problem grows. Or stated more bluntly, a brute-force approach doesn't work in all contexts. Under some especially stressful conditions, it fails spectacularly to deliver an answer in a reasonable amount of time.

This chapter opens with a description of how the Rabin-Karp algorithm avoids spectacular failure and the specific steps it takes to turn string-matching into a process that often grows only in proportion to the size of the text, not the size of the text times the size of the search string. 

This knowledge introduces you to a new approach to problem solving. The approach still has us search the entire problem space for a solution, but it splits this search work into what is called *online and offline work*. When the time from user input to solution matters, as it does in the Google search context, doing some of the work before a user gets involved (i.e., the offline work) and then the final bits of the work once we receive the user's input (i.e., the online work) can mean the difference between meeting a problem's requirements and spectacularly missing them.

Given the enormity of the Google search problem, this sort of approach is what we need to bypass the challenge that even a string-matching search---even one that scans through the text only once on a really fast machine---cannot scan 100 million gigabytes with any response time that a human would find reasonable. In other words, linear-time string matching won't solve our simplified Google search problem. We need something else.

Fortunately, Rabin-Karp provides us with a potential answer. It introduces us to the technique of *hashing*, which allows us to build offline an index (like a book index) of the webpages that contain words of interest. But when we split the work we do to solve the problem into online and offline components, we have created for ourselves a new challenge: How do we know what to search for before the user asks us a question? Obviously, we don't. We can, however, turn the preprocessing into work not just for one query, but (effectively) for all possible queries. This is what Google means when it talks about building an index. So our problem-to-be-solved becomes, *How do we build a book index?* A book index is simpler than Google's web index, but the online performance of an index is what we need to solve our simplified Google search problem!

```{admonition} Learning Outcomes
In this chapter, you will learn a new approach to problem solving that splits work into online and offline components. You will build a book index, which is a simplified version of Google's search index, using hash functions and hash tables, and distinguish common and exceptional situations though a discussion of hash collisions. By the chapter's end, you will be able to do the following:

*   Understand how hashing is the key to solving the Google search problem [design and CS concepts].
*   Design a simple hash function [design and programming skills].
*   Handle hash collisions in a solution that uses hashing [CS concepts and programming skills].
*   Describe other applications where hashing proves effective [design].
*   Discuss the operation of a hash table and explain how it achieves its performance [CS concepts].
*    Distinguish between an abstract data type, such as an associative memory, and concrete data types, like the Python dictionary type [CS concepts].
*   Understand the basic operation of random-access memories [CS concepts].
*   Recognize how the distinction between common and exceptional situations can lead to effective designs, as in how collisions are resolved in hash tables [design].
```

## Strings to numbers

To understand the brilliance of the Rabin-Karp algorithm, we begin with a focus on the matching loop. Recall what this matching loop looked like in `bf_strmatch` (see the code block below). Given a pattern string `p` of length `m` and a text string `t` of length `n`, this loop has a computational complexity of $O((n - m + 1) * m)$ because it makes up to *m* character comparisons on each iteration.

```{code-block} python
---
lineno-start: 10
---
### chap09/bf_strmatch.py
for s in range(n - m + 1):
    if p[0:m] == t[s:s+m]:
        print(f'Pattern occurs with shift {s}')
```

The function `rk_strmatch` hides this expensive comparison (i.e., `p[0:m] == t[s:s+m]`) behind a new if-statement (line 43) in the matching loop body. This statement contains an equality comparison between two integers, which has a computational complexity cost of $O(1)$.[^fn1] In other words, as long as this new comparison evaluates `False`, we remove a factor of $m$ from the matching loop's computational complexity.

```{code-block} python
---
lineno-start: 41
---
### chap10/rk_strmatch.py
for s in range(n - m + 1):
    if hp == ht:
        # Verify that this is an actual match
        if p[0:m] == t[s:s+m]:
            print(f'Pattern occurs with shift {s}')
```

What question is this equality comparison between two integers answering? It tries to answer the same one that the expensive comparison did: Does the pattern string match the text substring of length `m` starting at character location `s`? You are probably now wondering: How can that be? It works if we can have an integer hold the same information content as a string of length `m`. Read on to see how!

## A simple hash function

We call these integers *hashes* of their corresponding strings, and the specification for computing a hash looks something like this: *Given a string of arbitrary length, use the string's encoding to compute an integer.* This specification, illustrated in {numref}`Figure %s<c10_fig1_ref>`, is a bit simpler than what we'll discover we need, but it is enough to get us started.

```{figure} images/Smith_fig_10-01.png
:name: c10_fig1_ref

To create a hash, which is just an integer value, we'll write a function that takes a string of any length as input and returns an integer.
```

How might we algorithmically create a hash from a string? First, remember that every character has a unique numerical encoding, and when we're working with strings, we are just interpreting each number as a character. To create an integer hash that has some relationship to the characters in a string, we'll want to get access to the characters' numerical encodings, which we can do by using Python's built-in `ord` function.

```{code-block} python
print(ord('a'), ord('1'))
```

Knowing this, one way to create a hash function would be to read the input string as a sequence of digits, where each digit in the number output from the hash function is the ASCII encoding[^fn2] of the character at that digit's location. This is probably easier to understand through an example.

For a moment, let's pretend that `ord('1')` is not `49` but `1`. In fact, let's assume every digit between 0 and 9 has itself as its numerical encoding. What, then, is the hash of the string `'1983'`? It's just the number 1 followed by the number 9 followed by 8 and finally 3. Read together as a single number `1983`, these digits represent the number one thousand nine hundred and eighty-three.

Following this same logic with ASCII, in which each digit's encoding doesn't fit in a single base-10 digit but requires a base-256 digit (i.e., the encoding is 8-bits wide), `'1983'` becomes the number 49 followed by the number 57 followed by 56 and finally 51. Thinking about each of these digits in base-256 and then converting that number to base-10, the hash of `'1983'` is:

$((49 * 256 + 57) * 256 + 56) * 256 + 51 = 825,833,523$

You can experiment with this hashing algorithm using the following Python function called `simple_hash`.

```{code-block} python
---
lineno-start: 1
---
### chap10/simple_hash.py

def simple_hash(text):
    num = 0
    for i in range(len(text)):
        num = num * 256 + ord(text[i])
    return num

text = '1983'
print(f'{text} = {simple_hash(text)}')
```

## Updating a hash with O(1) work

There's a problem with this simple approach, which we'll get to in a moment, but let's first return to Rabin-Karp. Inside the matching loop, it uses the if-statement to compare the hash for the pattern string against a hash of an `m`-character substring of the text string. We could use `simple_hash` to calculate the hash of the pattern string without affecting our goal computational complexity measure because the pattern string remains the same for the entire execution of the matching loop. It is a value that is called *loop invariant*, and as such, we can compute the hash of the pattern string before we enter the matching loop. This is exactly one of the $O(m)$ terms we noticed in the last chapter in the pre-processing code of `rk_strmatch`.

But what about the substring of text to which we compare the pattern string? It changes from one iteration of the matching loop to the next. Luckily, there's a very simple pattern to how it changes.

To understand this pattern, first recognize that the matching loop begins with the first $m$ characters of the text string. As we did with the pattern string, we can compute the text string's starting hash before we enter the matching loop. It falls under the same $O(m)$ term that we computed for the pattern string's hash since both can be done within the same preprocessing loop.

Then comes the repeating pattern: Given a text substring of $m$ characters, the next substring to consider removes the leftmost character in the substring and adds a new character to the right-hand side of the substring. This action corresponds to the sliding of the pattern-string template one character to the right for each matching loop iteration, as shown in {numref}`Figure %s<c10_fig2_ref>`.

```{figure} images/Smith_fig_10-02.png
:name: c10_fig2_ref

To update a hash, our script must do three things: (1) remove the most significant digit; (2) shift up by one digit the significance of the remaining digits; and (3) add in a new least significant digit.
```

So how do we numerically adjust our simple hash knowing that we have to slice off the most significant digit, shift the remaining digits, and append a new least significant digit? Well, in the make-believe world where ASCII characters `'0'` through `'9'` are encoded with their numerical values, we turn the hash for `'1983'` (i.e., `1983`) into the hash for `'9834'` (i.e., `9834`) in the following way:

$(1983 - (1 * 10^3)) * 10 + 4 = 9834$

With real ASCII encodings and a pattern string of length `m`, the function `update_hash` implements this as an algorithm that we can repeatedly use on each iteration of the matching loop. Please note that the function computes the magnitude of a one in the most significant digit each time it is called, but this is also a fixed value once we know the size of the pattern string. As such, Rabin-Karp puts this calculation in its preprocessing code, which is the other $O(m-1)$ computational complexity cost that we had computed for preprocessing.

```{code-block} python
---
lineno-start: 8
---
### chap10/simple_hash.py
def update_hash(text, s, m, num):
    # Magnitude of the most-significant digit
    mag = 1
    for i in range(m-1):
        mag *= 256
    
    # Take off most-significant digit
    num = num - ord(text[s]) * mag
    
    # Add on a new least-significant digit
    num = num * 256 + ord(text[s + m])
    
    return num
```

Let's test this approach using an expansion of our running example.

```{code-block} python
---
lineno-start: 28
---
### chap10/simple_hash.py
text = '77719834777'
s = 3    # Pretend we're in the middle of the match loop
m = 4    # and the pattern string is 4 characters long
num = simple_hash(text[s:s+m])
print(f'{text[s:s+m]} = {num}')

num = update_hash(text, s, m, num)
print(f'updated hash = {num}')

s += 1   # Verify we got the right answer
num = simple_hash(text[s:s+m])
print(f'{text[s:s+m]} = {num}')
```

The following block of code puts together what we've discussed into something resembling the Rabin-Karp method.

```{code-block} python
---
lineno-start: 1
---
def rk_strmatch_partial(t, p):
    n = len(t)
    m = len(p)
    
    # Preprocessing steps
    
    # Constants in Rabin-Karp string-matching problem
    d = 256    # number of character encodings in ASCII
    
    # Compute the hash value of a 1 in the high-order position
    # (i.e., m-1th position), where digits have radix d
    hh = 1
    for i in range(m - 1):
        hh = hh * d
    
    # Calculate the hash values for p and t[0:m], since the
    # matching loop needs these values as it starts
    hp = 0
    ht = 0
    for i in range(m):
        hp = (hp * d) + ord(p[i])
        ht = (ht * d) + ord(t[i])
    
    # Matching step
    for s in range(n - m + 1):
        if hp == ht:
            print(f'Pattern occurs with shift {s}')
        
        if s < n - m:
            # Need to compute hash for next iteration
            ht = (ht - (ord(t[s]) * hh)) * d + ord(t[s+m])
```

## Allow collisions

The view is beautiful from up here, but Houston, we have a problem. This simple hashing function requires that the size of the space of hash numbers is the same as the size of the space of our pattern strings, which we'd like to be infinite. Let me make this size problem concrete for you with an example.

```{code-block} python
---
lineno-start: 41
---
### chap10/simple_hash.py
text = 'one thousand nine hundred and eighty-three'
print(f'{text} = {simple_hash(text)}')
```

Thank goodness Python doesn't put a bound on the size of the integers that it allows us to express! But we can't pretend that this really big number is any less expensive to process than the original string. We need to update the specification for our hashing function.

We want a hash function that maps an arbitrary-length string into a reasonably small range of numbers. When I say reasonable, I mean a set of numbers that requires nothing more than a single machine operation to do an equality comparison. We don't know much about machine operations at this point, but whatever we pick shouldn't create too big a number when running the types of operations we have in `rk_strmatch_partial`. Do you remember from the end of Act I that my machine could quickly count to several billion? We're probably safe if the results of the multiplications in `rk_strmatch_partial` don't exceed this number.

But the function `simple_hash` doesn't just multiply a number by 256. It repeatedly multiples a number by 256 (then adds a little bit) for `m` iterations. We need something that correctly handles multiplication as an operation but also puts a bound on the number of digits (really bits) that will be required to represent each result. By this point, you have probably guessed that we can use *modular arithmetic* to accomplish this goal.[^fn3]

We're almost back to the implementation for `rk_strmatch` that we presented in the last chapter. We reprint it here so that you can more easily follow along with the final bits of the explanation.[^fn4]

```{code-block} python
---
lineno-start: 13
---
### chap10/rk_strmatch.py
def rk_strmatch(t, p):
    n = len(t)
    m = len(p)
    
    # Preprocessing steps
    
    # Constants in Rabin-Karp string-matching problem
    d = 256    # number of character encodings in ASCII
    q = 65537  # a prime number
    
    # Compute the hash value of a 1 in the high-order position
    # (i.e., m-1th position), where digits have radix d
    hh = 1
    for i in range(m - 1):
        hh = (hh * d) % q
    
    # Calculate the hash values for p and t[0:m], since the
    # matching loop needs these values as it starts
    hp = 0
    ht = 0
    for i in range(m):
        hp = ((hp * d) + ord(p[i])) % q
        ht = ((ht * d) + ord(t[i])) % q
    
    # Matching step
    for s in range(n - m + 1):
        if hp == ht:
            # Verify that this is an actual match
            if p[0:m] == t[s:s+m]:
                print(f'Pattern occurs with shift {s}')
        
        if s < n - m:
            # Need to compute hash for next iteration
            ht = ((ht - (ord(t[s]) * hh)) * d
                  + ord(t[s+m])) % q
            if ht < 0:
                ht += q
```

In the preprocessing work for `rk_strmatch`, we set the variable `q` to the prime number `65537`. We could have used any prime number of reasonable size as the modulus for our modular arithmetic. The use of a prime modulus helps to more evenly spread the numbers generated by our hash function around the hash space. The actual spread depends on the strings you choose to hash[^fn5] at any particular time, but assuming some randomness in the input strings, a prime modulus helps pull that randomness into the hash space.

Let's update our specification (see {numref}`Figure %s<c10_fig3_ref>`) with what we've done.

```{figure} images/Smith_fig_10-03.png
:name: c10_fig3_ref

An updated specification for hashing that limits the range of integers produced. When two strings map to the same integer, it's called a hash collision (not shown).
```

Although we hope it happens infrequently, it is entirely possible that two (or more) input strings will hash to the same number. This is called a *hash collision*. Hope, of course, is not a problem-solving strategy, and any code that uses hashes as a shorthand for arbitrary-length strings must check when two hashes match. In particular, we must check that the two strings from which these hashes originated are in fact the same. If they aren't, we simply found a *collision* in the hash space.

Handling collisions is the purpose of the if-statement on line 42 in the code block above. Notice that, when the algorithm encounters a lot of hash collisions, its behavior reverts to the worst-case computational complexity of `bf_strmatch`.

You might wonder why the code only does something extra when it finds two hashes that match. What about the case when the two hashes don't match? Will we ever have a problem? Absolutely not. When two hashes are different, then the strings from which they were generated must have been different.

In summary, using a hash function that does a good job of mapping arbitrary-length strings to a fixed range of numbers with few collisions will allow the running time of `rk_strmatch` to approximate $O(n)$. 

## Other applications of hashing

We talked about hashing in the domain of string matching and, in particular, as a way of creating a *fingerprint* or small summary of a large amount of data. Creating such fingerprints has been found to be broadly useful beyond string matching. For example, we might use hashing to create a fingerprint of a large block of data that we wish to send across a computer network and on which we know there is a non-zero probability that some bits might not be transmitted properly. While similar in concept to the string-matching problem we just discussed, this problem domain has different requirements.

In this problem domain, we don't need a function that produces hashes that we can incrementally update as we scan across the data. In a typical scenario, a sender scans the data once and produces a hash. This sender then sends both the data and its hash across the network to a receiver. When the receiver has all the data, it computes the hash of the data it received and compares this hash against the one sent by the sender. If the two match, it is highly probable that the data and the hash were transmitted without any errors. However, if the two hashes don't match, the receiver knows that either the data or its hash was corrupted in transit, and it will request the sender to resend the data. Because we can't rule out a transmission error, it is even more important in this problem domain than in string matching to reduce the probability of a collision. This is just one example of how different problem domains can influence the prioritization of the properties and metrics of your solution.

Although we know a lot about string matching now, we still haven't found a solution to our simplified Google search problem that executes with a delay that a human would find acceptable. To solve this problem, we need to discuss yet another application of hashing.

## Indices for fast data retrieval

You know how a book index works: Given a keyword, you use the index to find a listing of the pages in the book that contain the keyword. We humans still must search the book index for the keyword, but once we have found the entry, we are directly rewarded with the pages on which we can find the keyword. This is how you should think of Google's search index. Google pulls from the string we type into its search box a set of keywords, and then it uses these keywords to quickly look up in its index the webpages that contain them.

Of course, Google's search index is going to contain a huge number of keywords because it indexes a huge number of webpages. How does Google quickly find the entry for our keywords among all the keywords in its index? You guessed it---through the power of hashing, as illustrated in {numref}`Figure %s<c10_fig4_ref>`.

```{figure} images/Smith_fig_10-04.png
:name: c10_fig4_ref

A simplified diagram of how Google goes from a string in its search box to a listing of webpages containing one or more keywords from the string.
```

## Hash tables

A hash function is a procedure that turns the unbounded space of all strings into a bounded range of integers. Built on top of this procedure, a *hash table* is a data structure that associates a storage location with each integer that the hash function can produce. The hash function plus "search index" block in {numref}`Figure %s<c10_fig4_ref>` is an example of a hash table.

Hash tables are a type of *associative array*, which is an *abstract data type*. The adjective "abstract" in this term emphasizes the fact that all we know about an associative array is its behavior (i.e., that it maps keys to values). We understand nothing about the runtime performance of its operations (e.g., how long it takes to get a value from a key).

A hash table, however, is more than the associative-array abstraction; it is a *concrete data type* with specific runtime performance goals for its individual operations. You use a hash table when you want an associative array that can quickly insert an item, delete an item, and search for an item. Stated in terms of computational complexity, each of these operations take $O(1)$ time (i.e., constant time) in a hash table.

In Python, the dictionary data type (`dict`) is a hash table data structure built into the language. You may recall that we used this data type in Chapter 4 to associate the values we wanted with the keys that described a query string (e.g., the value `'json'` with the key `'format'`).

## The speed of array-index operations

You might be saying, "Wait? What's going on in the hash table implementation that guarantees this performance?"

Let's step back and recognize that the search-index data structure we drew in {numref}`Figure %s<c10_fig4_ref>` looks a lot like the sequences and arrays we drew in Act I (e.g., {numref}`Figure %s<c02_fig3_ref>`). They were just a bunch of boxes placed one after another, and we used the square-bracket syntax (`[]`) to indicate which box contained the content we wanted. For example, if we wanted the fifth character in `the_line`, we just read from `CatInTheHat.txt`, we got it by typing `the_line[4]`.

To this point, you probably haven't thought about how long it takes your machine to grab the fifth character in a string like `the_line`. Does the machine start from the beginning of the string (i.e., `the_line[0]`) in the computer's memory, count five characters forward, and then return the character at that fifth location? If it did, then every index into a string would take time proportional to its location in the string.

It clearly doesn't, otherwise I wouldn't be raising this point. The beauty of our computers' memories, where our scripts execute, are that they are *random-access memories (RAM) indexed by byte addresses*. In a random-access memory, if you present the memory with a byte address (i.e., an index measured in bytes from the start of the memory), it will proceed immediately to read that location and give you back what's stored there (assuming that the address doesn't exceed the memory's size). There is no search involved; it is *direct access*.

Returning to our string example, to directly access a character in a string, the Python interpreter simply needs to know:

* the address of where a string starts in memory (i.e., the byte address of its first character);
* the size of a character in bytes in this string type (i.e., does the string just store 1-byte ASCII characters or something larger?), and
* the index of the character the programmer wants (i.e., the number within the square brackets).

The Python interpreter multiplies the index by the size of a character and adds this result to the starting address of the string to generate the address in memory where it can find the character you desire. A few quick pieces of simple math followed by a RAM access and you have your value.

```{code-block} python
byte_address = index * size_of(character) + starting_byte_address
```

To implement a hash table with insertion, deletion, and search operations that execute in constant time, we create an array (let's call it `hash_table`) as large as the hash space we need. Each location in this array will be a fixed number of bytes (let's call it `sz`) large enough to hold the largest value that we want associated with the hash table keys. Then, when a script asks for `hash_table[5]`, for example, the hash table code computes the memory address of the value we want by adding the address of `hash_table[0]` and `5` times `sz`. With this address, it directly accesses the memory location containing the value. Constant-time lookup. Insertion and deletion involve a similar set of operations.

## With high probability

A good hash table does two things: (1) computes hashes quickly and (2) minimizes the probability of a hash collision between two different inputs (e.g., keys in a Python dictionary). But because hash functions do not guarantee that the hashes for different inputs won't collide, hash tables must be able to determine which input is associated with the value stored at each table location. This requirement means that most implementations of hash tables store both the value and the input (that was hashed) in a hash table entry. It is then easy to do the comparison-check that avoids a wrong value being returned on a hash collision.

But this is where things get tricky. Let's assume that our hash table uses strings as keys. How long does it take to do string matching to verify that the stored value is associated with the input key? Aargh! We know this. In the best case, it is proportional to the length of the string. If we do brute-force string matching during our hash table lookup, the computational complexity of the operation is not $O(1)$, but $O(sizeOfTheInputString)$.

This annoying wrinkle is why most hash table implementations, including dictionaries in Python, require you to use immutable types for the hash table keys.[^fn6] Briefly, an object of an immutable type allows the Python interpreter to use the starting address of that object in memory as a small hash of the object. The cases we would need to enumerate to convince ourselves that this comparison suffices to identify a hash collision would make this description even more tedious than it has already become. And luckily, we're not here to build a hash table data type---just to use one and understand when we'll get good performance from it.

## Collision resolution

As much as I'd like to end at this point and complete the design of our book index, I must highlight one more detail that I glossed over in the preceding discussion: *What happens when we want to store two key-value pairs in our hash table where both keys hash to the same location in the hash table array?*

Not allowing such a thing to happen would break the desired behavior of the hash table as an associative array. Silently throwing away the previously stored key-value pair in that location is equally unacceptable. We must find two locations to store these two different key-value pairs that map to the same hash, and we need to have a process for searching all the possible locations when our first array lookup fails (i.e., the lookup key doesn't match the stored key).

There are several competing methods with different trade-offs for solving what's called *collision resolution* in hash tables. Two popular techniques are *separate chaining*, which uses extra storage outside the hash table array to hold the key-value pairs involved in collisions, and *open addressing*, which forces collisions into the currently unoccupied array locations (i.e., it uses no extra storage). Neither technique is superior over the other in all problem contexts. If you dive deeper into algorithms and data structures, you'll learn how to implement these schemes and learn the conditions under which one outperforms than the other.

To be clear, none of these techniques overcome the worst-case computational complexity of a hash table lookup. It is always possible, although unlikely with a good hash function, that all your keys produce the same hash. In the worst-case behavior, a table lookup has $O(n)$ work to do to find the key-value pair you desire, assuming you put $n$ key-value pairs into the hash table before this lookup.

## Specification for creating a book index

Enough about hash tables, their design, and their computational complexity. Let's use one to solve our book index problem. As I mentioned earlier, solving this problem will give us a flavor for what Google does to build its search index.

Our script will implement the following specification: *Given a simple text file and a special character sequence at the beginning of a line that indicates when we're moving from one reference unit to the next, print a sorted list of word-references pairs.* Because there are many small words in human languages that are not something we typically include in a book index, our implementation will also ignore any words in the text that are shorter than some character bound.

## Building top-down

Let's begin our design by deciding that we'll build the book index by processing our book a single line at a time, which is what we did to read a book in Chapter 1. It's often good to start with a script structure around which we have some experience. We can always change our mind and adjust the design once we gain further insights into the work we must do.

Let's also decide that we'll use a Python dictionary to store the word-references pairs as we process each line in the input text. But what exactly is a word-references pair?

Starting with the first part of this pair, our brains recognize words in a line of text, but what code should we write to extract words from a line of text? Unfortunately, this is not an easy question to answer because some characters, like an apostrophe, are not only punctuation marks (i.e., single quotes) but also parts of a word (e.g., in the contraction "isn't"). We'll "solve" this challenge by using a piece of code that we don't yet understand to create a word list from a line of text. We'll call this function `get_wordlist`, and it will take a line of text and return a Python list where each element in the list is a word.[^fn7]

What about this "references" thing in the other half of the word-references pair? Let's agree that this is a Python list of the book's unit numbers in which you can find one or more instances of the word in the word-references pair. If the units we want referenced in the book index are page numbers, this list would contain the list of pages on which you can find one or more instances of the word. If the book's units are chapter numbers, this list would contain the list of chapters in which you can find one or more instances of the word. And so forth.

Great! With these decisions made, we can start writing pseudocode.[^fn8]

```{code-block} python
---
lineno-start: 1
---
# Set book-unit pattern and initialize book-unit counter
# Read the input book as one long string
# Create an empty Python dictionary as our book index
# 
# for each line in the book:
#     if line starts with book-unit pattern:
#         increment book-unit counter
#     create wordlist from line
#     update book index given current book index, wordlist, book-unit counter
# 
# print out book index
```

Our pseudocode begins with a few initializations, and then it processes the book a line at a time, adding to the Python dictionary the words in each line. Although the pseudocode doesn't say it, we add only words greater than the minimum word length. We'll have to remember to do that in our update function.

There's nothing in this pseudocode that we haven't already learned how to do. So let's jump right to a script that implements much of what it describes.

```{code-block} python
---
lineno-start: 1
---
### chap10/index0.py
import sys
import re
import string

# Instead of asking the user for this information on each run of
# the program, we'll make them global constants. In production
# code, we'd define this information in a configuration file,
# which is a better way to have our user infrequently change what
# are relatively stable constants.
MIN_LEN = 4           # don't index any words shorter than 4 chars
UNIT_PAT = ''         # for pages in CatInTheHat.txt
UNIT_CNT_INIT = 1
'''
UNIT_PAT = 'CHAPTER'  # for chapters in JustDavid-chaps.txt
UNIT_CNT_INIT = 0
'''

# The magic function that turns a line into a wordlist
def get_wordlist(line):
    line = line.replace('--', ' ')
    return [re.sub('^[{0}]+|[{0}]+$'.format(string.punctuation),
                   '', w) for w in line.split()]

# An unfortunately difficult check because of empty lines as
# a unit break in CatInTheHat.txt
def found_new_unit(line):
    if UNIT_PAT == '':
        return UNIT_PAT == line
    else:
        return UNIT_PAT == line[0:len(UNIT_PAT)]

def update_index(d, wordlist, unitno):
    # Needs to be written
    return d

def build_index(txt):
    # Start with an empty dictionary
    d = {}
    
    # Iterate through each line watching for book-unit boundaries
    unitno = UNIT_CNT_INIT
    for line in txt.split('\n'):
        if found_new_unit(line):
            unitno += 1
        else:
            d = update_index(d, get_wordlist(line), unitno)
    
    # Print out the index
    print(d)

def main():
    # Check for proper usage and grab the input strings
    if len(sys.argv) == 1:
        txt = sys.stdin.read()
    elif len(sys.argv) == 2:
        with open(sys.argv[1]) as f:
            txt = f.read()
    else:
        print("Usage: python3 index0.py book.txt")
        print("   Or: python3 index0.py < book.txt")
        sys.exit()
    
    build_index(txt)

if __name__ == '__main__':
    main()
```

This script pulls out the work we need to do to update the dictionary (i.e., `update_index`, which is called on each iteration of the loop in `build_index`), since we haven't yet written that pseudocode. Right now, the function `update_index` simply returns the dictionary passed to it.

The only other piece of code worth highlighting is the one that implements the function `found_new_unit`. In one of the two text files that we'll use to test our script, `CatInTheHat.txt` uses blank lines to indicate the end of a page. Python naturally does the right thing in comparing two strings of different lengths, even if you ask it for a "substring" that is larger in size than the actual string. The only exceptional case is when one of the two strings is the empty string (`''`). The function `found_new_unit` handles this special case as a special case.

```{admonition} You Try It

Can you figure out what value is produced by the equality test on line 31 in `found_new_unit` when `UNIT_PAT` is the empty string and `line` is `'This is NOT a blank line'`? When you do, you'll understand why we need the test on line 28.[^fn9]

```

## Updating the index

What is missing from our script is the implementation of `update_index` and the proper printing of the dictionary `d` that is our book index. Right now, `update_index` doesn't put any words into the dictionary, and our printing code simply prints the contents of the dictionary (line 50). Let's fill in `update_index` first, because we can test the core aspects of our implementation even without a proper printing routine.

The function `update_index` should do something for each word in `wordlist`. Let's consider the different cases:

* The simple case is when we find that the word is not in the dictionary (i.e., not yet in the index). In this case, we need to add the word and the value associated with it (i.e., `unitno`) to the dictionary. We'll store the `unitno` as the first element in a list because we might later find the hashed word in another unit of the book.
* When the word is already in the dictionary, we still have work to do. We must check if the current unit number (e.g., the page or chapter number on which we now find this word) is already in the stored list of units. If it is, we have nothing to do (e.g., we don't add the same page number twice). If it isn't, we need to add the current unit number to the end of the references list.

The following code implements this functionality using the methods of a Python dictionary.

```{code-block} python
---
lineno-start: 32
---
### chap10/index1.py
def update_index(d, wordlist, unitno):
    for word in wordlist:
        # Update our dictionary
        if word in d:
            if unitno not in d[word]:
                d[word].append(unitno)
        else:
            d[word] = [unitno]
    
    return d
```

Let's run some simple tests to see if this works and what we might have forgotten. We will use Python's ability to easily create multiline strings using triple quotes.

```{admonition} You Try It
You can run this and the rest of this chapter's tests in one of two ways:

*   Using the interactive Python interpreter, copy lines 1−50 of `index0.py` above and paste them at the interactive interpreter's prompt. This will define the functions and global constants for you. Then copy the two statements in the code block below and paste them at the interactive interpreter's prompt. As you continue with the chapter, update your interactive interpreter's environment with the new functions we write (e.g., `update_index`).
*   Using the files in the book's GitHub repository, you'd run `python3 index1.py` (or whichever `index` script you'd like to try). The script waits for you to type the `txt` input. You can copy the text between the triple quotes below and paste that in the shell. To end the `txt` input, type Ctrl+D.
```

```{code-block} python
---
lineno-start: 1
---
txt = '''"Now! Now! Have no fear.
Have no fear!" said the cat.
"My tricks are not bad,"
Said the Cat in the Hat.'''
build_index(txt)
```

Oops. When you run this code block, you'll note that we forgot to filter out words less than `MIN_LEN`. That's an easy fix.

```{code-block} python
---
lineno-start: 32
---
### chap10/index2.py
def update_index(d, wordlist, unitno):
    for word in wordlist:
        # Skip short words
        if len(word) < MIN_LEN:
            continue
        
        # Update our dictionary
        if word in d:
            if unitno not in d[word]:
                d[word].append(unitno)
        else:
            d[word] = [unitno]
    
    return d
```

```{admonition} You Try It
Make sure you rerun the previous test `txt` to ensure that we fixed the problem and didn't introduce any new ones.
```

That looks much better, except ... do we really want both `'said'` and `'Said'` separately in our dictionary? Probably not. We will use the `lower` method on strings to make sure it doesn't matter how a word was capitalized.

```{code-block} python
---
lineno-start: 32
---
### chap10/index3.py
def update_index(d, wordlist, unitno):
    for word in wordlist:
        # Skip short words
        if len(word) < MIN_LEN:
            continue
        
        # No capitals
        word = word.lower()
        
        # Update our dictionary
        if word in d:
            if unitno not in d[word]:
                d[word].append(unitno)
        else:
            d[word] = [unitno]
    
    return d
```

This time we will test with "multiple pages" in our input.

```{code-block} python
---
lineno-start: 1
---
txt = '''"Now! Now! Have no fear.
Have no fear!" said the cat.
"My tricks are not bad,"
Said the Cat in the Hat.

"Why, we can have
Lots of good fun, if you wish,
With a game that I call
UP-UP-UP with a fish!"

"Put me down!" said the fish.
"This is no fun at all!
Put me down!" said the fish.
"I do NOT wish to fall!"'''

build_index(txt)
```

This looks quite good, but notice that you cannot predict the order of the keys in a Python dictionary. When we called `update_index` only once (e.g., in our first test above), it looked like the words were stored in the dictionary in the order we inserted them, but in our last test, after several calls to `update_index`, this was no longer true.

## Sort and strip

OK, but we should print the index so that it looks like a book index. This isn't that hard, since Python dictionaries allow you to iterate through the keys in a dictionary and even use Python's built-in `sorted` function to sort the keys.

The only other thing we'll want to do is strip away the square brackets when we convert the list of references to a string. Here's the final version of `build_index` and a rerun of the last test.

```{code-block} python
---
lineno-start: 50
---
### chap10/index32.py
def build_index(txt):
    # Start with an empty dictionary
    d = {}
    
    # Iterate through each line watching for book-unit boundaries
    unitno = UNIT_CNT_INIT
    for line in txt.split('\n'):
        if found_new_unit(line):
            unitno += 1
        else:
            d = update_index(d, get_wordlist(line), unitno)
    
    # Print out the index
    for w in sorted(d):
        pages = str(d[w]).strip('[]')
        print(f'{w}: {pages}')
```

```{code-block} python
---
lineno-start: 1
---
# Test using the last value of `txt`
build_index(txt)
```

```{admonition} You Try It
Run `index32.py` with `JustDavid-chaps.txt`, which you can find in the book's GitHub repository for  Chapter 10. In `index32.py`, you'll want to remove the triple quotes on line 17 and insert a triple quotes between lines 11 and 12. This will disable the `UNIT_PAT` and `UNIT_CNT_INIT` definitions that are appropriate for `CatInTheHat.txt` and enable those for `JustDavid-chaps.txt`. Now run `python3 index32.py JustDavid-chaps.txt` at your shell prompt.
```

You now have a general idea how Google takes a search string and quickly returns a list of pages containing the keywords in our search. It simply looks up the search string's keywords in a big hash table, and each lookup takes constant time. None of this online work takes time proportional to the size of all the pages in the indexed web.

Of course, Google does more than just web search, as we'll discuss in the following chapters, but this is a great start. Congratulations!

[^fn1]: I've said we care about loops, and since this statement isn't a loop, computer scientists say that it takes *constant time*, which is expressed as *O*(1). What they're really saying is that the operation doesn't depend on the length of the input; it is constant time work, no matter what the input.

[^fn2]: Unicode (https://home.unicode.org) gives us an encoding space big enough to capture many of the different characters used in human communication. Without loss of generality and to simplify my examples, though, I will instead use the older ASCII encoding, which encodes characters in 8 bits or 1 byte.

[^fn3]: If you need a refresher on the rules in modular arithmetic, I recommend the Khan Academy tutorial. https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/what-is-modular-arithmetic

[^fn4]: I've left out the debugging code that you'll find in \`rk\_strmatch.py\`, which won't aid during the explanation but will help you in visualizing what the function does as it runs.

[^fn5]: Human languages are, of course, not random in their distribution of letters and letter sequences. However, we do the best we can to not make the problem worse.

[^fn6]: Python Software Foundation, "Dictionaries," accessed March 27, 2025. https://docs.python.org/3/tutorial/datastructures.html\#dictionaries

[^fn7]: The cryptic code in \`get\_wordlist\` uses regular expressions, which we'll cover in Chapter 13.

[^fn8]: Recall that our specification requires the user to specify how we'll recognize that we're moving from one book unit to the next and what number to use as the initialization of that count of units. You'll see this in the pseudocode.

[^fn9]: The expression \`len(UNIT\_PAT)\` is 0 when \`UNIT\_PAT\` is the empty string, which makes the slice in \`line\[0:len(UNIT\_PAT)\]\` a slice of nothing, which is the empty string. That's not what we want, and we need to handle the case when \`UNIT\_PAT\` is the empty string with different logic.