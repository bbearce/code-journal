# PIL (Image mManipulation)

## Resize Image
```python
from PIL import Image

dest_path = "."
new_image_name = "new_image.jpg"

def resize(path):
    i = Image.open(path)
    size = i.size
    new_size = int(floor(size[0]*.75)), int(floor(size[1]*.75))
    i = i.resize(new_size, Image.ANTIALIAS)
    return i

new_image = resize(os.path.join(dest_path, "image.jpg"))
new_image.save(os.path.join(dest_path, new_image_name), optimize=True, quality=50)
```