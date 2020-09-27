# Image-Transformation

## Part 1: HSLAPixel
## Understanding Color Spaces
we will not be working with the physical properties of color that you may be familiar with from
other sources (the “RGB color space” for red-green-blue channels.) Instead, we will be using an alternative
color space that represents colors by human perception of color. The HSL color space uses the Hue, Saturation,
and Luminance of the color. From the Adobe Technical Guides page on “The HSB/HLS Color Model”, Adobe
explores these terms:

## Hue
Hue (denoted as h) defines the color itself, for
example, red in distinction to blue or yellow. The
values for the hue axis run from 0–360° beginning
and ending with red and running through green,
blue and all intermediary colors like greenish-blue,
orange, purple, etc.
There are two main hues :
* "Illini Orange" has a hue of 11.
* "Illini Blue" has a hue of 216.
![Hue](https://github.com/KEROLIS/Image-Transformation/blob/master/Hue.png)

## Saturation
Saturation (denoted as s) indicates the degree to which the hue differs from a neutral gray. The values run from
0%, which is no color saturation, to 100%, which is the fullest saturation of a given hue at a given percentage of
illumination.

![Saturation](https://github.com/KEROLIS/Image-Transformation/blob/master/staruation.png)

## Luminance
Luminance (denoted as l) indicates the level of illumination. The values run as percentages; 0% appears black
(no light) while 100% is full illumination, which washes out the color (it appears white).

![Luminance](https://github.com/KEROLIS/Image-Transformation/blob/master/Lumi.png)

![Overall](https://github.com/KEROLIS/Image-Transformation/blob/master/Overall.png)

## HSL Color Space
The full HSL color space is a three-dimensional space, but it is not a cube (nor exactly cylindrical). The area
truncates towards the two ends of the luminance axis and is widest in the middle range. The ellipsoid reveals
several properties of the HSL color space:
* At l=0 or l=1 (the top and bottom points of the ellipsoid), the 3D space is a single point (the color black
and the color white). Hue and saturation values don’t change the color.
* At s=0 (the vertical core of the ellipsoid), the 3D space is a line (the grayscale colors, defined only by
the luminance). The values of the hue do not change the color.
* At s=1 (the outer shell of the ellipsoid), colors are vivid and dramatic!


# Part 1 Programming Objectives 
the HSLAPixel class within the uiuc namespace in the file uiuc/HSLAPixel.h . Each
pixel contains four public member variables:
* h , storing the hue of the pixel in degrees between 0 and 360 using a double
* s , storing the saturation of the pixel as a decimal value between 0.0 and 1.0 using a double
* l , storing the luminance of the pixel as a decimal value between 0.0 and 1.0 using a double
* a , storing the alpha channel (blending opacity) as a decimal value between 0.0 and 1.0 using a double

# Compiling the code

To compile the code, you must use the terminal to enter the same directory where the **Makefile** is stored; it's in
the top directory next to **main.cpp** . As explained in the readings, use **cd** to change to the appropriate directory,
use **ls** to view the file list in that directory, and then type **make** to automatically start the compilation. If you
need to clear out the files you've previously built, type **make clean** . If you encounter any warnings or errors
during compilation, study the messages in the terminal carefully to figure out what might be wrong with your
code.
The primary compilation sequence, once successful for all parts of this MP, will build an executable file simply
called **ImageTransform** in the top directory. You'll be able to run it by typing **./ImageTransform** in that
directory to launch a sequence of image processing operations. However, first you will also want to compile
the test suite and make sure you are doing Parts 1 and 2 correctly with unit tests. To compile the tests, type
**make test** in the top directory. If successful, this will generate an executable called **test** , which you can run
by typing **./test** in the same directory. We use the widely-used C++ testing framework Catch. You are always
encouraged to write additional test cases; the executable Catch generates will run all of your tests and show you
a report.
![test](https://github.com/KEROLIS/Image-Transformation/blob/master/test.png)

## Part 2: Using uiuc::PNG
Now that we have an**HSLAPixel** , it's time to manipulate some images! First, let's understand the PNG class that
is provided for us! Note that PNG stands for "portable network graphics." It's a useful image file format for
storing images without degradation (called losssless compression). A photograph saved as PNG will have a
much larger filesize than a JPEG, but it will also be sharper and the colors won't bleed together as happens with
JPEG compression. Apart from photographs, though, PNG offers relatively small filesizes for simple graphics
with few colors, making it a popular choice for small details in a website user interface, for example.

Inside of **uiuc** directory, you may have noticed **PNG.h** and **PNG.cpp** .It's provided an already
complete PNG class that saves and loads PNG files and exposes a simple API for you to modify PNG files. (An
API, or application programming interface, just means the documented, surface-level part of the code that you
work with as someone who is making use of the library. You do not need to fully understand how the PNG class
works underneath in order to use it. But, it's good for you to practice surveying library code, seeing how it is
organized, and taking note of its intended usage.)

## For loading and saving PNG files:
* bool PNG::readFromFile(const std::string & fileName)
loads an image based on the provided file name, a text string. The return value shows success or failure. The meaning & is discussed in Week 3
in the lecture about variable storage; it means a direct reference to the memory is being passed, similar
to a pointer.

* bool PNG::writeToFile(const std::string & fileName)
writes the current image to the provided
file name (overwriting existing files).

# For retrieving pixel and image information:
* unsigned int PNG::width() const returns the width of the image.
* unsigned int PNG::height() const returns the height of the image.
* HSLAPixel & getPixel(unsigned int x, unsigned int y) returns a direct reference to a pixel at the specified location.

## Part 2 Programming Objectives

As you know, all C++ programs begin their execution with the main function, which is usually defined in
main.cpp. You can find that a main function has been provided for you that:
* Loads in the image alma.png
* Calls each image modification function
* Saves the modified images named as out-modification.png , where modification shows the function being tested (e.g. out-grayscale.png )

# The transformations 
## Function #1: illinify
To illinify an image is to transform the hue of every pixel to Illini Orange (11) or Illini Blue (216). The hue of
every pixel should be set to one or the other of these two hue values, based on whether the pixel's original hue
value is closer to Illini Orange or Illini Blue. Remember, hue values are arranged in a logical circle! If you keep
increasing the hue value, for example, what should eventually happen?
![illinify](https://github.com/KEROLIS/Image-Transformation/blob/master/out-illinify.png)

## Function #2: spotlight
To spotlight an image is to create a spotlight pattern centered at a given point ( centerX , centerY ).
A spotlight adjusts the luminance of a pixel based on the distance between the the pixel and the designated
center by decreasing the luminance by 0.5% per 1 pixel unit of Euclidean distance, up to an 80% decrease in
luminance at most.
For example, a pixel 3 pixels above and 4 pixels to the right of the center is a total of sqrt(3*3 + 4*4)
= sqrt(25) = 5 pixels away and its luminance is decreased by 2.5% (0.975 its original value). At a distance
over 160 pixels away, the luminance will always be decreased by 80% (0.2x its original value).
![spotlight](https://github.com/KEROLIS/Image-Transformation/blob/master/out-spotlight.png)

## Function #3: watermark
To watermark an image is to lighten a region
of it based on the contents of another image
that acts as a stencil.
You should not assume anything about the
size of the images. However, you need only
consider the range of pixel coordinates that
exist in both images; for simplicity, assume
that the images are positioned with their
upper-left corners overlapping at the same
coordinates.
For every pixel that exists within the bounds
of both base image and stencil, the luminance of the base image should be increased by +0.2 (absolute, but not
to exceed 1.0) if and only if the luminance of the stencil at the same pixel position is at maximum (1.0).
![watermark](https://github.com/KEROLIS/Image-Transformation/blob/master/out-watermark.png)

