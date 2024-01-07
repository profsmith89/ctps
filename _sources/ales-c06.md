# Chapter 6 #

## ALE 6.1: Capturing color

We've learned that digital images are nothing more than large two-dimensional arrays of pixels, which we can load into a pixel array and manipulate. Except for the `edges.py` script in which we ran a prepackaged convolution filter on a color image of my dog, we limited ourselves to grayscale images containing 8-bit pixels. With 8 bits, we could represent 256 different shades of gray from black (`0x00`) to white (`0xff`).

While black-and-white photos can be quite artistic, more often most of us capture color images. I used to take color pictures with a digital single lens reflex (DSLR) camera, but now I take all my photos (and videos) with my cellphone. The manufacturer describes the camera on the back of my cellphone as containing a 12-megapixel sensor, where mega- means $10^6$ or 1 million. 

The manufacturer-quoted number isn't an exact measure. The sensor in my cellphone is actually a 2D array of size 4290 by 2800, and the color pictures taken with this camera produce just over 36 megabytes (MB) of image data (by "just over" I'm telling you that the 36MB measure also isn't exact). Despite the inexactness of both numbers, you can use them to determine **the size of each color pixel in bytes**.

```{admonition} You Try It
Write out an equation that divides the size of the color images taken by my cellphone by the total number of pixels in my cellphone's sensor. That equation will tell you the number of bytes per pixel. In other words, you'll know how many bytes there are per pixel in a color (i.e., RGB-encoded) image. Do the math to figure out the answer.
```

You should have found that the simplest color images (i.e., those that use `RGB` mode) use three 8-bit numbers to represent the color of a pixel. In Python, each RGB pixel is a tuple of three 8-bit numbers, where the first byte (i.e., 8-bit value) represents how much red is in the pixel. The next indicates how much green the pixel contains, and the last value is the amount of blue. Three zeros would indicate the absence of any of the three colors and would display as black; recall that 0 represents black in every mode. Three values of 255 would appear as a white pixel.

## ALE 6.2: Diagonal fade to a color

In this chapter, the script `edge1.py` created a grayscale image containing a diagonal fade. For this exercise, you'll change this script so that it fades not from black to white, but from black to one of red, green, or blue. To do this, we'll need to change the mode of the image we create from `'L'` to `'RGB'`.

```{margin} HINT
`pixels[i,j]` no longer holds just a single integer. Focus on the color component you want and leave the other black (i.e., `0`).
```

**Step 1.** Given what you learned in ALE 6.1, complete line 16 in the script (`ale02.py`) to create a diagonal fade from black to red. 

```{code-block} python
---
lineno-start: 1
---
# chap06/ale02.py
from PIL import Image

# Width and height of our image
sz = (100, 100)

# Create a single plane of black and white pixels, initialized to black
im = Image.new('L', sz)

# Create direct access to the pixels in the image
pixels = im.load()

# Set the color of each pixel
for i in range(sz[0]):
    for j in range(sz[1]):
        # pixels[i,j] = ???

im.save('ale02.png')
```

**Step 2.** Try not just a diagonal fade from black-to-red, but also create ones from black-to-green and black-to-blue.

## ALE 6.3: Creating a checkerboard

Starting again with `edge1.py`, which creates an image containing a diagonal fade, change the body of the innermost for-loop in `edge1.py` so that it creates a checkerboard pattern. The checkerboard pattern should adhere to these rules:

* Each pixel contains either the color black or white; and
* The color of any pixel is not the same as its neighbors to the north, south, east, and west, i.e., if the current pixel is at `(i,j)`, the pixel to the north is at `(i,j-1)`, to the south `(i,j+1)`, to the east is `(i+1,j)`, and to the west is `(i-1,j)`. Don't worry about neighbors that are not within the pixel array.

You'll  only need to change line 17 in `ale03.py`. It currently sets every pixel in the `pixels` array to the color white.

```{code-block} python
---
lineno-start: 1
---
# chap06/ale03.py
from PIL import Image

# Width and height of our image
sz = (100, 100)

# Create a single plane of black and white pixels, initialized to black
im = Image.new('L', sz)

# Create direct access to the pixels in the image
pixels = im.load()

# Set the color of each pixel
for i in range(sz[0]):
    for j in range(sz[1]):
        # Create a checkerboard
        pixels[i,j] = 255

im.save('ale03.png')
```

## ALE 6.4: Counting with Python integers

If you look carefully at the script named `edge1.py`, you'll see that it doesn't do arithmetic on pixels, but on the loop indices `i` and `j`, which are values produced by the `range` function. If you pass these names to the built-in function `type`, it will tell you that these names refer to Python integers.

