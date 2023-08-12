# Chapter 7: Many Numbers But Not Any Number

In the last chapter, we heard from Jill Lepore that counting was a key metric in industrialization and measurement a key driver of science, but we looked mostly at the computational data type for counting: the integer.

Clearly, counting isn't the only thing we do with numbers, and the world isn't nicely discrete in everything we do. In fact, we perceive the world as mostly *continuous*. For example, time doesn't just move forward in discrete seconds, but also in tenths, hundredths, and infinitely small pieces of a second. Similarly, we don't solely measure distance in an integral number of meters, but also in centimeters, millimeters, and infinitely small pieces of a meter. The same logic applies to measuring volume and weight and force and pretty much anything else we measure.

If we're going to use computers to help us do creative things and generate new knowledge in our physical world, we're going to need to represent and manipulate *measured values*. For instance, we will soon measure the running time of our scripts, and in this, we will look at not just the time in seconds, but also fractions of a second.

## Floating-point numbers and numerical computing

Measurements with fractional units, like 6.574 seconds, are described as *real numbers*, and Python uses a datatype called `float` to represent such numbers. This name is a shorthand for *floating point (FP)*, [a standardized representation and a type of computational arithmetic.](https://standards.ieee.org/standard/754-2019.html)

There are many details involved in this standard and peculiarities involved in using FP numbers to solve your problems. There are entire courses that will teach you how to use a computer to accurately solve problems involving continuous variables, like time and distance. This area of computer science comes under many names, including *numerical computing* and *scientific computing*, and the techniques taught will help you to use computers to study the range of physical and social phenomena in the world around us.

```{admonition} Learning Outcomes
In this chapter, you will learn how computers represent numbers with fractional components, which is an important datatype if you're going write script that help you solve problems involving measured values. By the end of the chapter, you will be able to:

*   Understand the floating-point (FP) number standard and how computers use it to represent real numbers [CS concepts];
*   Explain the difference between applications that require integers and those that require FP numbers [design and CS concepts];
*   Discuss the difficulty computers have representing some real numbers [CS concepts and programming skills].
```

## Computers struggle with arithmetic?

We won't attempt to cover the breadth of numerical algorithms, but answer a fundamental question at the heart of numerical computing and in line with our efforts to understand how computers operate: *If computers see everything in the world as a collection of numbers and they've been constructed to do computation on numbers, why are some types of computation hard to do on a computer?*

While this might seem paradoxical, this problem occurs because any compact representation of real numbers needs to make a tradeoff between *range* (i.e., how large is the difference between the biggest and smallest real numbers that we can represent?) and *precision* (i.e., how close is a real number we can represent to the real number we want to represent?).

We first thought about the issue of range in an active-learning exercise in Chapter 6, when we investigated whether addition on Python integers would ever overflow the programming language's physical representation. While Python doesn't place a limit on the largest and smallest representable integer values (i.e., it has a dynamically expandable representation), most programming languages have a clearly stated limit on these values for each of their defined integer datatypes. And for FP numbers, the same is true in Python.

Python adheres to the FP standard representation IEEE 754. As we'll see in a moment, the range defined by this standard, while not infinite, is comfortably big. In other words, most of us won't write scripts involving real numbers that will overflow this FP representation.

Unfortunately, the same cannot be said of precision. You'll almost certainly write scripts that will have to deal with the issue of FP precision. Precision is, informally, a measure of how close the computer's representation of a real number is to its mathematical value. Sometimes the two are the same, but too often they're not.

I'll demonstrate with a computation we all learned to do when we were around 8 years old: What is the square of 0.1? You and I know the answer to be 0.01, but let's ask Python to do this computation for us:

```{code-block} python
---
lineno-start: 1
---
n = 0.1
n * n == 0.01
```

When you run this code block, it should surprise you that the value you know to be n-squared for `n = 0.1` is not the result of the multiplication of that number with itself. How can our fancy computers get the wrong answer to this simple arithmetic problem?!? Well, let's find out by writing a script to understand how the FP standard represents real numbers like `0.1`.

## The range of a FP number

Floating point represents a real number in the form of a *significand* times a *base* raised to an *exponent*, where the significand and the exponent are integers stored directly in the FP representation. The base is an integer greater than or equal to two. This base is defined in the standard and not stored explicitly in the representation. To know what real number is expressed by a particular FP representation, however, you also have to know where the representation assumes that the number's *radix point* (more commonly called a decimal point for base-10 numbers) is placed.

For example, we could encode 6.574 as follows:

```{code-block} python
6574 * 10**-3
```

where `6574` is the significand, `-3` is the exponent, `10` is the assumed base, and the radix point is assumed to sit just to the right of the rightmost digit of the significand.

The range of a FP representation depends on the range of integers we can store in that representation's exponent field. The IEEE 754 standard for *single-precision* FP numbers, for example, has an 8-bit exponent field, and the exponent field in its *double-precision* encoding is 11 bits. Clearly 11 is not twice 8, and the term "double precision" comes from the rough doubling of the size of the significand field (from 23 to 52 bits). Each encoding also includes a sign bit, which indicates the sign of the significand (not the exponent), and from this you can compute that double-precision FP numbers require twice as many bits as single-precision ones in the IEEE 754 standard.

The range of these two representations is impressive. While the single-precision representation can't represent the estimated number of atoms in the known universe, which somewhere between `10**78` and `10**82`, the double-precision representation with a maximum base-10 exponent of 308 easily can. You can also measure things as microscopically small as this maximum number is astronomically large.

## Precision

But what about precision? In base 10, we understand the fractional part of a real number as the group of digits to the right of the decimal point. The value of each of these digits to the total fraction is the digit multiplied by `10**-m`, where `m` is 1 for the digit immediately to the right of the decimal point, 2 for the next digit to the right, and so forth.

This is that expansion for the decimal fraction 0.574:

```{code-block} python
5 * 10**-1 + 7 * 10**-2 + 4 * 10**-3
```

Run this code block, and you'll notice something strange. To explain, let's move from base-10 numbers to binary (base-2) numbers because jump is where things get difficult.

A binary fraction can be expanded in the same way as a decimal fraction, except that we change the base from 10 to 2, and the coefficient of each term is either 0 or 1. Pulling these coefficients together creates the binary string that we'll use as the significand. And here's where precision comes into play: The difference between the significand we want in base-10 and the closest one we can represent in base-2 is a measure of precision in our format.

```{margin} Notation
The leading `0b` mimics the leading `0x` we saw on hexadecimal numbers. Following this prefix, we write a `0`, which indicates that there's no whole number here, and the radix point, which always proceeds the fractional part.
```

## Illustrating this issue of precision

We can get a sense for precision by writing a Python program that constructs the binary representation for a decimal fraction. Our script will prefix the answer we compute with the string `'0b0.'`, which will help us to remember that this is the binary representation of a fraction between 0 and 1.

Our script will expect two inputs. The first is the decimal fraction we want to represent and the second is the size in bits of our chosen FP representation's significand. If we wanted to mimic the precision of IEEE 754 single-precision, we'd input 23. The following script gets us started.

```{code-block} python
---
lineno-start: 1
---
'''Convert a decimal fraction into a binary string'''
import sys

# Grab input values
n = float(input("Decimal fraction to convert: "))
bits = int(input("Size of significand in bits: "))

# Error checking
if n < 0. or n >= 1.:
    sys.exit("ValueError: target must be in the range [0., 1.)")
```

Notice that we have to use the Python `float` type to represent the inputted decimal fraction. The `float` function converts a string that looks like a real number into a `float`.

To figure out what we need to do next, let's think about what we know at the start of our script:

```{margin} FP Literals
Notice that our error check in the script checks `n` against two FP literals. `0` is a Python integer. `0.` and `0.0` both represent zero as a FP number (and so is any number of zeros after the decimal point).
```

1. We know that the number the user inputted is between 0 and 1, including the value of 0 and excluding the value of 1, as our error check asserts.
2. If we think about our best guess at the binary encoding of `n` given these bounds and no significand bits, we would choose 0, since we know that it can't be 1.

This answer isn't precise, unless the user inputs the value `0.0`, but it's the best guess we have with zero significand bits in our FP representation. Hey, we have started to solve our problem! Let's write this in Python at the end of our growing script.

```{code-block} python
---
lineno-start: 12
---
# Initialize our bounds and our current best estimate
upper = 1.
lower = 0.
best = lower  # with 0 bits used, our best estimate is 0
encoding = ''
```

## One bit at at time

What would we do next if we wanted to add one bit to our significand? Since we have no bits in our significand, that bit would be the coefficient of the first term in the expansion we discussed earlier. Do we want this coefficient to be `0` or `1`? Well, which of these two values creates a new guess that is closer to the actual number `n` than our current guess? The one that is closer becomes our new best guess.

If we're updating our best guess as we add bits to our significand, this starts to sound like we're building a loop. As we know, loops have exit conditions, and the exit condition in this case is when the best guess is exactly `n`. If our current best guess isn't exactly `n`, then we loop to again add new bit to the significand, which will bring us closer to the input decimal fraction. Of course, we also have to exit this loop if we run out of significand bits.

```{admonition} You Try It
Before reading on, practice writing the pseudocode for the loop I just described. What type of loop might you use (i.e., a for- or a while-loop)? What's the condition we'll test that protects the break-statement in the loop?
```

## Searching for the smallest difference

As we slowly add bits to the righthand side of the significand, we increase the precision of our binary encoding by reducing the difference between the input decimal fraction `n` and our current `best` guess, which is a binary fraction that we'll store in `encoding`. The complete script is below.

I used a for-loop since we know the maximum number of bits in our significand; the loop must end at that maximum and a for-loop makes this easy to do and to understand. You could build an equivalent solution with a while-loop.

Inside the for-loop, I check to see if our best guess (`best`) matches `n`, and break out of the loop early if it is. Besides updating the string encoding of the significand, the loop body adjusts the bounds (i.e., `upper` and `lower`) as it updates `guess`, since every bit we add to the significand tightens one or the other of the two bounds.

```{code-block} python
---
lineno-start: 1
---
### chap07/fbin.py
'''Convert a decimal fraction into a binary string'''
import sys

# Grab input values
n = float(input("Decimal fraction to convert: "))
bits = int(input("Size of significand in bits: "))

# Error checking
if n < 0. or n >= 1.:
    sys.exit("ValueError: target must be in the range [0., 1.)")

# Initialize our bounds and our current best estimate
upper = 1.
lower = 0.
best = lower  # with 0 bits used, our best estimate is 0
encoding = ''

for i in range(1, bits+1):
    # Our new estimate always adds a least-significant one bit to the end
    # of our current lower bound, i.e., move up the value below the
    # target number.
    new_guess = lower + (2. ** -i)

    # Update the correct bound, depending upon whether the new estimate
    # overshot the target or not.
    if new_guess <= n:
        lower = new_guess
        encoding += '1'
    else:
        upper = new_guess
        encoding += '0'

    # And now update which of these is our current best estimate
    if n - lower < upper - n:
        best = lower
    else:
        best = upper

    if best == n:
        break

    # Print how we've done so far
    units = 'bit' if i == 1 else 'bits'
    print(f'With {i} {units}, {n} is between {lower} and {upper}')

print(f"\nWith maximum {bits} bits of encoding for n = {n},")
if best == n:
    print(f"  the EXACT encoding is '0b0.{encoding}'")
else:
    print(f"  the closest encoding is '0b0.{encoding}'")
print(f"  which has a decimal value of {best}")
```

Run this code block with the inputs of `0.574` and `23`, you'll see that the encoding for the significand is `'0b0.10010010111100011010100'`, which has a decimal value of `0.5740000009536743`. That's not `0.574`, but pretty close. The difference is the precision error.

```{margin} Print Rounds
If you set `n = 0.1` and then print `n` in the interactive Python interpreter, it will tell you that `n` is `0.1`. As [this tutorial page](https://docs.python.org/3/tutorial/floatingpoint.html) says, Python sometimes rounds what it prints. What it displays and what it stores may be different when dealing with FP numbers!
```

## FP errors accumulate

Unfortunately, precision doesn't just effect our ability to encode a real number as a FP number. FP operations make matters worse by accumulating the inaccuracies in representation. This is why `0.1 * 0.1` doesn't equal `0.01`. A slightly inaccurate FP representation of `0.1` times itself produces a result with more inaccuracy than the conversion of `0.01` into a FP number. Or stated another way, the computer doesn't know if you were trying to multiple `0.1` times itself or `0.10000002384185791` times itself, since single-precision FP represents both base-10 numbers with the same string of binary bits.

If your script operates with floating-point numbers and you need the results to be precise, you need to understand what operations you can safely perform and which you cannot. This is what textbooks and courses on numerical computing will teach you. The Wikipedia page on [accuracy problems in floating-point arithmetic](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Accuracy_problemshttps://en.wikipedia.org/wiki/Floating-point_arithmetic%23Accuracy_problems) can get you started on this tricky subject.

As if it wasn't hard enough to write a correct program!

\[Version 20230811\]