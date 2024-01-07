# What is a Digital Image

A rectangular array of numbers.

![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/pixels_1.png)

In the real world, an image can take on an almost unlimited number of values, but digital images have intensity values between zero (black) and 255 (white). It turns out that's all we need, 256 different intensity values to represent an image.

If we use less than 256 values, things will look cartoonish.


![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/pixels_2.png)

RGB:

![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/pixels_3.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_4.png](Images/Image_Processing_With_OpenCV_and_Pillow/pixels_4.png)

Image Masks:
![Images/Image_Processing_With_OpenCV_and_Pillow/pixels_5.png](Images/Image_Processing_With_OpenCV_and_Pillow/pixels_5.png)

Basic Image Types:

An image is a file on your computer. Two popular image formats, Joint Photographic Expert Group image or JPEG, and Portable Network Graphics or PNG, these formats reduce file size and have other features. No matter what Python library you use, you're going to have to load the image

# PIL Intro

![Images/Image_Processing_With_OpenCV_and_Pillow/PIL_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/PIL_1.png)


ImageOps:
![Images/Image_Processing_With_OpenCV_and_Pillow/PIL_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/PIL_2.png)

Quantizing:
![Images/Image_Processing_With_OpenCV_and_Pillow/PIL_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/PIL_3.png)


# Numpy Intro
![Images/Image_Processing_With_OpenCV_and_Pillow/numpy_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/numpy_1.png)

# OpenCV Intro
OpenCV is a library used for computer vision. It has more functionality than the PIL library, but is more difficult to use.

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv_1.png)

We can plot the image using Imshow, but the colors appear off. This is because the order of each channel is different in OpenCV unlike PIL that is RGB. OpenCV is BGR. This is the main difference between the arrays and PIL versus OpenCV. We can change the color space with conversion code, this changes the color space. We use the function cvtColor, the input is the color image and the color code BGR to RGB or blue, green, red to red, green, blue. We can now plot the image. You can also convert the image to gray-scale using cvtColor. The input is the original image and the BGR to gray color code. We can plot the image. We can save the image using imright, the input is the path and the image array.


![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv_2.png)

We can change the color space with conversion code, this changes the color space. We use the function cvtColor, the input is the color image and the color code BGR to RGB or blue, green, red to red, green, blue. We can now plot the image. You can also convert the image to gray-scale using cvtColor. The input is the original image and the BGR to gray color code. We can plot the image.
![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv_3.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv_4.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv_4.png)