From experience, we know that Python integers can hold values greater than 255 and less than 0, and so it is not the addition in our script that overflows and forces the saturation, but the act of trying to store the result of this addition into a location in the array `pixels` that causes an issue. In particular, the PIL library checks to see if the value we're trying to store overflows (or underflows) the 8-bit, unsigned representation used for each pixel in the `pixels` array.

That's great, but if we need to be careful with the values we put into the pixels of a black-and-white image, **do we need to be careful about the values we put into a Python integer?** In other words, **will addition on Python integers ever overflow?**

We could go read some documentation or google an answer to this question, but we can also experiment and see what happens.

**Step 1.** We have been using integers to enumerate the things in a set (e.g., shades of gray between black and white). Integers are also great when we simply want to count, and so, let's see how far we can count with Python integers before we hit an overflow condition.

The script `ale04_count.py` contains an infinite loop with no exit condition. Inside this loop, we add one to the variable `i`, which we initialized to `0`, which made it a Python `int`.

```{code-block} python
---
lineno-start: 1
---
### chap06/ale04_count.py
i = 0
next_bound = 1

while True:
    i += 1
    if i == next_bound:
        print(f'Reached {i}')
        next_bound *= 10
```

Because we're not interested in seeing every value that `i` takes on, there's a bit of extra code in this script to print only when `i` reaches a number that's a power of 10. Printing takes time, and we don't want to unnecessarily slow down our script. With a friend, walk through the statements in `ale04_count.py` and make sure you understand the entire script.

**Step 2.** If you were to run this script, when will it stop running? Think about this question for a bit, jot down the reason for your answer, and then read on.

**Step 3.** When we run `ale04_count.py`, we fly through the first bunch of powers of 10, showing that our computers can count much faster than we can. My computer gets to 100 million in about 10 seconds, and then it starts to take a noticeably long time between prints. If I listen carefully, I can hear its fan spin up as the processor starts working really hard. 

Notice that there's no break statement anywhere in the infinite loop, and so the only way that this loop will end is if a runtime exception occurs. Given the statements in the loop's body, we're waiting for either the addition or the multiplication to overflow.

```{tip}
Help! I Want It To Stop!

I hope you tried running `ale04_count.py`, and at some point, you'll want it to stop. Here's how:

*   If you started the script in a Shell, push the control-key and the letter-c-key at the same time, which we'll abbreviate as ctl+c. This key sequence tells the Shell to kill the execution of the currently running script.
*   If you clicked a play button in your IDE, the play button probably turned into a stop button, and if you click this stop button, the IDE will kill the execution of your running script.
```

If you haven't tried running `ale04_count.py`, now that you know how to stop it, run it now. Watch its output for a bit and then force it to stop using one of the two methods above.

**Step 4.** Let's think about the work your processor is doing while running `ale04_count.py`. It's not hard work; it's just adding 1 over and over again. But to get to a big number, it will take a lot of additions. Let's make this concrete by doing a quick calculation: 

