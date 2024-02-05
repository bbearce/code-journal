# Softmax and Multi-Class Classification

## Argmax review

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_1.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_1.png)

The argmax function returns the index corresponding to the largest value in a sequence of numbers. Here the largest value in Z is 100 and the corresponding index is zero, thus the argmax function will return zero. 

## Multiclass

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_2.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_2.png)

Instead of using one plane to classify the data, we will use one plane for each class. In this case, we have three equations representing three classes, but we can generalize to any number of classes.

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_3.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_3.png)

We can also use the graph to represent equations. In this case, nodes are representing the different components of X. We add nodes for each output Z. The edges represent the different learnable parameters with subscripts indicating the dimension. 

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_4.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_4.png)

This is the plane where Z equals zero, the line is where the decision plane intersects with the plane Z equals zero. We can overlay our sample images, we see the lines split the classes. 

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_5.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_5.png)

If the input is in the blue region, the value of z0 corresponding to the equation zero is the largest. This is where the blue plane has a higher value than the other regions. Therefore, anything in this region will be in class zero. 

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_6.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_6.png)

If the input is in the red region, the value of z1 corresponding to equation one is the largest. Therefore, anything in this region will be in class one. 

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_7.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_7.png)

If the input is in the yellow region, the value of z2 corresponding to equation Z is the largest. Therefore, anything in this region will be in class two. Just to note, the yellow line we see is where the plane is greater than zero. The yellow region is where the yellow plane is larger than the blue and red region. We can now use the planes to classify this unknown point.

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_8.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_8.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_9.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_9.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_10.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_10.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_11.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_11.png)

![Images/Image_Processing_With_OpenCV_and_Pillow/softmax_12.png](Images/Image_Processing_With_OpenCV_and_Pillow/softmax_12.png)



