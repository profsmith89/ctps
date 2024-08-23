# Chapter 10 #

## ALE 10.1: Are these data still good?

Whenever we send a message across a network or leave our data on a storage device, there's a non-zero chance that a real-world event (e.g., an electrical spike from a lightning strike) will cause one or more of data bits to flip their value (i.e., from a 1 to a 0 or a 0 to a 1). A common method for noticing when such data has been corrupted is to pair it with a hash of all the bits in a block of data. Computer scientists often refer to such a hash as a *checksum*, and in this ALE, you'll learn about and implement a historically popular one that requires just one extra bit per data block: CRC-1.

**Step 1.** CRC stands for *Cyclic Redundancy Check*, and it is a *non-cryptographic hash function*, which means that you can use it for error detection but not for any meaningful security purpose. CRC-1 is a fancy name for adding a single *parity bit* to the end of a short block of data. Because it is easy and space efficient to compute, the generation of a parity bit is often implemented directly in hardware. If a computer's memory includes parity bits, the hardware can alert the operating system when a running program tries to read a piece of memory that has been corrupted.

CRC-1 is called a parity bit because you are just recording a fact about the number of bits in the data that are a 1. Obviously with one parity bit, we can't record the number of 1-bits in a data block of length greater than 1, and so the parity bit simply records whether the number of 1-bits is an even or odd number. Because we can encode this even/odd fact in one of two ways (e.g., the parity bit being equal to 0 means either that the number of 1-bits is even or that the number of 1-bits is odd), the world defines CRC-1 either as an *even-parity* implementation or an *odd-parity* one.

In both implementations, you count the number of 1-bits in both the data block and the parity bit. *Even parity* sets the parity bit to 1 when that makes the total number of 1-bits even, and *odd parity* sets the parity bit to 1 when that makes the total into an odd number.

Now let's test your understanding: What value would you give the parity bit when your data block contains no 1-bits and you've been told to implement even parity?

**Step 2.** Let's implement a function that takes one parameter, an integer, and computes and returns the *even parity* bit (i.e., the function returns `0` or `1`).

The function `crc1_cnt` in the following code block is one possible implementation. Make sure you understand the method it uses to count the number of 1-bits in the input number.

```{code-block} python
---
lineno-start: 1
---
### chap10/ale01.py

def crc1_cnt(n):
    cnt_of_1s = 0

    # Add 1 to the count of 1-bits when `n`
    # is odd. Use integer-division to shift
    # down a bit, continuing until `n` is 0.
    while n != 0:
        if n % 2 == 1:
            cnt_of_1s += 1
        n = n // 2

    # Return the right value for even parity
    if cnt_of_1s % 2 == 1:
        return 1
    else:
        return 0
```

How would you change this code to produce the parity bit for odd parity?

You can test the original implementation of `crc1_cnt` using the following `test` function. After you edit `crc1_cnt` to implement odd parity, change the second parameter in the call to `test` to `'odd'` to verify your changed function.

```{code-block} python
---
lineno-start: 29
---
### chap10/ale01.py

def test(fun, parity='even'):
    if parity == 'even':
        p, q = 0, 1
    else:
        p, q = 1, 0

    # Test the function sent in as the parameter
    if fun(0) != p:
        return 'Failed for n = 0'
    if fun(1) != q:
        return 'Failed for n = 1'
    if fun(2) != q:
        return 'Failed for n = 2'
    if fun(3) != p:
        return 'Failed for n = 3'
    if fun(16) != q:
        return 'Failed for n = 16'
    return 'PASSED our unit tests!'


print('Testing crc1_cnt ...')
print(test(crc1_cnt, 'even'))
```

**Step 3.** Think back for a moment to ALE 9.1 where we learned how computers implement addition using nothing but bitwise operations. There we learned how the `sum` and `carry` parts of long-hand addition work with binary numbers, and now we can see that `sum` is just a parity computation!

Figure out what type of parity the XOR operator computes (HINT: write out its truth table). And then complete the body of the function `crc1_xor` so that it implements even parity.

```{code-block} python
---
lineno-start: 18
---
### chap10/ale01.py

def crc1_xor(n):
    even_parity = 0
    n_binstr = bin(n)[2:]  # removes the leading '0b'
    #print('DEBUG: n_binstr', n_binstr)

    # INSERT some kind of loop over n_binstr and
    # the body of the loop can be done with one
    # update to `even_parity` using a bitwise operator.

    return even_parity
```

