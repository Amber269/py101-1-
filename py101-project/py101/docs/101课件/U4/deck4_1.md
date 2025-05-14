# Programming for AI (Python): Chapter 4 Matplotlib
*The Chow Institute, 2025*

## What You Will Learn
- How to make quick plots in Python
- How to add aesthetic styles to your picture
- The way `matplotlib` manages pictures
- Figures as canvas
- Axes as plotting areas

## 4.1 A First Glance at Figures in Python
We’ve seen many times how to install a module. Let’s install “`matplotlib`” and load it.

```python
import matplotlib.pyplot as plt
import numpy as np
import matplotlib as mpl
```

### Drawing Lines with `matplotlib`
Drawing lines brings us back to high school physics. Remember when your physics teacher told you that it’s hard to find a real continuous line in life? When we want to draw a continuous line or curve, we put together fine enough points.

```python
x = np.linspace(0, 10, 100)
sinx = np.sin(x)
cosx = np.cos(x)
plt.plot(x, cosx)
```

### Integration with Pandas
`matplotlib` has a very natural bound with `pandas`. Once we make a `DataFrame` from `pandas`, it communicates with `matplotlib` smoothly.

```python
irisdata = pd.read_csv("iris/iris.data", header=None)
irisdata.columns = ["sw", "sl", "pw", "pl", "type"]
plt.plot("sw", "sl", data=irisdata)
```

#### In-class Exercise 4.1.1
The above code creates a messy picture. Why? Can you correct it with what you’ve learned from the last chapter?

## 4.2 Modifying Attributes of the Line
The lines we’ve drawn seem shabby. Let’s add some aesthetic elements. When drawing a line, several attributes are commonly used:
- **Markers**
- **Linestyle**
- **Linewidth**
- **Colors**

This page provides choices: [https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html)

### Using Attributes
Let’s use these attributes.

```python
plt.plot(x, sinx,
         marker='>', 
         linestyle='--', 
         linewidth=2, 
         color='g')
```

### Multiple Plots in One Cell
In VS Code (or notebook in general), in one cell, we can write two `plt.plot` functions.

```python
plt.plot(x, sinx, 
         marker='>', markersize=5,
         linestyle='--', 
         linewidth=2, 
         color='g')
plt.plot(x, cosx, 
         marker='d', markersize=5,
         linestyle=':', 
         linewidth=1, 
         color='c')
```

### Adding Legends
But now we are confused. What are the meanings of these lines? Here, `legend` comes to help.

```python
plt.plot(x, sinx, label='sin(x)',
         marker='>', markersize=5,
         linestyle='--', 
         linewidth=2, 
         color='g')
plt.plot(x, cosx, label='cos(x)',
         marker='d', markersize=5,
         linestyle=':', 
         linewidth=1, 
         color='c')
plt.legend()
```

### Saving the Plot
Lastly, let’s save the beautiful picture somewhere.

```python
plt.plot(x, sinx, label='sin(x)',
         marker='>', markersize=5,
         linestyle='--', 
         linewidth=2, 
         color='g')
plt.plot(x, cosx, label='cos(x)',
         marker='d', markersize=5,
         linestyle=':', 
         linewidth=1, 
         color='c')
plt.legend()
plt.savefig('sin_and_cos.png')
```

## 4.3 Organization of a Picture in `matplotlib`
`matplotlib` organizes pictures as **figures** and **axes**.
- A **figure** is like a canvas, where you can draw the lines, curves, shapes, etc.
- An **axis** is an area on the canvas, where the picture actually lives.
- Most of the time, we must draw on an axis within a figure.

Here is how the official documentation of `matplotlib` describes its layout: [https://matplotlib.org/stable/users/explain/quick_start.html](https://matplotlib.org/stable/users/explain/quick_start.html)

### Example of Elements in `matplotlib`
The first attempt to make an axis implicitly is simply telling Python that we need to divide the figure into different parts.

```python
plt.subplot(2, 1, 1)  # (#rows, #columns, which)
plt.plot(x, sinx)
```

#### In-class Exercise 4.3.1
Make another axis and plot the cosine curve in it.

#### In-class Exercise 4.3.2
Please read all questions before you draw.
1. Draw a plot of a quarter of a unit circle in the first quadrant.
2. Draw a half unit circle on \( y > 0 \).
3. Draw a unit circle.

### Manually Creating a Figure and Axis
Although convenient, `plt.subplot` creates regular cells for us to put in axes. Now let’s find out more by manually creating a figure and an axis.

```python
fig = plt.figure()
# https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.figure.html
ax = plt.axes()
# https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.axes.html
x = np.linspace(0, 10, 1000)
sinx = np.sin(x)
ax.plot(x, sinx)
# print(id(ax), id(fig.axes[0]), id(plt.gca()))
```

#### In-class Exercise 4.3.3
Create another axis and check their ID.

#### In-class Exercise 4.3.4
Create an empty figure. What is it like?

### Implicit Creation of Figure and Axis
If we just ask Python to make an axis, Python will implicitly create a figure first and then an axis in that figure.

```python
fig = plt.figure()
id(fig)
ax = plt.axes()
print(id(ax.get_figure()))
print(id(plt.gcf()))
```

### Working with `matplotlib` in a Notebook
Working with `matplotlib` in a notebook is very different from working with a script.

```python
ax = plt.axes()
print(id(ax.get_figure()))
print(id(plt.gcf()))
x = np.linspace(0, 10, 1000)
sinx = np.sin(x)
ax.plot(x, sinx)
```

#### In-class Exercise 4.3.5
Where is the output?

### Correct Way to Make a Plot in a Desired Place
- Put all code in one cell.
- Mark the figure you created in the earlier cell.

### Example 4.3.1: Changing a Figure Dynamically
```python
x = np.arange(1, 3)
y = x
x2 = np.arange(3, 11)
y2 = x2
x3 = np.arange(11, 21)
y3 = x3
fig = plt.figure()
# create an ax<->fig<->gcf()
plt.scatter(x, y, linewidth=5)  # <->ax.scatter<->gca().scatter
```

#### Example 4.3.1 (Continued)
```python
ax = fig.gca().scatter(x2, y2,  # get current ax
                       color='r', 
                       linewidth=5)
fig
fig.gca().scatter(x3, y3,  # get current ax
                  color='g', 
                  linewidth=5)
fig
```

### Modifying Attributes of the Axes
As before, we can modify the attributes of the axes.

```python
ax = fig_in_cell_3.gca()
ax.set(xlim=(-1, 11),  # plt.xlim
       ylim=(-1.5, 1.5),
       xlabel="x-axis",
       ylabel="y-axis",
       title="A sine curve")
fig_in_cell_3
```

*Source: [https://matplotlib.org/stable/gallery/showcase/anatomy.html](https://matplotlib.org/stable/gallery/showcase/anatomy.html)*