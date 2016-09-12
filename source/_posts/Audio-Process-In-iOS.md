---
title: Audio Process In iOS
permalink: audio-process-in-ios
id: 28
updated: '2014-12-17 09:47:12'
date: 2014-11-19 17:18:37
tags:
---

####CMTime

`CMTime` indicates either timestamps (a single time point, 12:00) or durations (5 seconds etc).

Two elements compose one `CMTime` struct, one is numerator and another denominator. Let's say it's `value` and `timescale`. Then the `value` is the total time unit we have and `timescale` will indicate one long one unit occupies since we use `1/timestamp` in calculation. So basically the total length of the `CMTime` would be `value * 1/timestamp`.

####FFT

No matter what framework we use to handle audio data in iOS, either AVFoundation, [EZPlot](https://github.com/syedhali/EZAudio) and any other third-party libraries, one thing we sometimes want to implement is audio visualization. We'd like to see the waveform of the sound from our microphone or some [music file](http://www.raywenderlich.com/36475/how-to-make-a-music-visualizer-in-ios). There are two types of graphs that we can make based on the audio data (those "floats"). One is called Time-Domain Plot and another is Frequency-Domain Plot.

![Time-Domain Plot](http://arachnoid.com/signal_processing/graphics/time-domain.png)
**Time Domain Plot**

![Frequency-Domain Plot](http://cnx.org/content/m13514/latest/guitar_E_330hz.png)
**Frequency Domain Plot**

As we can see, the time domain is based on the continuous audio input over time, which means the range of the time will be infinite. Whereas the frequency domain limits the frequency to a certain range normally (for human it's 20Hz to 20KHz), it's simply decomposing the time domain plot into multiple sinusoids. And each of the sinusoids represents a single frequency which populates the x-axle of the diagram and y-axle stands for the coefficients of the sinusoids in the decomposed formula.