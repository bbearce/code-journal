# DICOM Conversion to Viewable Image

> Much simpler than previous example and rescales appropriately.

```python
# Code to save 2D grayscale image as bmp
import os
import numpy as np
import torch
import pydicom
from PIL import Image
import pdb
#...
path_to_dicoms = './kmotion_data/0343922_E18868209/dicom_sorted/Series1'
path_to_pngs = './JPG_test/0343922_E18868209/dicom_sorted/Series1'
dir_list = os.listdir(path_to_dicoms)

def saveimage():
    for n, image_file_dicom in enumerate(dir_list):
        # pdb.set_trace()
        image = os.path.join(path_to_dicoms,image_file_dicom)
        ds = pydicom.dcmread(image)
        visual = ds.pixel_array
        visual = (visual - visual.min()) / (visual.max() - visual.min())
        filename_new = image[image.find("MR"):image.find(".dcm")] + ".png" 
        im = Image.fromarray((visual * 255).astype(np.uint8))
        im.save(os.path.join(path_to_pngs, filename_new))
#...

saveimage()
```

Source: [medium](https://medium.com/@vivek8981/dicom-to-jpg-and-extract-all-patients-information-using-python-5e6dd1f1a07d)

> This example doesn't scale the DICOM pixel array between values of 0-255 and therefore clips all data between 255 and whatever max value the pixel array has. Likewise for negative numbers, which are clipped by 0.

```python
import os
import pydicom
import cv2

# make it True if you want in PNG format
PNG = False 
# Specify the .dcm folder path
folder_path = "./"
# Specify the output jpg/png folder path
jpg_folder_path = "JPG_of_PNG_folder"

if __name__ == "__main__":
    images_path = os.listdir(folder_path)
    for n, image in enumerate(images_path):
        ds = pydicom.dcmread(os.path.join(folder_path, image))
        pixel_array_numpy = ds.pixel_array
        if PNG == False:
            image = image.replace('.dcm', '.jpg')
        else:
            image = image.replace('.dcm', '.png')
        cv2.imwrite(os.path.join(jpg_folder_path, image), pixel_array_numpy)
        if n % 50 == 0:
            print('{} image converted'.format(n))

```