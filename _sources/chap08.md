# Chapter 8: What Is My Problem? #

To conclude this act, we'll tackle what to do when we know we have a problem, but we don't know exactly what the problem is. This may sound silly, but it is often true. Imagine, for example, that we have a small business operating at a small profit, and we wonder whether it could be generating more profit. What types of customers is our business not reaching? Where could our business operate at less cost? These types of questions lead us to this chapter's motivating question: *How can we use computers to discover what problem(s) we have that we need to solve?*

Despite this business-focused example, such questions arise in many disciplines. A natural scientist might ponder: what truth about the world is captured in some experimental data? A social scientist might ask: what does the data gathered from the activities of many people explain about human society? A humanist might wonder: what similarities are discoverable from the many characteristics of two or more cultural artifacts? Even in this book, I might ask: what aspect of the material we've covered have I failed to help you understand? These are just a few illustrative examples.

In short, we want to know how to use computers to analyze data about our "business" for insights on where knowledge lurks and problems could be eliminated. This work is possible because the proliferation of digital technologies has made it possible to collect the mounds of data needed to answer them. Furthermore, the increasing availability of cheap and powerful cloud-based computational services makes it possible for anyone with computational skills to process those data looking for new insights. Taken together, this is why *data science* is a rapidly expanding field having impact in many professions and industries.

## Data science

What exactly is data science? It is the extraction of meaningful insights and new knowledge from data. As a process, it involves *data collection*, *data cleaning*, *data exploration and pattern detection*, and *model building* for prediction and decision-making. These four steps are often repeatedly done until we are able to identify a truly meaningful insight. Once we've discovered some new insight or useful fact about our world through these steps, there's a final step in which we communicate what we have discovered.

Acting like a data scientist is a perfect way for us to wrap up our discussion of numbers, since so much of a data scientist's work is statistical in nature. In this chapter, we'll focus on the initial steps in every data science process and the domain-agnostic aspects of these steps.[^fn1] In particular, we will focus on two sub-questions of our motivating question: (1) Once we have collected some data, why it is important to clean the data in our data set before we try to analyze them? And (2) once we've cleaned the data, what programming techniques are generally helpful in doing data exploration and pattern detection? We'll practice both of these skills through two applications on images: (1) a technique for removing a photobombing person from an image (i.e., data set cleaning); and (2) a technique for hiding information inside an image (i.e., data exploration and pattern detection).

```{admonition} Learning Outcomes
In this chapter, you will be introduced to the field of data science and learn how you can use computation to analyze data and discover new knowledge. We focus on the initial steps in every data-science problem (i.e., data collection, data cleaning, and data exploration) using images as our big data set. By the end of the chapter, you will be able to:

*   Enumerate the major steps in every data-science problem [CS concepts];
*   Explain why data cleaning is necessary and important [design and CS concepts];
*   Describe the different ways we can deal with the noise, duplicates, and other irrelevant information in our data sets [design and CS concepts];
*   Implement filters for digital images [CS concepts and programming skills];
*   Understand the hazards of overtraining [CS concepts];
*   Use bitwise operators to create masks and force certain bits to certain patterns [CS concepts and programming skills];
*   Explain the purpose of image steganography [CS concepts];
*   Understand and code different traversals over 2-dimensional data [CS concepts and programming skills];
*   Use Python's range function and list comprehensions [programming skills];
*   Relate arrays to sequences and strings, with a nod to Python's NumPy library [CS concepts and programming skills];
*   Discuss how 2-dimensional arrays are stored in a computer's memory [CS concepts].
```

## Images as data about the world

Instead of diving into a completely new context, we're going to continue with digital images and consider them a source of data about the world around us. Your first question should be, is this a reasonable thing to do?

Short answer: yes. We have learned that digital images, like those found in JPEG files, are essentially large, 2-dimensional (2D) arrays of numbers.[^fn2] While we've learned how to manipulate the data in these image files using some fairly simple scripts, it's important to keep in mind that it's not easy to do any of this work by hand. The images are just too big and the numbers too difficult for humans to interpret directly. This is actually good because data sets in data science are typically very large, and like digital images, you cannot realistically analyze them without the help of powerful computers. In other words, using one or more images as a data set isn't all that different than using any other large data set when talking about data cleaning and data exploration.

## Yes, you must clean up

We create digital images with tools (e.g., our smartphone cameras) that sample our physical world, and today's digital cameras capture the world around us in amazing richness. But does a digital image faithfully capture an instance in time?

If you think about the sampling that takes place, as we'll do in a moment, you'll start to realize that it isn't a perfect process. The consequences of imperfection often aren't particularly dire. But if an image full of bad pixels leads to your conviction for a crime you didn't commit or a diagnosis of cancer you don't have, you will wish that someone had done their data cleaning.

