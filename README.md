# Image Transformation Project

## Overview

* **Q10:** Image Transformation Using Matrices
* **Q11:** Interactive Image Transformation Toolbox

# Q10 – Image Transformation Using Matrices

The transformations used are:

1. Scaling
2. Rotation
3. Horizontal Shearing
4. Reflection in Y-axis
5. Projection onto X-axis

## Libraries Used
```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
from google.colab import files
```
numpy and matplotlib are self explanatory 

### PIL

PIL (Python Imaging Library) is used to open and process the uploaded image.

### Google Colab Files

This is used to upload an image directly from the computer.

# Image Upload

The program first asks the user to upload an image.

```python
uploaded = files.upload()
filename = list(uploaded.keys())[0]
```

The uploaded file name is obtained and then the image is opened.

```python
img = np.array(Image.open(filename).convert("RGB"))
```

The image is converted into RGB format and then converted into a NumPy array.

The height and width of the image are also found.

---

# Transformation Matrices

## A1 – Scaling

```text
[ 2    0 ]
[ 0   0.5 ]
```

This matrix changes the size of the image.

* X-direction is multiplied by 2.
* Y-direction is multiplied by 0.5.

Therefore, the image becomes wider and shorter.

---

## A2 – Rotation

```text
[ 0  -1 ]
[ 1   0 ]
```

This matrix rotates the image by **90 degrees**.

The information in the image is preserved.

---

## A3 – Horizontal Shear

```text
[ 1   1 ]
[ 0   1 ]
```

This matrix performs a horizontal shear.

The x-coordinate changes according to the y-coordinate, making the image look slanted.

---

## A4 – Reflection in Y-axis

```text
[ -1   0 ]
[  0   1 ]
```

This matrix reflects the image about the Y-axis.

The x-coordinate changes sign while the y-coordinate remains the same.

---

## A5 – Projection onto X-axis

```text
[ 1   0 ]
[ 0   0 ]
```

This matrix projects the image onto the X-axis.

The y-direction is removed, so information in the vertical direction is lost.

The rank of this matrix is 1.

---

# Transformation Process

The program first finds the centre of the image.

```python
cx = (width - 1) / 2
cy = (height - 1) / 2
```

The pixel coordinates are then created using:

```python
X, Y = np.meshgrid(np.arange(width), np.arange(height))
```

The coordinates are shifted so that the centre of the image becomes the origin.

```python
Xc = X - cx
Yc = Y - cy
```

The transformation matrix is then applied to the coordinates.

```python
new_X = A[0, 0] * Xc + A[0, 1] * Yc
new_Y = A[1, 0] * Xc + A[1, 1] * Yc
```

This gives the new position of every pixel.

The transformed pixels are then placed into a new image.

---

# Matrix Rank

The program also calculates the rank of every matrix.

```python
np.linalg.matrix_rank(A)
```

Rank tells us how many independent directions remain after the transformation.

* Rank 2 → Both dimensions are preserved.
* Rank 1 → One dimension is lost.

Therefore:

```text
A1 → Rank 2
A2 → Rank 2
A3 → Rank 2
A4 → Rank 2
A5 → Rank 1
```

---

# Q10 Summary

| Matrix | Transformation         | Information Lost |
| ------ | ---------------------- | ---------------- |
| A1     | Scaling                | No               |
| A2     | 90° Rotation           | No               |
| A3     | Horizontal Shear       | No               |
| A4     | Reflection in Y-axis   | No               |
| A5     | Projection onto X-axis | Yes              |

---

# Q11 – Interactive Image Transformation Toolbox

## Aim

To create an interactive image transformation toolbox where the user can upload an image and choose different transformations from a menu.

The toolbox provides:

```text
1. Rotate
2. Resize
3. Flip
4. Shear
5. Custom Matrix
6. Reset
7. Exit
```

---

# Uploading the Image

The user first uploads a JPG or PNG image.

```python
uploaded = files.upload()
```

The image is then opened using PIL.

```python
original = Image.open(filename).convert("RGB")
```

