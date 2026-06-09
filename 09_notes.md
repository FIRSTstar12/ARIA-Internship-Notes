# Point Transformation notes

## Useful imports

```python
import numpy as np

import matplotlib.pyplot as plt
```
## numpy functions

`np.set_printoptions()` = sets up how numpy displays numbers when you print arrays

`np.array()` = makes an arry that is fast at math, takes up less memory, and can do math on the whole array without a loop

`np.radians()` = converts degrees to radians

`np.cos()` = does cosine function but it must be in radians

`np.sin()` = does sine function but it must be in radians

`np.column_stack()` = takes your row and makes it vertical 
**Example:** 
Alice, Bob, Carol 
**becomes**
Alice
Bob
Carol

`np.zeros()` = creates a pre-filled array with zeros

`np.random.default_rng()` = generates one random number and uses everytime you call the variable connected to this function

`np.clip()` = keeps numbers with your range

`np.polyfit()` = finds the best fitting line/curve through your data

`np.linspace()` = creates an array of evenly spaced numbers between the start and stop points

`np.linalg.norm()` = calculates the size/length of a list of numbers

`np.mean()` = gets the average of a set of numbers

`np.max()` = gets the largets number in a set of numbers

`np.arange()` = creates a list of numbers from start to end points counting by a spesific number

`np.savetxt()` = saves your array to a file

`np.loadtxt()` = loads the file your array was saved too

## matplotlib.pyplot functions

`plt.figure()` = creates a blank chart or graph so you can draw things on it

`plt.scatter()` = draws a scatter plot

`plt.legend()` = creates a key/label for your chart/graph

`plt.axis()` = lets you control the boarders of the chart/graph

`plt.show()` = displays your chart/graph

`plt.imshow` = displys an image or 2D grid of numbers as a visual picture in the chart/graph

`pit.title()` = adds a title to the top of your chart

`plt.xlabel()` = a text label below the x-axis of the chart/graph

`plt.ylabel()` = a text label beside the y-axis of the chart/graph

## How to Get Distance

**Formula**

1. (x1,y1)
2. (x2,y2)
3. x1 - x2 = x
4. y1 - y2 = y
5. Take the square root of (x^2 + y^2)

**Code**

```python
def poseDistance2D(poseA, poseB):
    xDistance = poseA[0] - poseB[0]
    yDistance = poseA[1] - poseB[1]
    return np.sqrt(xDistance**2 + yDistance**2)
```

## How to Transform points

**Formula**

- For the x coordinate 
  - $$x' = x \cdot \cos(\theta) - y \cdot \sin(\theta) + t_x$$
- For the y coordinate 
  - $$y' = x \cdot \sin(\theta) + y \cdot \cos(\theta) + t_y$$

**Variables**

- x = original x coordinate 
- y = original y coordinate 
- θ aka theta = rotation angle
- tx = move left/right
- ty move up/down
- x′ = new x coordinate
- y′ = new y coordinate

**Code**

```python
def transformPts(points, pose):
    cos = np.cos(pose[2])
    sin = np.sin(pose[2])
    rotation = np.array([[cos,(sin * -1)],
                         [sin, cos]])
    translationVector = np.array([pose[0],pose[1]])
    return points @ rotation.T + translationVector 
```

## How to Get Threshold

**Formula**

1. $$\text{pixel} = \begin{cases} 1 & \text{if } p > threshold \\ 0 & \text{if } p \leq threshold \end{cases}$$
2. $$a = \frac{n\sum xy - \sum x \sum y}{n\sum x^2 - (\sum x)^2}$$
3. $$b = \frac{\sum y - a\sum x}{n}$$
4. $$y = ax + b$$

**Variables**
- p = original pixel brightness
- n = total number of points
- ∑x = Add up all x values
- ∑y = Add up all y values
- ∑xy = Multiply each x by its y, then add them all up
- ∑x^2 = Square each x, then add them all up
- a = slope
- b = y - intercept

**Code**

```python
threshold = 0.6
binaryImage = image2_noisy > threshold
plt.figure()
plt.imshow(binaryImage, cmap="gray")
plt.title("Thresholded Image")
plt.axis("off")
plt.show()

ys, xs = np.where(binaryImage)

a, b = np.polyfit(xs, ys, deg=1)

print(f"Estimated line: y = {a:.3f} x + {b:.3f}")

x_fit = np.array([0, width - 1])
y_fit = a * x_fit + b

plt.figure()
plt.imshow(image2_noisy, cmap="gray")
plt.plot(x_fit, y_fit, linewidth=3)
plt.title("Detected Line")
plt.axis("off")
plt.show()
```
## How to get ideal path

**Formula**
- $$x_{ref} = \cos(t)$$
- $$y_{ref} = \sin(t)$$

**Code**

```python
t = np.linspace(0, 8, 100)
x_ref = np.cos(t)
y_ref = np.sin(t)
```

## How to simulat drift

**Formula**

- $$x_{est} = \cos(t) + 0.05t + \epsilon_x$$
- $$y_{est} = \sin(t) - 0.03t + \epsilon_y$$


**Code**

```python
x_est = x_ref + 0.05 * t + 0.05 * rng.normal(...)
y_est = y_ref - 0.03 * t + 0.05 * rng.normal(...)
```

## How to measure error

**Formula**
- Position error at each moment
  - $$\text{error} = \sqrt{(x_{est} - x_{ref})^2 + (y_{est} - y_{ref})^2}$$
- Mean error
  - $$\text{mean error} = \frac{\sum_{i=1}^{n} \text{error}_i}{n}$$
- Max error
  - $$\text{max error} = \max(\text{error}_1, \text{error}_2, \dots, \text{error}_n)$$

**Code**

```python
errors = np.linalg.norm(est_traj - ref_traj, axis=1)
```