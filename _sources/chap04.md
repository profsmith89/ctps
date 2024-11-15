# Chapter 4: Query a Web Resource #

Now that we know the basics about functions and modules, we can use them as our ticket into the worldwide community of software developers. While we might begin by building our own reusable components (i.e., functions) and sharing them with the Python community, in this chapter we'll learn how to write a script using functions others have built and shared. In particular, we'll use functions to access information available on the Internet. It's time to use what we know to interact with the world!

```{admonition} Learning Outcomes
In this chapter, you will learn how to query networked resources through their application programming interface and the use libraries built by others. The scripts we'll write will give us access to the collective knowledge of the world. By the chapter's end, you will be able to:
*   Find and use powerful Python packages and libraries [design];
*   Describe the purpose and importance of an application programming interface (API) and know where to go to find published APIs [CS concepts and programming skills];
*   Write a script that requests information from an online resource like Wikipedia [programming skills];
*   Split apart a URL into its main components and explain purpose of each component [CS concepts];
*   Sketch the major components of the client-server programming model and interprocess communication [CS concepts];
*   Understand a basic protocol between two communicating processes, that network messages often consist of a header and a body, and what you can find in each [CS concepts];
*   Discuss the different ways a network request might fail and what HTTP status codes you'd receive [CS concepts and programming skills];
*   Create and use Python dictionaries [programming skills];
*   Use Python's multiple-assignment, the enumerate function, and the for-else statement to make your scripts easier to understand [design and programming skills];
*   Manipulate messages and files that use the JSON data-interchange standard [programming skills];
*   Explain the difference between blocking and non-blocking function calls [CS concepts].
```

## Packages and libraries

To access information out on the Internet, we will use the `requests` package. A *package* is simply a set of modules that a Python programmer distributes all together. The `requests` package is one of many available through the Python Package Index (`https://pypi.org/`), which is an online repository of software for the Python programming language.

Another name you'll often hear programmers use for a collection of modules is *library*. Programming languages like Python are typically distributed with what is called a *standard library*, which you can think of as the modules and packages available by default wherever the programming language is installed. The Python standard library[^fn1] includes built-in constants (e.g., `True`), data types (e.g., `str`), and functions (e.g., the file I/O facilities) that we have repeatedly used in our scripts. 

## APIs

How do we use the functions and data structures in published packages and libraries to accelerate the development of our own scripts? We use their *application programming interface (API)*.

