# Chapter 4 #

## ALE 4.1: This and that

Python allows you to assign multiple values to multiple names in one assignment-statement.  Here are the semantics:

* The lefthand side of the assignment operator must be a comma-separated list of names.
* The righthand side of the assignment operator can be
    * a comma-separated values, or
    * a value that adheres to the sequence abstraction.

The first value on the left is given the first name on the right; the second on the left is given the second on the right;  and so forth. For the operation to complete without an error, the comma-separated lists on both sides of the assignment operator must have the same number of elements. Similarly, the length of the sequence value on the right must be the same as the number of comma-separated names on the left.

This is not an operation you'd want to do all the time, since it can quickly become confusing to lump different things together in a single assignment statement. There are, however, two cases where this functionality is very helpful:

```{code-block} python
---
lineno-start: 1
emphasize-lines: 5
---
# CASE 1: Unpacking a sequence into its component pieces
# so that you can operate on each separately
answer = '16.83 million books are in the Harvard library'

num, magnitude, units = answer.split()[0:3]

print("num =", num)
print("magnitude =", magnitude)
print("units =", units)
```

```{code-block} python
---
lineno-start: 1
emphasize-lines: 5
---
# CASE 2: Swapping two values
x = 0
y = 1

x, y = y, x

print(f'x = {x}, y = {y}')
```

**Step 1.** Write an equivalent sequence of statements to implement the swapping of two values without using multiple assignment.

```{code-block} python
---
lineno-start: 1
---
# Swapping two values without multiple assignment
x = 0
y = 1

# YOUR CODE HERE

print(f'x = {x}, y = {y}')
```

 

**Step 2.** Look at the code in CASE 2 and the code that you wrote. Which is easier to understand?

## ALE 4.2: Tuples, a box for everything

A *tuple* is yet another kind of Python sequence.  They may look like a Python `list`, but they are immutable.

Be careful.  Tuples are always printed with surrounding parentheses, but you don't need parentheses to create a tuple. A comma is sufficient.

```{code-block} python
---
lineno-start: 1
---
t1 = 'is blue', 'Who changed the sky?!?'
print(t1)

t2 = 'is blue', 1
print(t2)

t3 = 0, 1
print(t3)

t4 = (0, 1)
print(t4)

print(t3 == t4)
```

**Step 1.** Run the code in the code block above. Notice the different ways you can make a tuple.

**Step 2.** And here is some stuff not to do. Execute each code statement and talk with your partner(s) about what's wrong.

```{code-block} python
# an error
x, y = 0, 1, 2
```

```{code-block} python
# not an error, but hard to understand
x, y = 0, (1, 2)
```

**Step 3.** We've been looking at tuples with just two values in them, but a tuple can also contain a single value or many values.

```{code-block} python
---
lineno-start: 1
---
a_big_tuple = 'a', 2, 'c', 4
print(a_big_tuple)

a_big_tuple = 'a', 2, 'c', 4,
print(a_big_tuple)

a_big_tuple = ('a', 2, 'c', 4,)
print(a_big_tuple)
```

Notice that you can add a comma after the last element in a tuple, and the Python interpreter happily throws it away. However, you need this to be able to define a tuple with a single element:

```{code-block} python
---
lineno-start: 1
---
a_tuple = 0,
print(a_tuple)

not_a_tuple = (0)
print(not_a_tuple)

print(a_tuple == not_a_tuple)
```

Before you execute the following code block, answer for yourself if the expression that is the parameter to `print` will evaluate `True` or `False`, given the definitions of the variables from above.

Now run the code block to check your work.

```{code-block} python
print(a_tuple[0] == not_a_tuple)
```

```{tip}
We've played around with the syntax of tuples, and now I'll mention one reason why they are useful. One of their most convenient uses is to return multiple values from a function. Remember that a function returns only a single object. A tuple is a convenient way to box up every result you have computed in a function and return them in a single object, which the caller can pull apart using multiple assignment!
```

## ALE 4.3: But it must be true!

When you're first coding a solution to a problem, you will find yourself making assumptions. You could document them in a comment, but it is often more helpful to include an *assert-statement* in your code.

The assert-statement takes a condition, which is what you expect to be `True` every time you hit this point in your code. As long as the condition evaluates `True`, execution proceeds as if the assert-statement wasn't there (i.e., your assumption holds). However, if the assert condition evaluates to `False`, the statement throws an `AssertionError`, which tells you that something about your thinking was wrong.

**Step 1.** To experience an assertion failure, run the following silly example.

```{code-block} python
---
lineno-start: 1
---
the_sky = 'is red'
assert(the_sky == 'is blue')
print('It must be because execution got here!')
```

**Step 2.** Be careful with your parentheses in this statement.  `assert` is **not** a command, but a kind of Python statement. We can rewrite the code block above in the follow ways:

