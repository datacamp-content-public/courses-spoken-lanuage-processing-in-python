---
title: 'Processing Speech Data with Python'
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
- The `.plot()` method takes arguments in the form of (`x_value`, `y_value`).
- The `alpha` value should be equal to something between `0.5` and `0.7`.

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
success_msg("Great effort! Notice the two sound waves are very similar in the beginning. Because the first word is `good` is both audio files, they almost completely overlap. A well-built speech recognition system would recognize this and return the same first word for each wave. Let's build one to do just that.")
```

---

## Chapter 2 Capstone Exercise

```yaml
type: TabExercise
key: 74985c4c1f
xp: 100
```

Trying to hear what someone is saying in a crowded place is harder than when you're face to face in a room. It's the same with transcribing speech to text. If there's noise in the background, it can be hard to accurately pickup the words being spoken.

To handle this, the models powering many speech recognition APIs have been trained on many different types of speech audio. So depending on how much noise in the background of your speech files, you still may get fairly good results.

If not, the `Recognizer()` class has a method which may help. `adjust_for_ambient_noise` listens for background noise at the beginning of an audio file and tries to set the `Recognizer()` energy level (the amount it listens) to an appropriate setting.

`@pre_exercise_code`
```{python}
import speech_recognition as sr
recognizer = sr.Recognizer()
clean_support_call = sr.AudioFile("clean-support-call.wav")
noisy_support_call = sr.AudioFile("noisy-support-call.wav")
```

***

```yaml
type: NormalExercise
key: 3fcc8b352c
xp: 35
```

`@instructions`


`@hint`


`@sample_code`
```{python}

```

`@solution`
```{python}

```

`@sct`
```{python}

```

***

```yaml
type: NormalExercise
key: b496cf1246
xp: 35
```

`@instructions`
A clean audio sample has been imported as `clean_support_call`.

Pass `clean_support_call` to the `with` statement to extract the audio and save it to `clean_support_call_audio`.

Then call `recognize_google` on `clean_support_call_audio`.

>play audio

`@hint`
The source of our audio file is `clean_support_call`.

`@sample_code`
```{python}
# Record the audio from the clean support call
with ______ as source:
  clean_support_call_audio = recognizer.record(source)

# Transcribe the speech from the clean support call
recognizer.recognize_google(______)
```

`@solution`
```{python}
# Record the audio from the clean support call
with clean_support_call as source:
  clean_support_call_audio = recognizer.record(source)

# Transcribe the speech from the clean support call
recognizer.recognize_google(clean_support_call_audio)
```

`@sct`
```{python}

```

***

```yaml
type: NormalExercise
key: 1e52498b6d
xp: 30
```

`@instructions`
We'll now do the same as before but this time with a noisy audio file.

The noisy audio file is saved under `noisy_support_call`.

Extract the audio in the `with` statement and then transcribe the speech with `recognize_google`.

`@hint`
The audio `source` is stored in `noisy_support_call`.

`@sample_code`
```{python}
# Record the audio from the noisy support call
with ______ as source:
  noisy_support_call_audio = recognizer.record(source)

# Transcribe the speech from the noisy support call
recognizer.recognize_google(______)
```

`@solution`
```{python}
# Record the audio from the noisy support call
with noisy_support_call as source:
  noisy_support_call_audio = recognizer.record(source)

# Transcribe the speech from the clean support call
recognizer.recognize_google(noisy_support_call_audio)
```

`@sct`
```{python}

```

***

```yaml
type: NormalExercise
key: 01f0ebe3cd
```

`@instructions`
The `recognizer()` class picked up on a few words but the transcription isn't anywhere near as good as the clean audio.

We'll try to fix this poor transcription with the `adjust_for_ambient_noise()` function.

The function takes an audio `source` and `duration` as inputs.

`duration` is how long the `recognizer()` class listens to the audio to adjust the energy threshold (the amount of listening).

Try to improve the audio transcription of `noisy_support_call` by calling the `adjust_for_ambient_noise` function on the `recognizer()` class and passing it a `duration` of 0.5.

`@hint`
The `adjust_for_ambient_noise` function takes `source` and `duration` as inputs in the form `recognizer.adjust_for_ambient_noise(source, duration=int()).

`@sample_code`
```{python}
# Record the audio from the noisy support call
with noisy_support_call as source:
	# Adjust the recognizer energy threshold for ambient noise
    recognizer.adjust_for_ambient_noise(source, duration=____)
    noisy_support_call_audio = recognizer.record(____)
 
# Transcribe the speech from the noisy support call
recognizer.recognize_google(____)
```

`@solution`
```{python}
# Record the audio from the noisy support call
with noisy_support_call as source:
	# Adjust the recognizer energy threshold for ambient noise
    recognizer.adjust_for_ambient_noise(source, duration=0.5)
    noisy_support_call_audio = recognizer.record(source)
 
# Transcribe the speech from the noisy support call
recognizer.recognize_google(noisy_support_call_audio, show_all=True)
```

`@sct`
```{python}

```

---

## Chapter 3 Capstone Exercise

```yaml
type: NormalExercise
key: db0e943fdb
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{python}

```

`@sample_code`
```{python}

```

`@solution`
```{python}

```

`@sct`
```{python}

```

---

## Chapter 4 Capstone Exercise

```yaml
type: NormalExercise
key: 28a7b53953
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{python}

```

`@sample_code`
```{python}

```

`@solution`
```{python}

```

`@sct`
```{python}

```
