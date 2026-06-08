# Memory-Based versus Generator-Based Data Loading.md

## Assignment Overview: Compare Memory-Based versus Generator-Based Data Loading

Welcome to the hands-on lab for working with satellite imagery and comparing memory-based versus generator-based data loading.

This set of instructions will guide you through all of the targeted tasks in the provided notebook. By following these steps, you'll gain crucial experience manipulating images in Python, understanding directory handling, and visualizing remote sensing data for deep learning projects.

You'll start by downloading the dataset and exploring the two main folders contained within. First folder, class_0_non_agri, contains images of non-agricultural land, and the second folder, class_1_agri, contains images representing agricultural land. Each class folder includes numerous images.

Your first step is to use Python's standard library (os) to list, sort, and handle file paths for these categories. You will use os.listdir() to obtain file names within the class_0_non_agri directory. Then, you will open and display the first image in non_agri_images. Your first exercise would be to look at the image dimensions of a single image in non_agri_images. Then, using memory-based loading, you will read all images in a list.

Next, to compare with lazy loading, you will display the first four non-agricultural images. Similarly, you will create the list of all agricultural images and calculate their number in your next exercise. Finally, you will end the lab by displaying the first four images of the agricultural land. If you get errors opening images, re-check your directory paths and ensure you're using the correct full absolute or relative paths.

By following these steps, you'll gain confidence in file handling, image loading, and visualization with common Python tools for deep learning. Complete all the code and questions to finish the lab successfully. You will need to download and save the finished lab on your computer for final evaluation at the end of this course. Good luck!


