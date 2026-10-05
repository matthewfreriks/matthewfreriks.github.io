---
layout: post
title: Why muscle signals are so hard to read
subtitle: What I learned about EMG, and why it matters for prosthetics
tags:
  - emg
  - biosignals
  - prosthetics
  - signal-processing
  - explainer
  - science-communication
status: published
project: biosignal-sensor
---

<!-- Based on your MBP20001 EMG assignment (2023), rewritten as an explainer.
     Read it through, change anything that doesn't sound like you, and add your own
     intro about why EMG interests you. The figures from your assignment came from
     journals, so they can't be republished here; make your own diagram or plot instead,
     e.g. from your biosignal sensor project once it exists. -->


Every time you move, your muscles produce tiny electrical signals. An electromyogram (EMG) records them. Doctors use it to diagnose nerve and muscle conditions, and engineers use the same signals to control prosthetic hands. Reading them well is harder than it sounds.

## The signal is tiny

Surface EMG signals measured through the skin are usually in the microvolt to millivolt range. Before anything useful can be done with them, they need careful amplification. A **differential amplifier** measures the difference between two electrodes, so interference that reaches both equally, like power-line noise picked up by the body, largely cancels out.

## Everything else is louder

Several kinds of noise compete with the signal:

- **Power-line interference** at 50 Hz in Australia, often removed with a notch filter.
- **Motion artefacts** from electrodes shifting on the skin, mostly at low frequencies. A high-pass filter removes much of this, but setting the cut-off too high also removes real signal.
- **The heart.** ECG signals are strong and can leak into EMG recordings, especially near the chest.
- **Electromagnetic interference** from nearby equipment, reduced with shielding and good cable routing.
- **Poor electrode contact**, which increases noise. Good skin preparation and checking connections before recording make a big difference.

## Why it matters for prosthetics

A myoelectric prosthetic hand reads EMG from the remaining muscles to decide when to open and close. If noise is mistaken for a signal, the hand moves when the user didn't intend it to. That's why so much of the engineering in these devices goes into filtering and pattern recognition, not just motors.

<!-- Finish with a line linking to your biosignal sensor project, e.g.
     "This summer I'm building my own low-cost EMG sensor to see these problems first-hand." -->
So in my Summer project I am going to apply the things I have learned to build my own EMG reader and possible to build out a external 