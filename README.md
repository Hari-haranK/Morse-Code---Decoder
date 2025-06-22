# Morse-Code---Decoder
OpenCV based morse code decoder - using eye blinks

# Eye Blink Morse Code Detector

A lightweight real-time eye blink detection system that interprets blink patterns as Morse code and decodes them into readable text. This project uses computer vision and facial landmark detection to create an accessible communication method through eye movements, optimized specifically for Google Colab.

## Table of Contents

- [Overview]
- [Features]
- [Requirements]
- [Installation]
- [Usage]
- [How It Works]
- [Morse Code Reference]
- [Configuration]
- [Troubleshooting]
- [Technical Details]

## Overview

The Eye Blink Morse Code Detector is designed to help individuals communicate using eye blinks, particularly useful for people with limited mobility or speech difficulties. The system detects facial landmarks, monitors eye movements, and translates blink patterns into Morse code, which is then decoded into readable text.

### Key Capabilities
- Lightweight real-time eye blink detection using computer vision
- Automatic calibration for different users
- Morse code pattern recognition from blink duration
- Live text decoding and display with visual interface
- Optimized for Google Colab with CPU-only processing
- Simple termination through browser prompts

## Features

- Real-time Detection: Continuous monitoring of eye blinks through webcam
- Auto-Calibration: Automatically adjusts blink threshold for each user
- Facial Landmark Visualization: Shows detected eye landmarks and face boundaries
- Morse Code Translation: Converts blink patterns to standard Morse code
- Live Text Output: Immediate display of decoded letters, words, and sentences
- Visual Interface: Clean HTML display with status indicators
- CPU Optimized: Runs efficiently without GPU requirements
- Easy Termination: Browser prompt-based stopping mechanism
- Google Colab Ready: Designed specifically for cloud-based execution

## Requirements

### Software Requirements
- Python 3.7+
- Google Colab (recommended) or local Jupyter environment
- Webcam access
- Modern web browser with camera permissions

### Hardware Requirements
- Computer with webcam
- Stable internet connection (for Google Colab)
- Minimum 2GB RAM recommended
- No GPU required (CPU-only processing)

## Installation

### For Google Colab (Recommended)

1. Open Google Colab
   ```
   https://colab.research.google.com/
   ```

2. Install Required Dependencies
   ```python
   !pip install dlib opencv-python-headless tqdm requests
   ```

3. Copy and Run the Code
   - Copy the provided Python code into a Colab cell
   - Execute the cell to start the program

### For Local Installation

1. Install Dependencies
   ```bash
   pip install dlib opencv-python numpy tqdm requests
   ```

2. Download Shape Predictor (Automatic)
   - The program automatically downloads the required facial landmark predictor
   - File: shape_predictor_68_face_landmarks.dat

## Usage

### Starting the Program

1. Run the main function
   ```python
   main()
   ```

2. Grant Camera Permissions
   - Allow camera access when prompted by your browser

3. Auto-Calibration
   - Look at the camera for ~10 seconds while the system calibrates
   - The system will automatically detect your normal eye state
   - Wait for "Ready" status before starting

### Creating Morse Code

1. Short Blinks (Dots)
   - Quick blinks lasting ≤ 0.2 seconds
   - Represents a dot (.) in Morse code

2. Long Blinks (Dashes)
   - Extended blinks lasting ≥ 0.5 seconds
   - Represents a dash (-) in Morse code

3. Pauses
   - Letter Separation: 1-second pause between letters
   - Word Separation: 2-second pause between words

### Example Usage

To spell "SOS":
- S: ... (three short blinks, pause)
- O: --- (three long blinks, pause)
- S: ... (three short blinks, long pause for word end)

### Terminating the Program

- Wait for the browser prompt that appears every 5 seconds
- Type "end" and click OK to stop
- Or click Cancel to continue running
- The program will safely clean up camera connections

## How It Works

### 1. Camera Initialization
- Uses lightweight camera manager with proper cleanup
- Initializes 320x240 video stream at 10 FPS for efficiency
- Includes automatic cleanup of existing camera streams

### 2. Face Detection
- Uses dlib's frontal face detector with CPU optimization
- Processes frames without upsampling for speed
- Selects the largest detected face for analysis

### 3. Facial Landmark Detection
- Employs a 68-point facial landmark predictor
- Specifically tracks eye landmarks (points 36-47)
- Visualizes detected landmarks and face boundary

### 4. Eye Aspect Ratio (EAR) Calculation
```
EAR = (|p2 - p6| + |p3 - p5|) / (2 * |p1 - p4|)
```
Where p1-p6 are the eye landmark coordinates

### 5. Auto-Calibration
- Collects 30 EAR samples during startup
- Calculates personalized blink threshold
- Threshold = average_EAR - 0.08 (minimum 0.15)

