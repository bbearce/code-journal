# Image Processing With OpenCV and Pillow

## What is a Digital Image
A rectangular array of numbers.

![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_1.png](Images/pixels_1.png)

In the real world, an image can take on an almost unlimited number of values, but digital images have intensity values between zero (black) and 255 (white). It turns out that's all we need, 256 different intensity values to represent an image.

If we use less than 256 values, things will look cartoonish.


![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_2.png](Images/pixels_2.png)

RGB:

![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_3.png](Images/pixels_3.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_4.png](Images/pixels_4.png)

Image Masks:
![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_5.png](Images/pixels_5.png)

Basic Image Types:

An image is a file on your computer. Two popular image formats, Joint Photographic Expert Group image or JPEG, and Portable Network Graphics or PNG, these formats reduce file size and have other features. No matter what Python library you use, you're going to have to load the image

### PIL Intro

![Images/Image_Processing_With_OpenCV_and_Pillow/PIL_1.png](Images/PIL_1.png)


ImageOps:
![Images/Image_Processing_With_OpenCV_and_Pillow/PIL_2.png](Images/PIL_2.png)

Quantizing:
![Images/Image_Processing_With_OpenCV_and_Pillow/PIL_3.png](Images/PIL_3.png)


### Numpy Intro
![Images/Image_Processing_With_OpenCV_and_Pillow/numpy_1.png](Images/numpy_1.png)

### OpenCV Intro
OpenCV is a library used for computer vision. It has more functionality than the PIL library, but is more difficult to use.

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_1.png](Images/opencv_1.png)

We can plot the image using Imshow, but the colors appear off. This is because the order of each channel is different in OpenCV unlike PIL that is RGB. OpenCV is BGR. This is the main difference between the arrays and PIL versus OpenCV. We can change the color space with conversion code, this changes the color space. We use the function cvtColor, the input is the color image and the color code BGR to RGB or blue, green, red to red, green, blue. We can now plot the image. You can also convert the image to gray-scale using cvtColor. The input is the original image and the BGR to gray color code. We can plot the image. We can save the image using imright, the input is the path and the image array.


![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_2.png](Images/opencv_2.png)

We can change the color space with conversion code, this changes the color space. We use the function cvtColor, the input is the color image and the color code BGR to RGB or blue, green, red to red, green, blue. We can now plot the image. You can also convert the image to gray-scale using cvtColor. The input is the original image and the BGR to gray color code. We can plot the image.
![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_3.png](Images/opencv_3.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_4.png](Images/opencv_4.png)

## Image Processing With Pillow

### Image Files and Paths
```bash
# Setup Environment
cd ~/Desktop; rm -r temp; # To remove
cd ~/Desktop; mkdir temp; cd temp; pyenv activate venv3.10.4;

wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png -O lenna.png

wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png -O baboon.png

wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png -O barbara.png  
```
Image processing and computer vision tasks include displaying, cropping, flipping, rotating,  image segmentation, classification, image restoration,  image recognition, image generation.  Also, working with images via the cloud requires storing, transmitting, and gathering images through the internet. 

```python
import os
from PIL import Image
from PIL import ImageOps 
import matplotlib.pyplot as plt
import numpy as np
# First, let's define a helper function to concatenate two images side-by-side. You will not need to understand the code below at this moment, but this function will be used repeatedly in this tutorial to showcase the results.

def get_concat_h(im1, im2):
    #https://note.nkmk.me/en/python-pillow-concat-images/
    dst = Image.new('RGB', (im1.width + im2.width, im1.height))
    dst.paste(im1, (0, 0))
    dst.paste(im2, (im1.width, 0))
    return dst

my_image = "lenna.png"

```
File Types:
* Joint Photographic Expert Group image (or `.jpg` `.jpeg`)
* Portable Network Graphics (or `.png`)

