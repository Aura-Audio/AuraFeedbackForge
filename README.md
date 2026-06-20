# 🎛️ FeedbackForge

An advanced, browser-based audio synthesis and feedback engine built entirely with the native Web Audio API. FeedbackForge Pro allows you to generate, manipulate, and visualize acoustic feedback loops in real-time using sine waves, colored noise, or dynamic frequency sweeps.

Whether you're a sound designer looking for unique textures, a developer exploring Web Audio routing, or just someone who likes to make noise, FeedbackForge Pro provides a deep, stable, and hardware-accelerated sandbox.

![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-yellow?style=flat-square)
![Web Audio API](https://img.shields.io/badge/Web_Audio_API-Native-blue?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

---

## 📸 UI Preview
*(Insert a screenshot of the application interface here)*

---

## ✨ Key Features

### 1. Zero-Dropout Dynamic Routing
Unlike traditional Web Audio setups that tear down the entire audio context when settings change, FeedbackForge uses dynamic node management. Add or remove active frequency bands *while playing* without any audio stutters, dropouts, or clicks.

### 2. Safe Acoustic Feedback Simulation
Real-world feedback requires physical delay (sound traveling through the air). We simulate this safely using a `DelayNode` in the feedback path. This prevents the Web Audio context from crashing due to infinite mathematical recursion (NaN/Infinity values) while sounding authentic.

### 3. Native Hardware-Accelerated LFO
The Low-Frequency Oscillator (LFO) is not driven by `requestAnimationFrame`. It uses native Web Audio `OscillatorNode`s connected directly to the `GainNode`'s AudioParam, ensuring zero-latency, drift-free, hardware-accelerated volume modulation.

### 4. Harmonic Saturation (WaveShaper)
Built-in `WaveShaperNode` algorithms allow you to inject analog-style harmonic distortion directly into the feedback loop. Turn sterile sine waves into screaming, harmonically rich leads.

### 5. Stereo Spatialization
Active frequency bands are automatically routed through `StereoPannerNode`s. Lower frequencies are panned slightly left, and higher frequencies slightly right, creating an immersive, wide stereo field.

### 6. Multiple Signal Sources
*   **Sine Waves:** Layered pure tones matching active frequency buttons.
*   **Noise Filters:** White, Pink, or Brown noise injected into bandpass filters.
*   **Frequency Sweep:** A logarithmic glide back-and-forth between your lowest and highest selected bands.

### 7. Preset Management
Save your custom setups directly to your browser via `localStorage`. Includes factory presets to get you started immediately.

---

## 🛠️ Tech Stack & Architecture

FeedbackForge Pro is built with **100% Vanilla JavaScript** and the **Native Web Audio API**. No external libraries or frameworks are used.

### Signal Flow / Audio Graph
The core routing of the application is designed to allow feedback while protecting the user's speakers and the browser's audio engine:

```text
[Oscillator / Noise Source] 
       ↓
[BiquadFilter (Bandpass)] ← (Dynamic Feedback Input)
       ↓
[StereoPannerNode]
       ↓
[Master GainNode] ← (Native LFO Modulation)
       ↓
[AnalyserNode] → (To Visualizer)
       ↓
[DynamicsCompressor] (Master Limiter) → [Speakers]

// Feedback Path (Safe Looping)
[AnalyserNode] → [WaveShaper (Saturation)] → [DelayNode (50ms)] → [Feedback GainNode] → (Back to BiquadFilter)
```

---

## 🚀 Getting Started

Because this is a purely client-side application, getting started is incredibly easy.

### Option 1: Direct Download
1. Download the `index.html` file (or whatever you named the provided code file).
2. Double-click the file to open it in your default web browser (Chrome, Firefox, Edge, and Safari recommended).

### Option 2: Clone the Repository
```bash
git clone https://github.com/your-username/feedbackforge-pro.git
cd feedbackforge-pro
```
Open `index.html` in your browser.

> **Note on Audio Contexts:** Modern browsers block audio from playing until a user interacts with the page. Clicking the "Start Audio" button satisfies this requirement and initializes the `AudioContext`.

---

## 🎛️ Usage Guide

1.  **Select a Source:** Choose between Sine Waves, Noise, or Frequency Sweep.
2.  **Toggle Frequencies:** Click the frequency band buttons (20Hz up to 24kHz) to activate or deactivate them. 
    * *If audio is already playing, the graph updates instantly.*
3.  **Adjust Parameters:**
    *   **Master Gain:** Overall volume.
    *   **Filter Q:** Toggles between Narrow (High Q / Resonant) and Wide (Low Q / Broad).
    *   **Feedback Amount:** Controls the gain of the feedback loop. Higher values mean the frequencies sustain longer.
    *   **Saturation:** Adds distortion to the feedback path.
4.  **Modulate:** Enable the LFO to automatically sweep the master volume. Choose your waveform (Sine, Square, Triangle, Sawtooth) and cycle length.
5.  **Save Presets:** Enter a name in the text box and click "Save" to store your current configuration in your browser's local storage.

---

## ⚠️ Safety Warning & Best Practices

**PROTECT YOUR HEARING AND EQUIPMENT.**

While FeedbackForge Pro utilizes a master `DynamicsCompressor` set to a strict limiter configuration (-3dB threshold, 15:1 ratio) and a delay node to prevent internal math crashes, audio feedback can still be extremely loud and sudden.

*   **Start Low:** Always start with Master Gain at `0.2` or lower and Feedback Amount at `0.1` before pressing start.
*   **Limit High Frequencies:** High-frequency feedback (4kHz+) is piercing. Use the Q toggle set to "Wide" when experimenting with high frequencies to prevent narrow, painful resonant peaks.
*   **Headphone Warning:** Use speakers if possible. If using headphones, keep the volume dial on your OS low.

---

## 🗺️ Roadmap

Future enhancements planned for FeedbackForge Pro:

- [ ] **MIDI Mapping:** Allow users to map frequency toggles and sliders to a physical MIDI controller.
- [ ] **Record/Export Audio:** Implement `MediaRecorder` to capture the output and download it as `.wav` or `.webm`.
- [ ] **Convolution Reverb:** Add a spatial reverb node post-compressor for ambient sound design.
- [ ] **Visualizer Themes:** Add multiple visualizer modes (Oscilloscope line, 3D bars, radial).

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/your-username/feedbackforge-pro/issues).

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏 Acknowledgements

*   The [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) for their invaluable Web Audio API documentation.
*   The Web Audio community for pioneering techniques in dynamic node routing and safe feedback generation.