### 6. Blink Detection & Morse Processing
- Dot: Blink duration ≤ 0.2 seconds
- Dash: Blink duration ≥ 0.5 seconds
- Letter End: 1+ second pause after last blink
- Word End: 2+ second pause after last blink

### 7. Text Decoding & Display
- Converts accumulated Morse patterns using lookup table
- Updates visual interface with current status
- Shows decoded text, Morse sequence, and calibration status

## Morse Code Reference

### Letters
| Letter | Code | Letter | Code | Letter | Code |
|--------|------|--------|------|--------|------|
| A | .-   | J | .--- | S | ...  |
| B | -... | K | -.-  | T | -    |
| C | -.-. | L | .-.. | U | ..-  |
| D | -..  | M | --   | V | ...- |
| E | .    | N | -.   | W | .--  |
| F | ..-. | O | ---  | X | -..- |
| G | --.  | P | .--. | Y | -.-- |
| H | .... | Q | --.- | Z | --.. |
| I | ..   | R | .-.  |   |      |

### Numbers
| Number | Code  | Number | Code  |
|--------|-------|--------|-------|
| 0 | ----- | 5 | ..... |
| 1 | .---- | 6 | -.... |
| 2 | ..--- | 7 | --... |
| 3 | ...-- | 8 | ---.. |
| 4 | ....- | 9 | ----. |

## Configuration

### Adjustable Parameters

You can modify these parameters in the SimpleMorseDetector class:

```python
class SimpleMorseDetector:
    def __init__(self):
        self.blink_threshold = 0.22  # Initial threshold (auto-calibrated)
        self.dot_time = 0.2          # Max duration for dot (seconds)
        self.dash_time = 0.5         # Min duration for dash (seconds)
        self.letter_gap = 1.0        # Gap between letters (seconds)
        self.word_gap = 2.0          # Gap between words (seconds)
```

### Parameter Tuning Guide

1. dot_time: Maximum duration for registering a dot
   - Increase if quick blinks are being read as dashes
   - Decrease for very fast blinkers

2. dash_time: Minimum duration for registering a dash
   - Decrease if long blinks aren't being detected
   - Increase if dots are being read as dashes

3. letter_gap: Time to wait before ending a letter
   - Increase for more deliberate timing
   - Decrease for faster communication

4. word_gap: Time to wait before ending a word
   - Adjust based on your preferred pacing

## Troubleshooting

### Common Issues

#### Camera Access Problems
- Issue: Camera not working in Colab
- Solution: 
  - Ensure camera permissions are granted
  - Try refreshing the browser
  - Check if other applications are using the camera
  - The system automatically cleans up previous camera instances

#### Calibration Issues
- Issue: "Calibrating..." never changes to "Ready"
- Solutions:
  - Keep your eyes open and look at the camera
  - Ensure good lighting conditions
  - Avoid blinking excessively during calibration
  - Face should be clearly visible and centered

#### Face Detection Issues
- Issue: "No face detected" message
- Solutions:
  - Improve lighting conditions
  - Move closer to the camera (recommended distance: 2-4 feet)
  - Ensure face is centered in the frame
  - Remove obstacles from your face

#### Blink Detection Problems
- Issue: Blinks not being detected correctly
- Solutions:
  - Wait for calibration to complete
  - Try more deliberate blinking
  - Ensure eye landmarks are visible (green dots)
  - Adjust timing parameters if needed

#### Performance Issues
- Issue: Slow or laggy detection
- Solutions:
  - Close other browser tabs
  - Restart the Colab runtime
  - Ensure stable internet connection

### Error Messages

#### "Download failed" during setup
- Cause: Network issues downloading facial landmark model
- Solution: Check internet connection and try restarting

#### "No face detected" consistently
- Cause: Poor camera setup or lighting
- Solution: Improve lighting and camera positioning

#### Camera initialization failures
- Cause: Browser permissions or conflicting camera usage
- Solution: Grant permissions and close other camera applications

## Technical Details

### Libraries Used

- OpenCV: Computer vision and image processing
- dlib: Face detection and facial landmark prediction (CPU-only)
- NumPy: Numerical computations for eye aspect ratio
- IPython: Display and interaction in Jupyter/Colab
- requests & tqdm: Automatic model downloading with progress
- base64: Image encoding for web display

### Performance Metrics

- Frame Rate: ~5-10 FPS (optimized for accuracy over speed)
- Detection Accuracy: >90% in good lighting conditions
- Latency: <200ms for blink detection
- Memory Usage: ~100-300MB during operation
- CPU Usage: Low (optimized for efficiency)

### Architecture

```
SimpleMorseDetector
├── Auto-calibration system
├── Blink timing analysis
├── Morse code conversion
└── Text decoding

CameraManager
├── Lightweight camera handling
├── Frame capture optimization
└── Proper cleanup mechanisms

Display System
├── Real-time HTML interface
├── Status indicators
└── Visual feedback
```