Data collection and cleaning are extremely important because what we do here matters greatly to the insights and new knowledge that comes out the backend of the data science process. As data scientists like to say, if the data are garbage, the results of the analyses will be garbage, no matter how "smart" are the analyses that come later. Yet people too often view powerful data-science analyses as flawless: "The computer crunched the numbers and this person isn't creditworthy for a loan." We have no idea if the person is creditworthy when the collected data is biased or the data cleaning poorly done.

But let's be honest, the actual act of data cleaning holds as much appeal for most people as does any other type of cleaning. If you do any data science in your life, at some point you will be severely tempted to jump directly from data collection to data analysis. For all those whose lives will be impacted by your work, resist this urge.

Luckily, we can make cleaning images a bit more fun than the cleaning required for other types of data sets. In fact, the work we'll do in a moment will help you visualize the impact of cleaning, and it will create a fun challenge for us in terms of data exploration. Don't tune out; what follows will be worth it!

## Understanding what might go wrong

How do we do data cleaning? Well, recall that each pixel in our image is simply one or more numbers, and we can easily "fix" any imperfections in our images (or, in general, the recorded samples in any digital data set) by rewriting these numbers.

Unfortunately, each data value we change can drive us closer to or farther from the truth. While we need to clean our data, we need to remember that there is danger and power in this post-capture processing. As such, it is important to understand what problems can occur during data capture, which of these problems we can correct, and what are the limits of what we can do to improve the veracity of our samples.

So let's begin by categorizing the ways in which our images might be incorrect, and by that I mean the pixels in the image do not reflect the scene we meant to capture. Recall that the pixels in a JPEG file represent samples of the colors we saw in some scene before us, and each pixel represents the color seen at each small point in this scene. Let's call the actual value we wanted to capture the *ground truth*. Reasonable people may disagree on the ground truth for any particular observation, but we won't concern ourselves with such disagreements. We will assume that there is a single, agreed-upon ground truth that is the value we want captured and recorded.

How might the value we record differ from this ground truth? Let's see if we can gain some intuition about the types of failures by constructing a list of example failures.

One thing that might happen is the existence of some physical phenomenon between our digital camera and the object we are imaging that distorts the object's color. For example, a red shirt may appear yellow because of glare or brown because of shadows. Or perhaps the shirt's color is undistorted to the camera's lens, but there's a hardware problem with our smartphone's camera. The camera's sensor array might record a pixel in the shirt as black because the sensor corresponding to this pixel wasn't functioning properly. Or maybe the camera correctly captures the shirt's color, but an error occurs that changes the value of a particular pixel as the image is transferred from our smartphone to our laptop. In all these cases, and many others like them, we would want to correct the incorrect value.

On the other hand, there are times when the captured value differs from the ground truth and we don't want or might not be able to correct this discrepancy. For instance, there are an infinite number of shades of color in the real world, but only a fixed number of RGB encodings (16,777,216 to be precise). A correctly functioning camera sensor will pick the closest color encoding to the ground truth, and it does us no good to change this value. We will come back to this issue when we talk about computing with the values in our data array, but for now, we will focus on the previous category of incorrect values (i.e., the ones we should change because there is a better representation in our encoding space of the ground truth).

```{admonition} You Try It
Before moving on, take a moment and think about collecting survey data. How might this data collection process result in incorrect captured values? Consider as many steps in the process of surveying people that you can imagine. 
```

## Noise and its removal

We describe incorrect values in our data that are a result of errors and bad samples as *noise*. Ideally, we want to make changes to our data to eliminate everything that is noise and replace it with the appropriate ground-truth values. In digital photography, this is called *image enhancement* or *photo retouching*. In data science, this is called *data cleaning*.

Unfortunately in real life, we might not always know the ground truth, and in general, we have three options:

