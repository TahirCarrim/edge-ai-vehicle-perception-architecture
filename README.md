# Intelligent Vehicle Monitoring System: Edge Architecture Review

**Target Hardware:** NVIDIA Jetson Nano (4GB RAM).

**Context:** BEng Computer Engineering Capstone Project.

**Focus:** C++ Development, Multi-threaded Edge AI, Stereo Vision, Vehicle Parameters Recognition.

---

## 1. Project Overview
This repository documents the high-level architectural logic and design of a **First-Principled implementation** (libraries only for camera capture and hardware setup) of an Intelligent Vehicle Monitoring System. The system was engineered to perform system-based real-time vehicle profiling, which includes:
- **Vehicle detection:** Utilising motion-based detection methods.
- **Depth-Based Speed Estimation:** Using two home web cameras for stereo vision.
- **Automatic Number Plate Recognition:** Multi-stage pipeline that uses plate localisation, character extraction, and Optical Character Recognition (OCR).
- **Logo Recognition:** Custom pipeline that uses logo localisation to find the Region-of-Interest (RoI), followed by feature classification.
- **Colour Recognition:** Utilises multiple colour spaces to provide accurate colour detection regardless of lighting conditions.


> **Intellectual Property Notice:** The specific implementation, source code, and original academic documents are restricted under university IP policies. This document serves as a high-level technical overview of my engineering logic and system design decisions developed during the project.

---

## 2. System Architecture (First-Principles Design):
The system was designed with a sequential pipeline due to the limited resources on the NVIDIA Jetson Nano. The profiling algorithms were triggered after an event was detected by the motion detection algorithm, which captured the vehicle as it was in frame.

**Hardware Setup:**
- **Stereo Calibration:** Calibration calculates the intrinsic and extrinsic parameters to compensate for lens distortion and misalignment between the two cameras. This allows for rectification of the two video streams, so that the epipolar lines are aligned.
- **Camera Stability:** To ensure that there is no misalignment, the cameras were fixed onto a mounting rig and attached to a tripod. This provided a fixed baseline and allowed for the rig to be deployed in the field (on the side of roads).

**Vision Pipeline Highlights:**
- **Synchronised Acquisition:** Hardware sync trigger was not available on the cameras, so the best alternative software approach was determined and used.
- **Event-Driven Triggering:** The system utilised a lightweight motion detection system. Once a vehicle is detected, the system utilises the detected frames to run the more computationally expensive algorithms.
- **Performance Observations:** Vehicle detection achieves a consistent 15fps and vehicle speed and recognition are processed within 10s on average, determined by testing on vehicles at 20km/h to 60km/h. The processing time decreases as the vehicle speed increases, as there are fewer frames captured, thus less to be processed.
