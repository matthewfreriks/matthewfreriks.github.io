---
title: Networked patient monitoring and IoT security
timeframe: Semester 2, 2024
order: 3
featured: false
summary: Simulated patient monitoring devices communicating over MQTT, with a control interface and a security review of the network they ran on.
skills:
  - Python
  - MQTT
  - Tkinter
  - IoT
  - cybersecurity
tags:
  - health-technology
  - iot
  - cybersecurity
  - python
---

*Individual project for TNE20003 Internet and Cybersecurity for Engineering Applications, Swinburne University of Technology.*

## What I built

A prototype Internet of Things system, written in Python, running on an existing MQTT messaging server. MQTT is a lightweight publish/subscribe protocol widely used by connected devices. The system had three simulated devices:

- **A patient temperature sensor** publishing readings every five seconds.
- **A heart monitor and IV controller** publishing heart rate and blood pressure, and listening for commands to raise or lower the IV rate.
- **A desktop control application** (Tkinter) that subscribes to the sensor data, displays incoming messages, and sends IV commands back to the monitor.

The control application uses a message queue to pass data safely from the network thread to the interface, so the window stays responsive while messages arrive.

## The security review

The second half of the project was a cybersecurity assessment of the messaging server, considering what would change if access expanded beyond the internal network. I identified four main risks and proposed a fix for each:

- **Weak authentication.** Usernames and passwords followed a predictable pattern, so one accidental post to a public topic could expose working credentials. Fix: independent, complex passwords and role-based access control.
- **Unencrypted messages.** MQTT sends data as plain text by default, so anyone on the network could read it. Fix: TLS encryption between devices and the server.
- **Insider and network access.** Even an internal-only server is exposed to anyone already on the network. Fix: network segmentation, traffic monitoring, and a VPN or zero-trust model for any outside access.
- **Denial of service.** A client could flood the server with messages until it stopped responding. Fix: rate limiting, quality-of-service settings and monitoring.

For a system carrying patient information, these protections are essential rather than optional.

## What I learned

<!-- In your own words: e.g. what publish/subscribe is good for in healthcare devices,
     or what surprised you about how easily insecure defaults leak data. -->
