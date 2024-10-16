
<div align="center">
  <h1>Gesture Volume Control Using OpenCV and MediaPipe</h1>
  <img alt="output" src="file:///C:/Users/Pragati/OneDrive/Pictures/Screenshots/Screenshot%202024-10-14%20094341.png" />
 </div>

# Gesture-Volume-Controller
The **Gesture Volume Controller** is an advanced machine learning project that allows users to adjust their device’s volume by making simple hand gestures in front of a camera, eliminating the need for physical volume buttons. By detecting the relative positions of the index finger and thumb, the project calculates the distance between them and adjusts the volume accordingly, making for a seamless and touch-free volume control experience.

### Main Components of the Project

The project has several key components, including real-time hand detection, distance calculation, and volume control. Let's break them down:

1. **Hand Detection and Tracking**: The project uses a webcam to capture live video feed and processes the frames to detect and track hand movements. The OpenCV library is used for video capture, while Mediapipe's hand-tracking features, implemented through the custom `handtracking_module`, are responsible for identifying hand landmarks (such as fingertips and joints). The module detects the positions of the thumb and index finger, which are used to control the volume.

2. **Distance Calculation**: Once the positions of the thumb and index finger are identified, the Euclidean distance between them is calculated using the `math.hypot()` function. This distance serves as the input for adjusting the volume level: the closer the fingers are to each other, the lower the volume, and vice versa. This distance is mapped to the system's volume range using interpolation techniques.

3. **Volume Control**: The project uses the **Pycaw** library to control the system’s volume. Pycaw is a Python wrapper for Windows Core Audio APIs, allowing direct control of audio levels programmatically. In this project, the `AudioUtilities` class is used to get access to the system’s audio devices, and the `IAudioEndpointVolume` interface is used to set the master volume based on the distance calculated between the fingers.

### Libraries Used

1. **OpenCV**: The Open Source Computer Vision Library (OpenCV) is used for real-time computer vision tasks. In this project, OpenCV handles the video capture from the webcam and performs image processing tasks, such as drawing circles, lines, and rectangles that visualize the hand-tracking results and volume levels on the video feed.

    ```python
    cap = cv2.VideoCapture(0)  # Capturing video from the default camera
    ```

2. **Mediapipe (via `handtracking_module`)**: Mediapipe is a framework developed by Google for building multimodal (video, audio, and sensor) machine learning pipelines. For this project, Mediapipe’s hand-tracking model, accessed through the custom `handtracking_module`, is used to detect and track the user's hand in real-time. The landmarks of the hand, such as the tips of the index finger and thumb, are key to gesture recognition.

    ```python
    detector = htm.handDetector(detectionCon=0.7)  # Hand tracking with confidence of 70%
    ```

3. **Pycaw**: Pycaw (Python Core Audio Windows Library) provides programmatic access to control the volume of a system on a Windows OS. By obtaining access to the system’s speaker device through `AudioUtilities.GetSpeakers()`, the project can control the audio volume by setting the master volume level dynamically based on the user’s hand gestures.

    ```python
    devices = AudioUtilities.GetSpeakers()
    interface = devices.Activate(IAudioEndpointVolume._iid_, CLSCTX_ALL, None)
    volume = cast(interface, POINTER(IAudioEndpointVolume))
    ```

4. **NumPy**: NumPy is a library for numerical operations in Python. In this project, NumPy’s `interp()` function is used for interpolation, which is crucial for mapping the hand distance (ranging from 50 to 300 pixels) to the system’s volume range (typically from -65 dB to 0 dB).

    ```python
    vol = np.interp(length, [50, 300], [minVol, maxVol])  # Map hand distance to volume
    ```

5. **Math**: The `math.hypot()` function calculates the Euclidean distance between two points, which in this case are the coordinates of the index finger and thumb. This distance serves as the primary input for controlling the volume.

    ```python
    length = math.hypot(x2 - x1, y2 - y1)  # Distance between thumb and index finger
    ```

### Working of the Project

The webcam captures a live video feed, which is processed frame by frame to detect the user's hand. Using the Mediapipe hand-tracking model, the project identifies key hand landmarks, such as the tips of the thumb and index finger. These landmarks' positions are used to calculate the distance between the two fingers, and this distance is then interpolated to correspond to a volume level.

A key feature of this system is the smooth transition between volume levels. The `np.interp()` function ensures that even small changes in finger distance result in proportional changes to the volume, giving the user precise control over the audio levels. The Pycaw library then sets the volume in real-time based on this calculation.

Additionally, the user interface provides a visual representation of the volume bar, updating it dynamically as the hand moves. The system’s FPS (frames per second) is also displayed, ensuring that the volume control operates smoothly in real time.

### Conclusion

The **Gesture Volume Controller** project leverages advanced hand-tracking technology and integrates it with system-level audio controls, providing users with a touch-free and intuitive way to control their device's volume. It combines real-time computer vision, gesture recognition, and audio control, showcasing the potential of AI-driven interfaces for everyday applications.
