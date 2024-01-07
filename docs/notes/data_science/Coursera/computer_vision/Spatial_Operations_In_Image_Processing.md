# Spatial Operations In Image Processing

In this video we will discuss Spatial Operations in Image Processing. In this video we will review:

* Convolution sometimes called Linear Filtering
* Edge Detection
* Median Filters


In this video we will show the one channel representation, but unless specified we can apply these operations to each channel independently.

![Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_1.png)

## Convolution
Spatial Operations consists of a neighbourhood, in this example we take a neighbourhood of 2 by 2 pixels we apply a function that involves each pixel in the neighbourhood and output the result we then shift the neighbourhood and repeat the process for each pixel in the image.

![Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_2.png)

The result is a new image that has enhanced characteristics, these spatial operations take advantage is spatial patterns in the image.


Convolution or linear filtering is a standard way to Filter an image. The filter is called the kernel. The different kernels perform different tasks.

Convolution is analogous to this linear equation. The input is the image `X`. Output will be another image `Z`. We have the parameter `W` this is known as a kernel or filter. 


$z = wx$

$Z = W * Xx$

The star this operation is the convolution operation. There is actually an analytical expression for it, but it's a little confusing. So let's just go over an example to understand what's going on. 

Here's our image X, here's our kernel w, it’s just a smaller array.

![Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_3.png)

The array is made up of values that have been pre determined for some operation. Let's go over the operation of convolution. We start off in the top right corner of the image, and we'll overlay the kernel with that region of the image. We see the kernel values in red, we'll multiply every element of the image by the corresponding element of the kernel. For the first row we'll get the following operation, multiplying the intensity value and summing the results this process is repeated for the second row, multiplying the intensity value and summing the results. Finally for the final row we multiply the intensity values and sum the results. We sum those elements together. The result is the first element of the image z.

![Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_4.png](Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_4.png)

We shift the kernel to the left represented by the different colors in red. We multiply all the elements of the kernel with the image. This gives us the second element of the output image z. We'll shift the kernel one more column and perform the exact same operation, multiplying every element of the kernel by the corresponding intensity value of the image, adding them all together. This gives us the next value. We then shift the kernel down and repeat the process until we get to the end of the column. Repeating the process until we get a new image. One problem is the images are different sizes. We can fix this by changing the size of the image x. 
 
 We change the size of the image by padding. In zero padding we add two additional rows of zeros and two additional columns of zeros.
 
![Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_5.png](Images/Image_Processing_With_OpenCV_and_Pillow/spatial_operations_in_image_processing_5.png)
 
We can also replicate the values of the border.

## Low Pass Filters
Low Pass Filters are used to smooth the image getting rid of noise. Consider the following image of a square. 

![Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_1.png)

We have the following noise pixel. We can reduce this noise by applying a smoothing filter or kernel. Smoothing filters average out the pixels within a neighbourhood. They are sometimes called low pass filters.

For mean filtering the kernel simply averages out the kernels in a neighbourhood. 

Let's see what happens to the pixel intensities. To explore how the kernel effects different areas we can plot the output image and its relationship to specific regions of this input. When we apply the kernel to this area of the image, the intensity values remain unchanged around the edge of the box. 

![Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_2.png)

The values change as the values of 255 are averaged out with the zeros. 

![Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_3.png)

Finally, when we get to the region with the noise pixel, we see the noise value is smaller.

![Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_4.png](Images/Image_Processing_With_OpenCV_and_Pillow/mean_filtering_4.png)


Comparing the original image and the output image we see the intensity of the noise has reduced but the edges appear less sharp.



In general there is a trade off between sharpness and smoothness, in the lab we will review several other kernels that explore this trade off.

## Edge Detection
Edge Detection is an important first step in many computer vision algorithms. Edges in a digital image are where the image brightness changes sharply. Edge detection uses methods to approximate derivatives and gradients for identifying these areas.


Consider the following image with a vertical and horizontal edge.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_1.png)

Let's take a look at the intensity values. Let’s plot the first row of the image, the horizontal axis is the column index, and the vertical axis is the intensity value. If we move in the right direction from pixel 1 to pixel 2, the intensity value increases. In the image, we can represent this as a vector, in the same manner, when moving from pixel 2 to pixel 3, the intensity value decreases. We can represent this as a vector pointing in the opposite direction. The direction of the vector represents if the adjacent pixel is increasing.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_2.png)