1. *Remove the sample.* While simple to understand and implement, this is not always possible or appropriate. For example, it doesn't make much sense to "remove" a pixel from an image. On the other hand, if your data are responses to a single-question poll, it might make sense to remove a nonsensical response.
2. *Impute (or guess) at the actual value of the sample.* How you do this is highly dependent upon the type of data you've collected. For a real-world image, we might guess at a pixel's correct color by checking the colors of its neighbors. Guessing responses to poll questions, in contrast, is quite dangerous.
3. *Flag the data as missing.* With this option, any subsequent analyses that compute with the data can be informed that these missing data elements need to be handled carefully. With our image example and assuming a subsequent image recognition analysis, we might set all missing and corrupted pixels to absolute black and then inform the image recognizer that such pixels are to be ignored. In the polling example, a later analysis might include an additional "not answered" category, which might indicate something unexpected was taking place with a question that had a lot of missing or nonsensical answers (e.g., the question wasn't easy to understand).

## The power to create new realities

If noise is anything we don't want captured in our data and our data sets are simply files of numbers, we can easily get carried away and start rewriting more than just incorrect or missing data values. We may decide to rewrite any value that we consider to be problematic. When done unconsciously, we might begin to change our data in a manner that makes it easy to validate our hypotheses (and therefore invalidate our research). When done consciously, we can create new realities.

My goal is to make you aware of the need to deal with noise in your data sets, understand that it is difficult to do it well, and gain some experience in cleaning a data set. Many computational techniques exist for properly cleaning different kinds of data sets. The space is large, and this is just an introduction.

## A process for eliminating photobombing

While thinking about noise and removing it can seem like necessary and tedious work, we'll start building your skills in this large space by working on something that's particularly satisfying to remove. All of us have probably taken a picture in which we captured someone or something that we really wish hadn't been there. When someone jumps into the frame of our picture on purpose, we call it *photobombing*. We'll talk here about a general approach for removing this unwanted subject, and you'll write a script that removes a photobombing person (or object) from an image in this chapter's active-learning exercises (ALEs) 8.2 and 8.3.

The first of these exercises has you work with three pictures of the circa-1909 [Harvard Classics](https://en.wikipedia.org/wiki/Harvard_Classics) sitting on my shelf at home, each of which has been photobombed by the CS50 debugging duck. The duck is the noise we wish to remove from our data. Notice that this declaration of noise puts us squarely in the space of consciously creating a new reality. As illustrated in {numref}`Figure %s<c08_fig1_ref>`, the picture we want is not the scene I *had captured*; it is the scene I *wished* I had captured.

```{figure} images/c08_fig1.png
:name: c08_fig1_ref

Given the three images along the top of this figure, you'll write a script that produces a single image (the lower one) without the photobombing CS50 duck.
``` 

The script you'll write is a example of a filter tool, of which there are many types. This one will load a set of images, which are basically identical except for the photobombing object, and then process them using a routine from a basic statistics package. You don't have to learn a lot about image processing to write a fun image filter!

As you can see in {numref}`Figure %s<c08_fig1_ref>`, the duck is never in the same place. If you think about the set of three pixels at any location `(x,y)` across the three input images, the majority of these pixels are what we want to see. To select a suitable pixel from this majority, we can compute the median of the three pixels. By doing this across every pixel location, we'll produce a new image without the duck. Even more impressive is that this approach works even if the photobombing subject overlaps with themself/itself in a minority of the images. With additional data (i.e., images), this approach is robust!

```{admonition} You Try It
Work through ALE 8.2 to create your own photobomb-removing script. Then try it on your own images, as described in ALE 8.3.
```

## This data is not wrong, but ...

We'll now move from erasing a photobomb (i.e., eliminating incorrect data in our data set) to a new application involving images. This application takes advantage of the fact that data collection can be messy beyond the capturing of incorrect information. In particular, we also want to remove from our data set any duplicates and other types of irrelevant information. Instead of making our downstream analyses deal with these headaches, we can instead enhance our cleaning routines.

Digital images are a great example of the fact that, when we digitize the real world, we often capture it in more precision than our application demands or requires. Sometimes this additional precision is handy and useful, as we experience when we enlarge a portion of a digital photograph captured in high resolution. Our eyes cannot distinguish the extra precision in a high-resolution image, but it keeps the image looking great as we zoom in on a portion of it. Yet if the resolution of our original image is small, enlarging it results in *pixelation*, which is the blocky look that occurs when we have no extra resolution to call upon and we're left with no choice but to enlarge a single pixel into a large square of that pixel's color. As an example of pixelation, {numref}`Figure %s<c08_fig2_ref>` zooms in on the picture of my dog from Chapter 6.

```{figure} images/c08_fig2.png
:name: c08_fig2_ref

An example of the pixelation that occurs when our enlargement of an image exceeds its resolution.
``` 

Despite this benefit of capturing extra resolution, sometimes too much precision in your data can cause pesky problems. Data scientists often use real-world data to train computer models and then use these models to predict what might (or will) happen on new inputs, as we'll do in Chapter 17. If during training we pay too much attention to specifics in the captured data, the models become *overtrained*. In other words, the model begins to pay too much attention to the specific details of the real-world situations your data captured and less to the properties of those situations that generalize to the broad class of situations your captured data is supposed to represent.

## Zero out the unnecessary details

Let's get a feel for the precision we don't need in a digital image that we have no plan on enlarging. In particular, let's remove some of the precise color information in a pretty picture of the Boston Public Garden taken on a lovely October morning. As illustrated in {numref}`Figure %s<c08_fig3_ref>`, we can remove some of the precise color information from this image and most of us won't see much of a difference between the two images. 

 

```{figure} images/c08_fig3.png
:name: c08_fig3_ref

An image of the Boston Public Garden. While this figure is black-and-white, the file `images/garden.png` in the book's code repository for this chapter is in color, i.e., its pixels are encoded in `RGB` mode. The figure on the left is this original image, and the one on the right is the resulting image after removing some of the original's fine-grained color information using the `zero.py` script we're about to develop.
``` 

\`RGB\` mode represents color pixels with a tuple of three 8-bit numbers: The first representing the amount of red in the pixel; the second the amount of green; and the last the amount of blue.[^fn3] We can manipulate these numbers using not only the common unary and binary *arithmetic* operators (e.g., the unary negate operator and the binary add operator), but with what are called unary and binary *bitwise* (or sometimes called *logical*) operators. Remember that a number like 240 is stored in the computer in its binary representation (i.e., 11110000), and bitwise operators allow us to operate directly on these bits.

For example, `&` is a binary bitwise operator in Python that takes two integers, and for each bit location in the input integers performs an AND operation. This operation, for single-bit inputs, produces a `1` only when both of its inputs are `1`. If either input is a `0`, the output is a `0`. For multi-bit inputs, the single-bit operation is performed at each bit position.

```{admonition} Terminology
:class: tip
On lines 2 and 3, the `0b` prefix on the integer literals tells the Python interpreter that they are expressed in base-2, not base-10. Lines 4, 5, and 7 use a Python format specification (i.e., `:04b`) that specifies how the value of the variable or expression before the colon should be printed. I want the value printed in binary (the `b`) with a minimum width of four digits (the `4`) and padded with leading zeros (the `0`) if necessary.
```

```{code-block} python
---
lineno-start: 1
---
# Example of bitwise AND on a sequence of bits
i = 0b0011
j = 0b0101
print(f'i     = 0b{i:04b}')
print(f'j     = 0b{j:04b}')
print( '--------------')
print(f'i & j = 0b{(i & j):04b}')
```

The `&` operation on single-bit inputs is like the Python Boolean operation `and` that we've used in the condition of an if-statement, where the output of the `and` operation is `True` only when both input operands are `True`. In contrast to `&` that operates only on integer operands, `and` expects its operands to be Booleans values (i.e., `True` or `False`) or ones that can be interpreted as Boolean values.[^fn4]

```{code-block} python
---
lineno-start: 1
---
# Similar calculations using the Boolean `and` operator
a = False
b = False
print(f'{a} and {b} = {a and b}')
b = True
print(f'{a} and {b} = {a and b}')
a = True
b = False
print(f'{a} and {b} = {a and b}')
b = True
print(f'{a} and {b} = {a and b}')
```

```{admonition} Terminology
:class: tip
Whoa, you might say. Head rush. Yes, the terminology here can be confusing. Many programming languages---Python included---talk about _binary_ operators, and when they do so, they're talking about operators that take two operands (like our familiar `+` operator). They're not talking about operations that operate on binary numbers in a bitwise fashion, which are called _bitwise_ operations. Finally, these languages distinguish Boolean operations from binary operators and bitwise operations. The Python `and` operation is a binary operator that works on two Boolean operands and produces a Boolean result. The Python `&` operation is also a binary operator, and its operation is built on the same logical AND concept, but applies this logical AND concept to each bit location in the input integers.
```

Let's see the `&` operation in action to help us understand it better. Take a look at the code in the function `zero_lowest_bits` below.

```{code-block} python
---
lineno-start: 9
---
### chap08/zero.py
def zero_lowest_bits(v):
    '''Zero lowest 4 bits of an 8-bit input'''
    return (v & 0b11110000)
```

This function takes a single argument `v`, performs a bitwise AND operation with that input and an integer literal, and returns the result. The crazy-looking value `0b11110000` is the integer `240`. It is nothing more than the 8-bit representation of `240` in base-2. We've simply expressed the base-10 integer `240` as a binary value because, in this form, it is easier to see what we're trying to do. And what we're doing is forcing the lowest four bits[^fn5] of the input value `v` to be all zeros.

Let's run a few tests on this new function using the interactive Python interpreter (or copy the previous code block and the subsequent ones into a Python script).

```{code-block} python
---
lineno-start: 1
---
n = 240
rounded_n = zero_lowest_bits(n)
print(rounded_n)
```

Calling `zero_lowest_bits` with the integer `240` produces the resulting value of `240`. Since the lowest 4 bits of `240` are already all zeros, our function doesn't change the value at all.

```{code-block} python
---
lineno-start: 1
---
n = 255
rounded_n = zero_lowest_bits(n)
print(rounded_n)
```

Calling `zero_lowest_bits` with the integer `255` returns the value `240`. Notice that `240` is 15 less than `255`. The value `255` has all ones in its lowest 4 bits, and when we zero these 4 bits, we have effectively made `255` smaller by `0b1111` or `15`.

Let's run two more tests.

```{code-block} python
---
lineno-start: 1
---
n1 = 256
r1 = zero_lowest_bits(n1)
n2 = 14
r2 = zero_lowest_bits(n2)
print(r1, r2)
```

How many bits does it take to encode `256` in binary? What are the bit values above the most significant `1` in the constant `0b11110000`?

If you can answer these two questions, then you'll understand why the docstring on the function `zero_lowest_bits` says it takes 8-bit inputs. We're going to pass it 8-bit values representing the `RGB` colors of our image's pixels, but if you passed the routine an integer with a value requiring more than 8 bits, our routine will ignore any bits above the lower 8 (i.e., it will zero them out as it does with the lower 4 bits).

```{admonition} You Try It

How many bits does it take to encode `14` in binary? Do you understand why this test also returns `0`?[^fn6]

```

Hopefully by now, you are starting to see that we're rounding the input numbers, as I hinted with the naming of the returned result in the earlier code examples. You practiced rounding base-10 numbers in grade school. What's `543` rounded down to the nearest 10? `540`. What's `543` rounded down to the nearest 100? `500`. What's `543` rounded down to the nearest 1000? `0`.  If you want to start thinking in hexadecimal, we rounded the formal parameter `v` in `zero_lowest_bits` down to the nearest `0x10` (or decimal 16). In other words, we dropped the least significant hexadecimal digit just like we can drop the lowest decimal digit.

## No visible difference

Now that you're on your way to becoming a person comfortable with numbers in multiple bases, we can use this new knowledge to remove unnecessary information in an image. In other words, the detail we remove from the image will next create imperceptible changes in the image.

Read through the code for `zero_image_lowest_bits` below. You'll notice that it processes the image pixels in a fashion that we have seen repeatedly: a nested pair of for-statements that run over each dimension of the input data set. The body of the innermost loop simply grabs a specified pixel (specified by the current value of the loop indices) from the source image (`src`), manipulates it, and stores the resulting value into the same location in a destination image (`dest`).

```{code-block} python
---
lineno-start: 14
---
### chap08/zero.py
def zero_image_lowest_bits(src, dest):
    '''Zeroing lowest 4 bits in all color channels of input image'''
    for x in range(src.size[0]):
        for y in range(src.size[1]):
            r, g, b = src.getpixel((x,y))
            new_r = zero_lowest_bits(r)
            new_g = zero_lowest_bits(g)
            new_b = zero_lowest_bits(b)
            dest.putpixel((x,y), (new_r, new_g, new_b))
```

```{code-block} python
---
lineno-start: 29
---
### chap08/zero.py
from PIL import Image

imfile = 'images/garden.png'

with Image.open(imfile) as im:
    # Create a new frame for the filtered image
    im_less = Image.new(im.mode, im.size)
    
    zero_image_lowest_bits(im, im_less)
    im_less.save('images/zeroed.png')
```

Run the code above (i.e., run `zero.py` and feed it `garden.png`), and compare the image of the Boston Public Garden before and after zeroing out the lowest bits of each pixel. Can you see a difference between the original and our filtered image? If you look closely, perhaps, but most people wouldn't notice any difference on a quick glance. This tells us that most of the color information that our eyes perceive is encoded in the most significant 4 bits of the color information, and therefore, we can play with the least significant bits and not really change how most people experience our picture.

```{admonition} You Try It
Feel free to try this with your own JPEG and PNG images, but just make sure that they are encoded in `RGB` mode and not too large in size!
```

## Image steganography

This "data cleaning" has removed redundant information and given us space within the image that we can use for another purpose, if we so desire. Remember that our pixels still contain 8-bit values even if the lowest four bits of each are zeros.

Let's say our desire is to send an image to another person but not have anyone observing this transmission know what we're sending. We can accomplish this by sending one image hidden inside another. Let's call the outer, observable image *the envelope image*, and the inner, unseen image *the hidden image*. In general, we could hide any information in the envelope image, and this is called *image steganography*.

The specific process we'd use would be to "clean" a nondescript digital image to remove its redundant information and designate it as our envelope image. In the freed space, we would store our hidden image. Since we have been clearing out the lowest 4 bits of every 8-bit pixel in our envelope image, we might also throw away the lowest 4 bits of every 8-bit pixel in our hidden image. Then we might combine the 4-bit pixel values from the envelope image with the 4-bit pixel values from the hidden image to produce a resulting image with 8-bit pixels that looks like the envelope image but also contains the hidden image.[^fn7]

While image steganography is not technique used in data science, it will set us up to do something interesting and fun with the data exploration step of the general data science process.

## Where is that pixel?

When we write a Python script to blend a hidden image into an envelope image, the two images might be the same size, and in this case, we're simply laying one image over the other. We could also place several small images in a large envelope image. In this, we'll want to say where in the envelope image we want to place each hidden image, and to do this, we need a coordinate system that uniquely identifies each pixel.

From Chapter 6, we know that PIL defines the upper lefthand corner pixel with the `(0,0)` coordinate.[^fn8] But beyond this, we haven't really thought about an image's coordinate system because everything we've done has had us process each and every pixel in the image.

## And how did we get there?

Because we had to touch every pixel, we also ignored the particulars of our *traversal* of the pixels in an image. Our scripts contained two nested for-loops,[^fn9] and we randomly picked one dimension for the outer for-loop (i.e., `src.size[0]`) and the other dimension fell to the inner for-loop (i.e., `src.size[1]`). We also choose, honestly randomly, to start each for-loop at index 0. These are choices, and by making different choices, we change the *order of traversal*.

We can learn about an image's dimensions by looking at its `size` attribute. [The PIL documentation says](https://pillow.readthedocs.io/en/stable/reference/Image.html#PIL.Image.Image.size) that this attribute is "a 2-tuple (width, height)." This means that `src.size[0]` provides us with a measurement of the width of the image. In a Cartesian plane, this would correspond to the x-axis, which is often how we labeled the outer loop in our scripts. You can also think of this index as numbering the columns of pixels in our image. The other value, `src.size[1]` in our scripts, represents the height of the image, or the number of rows of pixels in it.

## Visualizing a traversal

To visualize an image's coordinate system, let's build a script that marks its moves partway through the image's pixels. To start, we'll have this script traverse an image from its origin (i.e., the upper left-hand corner) to a specified `(x,y)` coordinate. The script's code would contain a nested for-loop structure that we've regularly used to traverse the pixels in an image. Let's choose to use the names `stop_x` and `stop_y` for our specified stopping point and `i` and `j` for the two loop variables. With these decisions made, a `traverse` function might look like the following:

```{code-block} python
---
lineno-start: 1
---
def traverse(im, stop_x, stop_y):
    '''Traverse an image from (0,0) until location (stop_x,stop_y)'''
    for i in range(im.size[0]):
        for j in range(im.size[1]):
            pixel = im.getpixel((i,j))
            if (i == stop_x and j == stop_y):
                return

traverse(yard, 100, 100)
```

To call this function with a valid `(x,y)` coordinate, we need to know the size of our image, which we know is stored in an image's `size` attribute.

```{code-block} python
garden = Image.open('images/garden.png')
print(garden.size)
```

Our envelope is a nice big image. How about we place our hidden image in it at coordinate `(100,100)`? This should shift the hidden image equally distant down and to the right from the envelope image's `(0,0)` origin.

```{code-block} python
traverse(garden, 100, 100)
```

As you'll notice, our `traverse` function doesn't do anything to each point it visits. Let's extend this function so that it calls a helper function (`do_something`) where we could change the values of the visited pixels, and let's have `traverse` return the changed image.

```{code-block} python
---
lineno-start: 36
---
### chap08/xverse.py
def do_something(pixel):
    return pixel

def traverse(im, stop_x, stop_y):
    '''Traverse an image from (0,0) until location (stop_x,stop_y)'''
       and do something at each visited pixel.  The input
       image is unchanged, and the changed image is returned.
    '''
    # Create an image frame and initialize it with the input image
    new_im = im.copy()
    
    for i in range(im.size[0]):
        for j in range(im.size[1]):
            pixel = im.getpixel((i,j))
            new_im.putpixel((i,j), do_something(pixel))
            
            if (i == stop_x and j == stop_y):
                return new_im

new_im = traverse(garden, 100, 100)
new_im.save('images/xverse.png')
```

## Inverting a pixel's color

Our function is structured as we desire, but the helper function still doesn't change the value of the visited pixels. What might we do to create a path of visited pixels that is easy for us to see? How about we invert our image? It's easy to create an inverted image by simply replacing the value of each color in each pixel with the result of the calculation: `255 - orig_color_value`.

The following code block does this by replacing our do-nothing `do_something` function with one that reverses the input pixel. Try it out, and then look at the resulting image.

```{code-block} python
---
lineno-start: 31
---
### chap08/xverse.py
def do_something(pixel):
    '''Invert the input pixel'''
    r, g, b = pixel
    return (255 - r, 255 - g, 255 - b)

inverted = traverse(garden, 100, 100)
inverted.save('images/xverse.png')
```

## Naming the traversal

If you display `xverse.png`, zoom in, and find where the `(100,100)` point is, you'll see that our `traverse` function looks like it visited the pixels in the order shown on the left-hand side of {numref}`Figure %s<c08_fig4_ref>`.

```{figure} images/c08_fig4.png
:name: c08_fig4_ref

The left-hand 2D array illustrates a column-major traversal, and the right-hand one a row-major traversal.
``` 

A traversal in this fashion is called visiting the elements of the 2D array in *column-major order*. We start at the pixel at location `(0,0)` and then visit the next pixel down column 0. This next pixel is at coordinate `(0,1)`. Then we visit `(0,2)` and continue in this direction all the way down the first column. We then start on the second column with the pixel at coordinate `(1,0)`. We stop when we reach the 101st pixel in the 101st column---remember that we started counting at 0! While tedious, this should make sense because our outer for-loop runs over the image's width (i.e., columns), and the inner loop runs over its height (i.e., rows).

If we switch the two for-loop statements so that the outer now runs over the height and the inner over the width, we'd see a different pattern in our output image, which corresponds to the right-hand side of {numref}`Figure %s<c08_fig4_ref>`. Notice that the following code block just switches the for-loop statements; it doesn't change the order of the loop variables in the coordinate tuples. Make sure you understand why.

```{code-block} python
---
lineno-start: 40
---
### chap08/xverse.py
def traverse(im, stop_x, stop_y):
    '''Traverse an image from (0,0) until location (x,y)
       and do something at each visited pixel.  The input
       image is unchanged, and the changed image is returned.
    '''
    # Create an image frame and initialize it with the input image
    new_im = im.copy()
    
    for j in range(im.size[1]):
        for i in range(im.size[0]):
            pixel = im.getpixel((i,j))
            new_im.putpixel((i,j), do_something(pixel))
            
            if (i == stop_x and j == stop_y):
                return new_im

inverted = traverse(garden, 100, 100)
inverted.save('images/xverse.png')
```

This `traverse` function implements what is called a *row-major* traversal. In general, different traversals matter in different problem domains. As a simple example, consider a calendar, which is a 2D array. The columns are days of the week, and the rows are weeks of the year. You'd use a column-major traversal to discover, for example, on which weekday you had the most appointments. You'd use a row-major traversal, if you wanted to know the total number of events in each week.

## Specifying the range you want

Each of our previous traversals started from the upper left-hand corner of the image. This is because each for-loop starts at 0. Of course, we don't have to start our traversal at the `(0,0)` corner of the image. We could start from any corner.

How would we change our `traverse` function if we wanted to run a column-major traversal starting at the lower right-hand corner? Well, we'd want the sequence of integers produced by the range function in the outer for-loop to run from `im.size[0] - 1` down to `0` (inclusive). Similarly for the inner loop. Remember that the indices of the pixel elements in this 2D array run from 0 to one less than width or height!

There is a general form of the Python `range` function, as described in [the Python documentation](https://docs.python.org/3/library/functions.html#func-range), that can help. In this general form, you specify the *start*, *stop*, and *step* values you'd like `range` to use.

```{admonition} You Try It
Play with the full form of the `range` function in the interactive Python interpreter using statements in the following code blocks. These statements capture the range sequence in a list using Python _list comprehensions_.
```

List comprehensions are syntactic shorthands for creating a list from using an *expression*, an *iterable*, and an optional *condition*. It replaces the writing of a for-loop to initialize a list, where the body of the loop computes an expression and appends each computed value to the list. If you want only some of the computed values, you include an if-statement based on the desired condition. The syntax is: `newlist = [expression for item in iterable if condition]`. The `range` function produces an iterable object.

Try these in the interactive Python interpreter:

```{code-block} python
# Default start to 0 and step to 1
[i for i in range(10)]
```

```{code-block} python
# Explicitly state start is 0 and step is 1
[i for i in range(0, 10, 1)]
```

```{code-block} python
# Run the same sequence backwards
[i for i in range(10 - 1, 0 - 1, -1)]
```

## Storing a 2D array in memory

I said that different analyses will naturally use different traversals. But when a programming language linearizes a multi-dimensional data set to store those individual values contiguously in the computer's memory, it also decides on an ordering of your data set's dimensions. 

Recall our discussion of hexdump in Chapter 6. The hexdump program displayed the contents of a file as a 2D array, but this was just for ease of display in a 2D computer window. Similarly, we like to think of our JPEG files as 2D images, and we display them that way, but the contents of these files are just a single long string of 8-bit numbers.

To understand this better, it might be easier to stop thinking of image files and recall the text files from earlier in this act. These text files were nothing more than a long sequence of characters corresponding to the characters in the book we were reading. Below I ran the standalone version of `hexdump` on the file containing the text to *The Cat in the Hat* by Dr. Seuss.

```{code-block} none
---
emphasize-lines: 2
---
### NOT executable
chap08$ hexdump -C CatInTheHat.txt
00000000  54 68 65 20 73 75 6e 20  64 69 64 20 6e 6f 74 20  |The sun did not |
00000010  73 68 69 6e 65 2e 0a 49  74 20 77 61 73 20 74 6f  |shine..It was to|
00000020  6f 20 77 65 74 20 74 6f  20 70 6c 61 79 2e 0a 53  |o wet to play..S|
...
```

You should think of all files as long sequences (i.e., a 1D array) of bytes, and in this way, files are a great analogy for our computer's memory, in which we store the contents of these files while we operate on them.

What are the implications of this on how we design and implement a programming language like Python? Well, programming language designers need to decide how to linearize our 2D arrays and place them into the 1D memory. In other words, they have to decide whether to store element `(0,1)` or element `(1,0)` after `(0,0)`. Depending on this choice, computer scientists talk about a programming language as storing its 2D arrays in row-major or column-major order.

In the next act, we'll learn how to measure the performance of our programs, and the order in which the elements of a data set are stored in memory can have a significant effect on how fast your code runs. But for now, you should just know that many programming languages choose one of these two orderings, and once this decision has been made, you (as a programmer in that language) are stuck with the implications of that decision. For example, C and C++ store their arrays in row-major order, while MATLAB and R use column-major order. When you write in those languages and if you're concerned about your program's performance, you'll want to consider the language's native ordering when you decide how you'll order the nested for-loops that visit the elements of your 2D arrays.

Other programming languages, including Python, don't force your array data into row-major or column-major order. We've talked about arrays as if they are a fundamental data type in Python, like integers and lists, but they're not. We've used the abstractions in the Python Image Library, which allowed us to think about our images as 2D arrays. For general scientific computing, you'll probably use [Python's NumPy library](https://numpy.org/), which allows you (the programmer) to specify whether it should use row- or column-major ordering for its `numpy.array` type. If you don't specify a preferred ordering, this library will default to row-major ordering. It has to pick one in order to store your array in your computer's memory!

## End of Act I

You have come to the end of the first act, and it's worth recognizing what you've learned and the skills you've developed. Both are significant.

You can turn ideas into Python expressions and statements, using a variety of arithmetic (e.g., `+`), comparative (e.g., `>`), and Boolean operations (e.g., `and`). You can name the results of these statements using the assignment operator (`=`), and use them in subsequent computations. More importantly, you know how to use Python's documentation, its built-in functions like `help` and `type`, and even community resources like [stackoverflow.com](https://stackoverflow.com/) to aid you in this work. 

You learned to string multiple statements together to perform complex calculations and solve non-trivial tasks. You can take a sequence of statements written for a specific purpose and make it the body of a function. Through careful thoughts about abstraction, your functions can operate on different kinds of objects that adhere to same abstraction. Of course, you know not only how to build your own functions, but to write scripts using functions built by others.

Besides computing results, you know how to control the execution order of your statements and functions using control-flow statements. You know that if-statements test a condition and control which subsequent statements should execute. While-loops and for-loops do this too, but you understand how these statements keep our scripts short by allowing us to iterate on the same set of statements until some condition is met.

Besides expressing what you want done, you also learned to think about the data manipulated by your scripts as coming in different, distinct types. You worked with simple strings and understand what makes them different from other sequence types (e.g., a Python `list`). You learned about operator overloading, and how the `+` operator changes its function depending upon the types of the data to which it is applied. And you learned to use complex data types, like file and network objects. We know only rudimentary aspects of these data types and how they are constructed, but through a variety of interfaces and methods, you were able to employ them to solve non-trivial problems (i.e., query the contents of the Harvard Library and create your own networked game).

Separate from these skills in computer programming, you've built a solid foundation for problem solving with computers. In fact, you can now use computers to solve not just an instance of a problem but to create a script capable of solving many similar problems.

You repeatedly practiced starting with a general statement of a problem and then decomposing it into its constituent parts. You gained significant practice in a particular approach to problem solving called worklist processing, and by understanding how computers digitally encode and represent real-world ideas and information, you can manipulate them.

Importantly, you're comfortable with problem solving as an iterative process. All too often, our first attempt at a script fails to run without an error, or if it does run to completion, the script produces unexpected results. You understand that you should avoid certain types of problematic code structures (e.g., a file open without a file close) and anticipate the potential problems in other design patterns (e.g., variables that are aliases for each other). I hope you have grown skeptical and regularly check the form of your input data and the results of some computations before you start using them. And when things inevitably still go wrong, you've begun to understand the Python interpreter's error messages and to work through your scripts a step at a time to verify which parts work and where the failure likely resides.

You are no longer a novice. You are now ready to learn to use computers to solve a more complex set of real-world problems.

\[Version 20240827\]

[^fn1]: In Chapter 17, we will explore the model building portion of the data science process.

[^fn2]: JPEG and other image file formats are more than simple 2D arrays. They contain metadata, like the time and date on which the image was captured, and they often compress the actual image data so that the file takes less space to store. Computer scientists like to explore the tradeoffs between processing time and storage space.

[^fn3]: ALEs 6.1 and 6.2 discuss \`RGB\` mode and encourage you to experiment with its pixel encoding.

[^fn4]: Python is lenient about what can be interpreted as a Boolean value, and while it isn't important to what we're doing here, you might want to read [the Python documentation on Boolean operations](https://docs.python.org/3/reference/expressions.html#boolean-operations) if you're curious.

[^fn5]: "Lowest" in this phrase refers to the right-hand side of the number, which is where we put the digits with the least significance to the whole number. Zeroing the 3 in 543 doesn't change the number as much as zeroing the 4 or the 5. The same is true in binary numbers.

[^fn6]: The base-10 number \`14\` in binary (i.e., base-2) is \`0b00001110\` as an 8-bit binary literal. By zeroing the lowest four bits of this number, we produce the value \`0\` (i.e., \`0b00000000\`).

[^fn7]: If you're in a course using this book, this chapter's programming problem set asks you to implement this process. Of course, there are many ways to hide the hidden image in an envelope image. For example, you could also distribute the full 8-bit pixels of the hidden image across two pixels of the envelope image, which had their lower 4 bits cleared. This alternate approach requires that the envelope image is as least twice as large as the hidden image.

[^fn8]: Here I'm using the tuple notation we use with PIL's \`getpixel\` and \`putpixel\` methods.

[^fn9]: If our data set was organized with more dimensions, our code would include a larger number of nested for-loops.