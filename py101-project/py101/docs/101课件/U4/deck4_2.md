# Matplotlib: Multiple Axes in One Figure
*The Chow Institute, 2025*

## 4.4 Multiple Axes in One Figure

### Introduction
We’ve learned how `matplotlib` manages pictures. Now it’s time for us to manage it. Recall how we made a quick plot in Python? We used `plt.subplot`! It has a sibling but is very different: `plt.subplots`.

#### In-class Exercise 4.4.1
What did `plt.subplot` return?

### Basic Usage of `plt.subplots`
A simple call of the `plt.subplots` function returns two objects:
- `fig`: The figure object.
- `ax`: The axes object.

Because axes are where the plotting is made, we use the `ax.plot` function.

```python
import matplotlib.pyplot as plt
import numpy as np

fig, ax = plt.subplots()
x = np.linspace(0, 10, 1000)
sinx = np.sin(x)
cosx = np.cos(x)
expx = np.exp(x)
tanhx = np.tanh(x)
ax.plot(x, sinx)
plt.show()
```

### Power of `plt.subplots`
But this seems tedious, why do we need the powerful `plt.subplots`? Its power lies in the parameters:

```python
fig, axs = plt.subplots(2, 2)
```

#### In-class Exercise 4.4.2
What is `axs`? How do you investigate it?

### Precise Control with `plt.subplots`
Now we can more precisely control where we want to plot.

```python
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2)
ax1.plot(x, sinx)
ax2.plot(x, cosx)
ax3.plot(x, expx)
ax4.plot(x, tanhx)
plt.show()
```

#### In-class Exercise 4.4.3
How many methods have we learned to make figures and axes?

### Sequential Creation of Figures and Axes
Another method is to create a figure and axes sequentially. We first create a figure and change its attributes:

```python
fig = plt.figure(figsize=(2.5, 2.5), facecolor='lightskyblue', layout='constrained')
fig.suptitle('Fig-Ax Figure')
```

Now let’s add an axis and change its outlook:

```python
ax = fig.add_subplot()
ax.set_title('Axes', loc='left', fontstyle='oblique', fontsize='medium')
plt.show()
```

### Mosaic Subplots
`plt.subplots` is more powerful than creating regular cells. This is what we call mosaic subplots.

```python
fig, axs = plt.subplot_mosaic([['A', 'right'], ['B', 'right']], figsize=(4, 3), layout='constrained')
```

#### In-class Exercise 4.4.4
What is `axs` like? Can you summarize how to make a mosaic plot?

### Customizing Mosaic Subplots
We can modify the axes after they are created:

```python
axs['A'].text(0.5, 0.5, 'subplot A', ha='center', va='center')
axs['right'].text(0.5, 0.5, 'subplot right', ha='center', va='center')
axs['B'].text(0.5, 0.5, 'subplot B', ha='center', va='center')

axs['A'].set_xlabel("x-axis of A")
axs['A'].set_xlim(-2, 3)
axs['B'].set_title('axs[B] title')
```

### Adding Annotations and Titles
We can add annotations and titles to the axes and figure:

```python
for row in range(2):
    for col in range(2):
        axs[row, col].annotate(f'axs[{row}, {col}]', (0.5, 0.5), ha='center', va='center', fontsize=18, color='darkgrey')
fig.suptitle('plt.subplots()')
plt.show()
```

### Arranging Axes Precisely
Lastly, we don’t like the margins sometimes, so we can tell Python how to arrange the axes precisely:

```python
fig.clf()
fig = plt.figure()
fig.add_axes([0, 0, 1, 1])
fig.add_axes([1, 1, 1, 1])
fig.add_axes([-1, 0, 1, 2])
plt.show()
```

## 4.5 The Example
Now we are ready for the example. Our goal is to make the following picture:

```
3.4
1.3
0.8
0.5
```

### Analyzing the Shape
Before we start to write code, let’s analyze its shape:

```
3.4
1.3
0.8
0.5
```

#### In-class Exercise 4.5.1
1. Make a skeleton of the target picture.
2. Add in the curves.
3. (Optional) Fill in the wedges with colors.