```python
cwd = os.getcwd()
image_path = os.path.join(cwd, my_image)
image = Image.open(my_image)
type(image) # <class 'PIL.PngImagePlugin.PngImageFile'>
image.show()

# Matplotlib
plt.figure(figsize=(10,10))
plt.imshow(image)
plt.show()

# Load in with PIL.Image
image = Image.open(image_path)

# Size
print(image.size) # (512, 512)

# Mode
print(image.mode)

```

The `Image.open` method does not load image data into the computer memory. The `load` method of `PIL` object reads the file content, decodes it, and expands the image into memory.

```python
im = image.load() 

# We can then check the intensity of the image at the $x$-th column and $y$-th row:
x = 0
y = 1
im[y,x] # (226, 137, 125)

# Save
image.save("lenna.jpg")
```

### Greyscale

```python
image_gray = ImageOps.grayscale(image) 
image_gray.show()
image_gray.mode # Mode is L for greyscale.
```

#### Quantization
The Quantization of an image is the number of unique intensity values any given pixel of the image can take. For a grayscale image, this means the number of different shades of gray. Most images have 256 different levels. You can decrease the levels using the method `quantize`. Let's repeatably cut the number of levels in half and observe what happens:

```python
# Half the levels do not make a noticable difference.
image_gray.quantize(256 // 2)
image_gray.show()

# Let’s continue dividing the number of values by two and compare it to the original image.
for n in range(3,8):
    plt.figure(figsize=(10,10))
    plt.imshow(get_concat_h(image_gray,  image_gray.quantize(256//2**n))) 
    plt.title("256 Quantization Levels  left vs {}  Quantization Levels right".format(256//2**n))
    plt.show()
```

#### Color Channels

```python
baboon = Image.open('baboon.png')
# We can obtain the different RGB color channels and assign them to the variables red, green and blue
red, green, blue = baboon.split()

# color vs each channel
get_concat_h(baboon, red).show()
get_concat_h(baboon, green).show()
get_concat_h(baboon, blue).show()

array= np.asarray(image)
print(type(array))
```

`np.asarray` turns the original image into a numpy array. Often, we don't want to manipulate the image directly, but instead, create a copy of the image to manipulate. The `np.array` method creates a new copy of the image, such that the original one will remain unmodified.

```python
array = np.array(image)
# summarize shape
print(array.shape)
# The Intensity values are  8-bit unsigned datatype.
array[0, 0]
```

#### Indexing
You can plot the array as an image:
```python
# Review
plt.figure(figsize=(10,10))
plt.imshow(array)
plt.show()

# We can use numpy slicing, for example, we can return the first 256 rows corresponding to the top half of the image:
rows = 256
plt.figure(figsize=(10,10))
plt.imshow(array[0:rows,:,:])
plt.show()
# We can also return the first 256 columns corresponding to the first half of the image.
columns = 256
plt.figure(figsize=(10,10))
plt.imshow(array[:,0:columns,:])
plt.show()
# If you want to reassign an array to another variable, you should use the `copy` method (we will cover this in the next section).
A = array.copy()
plt.imshow(A)
plt.show()
# If we do not apply the method copy(), the variable will point to the same location in memory. Consider the array B. If we set all values of array A to zero, as B points to A, the values of B will be zero too:
B = A
A[:,:,:] = 0
plt.imshow(B)
plt.show()
# We can also work with the different color channels. Consider the baboon image: 
baboon_array = np.array(baboon)
plt.figure(figsize=(10,10))
plt.imshow(baboon_array)
plt.show()
# We can plot the red channel as intensity values of the red channel.
baboon_array = np.array(baboon)
plt.figure(figsize=(10,10))
plt.imshow(baboon_array[:,:,0], cmap='gray')
plt.show()
# Or we can create a new array and set all but the red color channels to zero. Therefore, when we display the image it appears red:
baboon_red=baboon_array.copy()
baboon_red[:,:,1] = 0
baboon_red[:,:,2] = 0
plt.figure(figsize=(10,10))
plt.imshow(baboon_red)
plt.show()
```