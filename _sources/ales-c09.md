## Chapter 9 ##

## ALE 9.1: Adds, ands, and algorithms

In Chapter 8, you learned that both arithmetic and bitwise operations operate on integers, but you probably wondered how these two operations differed. To understand the difference, let's think about each operation algorithmically.

**Step 1.** **Bitwise operators, algorithmically.** Bitwise operations, such as `&` and `|`, perform for each bit in its inputs an *independent* computation. Because of this independence, you can think of these as *n* simultaneous computations for two *n*-bit input numbers.

For example, the following code block performs a bitwise-or on two 4-bit quantities.  The block sets `i` to the value `12` in binary and `j` to `10`, and then prints the bitwise-or of these two quantities, which yields the value `14` (again, printed in binary).

```{code-block} python
---
lineno-start: 1
---
### chap09/ale01.py

# Example of bitwise OR on a sequence of bits
i = 0b1100
j = 0b1010
print(f'i     = {bin(i)}')
print(f'j     = {bin(j)}')
print( '--------------')
print(f'i | j = {bin(i | j)}')
```

Algorithmically, each bit in one variable is OR-ed with its column pair in the other variable. Line up the two input numbers as binary values and think of each vertical pair bits as a column. A two-bit OR operation outputs a `1` only when both of its inputs are `1` and `0` otherwise. Notice that each column computation is independent, i.e., its inputs come only from the column.

**Step 2.** **Arithmetic operators, algorithmically.** Arithmetic operations like add (`+`) operate differently. When we add two integers, we certainly add the value of the digit at each location in one variable to the value of the digit in the corresponding location in the other variable, but the result is not necessarily a single digit. We might add two single-digit numbers and generate a two-digit result.

For example, add 38 to 69. We add the one's digits first (i.e., 8 plus 9) and get the result 17. With this result, the 7 goes in the column with the 8 and 9, which we'll call the *column sum*. The 1 is called the *carry*; we carry it from the one's column and place it at the top of the ten's column. It becomes part of that column's addition (i.e., 1 plus 3 plus 6), which produces its own column sum and carry.

Notice that the computation in a column *can affect* the computation in the columns to its left. Do a couple of long-hand addition problems to see this algorithm in action.

**Step 3. Arithmetic on binary numbers.** Adding two binary numbers is algorithmically no different than adding two base-10 numbers. It involves a sum digit and a carry digit.

If you do some examples of addition on binary numbers you'll soon learn that the column sum of two binary digits is a `1` if either, but not both input bits, are a `1`. In general, a sum of multiple bits is a `1` if an odd number of the inputs are `1` and `0` otherwise.

The *exclusive-or* operator (`^`) is what we need to calculate the column sum of two bits. It is a bitwise operator. The code block below computes the column sum we need for arithmetic on binary numbers using this bitwise operator. Notice that the block prints the result in a manner that forces leading zeros.

```{code-block} python
---
lineno-start: 10
---
### chap09/ale01.py

# Example of bitwise EXCLUSIVE-OR on a sequence of bits
i = 0b1100
j = 0b1010
print(f'i     = {bin(i)}')
print(f'j     = {bin(j)}')
print( '--------------')
print(f'i ^ j = 0b{(i ^ j):04b}')
```

What about the carry of an add operation on two single-bit inputs? Well, when does an add with two 1-bit inputs generate a carry? Only when we add two ones: `1 + 1 = 2`, and `2` is `0b10`, a two-bit number with `0` as its column sum and `1` as its carry. 

So, a carry occurs when both inputs are `1`, and if either input is a `0`, then the carry is a `0`.  This description is the same as the description of bitwise-and!

The following code block computes sum and carry for each column in our input numbers.

```{code-block} python
---
lineno-start: 19
---
### chap09/ale01.py

# The two components of an add for each digit column
i = 0b1100
j = 0b1010
print(f'i     = {bin(i)}')
print(f'j     = {bin(j)}')
print( '--------------')
print(f'sum   = 0b{(i ^ j):04b}')
print(f'carry = 0b{(i & j):04b}')
```

But in an add operation, the work in one column isn't independent of the work in all other columns. We saw that in our long-hand addition example. To complete the add algorithm then, given two consecutive columns, we have to integrate the carry from the one on the right with the sum and carry calculations of the one on the left. This turns `sum` and `carry` from computations on two bits to computations on three bits:

* `sum` continues to use the exclusive-or operator, since it does what we want. The exclusive-or of three bits is `1` only when there are an odd number of `1` inputs. See line 49 of the next code block.
* `carry` becomes a more complex computation as we want to know if any two of the three input values are `1`. See line 50 of the next code block.

