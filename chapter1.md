---
title: 'Chapter Title Here'
description: 'Chapter description goes here.'
free_preview: true
---

## Chapter 1 Capstone Exercise

```yaml
type: NormalExercise
key: e8c1edbe67
lang: python
xp: 100
skills: 2
```

You've seen how a soundwave can be turned into numbers and what it looks like when its graphed.

How about another similar soundwave? One slightly different?

In this exercise, we're going to plot the soundwave of `good_morning` against `good_afternoon`.

To have the `good_morning` and `good_afternoon` soundwaves on the same plot and distinguishable from eachother, we'll use Matplotlib's `alpha` parameter.



`@instructions`
1. Set the title to reflect what kind of plot we are making
2. Add the `good_afternoon` time variable (`time_ga`) and amplitude variable (`signal_ga`) to the plot.
3. Do the same with the `good_morning` time variable (`time_gm`) and amplitude variable (`signal_gm`) to the plot.
4. Set the alpha variable to a value between `0.5` and `0.7`.

`@hint`
The `.plot()` method takes arguments in the form of (`x_value`, `y_value`).

`@pre_exercise_code`
```{python}
import wave
import numpy as np
import matplotlib.pyplot as plt

good_morning = wave.open('good-morning.wav', 'r')
good_afternoon = wave.open('good-afternoon.wav', 'r')

soundwave_gm = good_morning.readframes(-1)
soundwave_ga = good_afternoon.readframes(-1)

signal_gm = np.frombuffer(soundwave_gm, dtype='int16')
signal_ga = np.frombuffer(soundwave_ga, dtype='int16')

framerate_gm = good_morning.getframerate()
framerate_ga = good_afternoon.getframerate()

time_gm = np.linspace(0, len(signal_gm)/framerate_gm, num=len(signal_gm))
time_ga = np.linspace(0, len(signal_ga)/framerate_ga, num=len(signal_ga))
```

`@sample_code`
```{python}
# Setup the plot
plt.figure()

# Setup the title and axis titles
plt.title('Good ______ vs. Good _______')
plt.ylabel('Amplitude')
plt.xlabel('Time (seconds)')

# Add the Good Afternoon data to the plot
plt.plot(______, ______)

# Add the Good Morning data to the plot
plt.plot(______, ______, alpha=______)

# Setup the legend with Good Afternoon first
plt.gca().legend(('Good ______', 'Good ______'))

# Show the plot
plt.show()
```

`@solution`
```{python}
# Setup the plot
plt.figure()

# Setup the title and axis titles
plt.title('Good Afternoon vs. Good Morning')
plt.ylabel('Amplitude')
plt.xlabel('Time (seconds)')

# Add the Good Afternoon data to the plot
plt.plot(time_ga, signal_ga)

# Add the Good Morning data to the plot
plt.plot(time_gm, signal_gm, alpha=0.5)

# Setup the legend with Good Afternoon first
plt.gca().legend(('Good Afternoon', 'Good Morning'))

# Show the plot
plt.show()
```

`@sct`
```{python}

```