A copy of the original image is stored.

```python
image = original.copy()
```

This is useful because the image can be restored using the Reset option.

---

# Displaying the Image

A function called `show_image()` is used to display the image.

```python
def show_image(img, title):
```

The function:

* Creates a figure
* Displays the image
* Adds a title
* Removes the axes
* Shows the result

---

# Interactive Menu

The toolbox uses:

```python
while True:
```

This keeps the menu running until the user selects Exit.

The user enters a number:

```python
choice = input("Enter your choice: ")
```

The program then checks the selected option using `if` and `elif`.

---

# 1. Rotate

The user enters an angle.

```python
angle = float(input("Enter rotation angle: "))
```

The image is then rotated.

```python
image = image.rotate(angle, expand=True)
```

For example:

```text
45 → rotates the image by 45°
90 → rotates the image by 90°
```

`expand=True` allows the output image to increase in size when required.

---

# 2. Resize

The user enters a resize factor.

```python
factor = float(input("Enter resize factor: "))
```

For example:

```text
2   → twice the original size
0.5 → half the original size
```

The new width and height are calculated and the image is resized.

---

# 3. Flip

The user can choose:

```text
1. Horizontal
2. Vertical
```

For a horizontal flip:

```python
image.transpose(Image.Transpose.FLIP_LEFT_RIGHT)
```

For a vertical flip:

```python
image.transpose(Image.Transpose.FLIP_TOP_BOTTOM)
```

---

# 4. Shear

The user can choose:

```text
1. Horizontal shear
2. Vertical shear
```

The user also enters a shear value.

For horizontal shear, the program uses:

```text
[ 1   shear ]
[ 0     1   ]
```

For vertical shear:

```text
[ 1     0   ]
[ shear  1  ]
```

The transformation is applied using PIL's affine transformation.

---

# 5. Custom Matrix

This option allows the user to enter their own 2×2 matrix.

The program asks for:

```text
a
b
c
d
```

These values form:

```text
[ a  b ]
[ c  d ]
```

For example:

```text
a = 1
b = 1
c = 0
d = 1
```

gives:

```text
[ 1  1 ]
[ 0  1 ]
```

which produces a horizontal shear.

The same pixel-coordinate method used in Q10 is then used to apply the custom matrix.

---

# 6. Reset

The Reset option returns the image to its original state.

```python
image = original.copy()
```

This is possible because the original image was saved separately when the program started.

---

# 7. Exit

When the user selects Exit:

```python
break
```

is used to stop the `while` loop and close the toolbox.

---

# Technologies Used

* **Python**
* **NumPy**
* **Matplotlib**
* **PIL / Pillow**
* **Google Colab**

---

# How to Run

## Q10

1. Open the Q10 Python file in Google Colab.
2. Run all the cells.
3. Upload a JPG or PNG image.
4. The program displays the original image.
5. The five transformations are applied one by one.
6. The transformed images are displayed.

## Q11

1. Open the Q11 Python file in Google Colab.
2. Run the program.
3. Upload a JPG or PNG image.
4. Select an operation from the menu.
5. Enter the required value.
6. The transformed image is displayed.
7. Continue using the menu or select Reset/Exit.

---

# Difference Between Q10 and Q11

| Q10                                | Q11                              |
| ---------------------------------- | -------------------------------- |
| Uses predefined matrices           | Uses an interactive menu         |
| Focuses on matrix transformations  | Focuses on user interaction      |
| Transformations are fixed          | User enters parameters           |
| Includes projection                | Includes resize and flip         |
| Shows matrix rank                  | Allows custom matrices           |
| Demonstrates mathematical concepts | Works like a small image toolbox |

---

# Conclusion

In **Q10**, different transformation matrices were used to understand how matrices affect an image. The program also demonstrates the concept of matrix rank and information loss.

In **Q11**, these ideas were extended into an interactive toolbox. The user can upload an image and choose different operations such as rotation, resizing, flipping, shearing and custom matrix transformation.

Both programs demonstrate how **linear algebra and image processing can be combined using Python**.

