# Chapter 8 #

## ALE 8.1: Parameters and Parentheses

We used the `Image.putpixel` method many times in this chapter. [Its documentation](https://pillow.readthedocs.io/en/stable/reference/Image.html#PIL.Image.Image.putpixel) says that it takes two input parameters: an `xy` coordinate of the pixel in the Image object, and the `value` we want there. The script `zero.py` used this method in the function `zero_image_lowest_bits` as follows:

```{code-block} python
---
lineno-start: 14
---
### chap08/zero.py
def zero_image_lowest_bits(src, dest):
    '''Zeroing lowest 4 bits in all channels of input image'''
    for x in range(src.size[0]):
        for y in range(src.size[1]):
            r, g, b = src.getpixel((x,y))
            new_r = zero_lowest_bits(r)
            new_g = zero_lowest_bits(g)
            new_b = zero_lowest_bits(b)
            dest.putpixel((x,y), (new_r, new_g, new_b))
```

**Step 1.** What's wrong with this version of the statement on line 23?

```{code-block} python
---
lineno-start: 23
---
            dest.putpixel(x, y, new_r, new_g, new_b)
```

**Step 2.** Would this version of `zero_image_lowest_bits` work? It clearly passes two actual parameters that match the two formal parameters in the `Image.putpixel` interface.

```{code-block} python
---
lineno-start: 15
---
def zero_image_lowest_bits(src, dest):
    '''Zeroing lowest 4 bits in all channels of input image'''
    for x in range(src.size[0]):
        for y in range(src.size[1]):
            r, g, b = src.getpixel((x,y))
            new_r = zero_lowest_bits(r)
            new_g = zero_lowest_bits(g)
            new_b = zero_lowest_bits(b)
            
            coordinate = (x,y)
            new_pixel = (new_r, new_g, new_b)
            dest.putpixel(coordinate, new_pixel)
```

## ALE 8.2: Erase the Duck

Now that we know how to read and write the pixels in a digital images, we're going to use that knowledge to solve a common problem: removing an unintended subject in a sequence of pictures containing no moving objects except for our unintended subject. This exercise was inspired by [John Nicholson's 2014 Nifty Assignment titled The Pesky Tourist](http://nifty.stanford.edu/2014/nicholson-the-pesky-tourist/), and we thank him for sharing it with the world.

How will our script remove the unintended subject (i.e., the noise in our digital image)? Our script will:

1. Expect you to give it *N* pictures, for *N* greater than 2, that are all of the same size and orientation;
2. Visit each pixel location in this common image frame, reading the pixel values there for each of the input pictures; and 
3. Compute the median of these values as the value of each pixel in the final filtered image.

Ready to get started?

**Step 1. Reading multiple input files.** The fifth step of our problem-solving process says that we should begin a new problem by considering what previously written scripts can help us with our current problem. In this case, the script from ALE 6.5 would be a great start as both that script and the one we're writing visit each pixel in an input image.

That script is repeated and renamed below:

```{code-block} python
---
lineno-start: 1
---
### chap08/ale02.py
from PIL import Image

# Grab the image filename
imfile = 'images/' + input('Filename of image: ')

with Image.open(imfile) as im:
    # Apply a filter that enhances the red and desaturates blue/green
    for x in range(im.size[0]):
        for y in range(im.size[1]):
            r, g, b = im.getpixel((x,y))
            im.putpixel((x,y), (r, g//50, b//50))

    im.save('images/out.png')
```

Change this script so that it reads *exactly three* input files from *the command line or from user input*.

**Step 2. Opening multiple input files.** Now that our script knows the names of three input files, we need it to open the three files.

Edit the `with` `Image.open` line so that it repeats the open-as phrase in the with-statement three times. Each open-as phrase should be separated by commas. This will cause the script to open (and later close) multiple image objects. It should look something like the following, with each `open_as_phrase` replaced with `Image.open(filename_var) as image_object_name`:

```{code-block} python
with open_as_phrase1, open_as_phrase2, open_as_phrase3:
```

The resulting with-statement might get quite long, and you can split it across multiple lines at a comma by using the line continuation character in Python, which is the backslash (`\`).

Take a look at [the Style Guide for Python Code (PEP 8) under "Maximum Line Length"](https://www.python.org/dev/peps/pep-0008/#maximum-line-length) to see an example of how this is done. As it says there, we prefer to use Python's implied line continuation in most long-line situations rather than this explicit line continuation syntax, but this is a common situation where you can't use anything but the explicit syntax.

**Step 3. Testing what we've done so far.** Take a moment and add some statements to your script so that it prompts the user to ask which input file the script should modify in the nested for-loops and then save the changed image in `out.png`.

As you do this work, don't worry about checking for all kinds of bad user input: You're just testing what you've written in steps 1 and 2, and we'll delete this extra code in a moment. Do, however, make sure that the correct file was displayed!

If you have access to the this book's Github repo, you can test your script using the image files with the prefix name `duck` or `photobomb` in the `images` directory under `chap08`.

When you're done testing your script, get rid of the prompt you just added, but make sure you leave the statements you added in Steps 1 and 2!

**Step 4. Finding the statistical median.** Alright, you are now ready to erase an unintended subject. The nested for-loops provide the logic we need to visit each pixel in an image, but the statements in the body of these nested for-loops are not the work we need done. The work we need is to grab the pixel values at each location `(x,y)` in the three input images and compute the median of these values.

```{margin} Read about the Function
You should [read about `statistics.median`](https://docs.python.org/3/library/statistics.html#statistics.median) before attempting to complete this step.
```

Although we could write a function that computed a median given a list of numbers, let's take advantage of the `statistics` library in Python, which has conveniently provided us with a `statistics.median` function. You'll notice that the `statistics.median` function may produce a value that is not one of the input data points. This will be a problem for us because we don't want our input integer values turned into an output floating-point value. Our color values are integers and need to stay as such. While you might force the value returned from `statistics.median` back into an integer, we can also simply use `statistics.median_low`. Ahh, what a helpful and a well-designed library!

Once you have a median value (and think about what that means for a pixel), you'll want to store the result in a new output image.

```{tip}
Write pseudocode reflecting what was just discussed. Don't try to jump right into Python code! Make sure you see the script's complete flow in pseudocode before you start translating these new pieces of logic into Python. It will save you time in the long run!
```

```{margin}
If you are unfamiliar with this children's game, [this Wikipedia article](https://en.wikipedia.org/wiki/Where%27s_Wally%3F) provides some background.
```

To test your code with our images, start with the three `photobomb` images in the `images` directory. If those work, check your code with the three `duck` images. The `photobomb` images were created by copying a single picture of Harvard Yard and then inserting Waldo from *Where's Waldo?* fame into this image in three different locations. The three `duck` images are a real sequence of three pictures with a common background and moving character. As such, many pixels---even those not obscured by the duck---are subtly different from picture to picture.

**Step 5. The need for more input files.** Congratulations on erasing the CS50 debugging duck and Waldo from our images! As wonderful as our current script is, it isn't very robust. For example, it expects exactly three input images, and the unintended object can't overlap with itself in any of the three images.

To see this second point for yourself, run your solution to the previous step with the first three images with the prefix `apsu`, which have a `.png` extension. Look at the resulting image very closely, and you'll see a shadowy image of the person's feet where the unintended subject overlapped with his previous self. 

**Step 6. Accepting and computing with any number of input files.** We will eliminate the problem identified in Step 5 by building a script that accepts more input images. The unintended object or subject could then overlap with itself/himself in some of the input images as long as the majority of the images contained only the desired background.

To begin, make a backup copy of your current script, and then follow the directions below:

1. Eliminate the statements that allowed the user to input the image filenames. Your final script will accept the input image filenames from the command line.
2. Fix the "proper usage" check so that it expects *at least 3 command line arguments*. Remember that the your program's name (but not the `python3` interpreter) counts as one of the command line arguments.
3. Time to write some pseudocode again. Think about a data structure that we can use to collect all the input filenames as well as keep track of our image objects that you'll open using these filenames. To this point, you were probably using a unique variable for each input filename and the result of each of the three `Image.open` commands. This worked because we knew that there were exactly three of each. We now need a data structure that can expand while the program runs to accommodate all the filenames and open-file objects we'll need. Can you think of a data structure that will allow us to work with a variable-length list of objects? Yes, we just said it! A Python `list` will do this quite nicely. We'll talk a lot more about different types of common data structures in the next act. Go through your script and add pseudocode for every piece of work that needs to change because you're using lists instead of single variables.
4. When you think you've got all the logic you think you need in pseudocode, start translating this pseudocode into Python. Don't forget that you can ask for the size of the `sys.argv` list and subtract `1` from this value to determine the number of input files. This might be very useful in creating a bound you could use in one or more for-loops that walk over the elements in a list.
5. While translating pseudocode into Python code, you'll undoubtedly realize that the nice with-statement we've been using requires us to know the exact number of files we want to open at the time we write the script. Yup, you are going to have to open each input image file and later explicitly close each one (using `Image.close`).

Once you've done all this work, run your script as follows:

```{code-block} none
python3 ale02.py apsu1.png apsu2.png apsu3.png apsu4.png apsu5.png apsu6.png
```

Did this produce a much cleaner resulting image?  It should! Go show your family and friends your amazing script. (You might rename your final script from `ale02.py` to `erase32.py`.)

```{margin} Hint
What's different in how we gathered the original data (i.e., images) in these two cases?
```

**Step 7. Why median, but not mode?** Take a look at the files `images/photobomb-mode.jpg` and `images/duck-mode.jpg`. These files were produced by our solution script, except that we used `statistics.mode` instead of `statistics.median` to compute the pixel we placed in the output image. As you'll see by looking at these two images, we successfully erased Waldo using the `mode` function, but not the duck. Why is this?

And when you figure out why median is more robust an approach than mode, please make sure you can explain why is it the first duck that remains. This doesn't have anything to do with the statistical properties of the function, but choices that the library designer made. What are you learning about quantitative reasoning with data here?

## ALE 8.3: Create and Fix Your Own Photobomb

In 8.2, you created a script that fixed photobombs in our pictures. Now it is your turn.

**Step 1.** Take a sequence of 3 or more pictures with your smartphone and download them to your computer. Here are some recommendations that will help you make the next step work well:

1. Find a place where you can set your phone and it won't move for your sequence of pictures. Ideally, use some kind of tripod.
2. In the scene you want to capture in a picture, nothing should be moving. If you want to capture a person in front of a beautiful backdrop, make sure that nothing in the whole scene will be moving, including your subject. In other words, the person needs to stand/sit still for a few pictures.
3. Now decide on the photobomb(s) that will be in your pictures and that your script will eliminate. If that's another person, have them move after you take each picture. Make sure that they move enough that they don't appear stationary. If you want to include two photobombing people, try it!
4. Take enough pictures that, for all points in the scene you want, the majority of the pictures show that each point unobscured.
5. On your computer, reduce the size of your images --- ideally no more than a few hundred kilobytes. The larger the images the longer your script will take to run!
6. Make sure you saved the reduced images in RGB, not RGBA, format.

**Step 2.** Run your images through the script you built in 8.2. Show your friends and family!

\[Version 20230722\]