You now know what we need to do for each bit column when adding two *n*-bit numbers. We simply need to do this for every pair of consecutive columns, just as we did in long-hand addition. And we can use this same column logic for the right-most column (i.e., the ones column in long-hand addition) if we set `carry_in` to be `0` initially.

The next code block shows what it takes to implement addition (via the `add` function) without ever using Python's add operator! This function uses only bitwise operations and some "wiring" between the columns as we've described. This "wiring" (or the use of one column's result in the computation of the next column's result) makes addition and the other arithmetic operators different than bitwise operators.

Go through each line of this `add` function and make sure you understand the purpose and operation of each. Draw pictures and follow through the code with an example input (e.g., `a=3` and `b=6`).

```{code-block} python
---
lineno-start: 29
---
### chap09/ale01.py

# Perform addition without using the `+` operator
def add(a, b):
    # adds two 8-bit numbers
    assert a >= 0 and a < 256
    assert b >= 0 and b < 256

    # turn input integers into binary strings with leading 0s
    a = f'{a:08b}'
    b = f'{b:08b}'

    # setup for the addition
    c = ''        # the answer, eventually
    carry_in = 0  # the initial carry in

    # add without the `+` operator, which starts at the rightmost bit!
    for i in range(7, -1, -1):
        # get the integer value of the next bits to add
        a_i = int(a[i])
        b_i = int(b[i])

        # do the sum and carry logic
        sum = carry_in ^ a_i ^ b_i
        carry = (carry_in & a_i) | (carry_in & b_i) | (a_i & b_i)

        # concatenate this sum to our growing answer
        c = str(sum) + c    # string concatentation!

        # setup for next bit location
        carry_in = carry

    return int(c, base=2)
```

**Step 4. Test our adder.** Does the function `add` work correctly? Let's test it!

```{code-block} python
---
lineno-start: 61
---
### chap09/ale01.py

# Test the `add` function
a = 3
b = 6
c = add(a, b)
print(f'{a} + {b} = {c}')
assert c == a + b, "`add` failed"

a = 38
b = 69
c = add(a, b)
print(f'{a} + {b} = {c}')
assert c == a + b, "`add` failed"
```

That's addition using only bitwise operators, which is all we have in hardware!

## ALE 9.2: Which runs faster?

Now that you know how the hardware in our computers performs an add operation using nothing but simple bitwise operations and a bit of wiring between bit locations, let's focus on the major algorithmic differences between arithmetic and bitwise operators, and then think about the computational complexity of each.

* Bitwise operations (e.g., `&`) on multi-bit numbers can compute each bit location in parallel. No column's computation depends upon the result of another column's.
* Arithmetic operations (e.g., `+`) on multi-bit numbers canNOT compute each bit location in parallel. The results in one column *possibly* depend upon the results of the columns to its right. We see this through the `carry` calculation, where the `carry` in one column becomes the `carry_in` in the next to the left.

Can you state the computational complexity of the algorithms behind the bitwise-and (`&`) and addition (`+`) operators based on the size of their inputs in bits? Assuming that the inputs are of length $n$ bits:

* Is the algorithm for bitwise-and  $O(1)$ or $O(n)$?
* Is the algorithm for addition $O(1)$ or $O(n)$?

When you figure it out, given the same integer inputs, you'll see that bitwise operations are faster than arithmetic operations for large values of $n$. We don't experience that when our computers operate on the values in our scripts because $n$ is small for the integers we typically use.

## ALE 9.3: Not add, but multiply

While addition is more complex than bitwise-and, multiplication is much more complex than addition. Let's think about the work involved in multiplying two integers.

**Step 1.** Do the long-hand multiplication of two 3-digit integers by hand, and then pictorially compare that to the work in adding those same two integers. 

**Step 2.** Now look at the work you did and imagine an algorithm for multiplication that mimics your long-hand work.

If you learned to do long-hand multiplication like I did, you multiplied each digit in one number by each digit in the other. Each multiplication possibly included the addition of a carry, if the previous pair of digits generated a result with a tens-digit. This produced $n$ partial products given two inputs of length $n$ bits. To complete the multiplication, you have to add together these $n$ partial products.

Is this algorithm $O(1)$, $O(n)$ , or $O(n^2)$?

A lot of work has been done over the years to make multiplication fast, but you now have an intuitive understanding how the hardware in our computing systems does the work of our most basic abstractions like addition and multiplication!

\[Version 20240314\]