We can represent this change by applying the following difference equation, and this works by subtracting the intensity value of the adjacent columns $j$ and $j+1$ in row $I$.


This computes an approximation of the gradient in the x-direction. In the following table, each row applies the equation to the intensity values for column 1 and 2.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_3.png)

The final column is the result of using the equation. We can overly the result over the image as vectors. We can apply the same logic to the next column, this time the values are negative.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_4.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_4.png)

It turns out we can perform similar Horizontal Derivative Approximations using convolution.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_5.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_5.png)

The Horizontal changes or gradient are computed by convolving the image with a kernel. These kernels are called `Sobel` operators. We can represent the output in the array of intensities $G_x$. All the values are zero except the elements corresponding to the horizontal edges. In this image the gray values have different ranges where black is negative, gray is zero, and white is positive. These represent vectors we discussed earlier.


![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_6.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_6.png)

We can use the same process to find vertical changes. This is computed by convolving the image with a kernel to get the image $G_y$. This time the vectors point in the vertical direction.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_7.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_7.png)

We can combine the output of each filter into a vector. If you are familiar with calculus this approximates the gradient. Taking the magnitude of the vectors we get the intensity values.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_8.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_8.png)

We can plot it as an image this represents the edges. We can also calculate the angle representing the direction.

![Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_9.png](Images/Image_Processing_With_OpenCV_and_Pillow/edge_detection_9.png)

## Mediam Filtering
Median Filters are another popular filter. They are better at removing some types of noise but may distort the image. The median filter outputs the median value of the neighbourhood. Consider the yellow pixel.

![Images/Image_Processing_With_OpenCV_and_Pillow/median_filters_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/median_filters_1.png)

Consider the region in this three by three neighbourhood. The resultant value of the output image is the median value of the 9 pixels. Depending on the padding we see the median is identical to the image in most regions. In the region with the noise sample we see the output is as follows.

![Images/Image_Processing_With_OpenCV_and_Pillow/median_filters_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/median_filters_2.png)


Overlaying the image values unlike the median filter, we see the noise is no longer there and the edges are straight, but the square is distorted as its missing a pixel.

![Images/Image_Processing_With_OpenCV_and_Pillow/median_filters_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/median_filters_3.png)

## OpenCV

Let’s apply some Spatial Operations in open cv. PIL is relatively simple, so check out labs for PIL examples. We will create an image “new image” that is a noisy version of the original first. 

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_filters_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_filters_1.png)

We create a kernel for mean filtering. The function filter2D performs 2D convolution between the image and the kernel on each color channel independently. The parameter values are in the lab. We see the new image has less noise, but the image is blurry.

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_filters_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_filters_2.png)

Image Sharpening involves smoothing the image and enhancing the edges. We can accomplish image sharpening by applying the following Kernel. 
We then apply fillters2D, comparing the image before and after we see it is much crisper.

We can perform edge detection consider the following image.

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_edge_detection_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_edge_detection_1.png)

We smooth the image using `GaussianBlur`, a low pass filter we will be discussing in the lab; this decreases changes that may be caused by noise that would affect the gradient.

We can approximate the derivative in the X or Y direction using the `Sobel` function .

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_edge_detection_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_edge_detection_2.png)

“ddepthis” is the output image depth. dx, dy is the order of the derivative in each direction; in this case it will be one. Finally, “k-size” size of the extended `Sobel` kernel; we can output the gradients as images.

![Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_edge_detection_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/opencv2_edge_detection_3.png)

We can approximate the magnitude of the gradient. We calculate absolute values, and convert the result to 8-bit using convertScaleAbs Then apply the function ”addWeighted” to calculate the sum of two arrays and assign it to grad we plot Grad as an image the areas with high intensity values represent edges.

## Code Lab
```bash
# Setup Environment
cd ~/Desktop; rm -r temp; # To remove
cd ~/Desktop; mkdir temp; cd temp; pyenv activate venv3.10.4;

wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/cameraman.jpeg
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png   
```

### PIL
```python
# Used to view the images
import matplotlib.pyplot as plt
# Used to load an image
from PIL import Image
# Used to create kernels for filtering
import numpy as np
# This function will plot two images side by side 
def plot_image(image_1, image_2,title_1="Orignal",title_2="New Image"):
    plt.figure(figsize=(10,10))
    plt.subplot(1, 2, 1)
    plt.imshow(image_1)
    plt.title(title_1)
    plt.subplot(1, 2, 2)
    plt.imshow(image_2)
    plt.title(title_2)
    plt.show()
# Spatial operations use the neighboring pixels to determine the present pixel value 
```

