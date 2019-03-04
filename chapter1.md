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

## Chapter 2 Capstone Exercise - Working with noisy audio

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
key: b496cf1246
xp: 35
```

`@instructions`
In this exercise, we'll start by transcribing a clean speech sample to text and then see what happens when we add some background noise.

A clean audio sample has been imported as `clean_support_call`.

Pass `clean_support_call` to the `with` statement to extract the audio and save it to `clean_support_call_audio`.

Then call `recognize_google` on `clean_support_call_audio`.

>play audio

`@hint`
- The source of our audio file is `clean_support_call`.

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
success_msg("Nice! Since the audio was clear, the Google Speech API was able to transcribe the spoken words perfectly.")
```

***

```yaml
type: NormalExercise
key: 1e52498b6d
xp: 35
```

`@instructions`
We'll now do the same as before but this time with a noisy audio file.

The noisy audio file is saved under `noisy_support_call`.

> play audio

Extract the audio in the `with` statement and then transcribe the speech with `recognize_google`.

`@hint`
- The audio `source` is stored in `noisy_support_call`.

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
success_msg("This time wasn't so great. The added noise in the background meant the Google Speech API couldn't decipher the words very well. Let's try adjusting for ambient noise.")
```

***

```yaml
type: NormalExercise
key: 01f0ebe3cd
xp: 30
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
success_msg("Well, the results still weren't to good. This should be expected with some audio files though, sometimes the background noise is too much. If your audio files have a large amount of background noise, you may need to preprocess them with an audio tool such as Audacity before using them with `speech_recognition`.")
```

---

## Chapter 3 Capstone Exercise

```yaml
type: NormalExercise
key: db0e943fdb
xp: 100
```

Converting audio files one by one is fine if you've got a small amount of files. 

But what if you've got a whole folder of different speech files?

In this exercise, we'll look at how to use `PyDub`'s `from_file()` and `export()` functions to go through a number of files with the wrong file type and convert them to `.wav` so they're compatible with the `recognizer()` class.

The `extension_list` has been setup to handle `.mp4`, `.mp3`, `.m4a` and `.aac` file types. If you have different files, you may want to add their type to the list.

The variable `wav_filename` is created by taking the original filename, removing the existing extension and changing it to `.wav`.

The code goes through each extension in the `extension_list` and each audio file in the current directory and creates a new `wav_filename`. Your job is to read the audio_file and then export it with the 'wav' format.

`@instructions`
- Pass `audio_file` to the `from_file()` function
- Export the `audio_file` with the format 'wav'

`@hint`
The format variable accepts a string as input.

`@pre_exercise_code`
```{python}
import os
import glob

os.chdir(wrong_types_dir)
```

`@sample_code`
```{python}
extension_list = ('*.mp4', '*.mp3', '*.m4a', '*.aac')

# Loop through the different extensions
for extension in extension_list:
	
    # Loop through the files in the folder
    for audio_file in glob.glob(extension):
    
        # Create the new .wav filename
        wav_filename = os.path.splitext(os.path.basename(audio_file))[0] + '.wav'
        
        # Read the audio_file from file and then export it with the format 'wav'
        AudioSegment.from_file(____).export(wav_filename, format='____')
        
        print(f"Creating {wav_filename}...")
```

`@solution`
```{python}
extension_list = ('*.mp4', '*.mp3', '*.m4a', '*.aac')

# Loop through the different extensions
for extension in extension_list:
	
    # Loop through the files in the folder
    for audio_file in glob.glob(extension):
    
        # Create the new .wav filename
        wav_filename = os.path.splitext(os.path.basename(audio_file))[0] + '.wav'
        
        # Read the audio_file from file and then export it with the format 'wav'
        AudioSegment.from_file(audio_file).export(wav_filename, format='wav')
        
        print(f"Creating {wav_filename}...")
```

`@sct`
```{python}
success_msg("Woohoo! You've successfully converted the folder of audio files from being non-compatiable with `speech_recognition` to being compatible!")
```

---

## Chapter 4 Capstone Exercise - Build a classification model

```yaml
type: NormalExercise
key: 28a7b53953
xp: 100
```

Now we've turned our transcribed audio data into Pandas dataframes, we'll build a model to classify what category our `update_order_details_text` belongs to.

You may be able to tell what category it belongs to straight away by looking at it, but what if you had 100's or 1000's of transcribed text files you wanted to classify?

To build the model we're going to be using `sklearn`'s `CountVectorizer` class to convert the text into numbers so the `MultinomialNB` class can learn what's different about them.

The data the model will train on is stored in `df_train`. The text in this dataframe already has labels.

The data the model will predict on is stored in `df_test`. The text in this dataframe doesn't have a label.

This model will work on our small example here but for larger amounts of text, you may want to consider something more sophisticated.

Once you've transcribed your audio into text using what you've learned in this course, you can learn about more advanced text processing techniques in [DataCamp's XYZ path...]? 

TK - NOTE: This would probably be better as a sequential exercise (step by step creating the classifier pipeline)

`@instructions`
- Create `test_features` using `df_test`
- Fit the model on `training_features`
- Use the model to predict the class of `test_features`

`@hint`
- The model is `.fit()` on `training_features` and makes a prediction on `test_features`.

`@pre_exercise_code`
```{python}
import pandas as pd
df_train = pd.DataFrame([{'text': complaint_text, 'class': 1}, 
                         {'text': wrong_size_order_text, 'class': 0}])

df_test = pd.DataFrame([{'text': update_order_details_text, 'class': 'unknown'}])
```

`@sample_code`
```{python}
from sklearn.naive_bayes import MultinomialNB
from sklearn.feature_extraction.text import CountVectorizer

# Create the vectorizer and use it to convert the text into numbers
vectorizer = CountVectorizer()
training_features = vectorizer.fit_transform(df_train["text"])
test_features = vectorizer.transform(____["text"])

# Create and fit the model on training_features, with the class column as labels
model = MultinomialNB()
model.fit(____, df_train["class"])

# Use the model to predict the class of the test_features
print(model.predict(____))
```

`@solution`
```{python}
from sklearn.naive_bayes import MultinomialNB
from sklearn.feature_extraction.text import CountVectorizer

# Create the vectorizer and use it to convert the text into numbers
vectorizer = CountVectorizer()
training_features = vectorizer.fit_transform(df_train["text"])
test_features = vectorizer.transform(df_test["text"])

# Create and fit the model on training_features, with the class column as labels
model = MultinomialNB()
model.fit(training_features, df_train["class"])

# Use the model to predict the class of the test_features
print(model.predict(test_features))
```

`@sct`
```{python}
success_msg("Consider it classified! Now you're ready to start capturing speech, converting it to text and classifying it into different categories. Massive effort!")
```
