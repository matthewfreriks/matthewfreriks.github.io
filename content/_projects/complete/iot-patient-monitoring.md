---
title: Networked patient monitoring and IoT security
timeframe: Semester 2, 2024
order: 3
featured: false
summary: Simulated patient monitoring devices communicating over MQTT, with a control interface and a security review of the network they ran on.
skills: [Python, MQTT, Tkinter, IoT, cybersecurity]
tags: [health-technology, iot, cybersecurity, python]
---

*Individual project for TNE20003 Internet and Cybersecurity for Engineering Applications, Swinburne University of Technology.*

This is an assessed project, so this page explains the design with short excerpts rather than publishing the full code.

## What I built

A prototype Internet of Things system, written in Python, running on an existing MQTT messaging server. MQTT is a lightweight publish/subscribe protocol widely used by connected devices: devices publish readings to named topics, and anything interested subscribes to those topics, without the devices needing to know about each other.

The system had three simulated devices:

- **A patient temperature sensor** publishing readings every five seconds.
- **A heart monitor and IV controller** publishing heart rate, blood pressure and IV rate, and listening for commands to raise or lower the IV rate.
- **A desktop control application** (Tkinter) that displays incoming data and sends IV commands back to the monitor.

### Organising the data with topics

Every message has a topic, structured like a file path. Grouping readings under `sensor/` and commands under `actuator/` means the control app can receive every sensor with a single wildcard subscription, and new sensors appear automatically without changing the app.

```text
<prefix>/sensor/temperature
<prefix>/sensor/heartrate
<prefix>/sensor/bloodpressure
<prefix>/sensor/IVrate
<prefix>/actuator/commands      <- commands from the control app

Control app subscribes to:  <prefix>/sensor/#
```

### Controlling the IV rate remotely

The heart monitor subscribes to the command topic and adjusts the IV rate when a command arrives. The adjustment is stored separately from the base rate, so the device always knows where it started.

```python
def on_message(client, userdata, msg):
    global iv_adjustment
    command = msg.payload.decode().strip().lower()
    if command == "increase":
        iv_adjustment += 1
    elif command == "decrease":
        iv_adjustment -= 1

# In the publishing loop, every five seconds:
iv_rate = BASE_IV_RATE + iv_adjustment
client.publish(topic_iv_rate, iv_rate)
```

### Keeping the interface responsive

The trickiest part was the interface. MQTT receives messages on a background network thread, but Tkinter only allows the window to be changed from the main thread, and updating it from the wrong thread causes crashes. The fix was a thread-safe queue: the network thread only adds messages to the queue, and the window empties it every 100 ms.

```python
message_queue = queue.Queue()

def on_message(client, userdata, msg):        # network thread
    message_queue.put(f"Received {msg.payload.decode()} from {msg.topic}\n")

def process_message_queue():                  # main (GUI) thread
    while not message_queue.empty():
        text_area.insert(tk.END, message_queue.get())
        text_area.see(tk.END)
    root.after(100, process_message_queue)    # check again in 100 ms
```

## The security review

The second half of the project was a cybersecurity assessment of the messaging server, considering what would change if access expanded beyond the internal network. I identified four main risks and proposed a fix for each:

- **Weak authentication.** Usernames and passwords followed a predictable pattern, so one accidental post to a public topic could expose working credentials. Fix: independent, complex passwords and role-based access control.
- **Unencrypted messages.** MQTT sends data as plain text by default, so anyone on the network could read it. Fix: TLS encryption between devices and the server.
- **Insider and network access.** Even an internal-only server is exposed to anyone already on the network. Fix: network segmentation, traffic monitoring, and a VPN or zero-trust model for any outside access.
- **Denial of service.** A client could flood the server with messages until it stopped responding. Fix: rate limiting, quality-of-service settings and monitoring.
- **Data readable by the server.** Even with TLS, the broker decrypts every message, so anyone with access to it can read patient data. Fix: two layers of encryption. The outer layer carries only what the server needs to route messages and control access, while the patient data inside is encrypted end to end, from the sending device to the authorised recipient, and never readable by the server. Using an authenticated method such as AES-GCM also means any tampering is detected. The trade-offs are that the outer layer still reveals who is communicating and how often, and that every device needs its keys distributed and replaced securely.

For a system carrying patient information, these protections are essential rather than optional.

## What I learned
The biggest gap I found, but didn't get to implement, was protecting data on its way from the devices to whatever collects it. MQTT makes data easy to share, which is fine for readings that aren't sensitive or private, especially when the server sits behind a password and VPN. Patient data needs more. Encrypting the connection with TLS protects data in transit, but the server still decrypts and sees every message in plain text.
A stronger design would use two layers: an outer layer the server can read to route messages and control who receives them, and an inner layer protecting the patient data itself, encrypted on the device and only decrypted by its intended recipient. Even if a message were intercepted or the server compromised, the data would stay unreadable. That layered, end-to-end approach is what I'd build next.