#### Linear Filtering
Filtering involves enhancing an image, for example, removing the noise from an image. Noise can be caused by a bad camera or bad image compression. The same factors that cause noise may lead to blurry images. We can apply filters to sharpen these images. Convolution is a standard way to filter an image. The filter is called the kernel and different kernels perform different tasks. In addition, Convolution is used for many of the most advanced artificial intelligence algorithms. We simply take the dot product of the kernel and an equally-sized portion of the image. We then shift the kernel and repeat.

```python
# Loads the image from the specified file
image = Image.open("lenna.png")
# Renders the image
plt.figure(figsize=(5,5))
plt.imshow(image)
plt.show()
# The images we are working with are comprised of RGB values, which are values from 0 to 255. Zero means white noise, this makes the image look grainy:
# Get the number of rows and columns in the image
rows, cols = image.size
# Creates values using a normal distribution with a mean of 0 (loc) and standard deviation (size) of 15, the values are converted to unit8 which means the values are between 0 and 255
noise = np.random.normal(loc=0,scale=15,size=(rows,cols,3)).astype(np.uint8)
# Add the noise to the image
noisy_image = image + noise
# Creates a PIL Image from an array
noisy_image = Image.fromarray(noisy_image)
# Plots the original image and the image with noise using the function defined at the top
plot_image(image, noisy_image, title_1="Orignal", title_2="Image Plus Noise")
```
When adding noise to an image sometimes the value might be greater than 255, in this case 256, is subtracted from the value to wrap the number around keeping it between 0 and 255. For example, consider an image with an RGB value of 137 and we add noise with an RGB value of 215 to get an RGB value of 352. We then subtract 256, the total number of possible values between 0 and 255, to get a number between 0 and 255.

#### Filtering Noise
To be able to create customer kernels and use predefined filters we must import the following library

```python
from PIL import ImageFilter
```
Smoothing filters average out the Pixels within a neighborhood, they are sometimes called low pass filters. For mean filtering, the kernel simply averages out the kernels in a neighborhood.
```python
# Create a kernel which is a 5 by 5 array where each value is 1/36
kernel = np.ones((5,5))/36
# Create a ImageFilter Kernel by providing the kernel size and a flattened kernel
kernel_filter = ImageFilter.Kernel((5,5), kernel.flatten())
# The function filter performs a convolution between the image and the kernel on each color channel independently.
# Filters the images using the kernel
image_filtered = noisy_image.filter(kernel_filter)
# We can plot the image before and after the filtering. We see the noise is reduced, but the image is blurry:
plot_image(image_filtered, noisy_image,title_1="Filtered image",title_2="Image Plus Noise")

# A smaller kernel keeps the image sharp, but filters less noise, here we try a 3x3 kernel. You can see her shoulders are sharper in this image but the green noise is brighter than the filtered image above.
# Create a kernel which is a 3 by 3 array where each value is 1/36
kernel = np.ones((3,3))/36
# Create a ImageFilter Kernel by providing the kernel size and a flattened kernel
kernel_filter = ImageFilter.Kernel((3,3), kernel.flatten())
# Filters the images using the kernel
image_filtered = noisy_image.filter(kernel_filter)
# Plots the Filtered and Image with Noise using the function defined at the top
plot_image(image_filtered, noisy_image,title_1="Filtered image",title_2="Image Plus Noise")
```

#### Gaussian Blur
To perform Gaussian Blur we use the `filter` function on an image using the predefined filter `ImageFilter.GaussianBlur`

Parameters:  
* radius: blur kernel radius, default 2

```python
# Filters the images using GaussianBlur
image_filtered = noisy_image.filter(ImageFilter.GaussianBlur)
# Plots the Filtered Image then the Unfiltered Image with Noise
plot_image(image_filtered , noisy_image,title_1="Filtered image",title_2="Image Plus Noise")
# Lets try using a 4 by 4 kernel
# Filters the images using GaussianBlur on the image with noise using a 4 by 4 kernel 
image_filtered = noisy_image.filter(ImageFilter.GaussianBlur(4))
# Plots the Filtered Image then the Unfiltered Image with Noise
plot_image(image_filtered , noisy_image,title_1="Filtered image",title_2="Image Plus Noise")

```

#### Image Sharpening
Image Sharpening involves smoothing the image and calculating the derivatives. We can accomplish image sharpening by applying the following Kernel.

