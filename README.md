# Image Transformation Project

This project is made in **Python using Google Colab**.

It has two questions:

* **Q10:** Apply different image transformations using matrices.
* **Q11:** Make a simple Image Transformation Toolbox where the user can choose different transformations from a menu.

I used Python libraries like NumPy, Matplotlib and PIL for this project.

---

# Q10 - Image Transformations Using Matrices

## What is the aim?

The aim of this question is to take an image and apply different transformations to it using **2 × 2 matrices**.

The transformations used are:

1. Scaling
2. Rotation
3. Shearing
4. Reflection
5. Projection

The program also checks the **rank of each matrix** to see if any information is lost.

---

## Libraries Used

```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
from google.colab import files
```

### NumPy

I used NumPy for:

* Creating matrices
* Matrix multiplication
* Finding matrix rank

### Matplotlib

I used Matplotlib to display the images.

### PIL

PIL is used to open and work with the uploaded image.

### Google Colab files

This is used so that I can upload an image from my computer.

---

# Uploading the Image

The program first asks the user to upload an image.

```python
uploaded = files.upload()
filename = list(uploaded.keys())[0]
```

The uploaded image is then opened using PIL.

```python
img = np.array(Image.open(filename).convert("RGB"))
```

The image is converted into a NumPy array because it makes it easier to work with the pixels.

---

# Transformation Matrices

I used the following matrices.

## A1 - Scaling

```text
[ 2    0 ]
[ 0   0.5]
```

This makes the image:

* Wider in the x direction
* Smaller in the y direction

The rank is 2, so no information is lost.

---

## A2 - Rotation

```text
[ 0  -1 ]
[ 1   0 ]
```

This rotates the image by **90 degrees**.

The rank is 2, so the transformation does not remove a dimension.

---

## A3 - Horizontal Shear

```text
[ 1  1 ]
[ 0  1 ]
```

This moves the pixels horizontally depending on their y position.

It makes the image look slanted.

The rank is 2.

---

## A4 - Reflection

```text
[ -1   0 ]
[  0   1 ]
```

This reflects the image in the **Y-axis**.

 the image gets flipped horizontally.
 
The rank is 2.

---

## A5 - Projection

```text
[ 1  0 ]
[ 0  0 ]
```

This keeps the x direction but removes the y direction.

Because one direction is removed, some information from the image is lost.

The rank is 1.

---

# How the Transformation Works

First, the center of the image is found.

```python
cx = (width - 1) / 2
cy = (height - 1) / 2
```

This is done so that the transformation happens around the center of the image.

Then a grid of x and y coordinates is created.

```python
X, Y = np.meshgrid(np.arange(width), np.arange(height))
```

The coordinates are moved so that the center becomes the origin.

```python
Xc = X - cx
Yc = Y - cy
```

Then the transformation matrix is applied.

For example:

```python
new_X = A[0,0] * Xc + A[0,1] * Yc
new_Y = A[1,0] * Xc + A[1,1] * Yc
```

This gives the new position of each pixel.

Finally, the pixels are put into their new positions and the transformed image is displayed.

---

# Matrix Rank

The program uses:

```python
np.linalg.matrix_rank(A)
```

to find the rank of the matrix.

In simple words:

* **Rank 2:** Both directions are still present.
* **Rank 1:** One direction has been lost.
* **Rank 0:** Everything is mapped to zero.

For this question:

| Matrix | Transformation | Rank | Information Lost |
| ------ | -------------- | ---: | ---------------- |
| A1     | Scaling        |    2 | No               |
| A2     | Rotation       |    2 | No               |
| A3     | Shear          |    2 | No               |
| A4     | Reflection     |    2 | No               |
| A5     | Projection     |    1 | Yes              |

---

# Q11 - Image Transformation Toolbox

##  aim

The aim of Q11 is to make a small interactive program where the user can upload an image and select what transformation they want to apply.

The menu has these options:

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

Just like Q10, the user first uploads an image.

The image is stored as the original image so that it can be restored later.

```python
original = Image.open(filename).convert("RGB")
image = original.copy()
```

`original` keeps the first uploaded image.

`image` is the image that we keep changing.

---

# Displaying the Image

I made a small function called `show_image()`.

```python
def show_image(img, title):
    plt.figure(figsize=(7, 5))
    plt.imshow(img)
    plt.title(title)
    plt.axis("off")
    plt.show()
```

This function is just used to display the image.

Instead of writing the same display code again and again, I can just call:

```python
show_image(image, "Rotated Image")
```

---

# Menu

The program uses:

```python
while True:
```

This keeps the menu running until the user selects Exit.

The user enters a number and the program performs the selected operation.

---

# 1. Rotate

The user enters an angle.

Example:

```text
Enter rotation angle: 45
```

The image is rotated using:

```python
image.rotate(angle, expand=True)
```

`expand=True` makes sure the rotated image is not unnecessarily cut off.

---

# 2. Resize

The user enters a resize factor.

For example:

```text
Enter resize factor: 2
```

This makes the image approximately twice as large.

If the factor is:

```text
0.5
```

the image becomes half its original size.

---

# 3. Flip

The program gives two choices:

```text
1. Horizontal
2. Vertical
```

Horizontal flip means the image is flipped left to right.

Vertical flip means the image is flipped upside down.

---

# 4. Shear

The program asks whether the user wants:

```text
1. Horizontal shear
2. Vertical shear
```

Then it asks for a shear value.

Shearing makes the image look slanted.

---

# 5. Custom Matrix

This is probably the most important part related to Q10.

The user can enter their own 2 × 2 matrix.

For example:

```text
a = 1
b = 0
c = 0
d = 1
```

This gives:

```text
[ 1  0 ]
[ 0  1 ]
```

which is the identity matrix, so the image basically stays the same.

Another example is:

```text
[ -1   0 ]
[  0   1 ]
```

which can be used for reflection.

The program applies the matrix to the image coordinates and creates a new image.

---

# 6. Reset

If I have applied many transformations and want the original image back, I can select:

```text
6. Reset
```

The program uses:

```python
image = original.copy()
```

So the image goes back to the one that was uploaded at the beginning.

---

# 7. Exit

When the user selects:

```text
7. Exit
```

the loop stops and the program ends.

---