```{code-block} python
---
lineno-start: 1
---
# Adds a space after `assert`
the_sky = 'is red'
assert (the_sky == 'is blue')
print('It must be because execution got here!')
```

```{code-block} python
---
lineno-start: 1
---
# Removes the parentheses on the assert condition
the_sky = 'is red'
assert the_sky == 'is blue'
print('It must be because execution got here!')
```

Run the two code blocks to prove to yourself that they're all equivalent.

**Step 3.** An assert-statement also takes an optional second argument, which is the string you'd like printed when the assertion fails. Make sure you see the difference between the first assert-statement, which doesn't do what we want, and the two that follow, which do.

```{code-block} python
---
lineno-start: 1
---
# The INCORRECT way to include an assert message
the_sky = 'is red'
assert(the_sky == 'is blue', 'Who changed the sky?!?')
print('It must be because execution got here!')
```

```{code-block} python
---
lineno-start: 1
---
# The CORRECT way to include an assert message
the_sky = 'is red'
assert the_sky == 'is blue', 'Who changed the sky?!?'
print('It must be because execution got here!')
```

```{code-block} python
---
lineno-start: 1
---
# The CORRECT way to use parentheses around the assert condition
the_sky = 'is red'
assert (the_sky == 'is blue'), 'Who changed the sky?!?'
print('It must be because execution got here!')
```

**Step 4.** Where you put the closing parenthesis makes all the difference. Keep in mind that parentheses mean many different things in Python depending upon the context, as we just saw with tuples!

```{code-block} python
---
lineno-start: 1
---
# Parentheses surrounding a comma-separated list of objects
# AFTER A FUNCTION NAME mean: "Here are the arguments."
print(the_sky == 'is blue', 'Who changed the sky?!?')

# Parentheses surrounding a comma-separated list of objects
# IN SOME OTHER CONTEXTS mean: "Make a tuple."
a_tuple = ('is blue', 'Who changed the sky?!?')
print(a_tuple)
```

Can you explain what was wrong in this INCORRECT use of assert?

```{code-block} python
assert(the_sky == 'is blue', 'Who changed the sky?!?')
```

## ALE 4.4: A Mashup

Let's combine what we learned in the ALEs 4.1-4.3 with the basics of web programming from the chapter to create what's called a *mashup*.

The `qweb8.py` script at the end of the chapter queries a single web resource. Had we programmed it to query multiple resources, we would written what's called a mashup: an application that grabs content from multiple web services to power its own new service. We can build an amazing range of applications by taking advantage of the information available to us around the Internet. Look at what the researchers at IBM accomplished with [Watson, a software program running on 100 servers that was able to quickly sift through 200 million pages of information](https://www.techrepublic.com/article/ibm-watson-the-inside-story-of-how-the-jeopardy-winning-supercomputer-was-born-and-what-it-wants-to-do-next/). In 2011, IBM Watson took on the best-ever human contestants in the TV quiz show Jeopardy, and it beat them. 

Today, we talk to our cell phones and home appliances running software agents (e.g., Apple's Siri, Amazon's Alexa, Google's Assistant, and Microsoft's Cortana), which do much the same work: You ask them a question. They analyze and parse the wave form that is your question, trying to understand what you want to know. Then they query many different resources around the Internet and evaluate what they get back to determine if the information is pertinent to the question they think you asked. Finally, they take the best ranked response and present it to you.

```{margin}
As September 2020, none of the assistants I tried could answer this seemingly simple question. In July 2023, I asked ChatGPT 3.5 this question, and it answered it, although it admitted that its data was two years old.
```

While these software agents are quite sophisticated, we can build a script that does mashup work. In particular, let's say we want to know: *does Harvard Library contain more books than Wikipedia has articles?* 