If it took 10 seconds to get to 100 million, estimate how long will it take to get to a trillion, which is 1/30th of the [US debt](https://www.usdebtclock.org/) (as of 2022) and therefore a number we may want to represent in a script?

**Step 5.** You should have found that the answer is a lot longer than you'd like to wait. Maybe `ale04_count.py` isn't the best way to find if addition overflows.

Edit the script in the code block below (`ale04_double.py`), so that it doubles `i` on each loop iteration rather than adding 1 to `i` (i.e., write Python code for the comments on lines 4 and 8).

Because this script will increase `i` so much faster than our last script, I've inserted a call to `time.sleep` and specified that the script should pause for one-half a second each time it passes the `next_bound`. If you comment out this call, you'll see why this is necessary.

```{margin} Tie Back To Bits
When you complete it, `ale04_double.py` adds another bit (i.e., another binary digit) to the length of `i` for each execution of its loop body. Make sure you fully understand why this is true.
```

```{code-block} python
---
lineno-start: 1
---
### chap06/ale04_double.py
import time

# FIXME: initialize i appropriately
next_bound = 1

while True:
    # FIXME: update i so it doubles each time
    if i > next_bound:
        print(f'Passed {i}')
        next_bound *= 10
        time.sleep(0.5)
```

**Step 6.** The two scripts in this exercise demonstrate two different *growth rates*: the first exhibits *linear growth* (i.e., two successive values differ by a constant amount); and the second *geometric growth* (i.e., two successive values differ by a constant ratio). We'll encounter growth rates again in Act II.

**Step 7.** Will addition on Python integers ever overflow? Let's finally answer this question, which was our goal in this exercise. When you run `ale04_double.py` and see `1099511627776`, the script has passed a trillion. It probably took less than 10 seconds to pass that value on your computer. 

You can let `ale04_double.py` continue to run for a while, but at some point, kill its execution. This script clearly demonstrates that we can count pretty high in Python. In fact, Python does not restrict the size of an integer. As long as your computer has the storage necessary to hold whatever big number you can dream up, Python can handle it. This is a very nice feature of this language, and one that is not true in every other programming language.

## ALE 6.5: Color the duck's world

Our earlier exercises created their own images. In this exercise, you will start learning how to modify an existing digital image. You'll also learn a new set of methods for accessing and changing the value of a particular pixel in your open image.

**Step 1.** In your favorite JPEG viewer, open the color image named `duck.jpg`, which you can find in `chap06/images` of the book's GitHub repo. It should look like a perfectly normal picture that you might have taken with your own cellphone camera.

**Step 2.** The script `ale05.py` uses the Python PIL library to open, read, and recolor a single input image. Read through the script and then we'll briefly discuss what's new in it.

```{code-block} python
---
lineno-start: 1
---
### chap06/ale05.py
from PIL import Image

# Grab the image filename
imfile = 'images/' + input('Filename of image: ')

with Image.open(imfile) as im:
    # Apply a filter that enhances the red and desaturates blue/green
    for x in range(im.size[0]):
        for y in range(im.size[1]):
            r, g, b = im.getpixel((x,y))
            im.putpixel((x,y), (r, g//50, b//50))

    im.save('ale05.png')
```

Because we're working with an existing image, the first thing we have to do is open the image file as we did in `edges.py`.

An existing image contains a 2D image array just like we encountered when we built a new image, and this means that we'll still need our familiar nested for-loops if we are going to apply a filter to each pixel in the image.

How big is the image we opened? Well, we knew the size of the images we created because we specified them. Luckily, we can also ask an existing image for its width and height in pixels through the attribute called `size` because `Image.open` fills in this attribute. This attribute returns a 2-tuple of `(width,height)` over which we can iterate.

The body of our innermost for-loop looks a bit different than what we've previously seen. It uses two new methods on the image object: `getpixel` and `putpixel`. These two methods each take a parameter, which is a 2-tuple representing the `(x,y)` coordinate of the pixel we wish to get or put. In the coordinate system of the images we manipulate, `x` indexes the columns (of which there are `width` columns) and `y` indexes the rows (of which there are `height` of them).

So far, this should remind you of the x-y Cartesian plane you probably encountered at some point in your prior schooling. The only twist in the image world is that index `(0,0)` is the upper left corner of the image.

With `getpixel`, the pixel comes back as a tuple, which we can pull apart with our assignment statement. To write a pixel using `putpixel`, we not only want to specify the `(x,y)` coordinate of the pixel in the image, but also the RGB tuple `(r,g,b)` we want to write.

I'll discuss the `//` operator in a moment.

**Step 3.** Run `ale05.py` and feed it the image file `duck.jpg`. Can you an understand why it displays a reddish version of our input picture?

We enhanced the red in our image by desaturating the blue and green values of each pixel. In particular, we scaled down the green and blue values by some fixed factor, which we did through division.

If you look up the division operator in Python, you'll see that there are two: `/` and `//`. The single forward-slash operator (`/`) is called *division*, and if you feed it two integer operands, it will produce a *floating-point value*. A floating-point number is number with a decimal point, which in Python are given the type `float`. For example, `42` is an `int` in Python, but `42.0` is a `float`. We need float-point values because we can't represent a number like one-half (i.e., one divided by two) as an integer. We need both a whole and a fractional part. We'll talk more about floating-point numbers in Chapter 7, but right now we need to scale our green and blue values so that they continue to be integers, since that's what we're storing in our image files.

The double forward-slash operator (`//`) is called *floor division* in Python, and if you feed it two integer operands, it will produce an integer result. This integer result is equivalent to what you'd get if you applied the floor function to the floating-point result from the `/` operator. With positive results, you can think of `//` returning just the whole number part of the result. 

**Step 4.** Can you edit `ale05.py` so that it highlights the blue aspects of the input image? How about the green aspects?

**Step 5.** Notice that the PIL library functions are smart enough not to care if we feed a JPEG or PNG file to our script. You can test this by running `ale05.py` with other input files. Try some of your own digital photos. Remember to put your digital photos in the images directory!

```{admonition} Not Too Big!
The amount of time it takes to process an image is proportion to the size of the image in bytes. Why? Because the bigger your image is the more pixels it has, and the script has to read and write each one. I've been using images of 100-200 kilobytes. Don't use ones with many megabytes.
```

\[Version 20230710\]
