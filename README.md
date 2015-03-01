#Problem

We want to shift the tempo of a song to make a transition into another song

#Question

How do we alter the tempo of a song to match another song?

How can we shift the tempo without making it noticeable to the listener?

#Resources
1. [Echonest API]
2. [Echonest Remix API]
3. [Tempo change study]

### 1. Mini-abstract and releveance of [Echonest API]

The Echonest API is an API for song related information.  It has methods that allow the user to get song information, and contains classes that allow a song to be altered.  Using the pyechonest.track module, you can find the tempo and time signature of the song.

```python
import pyechonest.track as track

t = track.track_from_filename('file.mp3')
t.tempo
t.time_signature
```

The above commands will yield the tempo and time_signature of a song.

### 2. Mini-abstract and relevance of [Echonest Remix API]

The Echonest Remix API contains methods to alter a song.  Specifically for our case, it allows the beat of a song to be altered.

Example code taken from https://github.com/echonest/remix/blob/master/examples/stretch/simple_stretch.py

```python
import math
import os
import sys
import dirac
from echonest.remix import audio

audiofile = audio.LocalAudioFile(input_filename)
beats = audiofile.analysis.beats
collect = []

for beat in beats:
  beat_audio = beat.render()
  scaled_beat = dirac.timeScale(beat_audio.data, ratio)
  ts = audio.AudioData(ndarray=scaled_beat, shape=scaled_beat.shape,
      sampleRate=audiofile.sampleRate, numChannels=scaled_beat.shape[1])
  collect.append(ts)
  
out = audio.assemble(collect, numChannels=2)
out.encode(output_filename)
```
Using the audio and modify modules from echonest.remix, one can alter the time of a beat using the dirac.timeScale method.  Using this altered beat, you can re-apply the previous characteristics of the beat and make a new song out of these beats.

### 3. Mini-abstract and relevance of [Tempo change study]

This is an article describing the results of a study in human perception of tempo change.  This is important for us so we know what threshold we can alter the beat of a song before it becomes too obvious to the listener.  The results of this article showed that humans perceive tempo changes when the tempo is altered by approximately 8%.

[Echonest API]: http://developer.echonest.com/docs/v4
[Echonest Remix API]: http://echonest.github.io/remix/
[Tempo change study]: http://psyencelab.com/images/Just_Noticeable_Difference_and_Tempo_Change.pdf