```python
# Common Kernel for image sharpening
kernel = np.array([[-1,-1,-1], 
                   [-1, 9,-1],
                   [-1,-1,-1]])
kernel = ImageFilter.Kernel((3,3), kernel.flatten())
# Applys the sharpening filter using kernel on the original image without noise
sharpened = image.filter(kernel)
# Plots the sharpened image and the original image without noise
plot_image(sharpened , image, title_1="Sharpened image",title_2="Image")

# We can also sharpen using a predefined filter
# Sharpends image using predefined image filter from PIL
sharpened = image.filter(ImageFilter.SHARPEN)
# Plots the sharpened image and the original image without noise
plot_image(sharpened , image, title_1="Sharpened image",title_2="Image")
```

#### Edges
Edges are where pixel intensities change. The Gradient of a function outputs the rate of change; we can approximate the gradient of a grayscale image with convolution. Consider the following image:

```python
# Loads the image from the specified file
img_gray = Image.open('barbara.png')
# Renders the image from the array of data, notice how it is 2 diemensional instead of 3 diemensional because it has no color
plt.imshow(img_gray ,cmap='gray')
plt.show()
# Filters the images using FIND_EDGES filter
img_gray = img_gray.filter(ImageFilter.FIND_EDGES)
# Renders the filtered image
plt.figure(figsize=(10,10))
plt.imshow(img_gray ,cmap='gray')
plt.show()
```

##### Median
Median filters find the median of all the pixels under the kernel area and the central element is replaced with this median value. 

We can apply median filters to regular images but let’s see how we can use a median filter to improve segmentation. Consider the cameraman example: 

```python
# Load the camera man image
image = Image.open("cameraman.jpeg")
# Make the image larger when it renders
plt.figure(figsize=(10,10))
# Renders the image
plt.imshow(image,cmap="gray")
plt.show()
# Median filtering blurs the background, increasing the segmentation between the cameraman and the background
image = image.filter(ImageFilter.MedianFilter)
plt.figure(figsize=(10,10))
# Renders the image
plt.imshow(image,cmap="gray")
plt.show()
```

### OpenCV
```bash
# Setup Environment
cd ~/Desktop; rm -r temp; # To remove
cd ~/Desktop; mkdir temp; cd temp; pyenv activate venv3.10.4;

wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/cameraman.jpeg
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png   
```

```python
# Used to view the images
import matplotlib.pyplot as plt
# Used to perform filtering on an image
import cv2
# Used to create kernels for filtering
import numpy as np

def plot_image(image_1, image_2,title_1="Orignal",title_2="New Image"):
    plt.figure(figsize=(10,10))
    plt.subplot(1, 2, 1)
    plt.imshow(cv2.cvtColor(image_1, cv2.COLOR_BGR2RGB))
    plt.title(title_1)
    plt.subplot(1, 2, 2)
    plt.imshow(cv2.cvtColor(image_2, cv2.COLOR_BGR2RGB))
    plt.title(title_2)
    plt.show()

```

#### Linear Filtering
```python
# Loads the image from the specified file
image = cv2.imread("lenna.png")
print(image)
# Converts the order of the color from BGR (Blue Green Red) to RGB (Red Green Blue) then renders the image from the array of data
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
plt.show()
# The images we are working with are comprised of RGB values which are values from 0 to 255. Zero means white noise, this makes the image look grainy:
# Get the number of rows and columns in the image
rows, cols,_= image.shape
# Creates values using a normal distribution with a mean of 0 and standard deviation of 15, the values are converted to unit8 which means the values are between 0 and 255
noise = np.random.normal(0,15,(rows,cols,3)).astype(np.uint8)
# Add the noise to the image
noisy_image = image + noise
# Plots the original image and the image with noise using the function defined at the top
plot_image(image, noisy_image, title_1="Orignal",title_2="Image Plus Noise")
```

When adding noise to an image sometimes the value might be greater than 255, in this case, 256, is subtracted from the value to wrap the number around keeping it between 0 and 255. For example, consider an image with an RGB value of 137 and we add noise with an RGB value of 215 we get an RGB value of 352. We then subtract 256, the total number of possible values between 0 and 255, to get a number between 0 and 255.


#### Filtering Noise
Smoothing filters average out the Pixels within a neighborhood, they are sometimes called low pass filters. For mean filtering, the  kernel simply averages out the kernels in a neighborhood.