Test your implementation to verify that you've done your work correctly.

```{code-block} python
---
lineno-start: 53
---
### chap10/ale01.py

print('Testing crc1_xor ...')
print(test(crc1_xor, 'even'))
```

## ALE 10.2: The BSD checksum

When we type commands in the Shell on Replit, we are interacting with a program running on a Unix operating system. For those of you running on a MacBook, MacOS is basically a Unix operating system under its hood. In fact, MacOS is a descendent of [BSD Unix](https://www.youtube.com/watch?v=Sne3m0qLEMk), which was developed at the University of California, Berkeley in the 1970s. It was an extension of the original UNIX operating system developed just a few years earlier at Bell Labs, which started in 1925 as the research arm of Western Electric and AT&T.

One of the extensions that the Berkeley researchers added to their version of Unix was the BSD checksum utility called `sum`, which can still be found in MacOS. It was meant as mechanism to create a fingerprint (i.e., a checksum or hash) of a file so that you could later tell if the file had been corrupted (i.e., had one of its bytes changed).

The BSD checksum employed a very simple algorithm, which we describe in pseudocode below. Instead of taking a file of bytes as input, our function will take a string of ASCII (8-bit) characters.

Notice that this routine creates a 16-bit result, which means an integer between `0` and `65535`. It does a little bit of bit manipulation, which is the hardest part of creating the Python code corresponding to this pseudocode. Otherwise, it is just adding the numerical value of each character together and keeping the lowest 16 bits of the resulting sum.

Complete the implementation of `bsd_checksum` and verify it using the following test-code block.

```{code-block} python
---
lineno-start: 1
---
### chap10/ale02.py

def bsd_checksum(s):
    '''Given a string of 8-bit characters
       compute a 16-bit, BSD-style checksum.'''

    # Starting value for our 16-bit checksum
    checksum = 0

    # foreach character c in the input string s
        # Rotate the current checksum value right by 1 bit, where
        # rotate takes the existing least significant bit and
        # makes it the new most significant bit.

        # To this rotated value, add numerical value of
        # the current character c.

        # Store only the least-significant 16 bits of the previous
        # result as your new checksum.

    #print('DEBUG:', checksum)
    return checksum
```

```{code-block} python
---
lineno-start: 24
---
### chap10/ale02.py

# Test code
print('Testing bsd_checksum ...')
assert bsd_checksum('a') == 97, "Failed for s = 'a'"
assert bsd_checksum('aa') == 32913, "Failed for s = 'aa'"
assert bsd_checksum('bb') == 147, "Failed for s = 'bb'"
assert bsd_checksum('test') == 16597, "Failed for s = 'test'"
assert bsd_checksum('This is a test\n') == 11640, \
                    "Failed for s = 'This is a test\n'"
print('PASSED our unit tests!')
```

Congratulations! You now know several kinds of hash functions, all of which convert inputs of unbounded size into an output that is a small integer, with some hashes as small as a single bit!

## ALE 10.3: Find colliding words

The script `rk_strmatch.py` currently finds substrings in the text that match a given pattern. If the pattern fed is a word, can you change this script so that it prints **not substring matches** but **the words that collide with the pattern word** (i.e., find two words that produce the same hash value)? Let's try!

In doing this work, you'll probably want to:

1. Pull out the code that computes a hash in `rk_strmatch` into its own function and make the hashing function constants into globals.
2. Make sure that the substring that matches the pattern word is itself a word. You can assume that words contain only the letters a-z.
3. Check your work by printing both the hash of the pattern word and the hash of the colliding words.
4. Print a colliding word only once, even if it occurs many times in the input text.

Start by copying `rk_strmatch.py` into a file called `ale03.py`, and then edit that file so that you don't destroy your good copy of the original script.

To test your script, you can try the following two commands:

* `python3 ale03.py JustDavid-chaps.txt book`
* `python3 ale03.py JustDavid-chaps.txt pale`

The test with "book" should print:

```{code-block} python
chap10$ python3 ale03.py JustDavid-chaps.txt book
pattern hash("book") = 3324
No collisions found
```

The test with "pale" should print:

```{code-block} python
chap10$ python3 ale03.py JustDavid-chaps.txt pale
pattern hash("pale") = 64517
COLLISION: hash("spot") = 64517
COLLISION: hash("tops") = 64517
```

\[Version 20240822\]