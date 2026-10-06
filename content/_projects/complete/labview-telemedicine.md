---
title: Telemedicine system in LabVIEW
timeframe: Semester 2, 2024
order: 4
featured: false
summary: A multi-user telemedicine application connecting local doctors with remote specialists, with messaging, voice, video, a temperature sensor and remote device control.
skills:
  - LabVIEW
  - NI myDAQ
  - networking
  - state machines
  - teamwork
tags:
  - health-technology
  - iot
  - embedded-systems
  - labview
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

In the final weeks I also took on integrating the team's features, reworking sections that didn't connect so the complete system worked for our presentation.

## What I learned

**Choosing the right tool.** LabVIEW excels at connecting to hardware and building measurement systems, but the further we pushed it towards networked, Internet of Things features like voice and video, the harder it became. For that kind of system I'd now reach for Python or another general-purpose language. Working at LabVIEW's lower level did have an upside, though: building the connections between clients and the server myself meant I understood exactly how they worked, and I enjoyed that challenge.

**Making sure the pieces fit together.** We split the work into separate features. I explained to the team how each part should work, including the data flow between them and how the base of the program was structured, but explaining wasn't enough: as the presentation approached, several parts didn't connect, and I took responsibility for integrating them, reworking and in some cases rebuilding sections so the full system ran for the demonstration. Next time I'd give each person a skeleton to build into, such as a template subVI with its inputs and outputs already defined, so their work would plug directly into the server. Agreeing on those connections at the start, and testing the whole system together regularly, would have caught the problems much earlier.
