# Gesture Context Conversation Lab

A browser-based interactive prototype demonstrating how the **same physical gesture can communicate different meanings depending on conversational context**.

The application uses a webcam and **MediaPipe Gesture Recognizer** to detect hand gestures in real time. Rather than mapping each gesture to a single fixed command, the system interprets the gesture according to the current interaction state.

## Concept

Human gestures do not always have one fixed meaning.

Their meaning depends on **when they occur and what is happening in the interaction**.

This prototype demonstrates the interaction model:

**GESTURE → CONTEXT → INTENT → ACTION**

For example, an open palm ✋ can mean:

- **Cancel** while the camera is counting down
- **Reject** while reviewing a captured photo
- **No action** when there is nothing to cancel or reject

The system therefore considers both the detected gesture and the current conversational state before deciding what action to perform.

---

## Interaction Flow

| Context | Gesture | Interpretation | Action |
|---|---|---|---|
| READY | ✌️ V Sign | Request a photo | Start countdown |
| COUNTDOWN | ✋ Open Palm | Stop / Cancel | Cancel countdown |
| REVIEW | 👍 Thumbs Up | Accept photo | Keep photo |
| REVIEW | ✋ Open Palm | Reject photo | Discard photo |
| CONFIRMED | ✌️ V Sign | Request another photo | Start a new photo flow |

This demonstrates a simple **context-aware conversational interface** in which gesture meaning changes dynamically according to the state of the interaction.

---

## Features

- Real-time webcam input
- Hand gesture recognition
- MediaPipe Gesture Recognizer
- Detection confidence display
- Hand landmark visualization
- Context-aware gesture interpretation
- Conversational state management
- Voice feedback using the Web Speech API
- Photo countdown
- Photo capture from webcam
- Photo review
- Gesture-based confirmation and rejection
- Save confirmed photos
- Live conversation history
- Real-time display of:

  - Gesture
  - Context
  - Intent
  - Interaction strategy
  - System action

---

## Supported Gestures

### ✌️ V Sign

In the **READY** state:

> Request a photo

The system begins a three-second countdown.

In the **CONFIRMED** state:

> Request another photo

A new photo-taking sequence begins.

### ✋ Open Palm

During **COUNTDOWN**:

> Cancel the current photo request

During **REVIEW**:

> Reject the captured photo

This is the key example showing that the **same gesture can represent different intentions depending on context**.

### 👍 Thumbs Up

During **REVIEW**:

> Accept the captured photo

The image is confirmed and can then be saved.

---

## Conversational States

The prototype uses four main interaction states.

### READY

The system is waiting for the user to request a photo.

**Expected gesture:** ✌️

### COUNTDOWN

A photo request has been recognized and the system is preparing to capture the image.

**Available gesture:** ✋ to cancel

### REVIEW

A photo has been captured and the system waits for the user's decision.

**Available gestures:**

- 👍 Keep the photo
- ✋ Reject the photo

### CONFIRMED

The photo has been accepted.

**Available gesture:** ✌️ to take another photo

---

## System Architecture

The interaction can be represented as:

```text
Webcam
   ↓
MediaPipe Gesture Recognizer
   ↓
Gesture Detection
   ↓
Current Conversational Context
   ↓
Intent Interpretation
   ↓
Interaction Strategy
   ↓
System Action
   ↓
Visual + Voice Feedback
```

Unlike a simple gesture-command interface, the detected gesture alone does not determine the action.

Conceptually:

```text
Action = f(Gesture, Context)
```

This allows one gesture to support multiple meanings within the same interaction.

---

## Example

Suppose the system detects:

```text
✋ Open Palm
```

The resulting action depends on the current context.

```text
COUNTDOWN
✋ → Stop / Cancel
```

but:

```text
REVIEW
✋ → Reject Photo
```

and:

```text
READY
✋ → No Action
```

Therefore:

```text
Same Gesture
     +
Different Context
     =
Different Intent
```

---

## Requirements

The project requires a modern browser with:

- JavaScript ES Modules
- Webcam access
- `navigator.mediaDevices.getUserMedia()`
- Web Speech API support for voice feedback

The application also requires the following MediaPipe files:

```text
index.html
vision_bundle.mjs
gesture_recognizer.task
wasm/
```

The exact HTML filename can be changed as needed.

---

## Running the Project

Because the application uses JavaScript modules and local MediaPipe resources, it should be served through a local web server rather than opened directly using `file://`.

For example, using Python:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

Select **Start Camera** and allow webcam access when prompted.

---

## How to Use

1. Start the local web server.
2. Open the application in a browser.
3. Click **Start Camera**.
4. Allow webcam access.
5. Show ✌️ to request a photo.
6. During the countdown, show ✋ to cancel if needed.
7. After the photo is captured:
   - Show 👍 to keep it.
   - Show ✋ to reject it.
8. After confirmation, show ✌️ to take another photo.

The interface displays how the system interprets each gesture as:

```