```python
# Create a kernel which is a 6 by 6 array where each value is 1/36
kernel = np.ones((6,6))/36

```

The function `filter2D` performs 2D convolution between the image `src` and the  `kernel` on each color channel independently. The parameter <a href="https://docs.opencv.org/master/d4/d86/group__imgproc__filter.html?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCV0101ENCoursera872-2022-01-01#filter_depths">ddepth</a> has to do with the size of the output image, we will set it to -1 so the input and output are the same size.

```python
# Filters the images using the kernel
image_filtered = cv2.filter2D(src=noisy_image, ddepth=-1, kernel=kernel)
# We can plot the image before and after the filtering; we see the noise is reduced, but the image is blurry:
# Plots the Filtered and Image with Noise using the function defined at the top
plot_image(image_filtered, noisy_image,title_1="Filtered image",title_2="Image Plus Noise")
# A smaller kernel keeps the image sharp, but filters less noise, here we try a 4x4 kernel
# Creates a kernel which is a 4 by 4 array where each value is 1/16
kernel = np.ones((4,4))/16
# Filters the images using the kernel
image_filtered=cv2.filter2D(src=noisy_image,ddepth=-1,kernel=kernel)
# Plots the Filtered and Image with Noise using the function defined at the top
plot_image(image_filtered , noisy_image,title_1="filtered image",title_2="Image Plus Noise")

```

#### Gaussian Blur
The function  `GaussianBlur` convolves the source image with the specified Gaussian kernel It filters noise but does a better job of preserving the edges. It has the following parameters:

Parameters:

* `src`: input image; the image can have any number of channels, which are processed independently.
* `ksize`: Gaussian kernel size.
* `sigmaX`: Gaussian kernel standard deviation in the X direction.
* `sigmaY`: Gaussian kernel standard deviation in the Y direction; if sigmaY is zero, it is set to be equal to sigmaX .

```python
# Filters the images using GaussianBlur on the image with noise using a 4 by 4 kernel 
image_filtered = cv2.GaussianBlur(noisy_image,(5,5),sigmaX=4,sigmaY=4)
# Plots the Filtered Image then the Unfiltered Image with Noise
plot_image(image_filtered , noisy_image,title_1="Filtered image",title_2="Image Plus Noise")
# Sigma behaves like the size of the mean filter, a larger value of sigma will make the image blurry, but you are still constrained by the size of the filter, there we set sigma to 10
# Filters the images using GaussianBlur on the image with noise using a 11 by 11 kernel 
image_filtered = cv2.GaussianBlur(noisy_image,(11,11),sigmaX=10,sigmaY=10)
# Plots the Filtered Image then the Unfiltered Image with Noise
plot_image(image_filtered , noisy_image,title_1="filtered image",title_2="Image Plus Noise")
```

#### Image Sharpening
Image Sharpening  involves smoothing the image and calculating the derivatives.  We can accomplish image sharpening by applying the following Kernel.

```python
# Common Kernel for image sharpening
kernel = np.array([[-1,-1,-1], 
                   [-1, 9,-1],
                   [-1,-1,-1]])
# Applys the sharpening filter using kernel on the original image without noise
sharpened = cv2.filter2D(image, -1, kernel)
# Plots the sharpened image and the original image without noise
plot_image(sharpened , image, title_1="Sharpened image",title_2="Image")
```

#### Edges
Edges are where pixel intensities change. The Gradient of a function outputs the rate of change; we can approximate the gradient of a grayscale image with convolution. There are several methods to approximate the gradient, let’s use the Sobel edge detector. This combines several convolutions and finding the magnitude of the result. Consider the following image:

```python
# Loads the image from the specified file
img_gray = cv2.imread('barbara.png', cv2.IMREAD_GRAYSCALE)
print(img_gray)
# Renders the image from the array of data, notice how it is 2 diemensional instead of 3 diemensional because it has no color
plt.imshow(img_gray ,cmap='gray')
plt.show()
# We smooth the image, this decreases changes that may be caused by noise that would  affect the gradient.
# Filters the images using GaussianBlur on the image with noise using a 3 by 3 kernel 
img_gray = cv2.GaussianBlur(img_gray,(3,3),sigmaX=0.1,sigmaY=0.1)
# Renders the filtered image
plt.imshow(img_gray ,cmap='gray')
plt.show()
```

We can approximate the derivative in the X or Y direction  using the `Sobel` function, here are the parameters:

