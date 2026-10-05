---
title: Telemedicine system in LabVIEW
status: done
timeframe: Semester 2, 2024
order: 4
featured: false
summary: A multi-user telemedicine application connecting local doctors with remote specialists, with messaging, voice, video, a temperature sensor and remote device control.
skills: [LabVIEW, NI myDAQ, networking, state machines, teamwork]
---

*ENG20010 Engineering Technology Design Project, Swinburne University of Technology. Group project with Maryam Zaman, Nguyen Phuc (Ben) Duong and Paul Avice-Demay.*

## The brief

Design a telemedicine application that lets doctors in remote Australian communities consult specialists elsewhere, combining communication with live data from medical devices. Built in LabVIEW, with a state machine architecture and an NI myDAQ connecting the software to hardware.

## What we built

A client-server application with:

- **A login system** with separate interfaces for patients, doctors and specialists.
- **Text messaging, voice calls and video calls** between users.
- **Live temperature readings** from a sensor through the NI myDAQ.
- **Remote control of a servo motor**, standing in for a medical device a specialist could adjust from afar.
- **Image sharing**, so local doctors could send examination photos to specialists.

## My role

I built the server that relays communication between clients, and completed the core features: text messaging, temperature sensor integration and servo control. I also supported teammates working on the dashboard, image sharing and voice calls.

<!-- Optional: add a screenshot of the interface, or a still from the demo video
     (avoid anything showing login details or personal information). -->

## What I learned

<!-- In your own words: e.g. what a state machine made easier, the challenges of
     sending voice and video over a network in LabVIEW, or splitting work across a team. -->