An API begins with the functions and data structures defined in the package or library, but it doesn't stop there. As we discussed in the previous chapter, the public interface for a function is often not enough to fully understand how to use it, and so an API isn't complete without well-written documentation, like the information we discussed putting in the docstring of the functions we write. To emphasize, an API refers to both the function interfaces and data structure definitions we can `import` into our scripts as well as the documentation for these interface and definitions that tell us how we can properly use them. You have already been using an API when you used Python's built-in types (like strings), their methods (like string-find), and Python's documentation (like the documentation on Python's sequence types).

As you've experienced, a module and its API simplifies the work we need to do in building solutions to the problems that interest us because we can use a module's functions and data structures without having to write this functionality ourselves or even understand exactly how the functions and data structures operate. Because Python has a treasure trove of existing libraries, our scripts are now no longer limited by what's built in the Python language or what we know how to build ourselves. This is incredibly powerful for it expands the limits of our dramatic stage and permits us to reach beyond our own laptops to take advantage of everything on the Internet. Let's start doing that now.

## A new problem

To date, we've affirmatively answered some simple questions: Can I write a program that converts a book into a theatrical script? Can I write a function that replaces a subsequence of a string with a different subsequence? Here's another seemingly simple question: Can I write a program to check if a book exists in my local library? Maybe I want to know if there's a copy of Dr. Seuss's *The Cat in the Hat* somewhere in Harvard Library's miles of bookshelves.

We all know how to do this work ourselves. We'd simply look up the book's title in the library's card catalog. At Harvard, what used to be a physically imposing room full of cabinets with drawers of index cards[^fn2] is now an online resource called HOLLIS[^fn3]. So, my question really is: Can I write a script that sends a query from my computer to some computer running HOLLIS? And I had better know how to craft a query that the computer running HOLLIS will understand, and my program had better be able to interpret whatever response it gets back.

Notice that we have started to decompose the question into smaller problems that we need to accomplish. Here's some pseudocode that enumerates the steps we've discussed so far:

```{code-block} python
---
lineno-start: 1
--- 
# Craft a request HOLLIS will understand about _The Cat in the Hat_
# Send that request from our computer to the one running HOLLIS
# Read the response from the HOLLIS computer
# Print the answer to our question
```

This looks simple enough, as long as we can find libraries with APIs that make, for example, sending a request from one computer to another as simple as a function call. Think about this. There is almost certainly a boatload of work that needs to be done to send a message from our computer to some other computer, and we'd love it if someone else had figured out how that exactly works and has hidden that work behind the interface of a simple function call. Luckily, there is such an API, and we'll use it to do something pretty amazing.[^fn4]

## Searching Wikipedia

HOLLIS is programmed to provide its users with lots of information in response to their queries. You'd learn a book's catalogue number, publisher, and publication date. You'd find a short description of its subject matter and any notes that a librarian attached to the entry. You'd also know exactly where you could find the book on Harvard's shelves.

Since we've learned that it's best to start problem solving with a simple instance, let's write most of what we need in our script while targeting a simple web resource. In particular, let's ask whether Wikipedia has an entry for *The Cat in the Hat*. The pseudocode for this problem is the same as above, except we replace HOLLIS with wikipedia.org.

Here is one translation of that pseudocode into Python code:

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb1.py
import requests

def main():
    print('Searching wikipedia for "The Cat in the Hat"')
    
    # Craft a request wikipedia will understand about The Cat in the Hat
    s = 'The Cat in the Hat'.replace(' ', '+')
    url = f"https://en.wikipedia.org/w/api.php?action=query&list=search&srsearch={s}&srlimit=1&format=json"
    
    # Send that request from our computer to the one running wikipedia
    # Read the response from the wikipedia computer
    response = requests.get(url)
    
    # Print the answer to our question ... sorta 
    if response.status_code == 200:
        print(f"Our request to Wikipedia succeeded!")
    else:
        print(f"Hmmm, something might have gone wrong.")

if __name__ == '__main__':
    main()
```

This script uses Python's `requests` library, which is described as "an elegant and simple HTTP library for Python, built for human beings."[^fn5] This library is not part of Python's standard library, and you may have to follow a few steps explained in the library's documentation to get it installed for your use. Once installed, the API provided by this library makes it very simple to complete the middle two steps of our pseudocode, as illustrated by the single `requests.get` call on line 13.

This call and its returned result hide all the hard and confusing parts of communicating across whatever network is connected to your computer. Lines 8-9 do a small amount of string-processing work to set up the parameter to this call.[^fn6] Lines 16-19 inspect the object returned by `requests.get` to determine whether Wikipedia understood our question.

```{admonition} You Try It
Run the `qweb1.py` and it will respond that it successfully talked to a Wikipedia server. You may need to install the `requests` package.
```

## The client-server programming model

Congratulations! You have successfully executed and generally understood your first network program. We all use network programs every day, from the web browsers running on our laptops to the bank apps running on our cell phones. Nearly every one of these network applications follows the same, straightforward programming model illustrated in `qweb1.py`. Before we dive deeper into its code, let's take a moment and understand the basics of this client-server programming model.

We have written a script that acts as a *client* in this client-server model. To complete its task, it makes a *request* of some other computational entity called a *server*. Typically, this server runs on some other computer, and the machines on which the client and server run are connected by a *computer network*. It is, however, possible that the client and the server are in fact running on the same machine, and we will take advantage of this ability in some of our upcoming examples.

```{figure} images/Smith_fig_04-01.png
:name: c04_fig1_ref

Two examples of the client-server programming model. The left-hand picture shows the client and server processes on two separate computers, which perform interprocess communication over a network. The right-hand picture shows the client and server processes on the same machine; later chapters will discuss how we can perform interprocess communication in this configuration. The model abstracts aways this distinction, which means the same script handles both cases.
```

{numref}`Figure %s<c04_fig1_ref>` illustrates both of these cases. It is important to keep in mind that we're talking about a script that becomes a running program when we talk about clients and (in next chapter) servers. While you might hear someone point to a computer and say that it is their server, they probably mean that they have reserved this computer to run one or more server programs that respond to the requests of one or more client programs.

The computer science term for a running program (or more precisely a running instance of a script) is a *process*, and when the client and server processes exchange information, it is called *interprocess communication*. Two processes sending data across a computer network is one form of interprocess communication.

Whether the client and server are running on two separate, networked computers or both on the same computer is a detail hidden by the abstraction of the client-server programming model. And when networks are involved, the model hides the details of whether it is a wired network, a wireless network, or some combination of networks using lots of different transport mechanisms and data exchange protocols!

## Resources, transactions, and protocols

Getting back to the structure of the client-server programming model, a server program is responsible for managing some *resource* that client programs find useful. A web server, like wikipedia.org, manages access to the files that comprise the articles in that online encyclopedia. An email server manages the transfer of email messages between email clients. A gaming server manages the shared state of the game and keeps each clients' view of the game globally consistent.

The signature operation that takes place repeatedly in the client-server programming model is a *transaction*. The Python `requests` library hides the four fundamental steps of a transaction between a client and a server behind the `requests.request` function interface, of which we have used a specific type of request in our network program. As illustrated in {numref}`Figure %s<c04_fig2_ref>`, the five steps are: (1) the client sends a request to the server; (2) the server receives the request and interprets it; (3) the server manipulates the resource it controls in the requested manner (assuming it is a valid request); (4) the server sends a response to the client; and (5) the client receives the response and interprets it.

```{figure} images/Smith_fig_04-02.png
:name: c04_fig2_ref

Graphical illustration of a transaction between a client and a server, where time moves down the page and the bold vertical lines below the client and server labels represent what is taking place at each instance in time on each machine.
```

When we make a `requests.get` function call, our script (the client) sends a request (part of the value of `url`) to the server (another part of the value of `url`). Steps (2-4) are hidden from our client in the execution of the `requests.get` function. Step (5) is the value that's returned by the `requests.get` function.

It's now time to own up to something I mentioned when I introduced the Python `requests` library but have ignored throughout our discussion of the client-server programming model. The `requests` API does not allow us to do *any* type of client-server communication, but client-server communication using one specific *protocol* (i.e., the HTTP protocol).

What is a protocol? It is a fancy way of saying that two communicating processes can't communicate unless they use the same language. Sound familiar? Encoding standards and communication protocols are related concepts. Clients and servers cannot communicate unless they agree to use the same method and meanings for the packing and unpacking of the data that they exchange.

In the Worldwide Web, browsers (i.e., clients) and web servers interact using a text-based protocol known as the *hypertext transfer protocol (HTTP)*. We see pieces of this protocol in the `url` string. In fact, HTTP defines a handful of methods that are the only ways that clients can ask servers to perform actions on their resources. One of these methods is GET, which simply asks the server to return a representation of the specified resource. The other popular HTTP method is POST, which essentially asks the server to accept the data accompanying the request as a new piece of the specified resource. Our script invokes the simpler GET method. That's why the function name we use from the `requests` library is `get`!

## The URL

When we "get" content from a web server, the data we receive comes in part from some file that the server manages, and as you might expect, each file on the server has a unique name. When we type a *URL (a universal resource locator)* in the address bar of a browser, this URL includes a *path* in the server's file system to the file (i.e., resource) that interests us. The script `qweb2.py` rewrites the setup work we did in `qweb1.py` so that we can see the four main components of the URLs used in the HTTP protocol.

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb2.py
import requests

def main():
    print('Searching wikipedia for "The Cat in the Hat"')
    
    # Highlight the 4 components of a URL for HTTP
    protocol = 'https'
    hostname = 'en.wikipedia.org'
    path = '/w/api.php'
    query = '?action=query&list=search&srsearch=The+Cat+in+the+Hat&srlimit=1&format=json'
    
    # Build the URL and launch a `get` request
    url = f"{protocol}://{hostname}{path}{query}"
    response = requests.get(url)
    
    if response.status_code == 200:
        print(f"Our request to Wikipedia succeeded!")
    else:
        print(f"Hmmm, something might have gone wrong.")

if __name__ == '__main__':
    main()
```

The first piece of a URL indicates the protocol we are using, which in our case is HTTPS. HTTPS is an extension of the HTTP protocol that encrypts the request and response data that's sent back and forth across the network.

```{tip}
Whenever possible, you should use HTTPS rather than HTTP to thwart someone snooping on your network traffic.
```

The next major piece of a URL is the *hostname* specified as an Internet Domain Name. If we want to communicate with a process on another machine, we must be able to name that machine, and Internet Domain Names are a standardized way of doing just that. There's a whole interesting question of, given a valid hostname, how do you route messages through computer networks to the computer with that hostname, especially when that computer might be a mobile device that connects to different parts of a computer network at different times. We don't have to concern ourselves with this problem because it is hidden from us below the abstraction barrier, but you should realize that this is more complicated than putting a home address on an envelope, since most houses aren't mobile homes.

The third piece of a URL for the HTTP protocol is the path to the specific resource on the host computer that we'd like. This path is specified in a form very similar to the filesystem paths we'll discuss in detail in Chapter 13. This is not surprising since we are, in fact, specifying a file in the host's filesystem.

Finally, HTTP allows us to specify an optional query at the end of our URL. The question mark (`?`) separates the path from the query. The query itself is a string comprised of name and value pairs, with each pair separated by an ampersand (`&`) character.

## The Programmable Web

You might wonder, how did I know to create this specific URL? If we point our browser to en.wikipedia.org and type "The Cat in the Hat" in the search box there, the URL to which we're directed is `https://en.wikipedia.org/wiki/The_Cat_in_the_Hat` and not the one in `qweb2.py`. That's because we want to use the Web as a programmable platform, and to do that, we need to access the parts of the Web with published APIs.

To learn what Internet-based APIs exist, I visited the ProgrammableWeb, which maintained a directory of these things until it shut down in early 2023.[^fn7] That site pointed me to MediaWiki's API, on which Wikipedia's API is built.[^fn8] There I learned that the API endpoint for Wikipedia in English is `https://en.wikipedia.org/w/api.php`.

To converse with the server at this endpoint, we must craft a *query string*. In this string, the MediaWiki site explained that we should set the parameter named `action` to the value `query` when we want to fetch data. If then we set the `list` parameter to `search`, we can perform a full text search. Since we're doing a search, we set the parameter `srsearch` with the value of our search string and the parameter `srlimt` to the number of page matches that we'd like returned. Finally, we set the parameter `format` to `json` so that the Wikipedia API knew that we want our response in JSON format, a text-based format that we'll discuss in a moment.

## Python dictionaries

It's ugly and error prone to write out these cryptic query strings ourselves. You've undoubtedly noticed that we can't use spaces in a query string and must replace each space character with a plus sign (`+`), as we did on line 11 in `qweb2.py`. But what if you wanted a search for a string containing a `+` character? We probably don't want to become an expert in URL encoding to simply ask our question to Wikipedia. Luckily, we don't have to because the author of the `requests` library realized this too!

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb3.py
import requests

def main():
    print('Searching wikipedia for "The Cat in the Hat"')
    
    # Concatenate the first 3 components of a URL for HTTP
    protocol = 'https'
    hostname = 'en.wikipedia.org'
    path = '/w/api.php'
    url = protocol + '://' + hostname + path
    
    # Describe the query string as a Python dictionary
    query = {'action': 'query',
             'list': 'search',
             'srsearch': 'The Cat in the Hat',
             'srlimit': 1,
             'format': 'json'
    }
    
    response = requests.get(url, params=query)
    
    if response.status_code == 200:
        print(f"Our request to Wikipedia succeeded!")
    else:
        print(f"Hmmm, something might have gone wrong.")

if __name__ == '__main__':
    main()
```

This third version of our script employs a simpler way to specify the query. It pulls the `query` string out of `url` (line 11 of `qweb3.py`) and passes it in another form to the `params` parameter of the `requests.get` function (see line 21 of `qweb3.py`). This parameter expects a specific kind of data structure that we haven't yet encountered. It expects a Python dictionary.[^fn9]

Don't confuse Python dictionaries with Python's sequence data types. Both use the index operator (i.e., `[]`) to access a particular entry, but the dictionary abstraction doesn't enforce an ordering on its elements as the sequence abstraction does.[^fn10] Since we cannot specify which entry we want in a dictionary by specifying its position in the data structure, dictionaries associate each entry with a *key*, which Python requires to be of an immutable type. Recall that strings are immutable, and so we can use them as keys in our dictionary. We can also use numbers as keys, since they too are immutable objects in Python, which means you could make a dictionary object look like a list object, but let's get back to our example.

Python dictionaries create a mapping from keys to values, and we initialize a dictionary with a set of `key:value` pairs such as `'action':'query'` and `'srlimit':1`. 

```{code-block} python
---
lineno-start: 13
---
# Describe the query string as a Python dictionary
query = {'action': 'query',
         'list': 'search',
         'srsearch': 'The Cat in the Hat',
         'srlimit': 1,
         'format': 'json'
}
query    # Not in qweb3.py
```

Run the code block above in the interactive interpreter. Once we've initialized `query`, we can ask for the value associated with the key `action` by typing `query['action']`, which would return `'query'`. Make sure you recognize the difference between `query`, which is the name of our dictionary object, and `'query'`, which is a string literal!

```{code-block} python
query['action']
```

As another example, `query['srlimit']` would return `1`.

```{code-block} python
query['srlimit']
```

In initializing a dictionary, we enclose all the `key:value` pairs in a pair of curly braces (`{}`) and separate the `key:value` pairs with commas. A pair of curly braces without any `key:value` pairs creates an empty dictionary.

```{code-block} python
an_empty_dictionary = {}
```

Many of Python's built-in functions work as well on dictionaries as they did with Python's other data structures. For example, `len(query)` would return `5` for our example, since there are 5 key-value pairs in `query`.

```{code-block} python
len(query), len(an_empty_dictionary)
```

Typing `query['srsearch'] = "Oh, the Places You'll Go!"` replaces one Dr. Seuss book title with another, and this functionality means dictionaries are mutable objects in Python.

```{code-block} python
print(query)
query['srsearch'] = "Oh, the Places You'll Go!"
print(query)
```

And `'rlimit' in query` returns `False` because our dictionary doesn't contain that misspelled key.

```{code-block} python
'rlimit' in query
```

If we want to remove a key and its value from a Python dictionary, we type `del d[key]` where `d` is our dictionary and `key` is the key to be removed. If you specify a key that isn't in the specified dictionary when indexing or using `del`, the Python interpreter will raise a `KeyError` exception.

 

```{admonition} You Try It
Go ahead and create a code block in which you can experiment with these Python dictionary commands for yourself. Feel free to look at the Python tutorial on dictionaries (https://docs.python.org/3/tutorial/datastructures.html#dictionaries) for more examples.

Another thing you might try is to print the actual URL used in our `requests.get` call. You can see this string if you print `response.url`, which is the URL that produced the response we received.
```

## An HTTP response

So far, we have spoken to a Web API, but we haven't done much with the HTTP response. In fact, our current script simply looks at something called the *status code* on the response and checks to see if this code is equal to `200`. While surfing around the Internet, you may have seen the phrase `200 OK` or `404 Not Found`. In general, the first is something good and the second isn't. Unfortunately, the first, while good in some ways, may not mean we were completely successful in accomplishing what we hoped to do.

Instead of acting upon the value of the `status_code` in our received response, let's change our script for a moment and have it simply print the value of `status_code`. We will then manipulate the parameters to `requests.get` and see which affect the status code. We'll modify `qweb4.py` in our experiments.

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb4.py
import requests

def main():
    print('Searching wikipedia: changed "<nothing>"')
    # print('Searching wikipedia: changed "path"')
    # print('Searching wikipedia: changed "hostname"')
    # print('Searching wikipedia: changed "srsearch"')
    
    # Concatenate the first 3 components of a URL for HTTP
    protocol = 'https'
    hostname = 'en.wikipedia.org'
    # hostname = 'en.wikipediaasdfghjasdfghj.org'
    path = '/w/api.php'
    # path = '/asdfghjasdfghj/api.php'
    url = protocol + '://' + hostname + path
    
    # Describe the query string as a Python dictionary
    query = {'action': 'query',
             'list': 'search',
             'srsearch': 'The Cat in the Hat',
             # 'srsearch': 'asdfghjasdfghj',
             'srlimit': 1,
             'format': 'json'
    }
    
    response = requests.get(url, params=query)
    
    print("response.status_code =", response.status_code)

if __name__ == '__main__':
    main()
```

If you run the script as is, it reports a status code of 200. How can we make it generate a 404 status code?

If we search the Internet for "HTTP status 404 Not Found" we learn that it is a shorthand for "the server cannot find the requested resource."[^fn11] Recall that "requested resource" in this context refers to the path portion of our URL, which was supposed to take us to a resource controlled by this server. Ok, let's test that. What happens if we put garbage in the path? In particular, let's change this value from `'/w/api.php'` to `'/asdfghjasdfghj/api.php'` and rerun `qweb4.py`. 

```{admonition} You Try It
Switch which assignment to `path` is commented out in `qweb4.py` and run it.
```

Bingo! We got a 404 status code. This means that we were able to talk to a server with the hostname we specified and the server knew how to interpret an HTTP GET request, but this server didn't have any resource at the path `'/asdfghjasdfghj/api.php'`.

What if we mistype the hostname of the server?[^fn12]

```{admonition} You Try It
Restore the good assignment to `path`. Now change the assignment to `hostname` from `'en.wikipedia.org'` to `'en.wikipediaasdfghjasdfghj.org'`. Run this version of `qweb4.py`.
```

Our modified script dies with many error messages. In this mess, you'll find "Failed to establish a new connection" meaning that the `requests` library tried several times to connect to the server but failed each time. Ideally, we would write our script so that it caught this error condition and handled it gracefully. Obviously, we cannot check the status code of a response if we never make a connection in the first place!

As a last experiment, what do you think the status code will be if we use a good hostname, a valid path, but ask to search for a term that doesn't exist in Wikipedia?

```{admonition} You Try It
Restore the good assignment to `hostname`. Now change the value for key `'srsearch'` in the definition for `query` from `'The Cat in the Hat'` to `'asdfghjasdfghj'`. Run this final version of `qweb4.py`.
```

It's `200`. The same status code we got when we searched for `'The Cat in the Hat'`. Why is that? You can verify for yourself that Wikipedia will tell you that it doesn't contain a page with this particular string.

## The response header

To figure this out, we need to look at more than the response's status code. Fortunately, HTTP request and response messages were designed to be human readable. Printing them can be instructional.

Roughly speaking, these messages consist of a *header* and a *body*. We can think of the header as containing the information we might see on the outside of an envelope (e.g., who the letter is meant for and the date it was posted at the post office). The body is the contents of the letter sealed in the envelope. Or if you prefer email, the header contains information like the "from" and "subject" fields, while the body is what is written below the subject line in most email clients.

The status code that we've been printing is just one part of the response header. We can inspect the other parts by printing `response.headers`, which consists of what looks like a dictionary of key-value pairs. The `qweb5.py` script uses a for-loop to print each key-value pair in `response.headers`. When we write this for-loop statement, we specify two iteration variables (i.e., `key` and `value`) separated by a comma so that the interpreter knows that we want each pair pulled apart. This is called *multiple assignment*.[^fn13]

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb5.py
import requests

def main():
    print('Searching wikipedia for "The Cat in the Hat"')
    
    # Concatenate the first 3 components of a URL for HTTP
    protocol = 'https'
    hostname = 'en.wikipedia.org'
    path = '/w/api.php'
    url = protocol + '://' + hostname + path
    
    # Describe the query string as a Python dictionary
    query = {'action': 'query',
             'list': 'search',
             'srsearch': 'The Cat in the Hat',
             'srlimit': 1,
             'format': 'json'
    }
    
    response = requests.get(url, params=query)
    
    # Print the response headers with a line per key
    print("response.headers =")
    for key, value in response.headers.items():
        print(f"    {key}: {value}")
    
    # This is how we can access one of the keys
    print("Content-Type:", response.headers['Content-Type'])

if __name__ == '__main__':
    main()
```

We're not too concerned with most of the header, but we are interested in the value of the key `'Content-Type'` for this indicates how the server has encoded the body of the response. The value of this key is `'application/json; charset=utf-8'`. As we learned in the last chapter, the Python interpreter we're using defaults to `utf-8` for character encodings and having the Wikipedia server return a response in this encoding format makes our lives easy. Beyond the encoding of the individual characters in the response's body, this value tells us that the body is organized using the JSON data-interchange standard, which is what we asked it to do when we specified `'format': 'json'` as one of our `query` parameters.

## JSON and the response body

JSON stands for JavaScript Object Notation, and it is meant to be a standard for exchanging structured data that is easy for machines to generate and parse, while also being relatively easy for humans to read and write.[^fn14]

As we saw when we looked at the body of our `response`, JSON-structured data resembles a dictionary of key-value pairs. Some of the values might be singleton values like a string or an integer, and other values might themselves be lists (i.e., enclosed in square brackets) or dictionaries (i.e., enclosed in curly braces). 

By importing the `json` module, we can take advantage of someone else's work to make it straightforward for us to read and write message bodies in JSON format.

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb6.py
import requests
import json

def main():
    print('Searching wikipedia for "The Cat in the Hat"')
    
    # Concatenate the first 3 components of a URL for HTTP
    protocol = 'https'
    hostname = 'en.wikipedia.org'
    path = '/w/api.php'
    url = protocol + '://' + hostname + path
    
    # Describe the query string as a Python dictionary
    query = {'action': 'query',
             'list': 'search',
             'srsearch': 'The Cat in the Hat',
             'srlimit': 1,
             'format': 'json'
    }
    
    response = requests.get(url, params=query)
    
    # Print the response body directly
    print("response.text =", response.text)
    
    print()
    
    # Read the response body in JSON format and print it
    j = response.json()
    print("response.json() =", json.dumps(j, indent=4))
    
    print()
    
    # Print just the title from the JSON-structured response
    print("Title =", j['query']['search'][0]['title'])

if __name__ == '__main__':
    main()
```

The `json` module defines a function called `dumps` that we use to convert JSON data into a string. Printing this string, we can see the structure of the data. We see that the value for the key `'query'` in our JSON-structured data is itself a dictionary. This dictionary tells us things about the search (i.e., the total number of hits that the server got when performing the search), and a list of `'search'` results. Since we limited the server to returning only the top hit (i.e., `'srlimit': 1` in our `query` parameters), this list contains only a single element, which itself is a dictionary describing some characteristics of the article it found. Reading the JSON, we can see that this is the book we were hoping to find in Wikipedia!

How would we access the title of the article returned in the JSON body? Well, following our description in the previous paragraph, we'd simply type:

```{code-block} python
response.json()['query']['search'][0]['title']
```

And how would we change this to access the fifth character of the title string? We would add a `[4]` to the end of what we just typed, because the result of the previous expression is a string and a string is a sequence!

```{code-block} python
response.json()['query']['search'][0]['title'][4]
```

While we will continue to play with JSON-structured response bodies, you may see (or desire) other types of responses. For instance, the default response format for the Harvard Library API is something that looks like JSON, but is actually another standard called XML. As we'll see in our next script (`qweb7.py`), we can indicate that we'd like a particular response encoding by changing the `Accept` value in the request header.

## Enumerating answers from HOLLIS

The `qweb7.py` script below communicates not with Wikipedia, but (finally!) Harvard Library.[^fn15] You should recognize and understand every statement except for the details of the for-loop that processes the returned response.

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
             'limit': 2
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

In this script, we have asked HOLLIS to return not just the top search result, but the top two results. The script's final lines dig down into the JSON-structured data to grab the Python list of results, and they pass that list to the Python built-in function `enumerate`. This is a very useful little function when what you need is not just each value in a list, but each value paired with its list index. We pull this returned *tuple*[^fn16] apart (using multiple assignment) in the for-loop, placing the index in `i` and the dictionary that is the next value from the list in `item`. From this dictionary, we grab the information associated with the key `'titleInfo'` and print it.

## Beyond printing

Now that we understand the basics of grabbing data off the Internet, we can start having some real fun. Printing pieces of the data we've collected has helped us see what we've been getting back from these Web APIs, but why should we recreate what the software engineers at Wikipedia and Harvard Library have already spent years thinking about how to display? Imagine that we wanted to develop a script that searched multiple different libraries for a particular book. When it finds the book we're interested in, we instruct this script to open the result in a browser window. We can see how this might be done using the HOLLIS URL in the response we just received.

```{code-block} python
---
lineno-start: 1
---
### chap04/qweb8.py
import requests
import webbrowser

def h_lib(book):
    # Concatenate the first 3 components of a URL for HTTP
    protocol = 'http'
    hostname = 'api.lib.harvard.edu'
    path = '/v2/items.json'
    url = protocol + '://' + hostname + path
    
    # Describe the query string as a Python dictionary
    query = {'q': book, 'limit': 5}
    
    # Add a field to the request header saying what we accept
    accept = {'Accept': 'application/json'}
    
    response = requests.get(url, params=query, headers=accept)
    
    # Return a list of matching items from the received response
    if response.json()['pagination']['numFound'] == 0:
        return []
    else:
        return response.json()['items']['mods']

def match(item, desired_book):
    if type(item['titleInfo']) == list:
        ti = item['titleInfo'][0]
    else:
        ti = item['titleInfo']
    return ti['title'].lower() == desired_book.lower()

def get_url(items):
    if type(items) == list:
        for item in items:
            if '@otherType' not in item or item['@otherType'] != 'HOLLIS record':
                continue
            return item['location']['url']
    else:
        return items['location']['url']

def main():
    desired_book = input("What's the title of your desired book? ")
    
    print(f'Searching HOLLIS for "{desired_book}"')
    items = h_lib(desired_book)
    
    # Launch a browser window if we find the desired book
    for item in items:
        if match(item, desired_book):
            # Grab this book's HOLLIS url
            hollis_url = get_url(item['relatedItem'])

            # Open a browser window to the appropriate HOLLIS page
            browser = webbrowser.get('safari')
            browser.open(hollis_url)
            break
    else:
        print("Your desired book isn't in HOLLIS.")

if __name__ == '__main__':
    main()
```

```{admonition} You Try It
To run `qweb8.py`, you'll need an IDE that is running locally on your machine (i.e., not in the cloud) so that you can launch a window running a web browser. To run `qweb8.py`, you should replace `safari` on line 55 with the appropriate name for the web browser on your machine. Also, be aware that the script only checks the title you input against what HOLLIS considers the interesting part of the title. To match "The Cat in the Hat" you input "cat in the hat". You could extend `qweb8.py` with the title-printing logic in `qweb7.py`.
```

The script `qweb8.py` creates a function `h_lib` to hide the details of making a search query on HOLLIS. This should all look familiar except for the very end of the function, where we check for a response that contains no matches.

The more interesting new code is in `main`, which uses the for-else-statement introduced in Chapter 3's active-learning exercises. In this script, we receive back a list from `h_lib` in which we hope to find a matching item. If we iterate through this entire list and never find our desired item, we'd like to note this for the script's user. This is the purpose the else-clause indented at the same level as the for-statement.[^fn17] 

Think about how we'd do that using just a for-statement. We'd probably set a boolean flag (e.g., `found = False`) before we began the loop. In the loop, we'd set that flag to `True` if we found our desired item. And then check the flag at the completion of the loop to see if we checked the entire list and didn't find our desired item. If you found that description confusing, you're not alone. If you write out that code, you'll see it isn't any easier to understand.

The for-else-statement does away with this flag in this exact use case. The if-statement in the for-loop contains the condition we'd like to find `True`, and when the if-statement's condition is `True`, we break out of the for-loop. This is exactly what we'd like to think about this loop doing. But what if this loop iterates all the way through because we never found our desired item. Here's where the else-clause of the for-statement executes, alerting us that we failed in our search. Now that, my friends, how you create a programming language feature for a common construct.

And what did the script do when it found our desired book? It grabbed the HOLLIS URL for the book out of the matching record and launched a web browser to display the resource at that URL. Did I have to know anything specific about the operation of web browsers? No! A wonderfully helpful individual created the `webbrowser` library; I simply imported and used this library after spending only a few minutes with its API description.

## Blocking and non-blocking function calls

Let's pause for a moment and look at two of the powerful function calls we made in `qweb8.py`. The `requests.get` call on line 18 and the `browser.open` call on line 56 are different in a fundamentally interesting way. I don't mean that one returns a Python list and the other returns the special Python object `None`. No, I want you to think about when lines 21 and 57 execute.

Line 21 can't execute until we have completed the `requests.get` call and have a `response` object. This call *blocks* the further execution of our script until it completes. In particular, our script has to wait for some server process to do some work before the process running our script can continue. On the other hand, our script doesn't care how long a process that runs the web browser takes to display the URL we asked it to render. Our script can merrily move on to the work in line 57 and beyond. The call to `webbrowser.open` is an example of a *non-blocking* call.[^fn18]

This is your first taste of *concurrency* (also called *parallelism*), and it is a fascinating and tricky subject. For now, be aware whether the library call you're making will block and think about the implications of that action on the rest of what you write in your script.

[^fn1]: https://docs.python.org/3/library/

[^fn2]: To learn more, you might read this article: Karen Coyle, "The Evolving Card Catalog," American Libraries (January 4, 2016). https://americanlibrariesmagazine.org/2016/01/04/cataloging-evolves/

[^fn3]: https://library.harvard.edu/services-tools/hollis

[^fn4]: In the next chapter, we will peek under the covers of this API, just as we did with string-replace. We'll also start solving problems that require more general communication between two networked computers. We won't, however, go too deep, and if you want to know exactly how computers communicate over wired and wireless networks, find yourself a book or course on computing networking.

[^fn5]: https://requests.readthedocs.io/en/latest/

[^fn6]: Line 9 uses Python's *formatted string literals*, which are called *f-strings* for short. By placing the letter \`f\` (or \`F\` if you prefer) before your string literal, you can embed one or more Python expressions in the string literal by surrounding them with curly braces (i.e., \`\{ \}\`). The string literal is built with the value of the expression at that location. See Formatted String Literals in the Python documentation for additional information: https://docs.python.org/3/tutorial/inputoutput.html\#tut-f-strings

[^fn7]: You can read about the ProgrammableWeb on wikipedia (https://en.wikipedia.org/wiki/ProgrammableWeb). Some current online API directories include: Rapid API Hub (https://rapidapi.com/hub); API list (https://apilist.fun/); and Public APIs (https://github.com/public-apis/public-apis).

[^fn8]: https://github.com/public-apis/public-apis

[^fn9]: https://docs.python.org/3/tutorial/datastructures.html\#dictionaries

[^fn10]: While the dictionary *abstraction* doesn't force an ordering on its elements, Python's *implementation* of the dictionary data type does guarantee the elements to be in insertion order, as of version 3.7. Personally, I would suggest you forget this "feature." This implementation detail might change in a later release, as it doesn't flow from the dictionary abstraction. Plus, what is the insertion order after you've updated some elements and deleted a few? I don't know and don't care to figure it out since it isn't core to the dictionary abstraction. Of course, this is just my opinion.

[^fn11]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

[^fn12]: Be careful! I badly mistype the hostname because you should always be careful with slightly mistyped hostnames in Internet URLs. Sometimes these sites are put up by nefarious people who want you to think you are at the correctly typed location, where they might (for example) encourage you to type in your bank account information.

[^fn13]: You can learn more about multiple assignment in this chapter's active-learning exercises.

[^fn14]: https://www.json.org/json-en.html

[^fn15]: To communicate with the Harvard Library system, \`qweb7.py\` uses the LibraryCloud API (https://harvardwiki.atlassian.net/wiki/spaces/LibraryStaffDoc/pages/43287734/LibraryCloud+APIs). At the time of this book's writing, \`qweb7.py\` successfully runs on Replit, but fails in the PythonAnywhere's environment (i.e., fails with a "403 Forbidden" HTTP status code, which means that the server understood the request but refused it). It isn't always easy to understand why a request fails.

[^fn16]: You can learn more about tuples in this chapter's active-learning exercises.

[^fn17]: Note that this else-statement is not paired with the if-statement in the for-loop. We don't print if any one item doesn't match our desired book, but only if all items don't match.

[^fn18]: For those paying close attention, \`webbrowser.open\` is non-blocking if you call a graphical browser. As the documentation (https://docs.python.org/2/library/webbrowser.html) says, text-mode browsers, which presumably use the same terminal window as the executing script, will block.