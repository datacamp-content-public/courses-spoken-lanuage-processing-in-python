---
title: 'How do you process speech data?'
description: ""
---

## Introduction to SpeechRecognition Python Library

```yaml
type: VideoExercise
key: 82952ba503
xp: 50
```

`@projector_key`
f657fa6de1364fcda13b149e26867f1e

---

## Import the speech_recognition library

```yaml
type: NormalExercise
key: 081d68fe12
xp: 100
```

To save typing out `speech_recognition` every time we want to use the library, it's common practice to import it as `sr`.

This is much like Pandas is imported as `pd` and NumPy is imported as `np`.

`@instructions`
- Import the `speech_recognition` library as `sr`

`@hint`
- In Python, it's common practice to abbreviate the name of a library to it's lowercased initials or something which is relatable to the original name.

`@pre_exercise_code`
```{python}

```

`@sample_code`
```{python}
# Importing the abbreviated version of the speech_recognition library
import ____ as ____
```

`@solution`
```{python}
# Importing the abbreviated version of the speech_recognition library
import speech_recognition as sr
```

`@sct`
```{python}

```

---

## Create an instance of the Recognizer() class

```yaml
type: NormalExercise
key: 9f36744d2f
xp: 100
```

Now you've imported the `speech_recognition` library as `sr`, we'll use it to access the `Recognizer()` class.

As you'll see in future exercises, the `Recognizer()` class is very helpful. But it won't be helpful if it doesn't exist.

Accessing a class from a library is often referred to as creating an instance of that class.

It's like telling Python to go into the `speech_recognition` library and pull out `Recognizer()` so we can use it.

Classes are accessed from a library in the form:

`instance_of_class = library.Class()`

In this exercise we'll create an instance of `Recognizer()` and call it `recognizer`.

Remember, the `speech_recognition` library has been imported as `sr`.

`@instructions`
- Access the `Recognizer()` class from the `speech_recognition` library and save it to `recognizer`

`@hint`
- The `speech_recognition` library has been imported as `sr`

`@pre_exercise_code`
```{python}

```

`@sample_code`
```{python}
import speech_recognition as sr

# Create an instance of the Recognizer() class
recognizer = ____.Recognizer()
```

`@solution`
```{python}
import speech_recognition as sr

# Create an instance of the Recognizer() class
recognizer = sr.Recognizer()
```

`@sct`
```{python}

```

---

## Pick the speech_recognition API

```yaml
type: PureMultipleChoiceExercise
key: b3a3ddeee5
xp: 50
```

Which of the following is not a speech recognition API within the `speech_recognition` library?

`@hint`
- All of the speech recognition API calls begin with `recognize_`

`@possible_answers`
- `recognize_google()`
- `recognize_bing()`
- `recognize_wit()`
- `what_does_this_say()`

`@feedback`