* `src`: input image
* `ddepth`: output image depth, see combinations; in the case of 8-bit input images it will result in truncated derivatives
* `dx`: order of the derivative x
* `dx`: order of the derivative y
* `ksize` size of the extended Sobel kernel; it must be 1, 3, 5, or 7

dx = 1 represents the derivative in the x-direction.  The function approximates  the derivative by  convolving   the image with the following kernel  

$$
\begin{bmatrix} 
1 & 0 & -1 \\\\
2 & 0 & -2 \\\\
1 & 0 & -1 
\end{bmatrix}
$$

```python
# We can apply the function:
ddepth = cv2.CV_16S
# Applys the filter on the image in the X direction
grad_x = cv2.Sobel(src=img_gray, ddepth=ddepth, dx=1, dy=0, ksize=3)

# We can plot the result 
plt.imshow(grad_x,cmap='gray')
plt.show()

```

dy=1 represents the derivative in the y-direction.  The function approximates  the derivative by  convolving   the image with the following kernel 

$$
\begin{bmatrix} 
\ \ 1 & \ \ 2 & \ \ 1 \\\\
\ \ 0 & \ \ 0 & \ \ 0 \\\\
-1 & -2 & -1 
\end{bmatrix} 

$$

```python
# We can apply the function and plot the result
# Applys the filter on the image in the X direction
grad_y = cv2.Sobel(src=img_gray, ddepth=ddepth, dx=0, dy=1, ksize=3)
plt.imshow(grad_y,cmap='gray')
plt.show()
#  We can approximate the  gradient by  calculating absolute values, and converts the result to 8-bit:
# Converts the values back to a number between 0 and 255
abs_grad_x = cv2.convertScaleAbs(grad_x)
abs_grad_y = cv2.convertScaleAbs(grad_y)
```
Then apply the  function `addWeighted` to calculates the sum of two arrays as follows:

```python
# Adds the derivative in the X and Y direction
grad = cv2.addWeighted(abs_grad_x, 0.5, abs_grad_y, 0.5, 0)
# We then plot the results, we see the image with lines have high-intensity values representing a large  gradient   
# Make the figure bigger and renders the image
plt.figure(figsize=(10,10))
plt.imshow(grad,cmap='gray')
plt.show()
```

#### Median

Median filters find the median of all the pixels under the kernel area and the central element is replaced with this median value. 

We can apply median filters to regular  images but let’s see how we can use  a median filter to improve segmentation.   Consider the cameraman example 

```python
# Load the camera man image
image = cv2.imread("cameraman.jpeg",cv2.IMREAD_GRAYSCALE)
# Make the image larger when it renders
plt.figure(figsize=(10,10))
# Renders the image
plt.imshow(image,cmap="gray")
plt.show()
```

Now let's apply a Median Filter by using the `medianBlur` function. The parameters for this function are `src`: The image and `ksize`: Kernel size

```python
# Filter the image using Median Blur with a kernel of size 5
filtered_image = cv2.medianBlur(image, 5)
# Make the image larger when it renders
plt.figure(figsize=(10,10))
# Renders the image
plt.imshow(filtered_image,cmap="gray")
plt.show()
```

We would like to find the cameraman, but median filtering captures some of the background.

#### Threshold Function Parameters

`src`: The image to use
`thresh`: The threshold
`maxval`: The maxval to use
`type`: Type of filtering

The threshold function works by looking at each pixel's grayscale value and assigning a value if it is below the threshold and another value if it is above the threshold. In our example the threshold is 0 (black) and the type is binary inverse so if a value is above the threshold the assigned value is 0 (black) and if it is below or equals the threshold the maxval 255 (white) is used. So if the pixel is 0 black it is assigned 255 (white) and if the pixel is not black then it is assigned black which is what THRESH_BINARY_INV tells OpenCV to do. This is how it would work without THRESH_OTSU.

Since we are using THRESH_OTSU it means that OpenCV will decide an optimal threshold. In our example below the threshold, we provide does not get used in the filter OpenCV will use an optimal one.

```python
# Returns ret which is the threshold used and outs which is the image
ret, outs = cv2.threshold(src = image, thresh = 0, maxval = 255, type = cv2.THRESH_OTSU+cv2.THRESH_BINARY_INV)

# Make the image larger when it renders
plt.figure(figsize=(10,10))

# Render the image
plt.imshow(outs, cmap='gray')
plt.show()
```