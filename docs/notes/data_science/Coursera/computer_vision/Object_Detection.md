# Object Detection

We will learn about:  
* Sliding Windows
* Bounding Box
* Bounding Box Pipeline
* Score

## Sliding Windows

If we want to detect a dog, we consider a fixed window size. If chosen properly, the dog will occupy most of the window. This is essentially a sub-image that we would like to classify as the dog. The other sub-images would be classified as background. 

![sliding_windows_1.png](Images/Object_Detection/sliding_windows_1.png)

We then shift the window, and classify the next sub-image.
![sliding_windows_2.png](Images/Object_Detection/sliding_windows_2.png)

We repeat the process. When we get to the horizontal border, we move a few pixels down in the vertical direction and repeat the process.
![sliding_windows_3.png](Images/Object_Detection/sliding_windows_3.png)


When the object occupies most of the window, it will be classified as a dog. 
![sliding_windows_4.png](Images/Object_Detection/sliding_windows_4.png)

Problems include many overlapping detections.
![sliding_windows_5.png](Images/Object_Detection/sliding_windows_5.png)

Object Sizes too are an issue. One way to solve this is to resize the image.
![sliding_windows_6.png](Images/Object_Detection/sliding_windows_6.png)

The same object can have different shapes.
![sliding_windows_7.png](Images/Object_Detection/sliding_windows_7.png)

There is also the problem of overlapping objects.
![sliding_windows_8.png](Images/Object_Detection/sliding_windows_8.png)

## Bounding Boxes

Bounding boxes is another method for object detection. It can be used independently, with sliding windows or with other more advanced methods.
![bounding_box_1.png](Images/Object_Detection/bounding_box_1.png)

The bounding box is a rectangular box that can be determined. With the lower right corner of the rectangle with coordinates y0 and x0, and the width and height. The y and x are not the same as the classification labels y and the image x, so we will color them blue.
![bounding_box_2.png](Images/Object_Detection/bounding_box_2.png)


It can also be determined by the coordinates in the upper left corner, ymin and xmin, and the lower right corner, the xmax and ymax. Remember, these are not the labels and the image, they are just to illustrate the coordinates of the bounding box we will call box.

The goal of object detection is to predict these points, so we add a hat to indicate its prediction. 
![bounding_box_3.png](Images/Object_Detection/bounding_box_3.png)

## Bounding Box Pipeline

Like classification, we have the class y and x, we also have the bounding box. Just like classification, we have a data set of classes and their bounding boxes. Similar to classification, we use the data set to train the model. 
![bounding_box_4.png](Images/Object_Detection/bounding_box_4.png)


Similar to classification, we use the data set to train the model. We include the box coordinates. The result is an object detector with updated learning parameters.

We input the image with the objects we would like to detect. We have the predicted class and the box coordinates, in this case a dog. We also have the predicted class cat and the box coordinates, the predicted class bird and box coordinates, and another class predicted as bird and the box coordinates. 
![bounding_box_5.png](Images/Object_Detection/bounding_box_5.png)


## Score
Many object detection algorithms provide a score, letting you know how confident the model prediction is. 

![bounding_box_6.png](Images/Object_Detection/bounding_box_6.png)


For each detection, a score is provided. We can adjust so we only accept detections above a specific score. Here we have detected one dog and two cats. It looks like we detected both a cat and a dog in the dog's location. Examining the score, we see that one of the cat predictions has a low score of 0.5. If we only accept scores above 0.9, we correctly detect the cat and dog. 
![bounding_box_7.png](Images/Object_Detection/bounding_box_7.png)
