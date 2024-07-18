# Chapter 7 #

## ALE 7.1: Print rounds

Working with floating-point numbers is hard, and sometimes the Python interpreter makes your life harder by hiding the imprecision we've discussed. 

**Step 1.** As an example, try these two statements in the interactive Python interpreter:

```{code-block} python
n = 0.1
n
```

The interpreter tells you that `n` is `0.1`. But we know that we cannot exactly represent 0.1 in either single- or double-precision FP. Use `fbin.py` from the chapter to prove this to yourself.

As explained on [this tutorial page](https://docs.python.org/3/tutorial/floatingpoint.html), Python sometimes rounds what it prints. What it displays and what it stores may be different when dealing with FP numbers!

**Step 2.** Still don't quite believe it? Continue with our code example in the interactive Python interpreter. What do you get when you add `n` to itself?

```{code-block} python
n + n
```

It rounded the result again. What does `fbin.py` tell you about trying to represent 0.2 as a single- or double-precision FP number?

**Step 3.** Does the interpreter always round?

```{code-block} python
n + n + n
```

Hmm, no.

**Step 4.** And how do you like these answers?

```{code-block} python
n + n == 0.2
```

```{code-block} python
n + n + n == 0.3
```

## ALE 7.2: Loops you expect to stop

Do both of these loops do the same thing?

```{code-block} python
# Option A
n = 0.1
while n < 1.0:
    print(n)
    n += 0.1
```

```{code-block} python
# Option A
n = 0.1
while n != 1.0:
    print(n)
    n += 0.1
```

The two options differ only in the comparison operator that ends the loop. With integers, it makes no difference which operator you use. With FP values it does because exact comparisons are fragile.

## ALE 7.3: Which to do first

TBD: Create an ALE that illustrates what happens when you add big and small FP numbers together, and explain why adding small ones first is a good idea. (Suggestion from BWK.)

\[Version 20240718\]