The script `mashup32.py` (listed below) breaks this question into its two pieces and asks each to [Wolfram Alpha](https://www.wolframalpha.com/), a site on the Internet with the goal of accepting free-form questions and returning relevant and clearly presented answers. We can think of it as a voice assistant without the voice interface.

```{code-block} python
---
lineno-start: 1
---
### chap04/mashup32.py -- not executable without a Wolfram Alpha dev key
import requests
import xmltodict

def walpha(query, appid):
    print(f'Asking Wolfram|Alpha "{query}" ...')
    response = requests.get(
        'http://api.wolframalpha.com/v2/query',
        params={'input': query,
                'appid': appid,
                'podtitle': 'Result'
                }
    )
    assert(response.status_code == 200)

    jlike_response = xmltodict.parse(response.text)
    answer = jlike_response["queryresult"]["pod"]["subpod"]["plaintext"]
    print(f'... answered "{answer}"')
    return answer

def main():
    # Compare wikipedia's and Harvard library's knowledge base.

    # Grab Mike's Wolfram Alpha develop key
    with open('walpha-id.txt') as f:
        appid = f.readline().strip()

    h_query = 'how many books are in the harvard library'
    h_answer = walpha(h_query, appid)
    h_num, h_magnitude, h_units = h_answer.split()[0:3]

    w_query = 'how big is wikipedia'
    w_answer = walpha(w_query, appid)
    w_num, w_magnitude, w_units = w_answer.split()[0:3]

    assert(h_magnitude == w_magnitude)
    h_num, w_num = float(h_num), float(w_num)
    if h_num > w_num:
        print(f'Harvard Library has more {h_units} ({h_num} {h_magnitude})',
              f'than Wikipedia has {w_units} ({w_num} {h_magnitude}).')
    elif h_num < w_num:
        print(f'Wikipedia has more {w_units} ({w_num} {w_magnitude})',
              f'than Harvard Library has {h_units} ({h_num} {h_magnitude}).')
    else:
        print("They're the same size ({h_units} and {w_units})!")

if __name__ == '__main__':
    main()
```

**Step 1.** See if you can understand the general logic of this script. It does contain two programming language features we haven't studied yet:

1. It accepts and manipulates *floating-point numbers*, which are simply numbers with a decimal point and possibly a fractional part. We'll cover this data type in Chapter 7.
2. The Wolfram Alpha API also doesn't allow me to choose the format I'd like for the results; it only returns results in XML. No problem! I just looked up and used a library that converted this XML into a JSON-like dictionary.

**Step 2.** Would you like to run `mashup32.py`? If you try, you'll find that you don't have a file called `walpha-id.txt`. This file exists in my personal world --- I have a Wolfram Alpha developer `appid`. Without an `appid`, the Wolfram Alpha API won't accept your query. I'd say most sites with APIs require you to register as a developer so that they can identify bad actors (e.g., someone who tries to overload their servers). 

If you want to build your own mashup scripts, just realize that you'll probably have to register and receive an `appid` on the sites you use. Since this `appid` is specific to you, you shouldn't share it with others or store it in public code repositories like GitHub. You'll notice that I grab mine from a local file on my machine so that I can share with you my script, but not my `appid`. You too should do something like this when you write your scripts.

```{tip}
Don't ever write your private information as a literal in the scripts you develop or store it in GitHub.
```

**Step 3 (optional).** What might you try to do now that you have some skill in web programming? For example, `mashup32.py` uses the size of the English version of Wikipedia. You may consider this unfair because Harvard Library's book count includes books written in many different languages. You could sign up for your own Wolfram Alpha `appid` and change `mashup32.py` to ask your own version of the question. Perhaps a software agent of this type might make an exciting project for you.

## ALE 4.5: JSON makes my eyes ache

The `qweb7.py` script illustrated how we can pull apart a response from Harvard's LibraryCloud API and determine if the Harvard Library system has a copy of *The Cat in the Hat* by Dr. Seuss*.* But this script printed only the titles of each resource in the response.

I've copied `qweb7.py` below, except that it asks for the top 4 results. See if you can extend the code in the following ways. Each will require you to read through the dumped JSON response and figure out the sequence of indexing operations you need to do to access the information you need to print. Be careful as some JSON fields may be a Python dictionary in one item and a Python list in another, as our code illustrated with the `'titleInfo'` field.

1. Print the author's name.
2. Print the type of resource (i.e., the item is a text or a still image).
3. If the resource is a text and the item includes an abstract, print it. The abstract for *The Cat in the Hat* is "Two children sitting at home on a rainy day are visited by the Cat in the Hat who shows them some tricks and games."

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb7.py
import requests
import json

def main():
    print('Searching HOLLIS for "The Cat in the Hat"')

    # Concatenate the first 3 components of a URL for HTTP
    protocol = 'https'
    hostname = 'api.lib.harvard.edu'
    path = '/v2/items.json'
    url = protocol + '://' + hostname + path

    # Describe the query string as a Python dictionary
    query = {'q': 'The Cat in the Hat',
             'limit': 4
    }

    # Add a field to the request header saying what we accept
    accept = {'Accept': 'application/json'}

    response = requests.get(url, params=query, headers=accept)

    # Read the response body in JSON format and print it
    j = response.json()
    print("response.json() =", json.dumps(j, indent=4))

    print()

    if j['pagination']['numFound'] == 0:
        print('Zero results')
    else:
        # Process each returned response
        for i, item in enumerate(j['items']['mods']):
            # Print title info
            ti = item['titleInfo']
            if type(ti) == list:
                # Lots of title info; just print the first
                ti = ti[0]
            print(f"Title #{i}: ", end='')
            if 'nonSort' in ti:
                print(ti['nonSort'], end='')
            print(ti['title'])

if __name__ == '__main__':
    main()
```

\[Version 20230709\]
