# 👁️ Visual Aid for Visually Impaired


## 📖 Overview
**Vision Crafters** is a mobile application that empowers visually impaired individuals with real-time environmental awareness. Using the device's camera and **Gemini API**, the app converts visual input into detailed audio descriptions and provides hazard alerts. It features gesture controls, conversational assistance, and stores interaction logs locally using **SQLite**.

🎥 **[Project Overview Video](https://youtu.be/lGWhG0DqqO4)** – See how Vision Crafters works, its key features, and how it benefits visually impaired users.  

---

## 🧩 Key Features
- **Scene Understanding & Conversational Assistance**: Describes images/videos and answers environment-related questions using the **Gemini API**.
- **Text-to-Speech & Speech-to-Text**: Allows audio-based interactions for feedback and queries.
- **Gesture Controls**: Capture media and interact with the app without needing to press specific buttons.
- **Hazard Detection**: Alerts users to potential dangers in their environment.
- **Local Log Storage**: Saves interaction logs locally in **SQLite** for offline review.

---

## 🎯 Target Users & Benefits

**For visually impaired individuals:**
- Understand surroundings through voice feedback.
- Navigate safely with real-time hazard alerts.
- Interact with the app more easily using gesture-based controls.


**For caregivers:**
- Review stored interaction logs for safety and activity insights.

---

## 🔄 Workflow Summary

1. Capture images or videos via the camera or gestures.
2. Process media through **Gemini API** to describe the scene and detect hazards.
3. Play audio feedback using TTS.
4. Handle user queries related to the environment.
5. Raise alarm if hazardous elements are detected.
6. Save logs locally using **SQLite** for offline access.

<div align="center">
    <img width="800" alt="Diagram" src="https://github.com/user-attachments/assets/550efafe-6016-46f2-b985-e131a824615f" />
</div>

---

## 🛠️ Tech Stack

- **Frontend**: Flutter (built with Flutter 3.21)
- **Backend**: Firebase Cloud Functions (Python) for Gemini API calls
- **AI API**: Vertex AI - Gemini API (via GCP)
- **Local Storage**: SQLite database

---

## ⚙️ Prerequisites

- [Flutter](https://docs.flutter.dev/get-started/install) (built with 3.21)
- [Python 3.11](https://www.python.org/downloads/)
- [Google Cloud CLI](https://cloud.google.com/sdk/docs/install)
- [Firebase CLI](https://firebase.google.com/docs/cli)
- [FlutterFire CLI](https://firebase.google.com/docs/flutter/setup?platform=android#install-cli-tools)
- [GCP](https://cloud.google.com) project with Vertex AI API & billing enabled  
> ⚠️ Vertex AI is a paid service
- [Firebase](https://firebase.google.com/) project (for local Firebase Cloud Functions)

---

## 🚀 Setup Instructions

### 1️⃣ Flutter App Setup

```bash
git clone https://github.com/vision-crafters/VisionCrafters-G38.git
cd VisionCrafters-G38/Flutter
flutter pub get
flutterfire configure
```

- Create a `.env` file in `/Flutter` directory:
```env
HOST=<your-ip-address>
```
> Use the IPv4 address of the machine running Firebase Emulators.

### 2️⃣ Backend Setup (Firebase Cloud Functions)

```bash
cd ../Backend/functions
python -m venv venv
source venv/bin/activate  # or .\venv\Scripts\activate on Windows
pip install -r requirements.txt
```

- Add your Gemini API key:
```env
GOOGLE_API_KEY=<your-api-key>
```
in `Backend/functions/.env`.

---

## ▶️ Running the Project

### Start Firebase Emulators:
```bash
cd ../
firebase emulators:start
```

### Build & run the app:
```bash
cd ../Flutter
flutter run
```

> ⚠️ **Use a physical Android device** for video capture testing.

---

## 🧠 Experimental Approaches

In addition to the primary **Gemini API** implementation, we explored the following experimental features for future development:

- **Local Vision-Language Model (VLM)**: A **Flask server** is used to serve a locally hosted Vision-Language Model. The model processes video input by extracting keyframes using **ffprobe** (part of **ffmpeg**). This approach does not perform as well as the Gemini API, especially for video understanding.

- **Retrieval-Augmented Generation (RAG) Pipeline**: We implemented a **RAG pipeline** where keywords extracted from images and user query are used to search for relevant information on **Wikipedia**. The search results are then embedded and stored in Pinecone, allowing for real-time retrieval and use in generating responses. The pipeline has challenges with response time and scalability, making it unsuitable for use in its current form.

For the **local VLM** setup (experimental):
- Ensure **ffmpeg/ffprobe** is installed on your machine for video processing.
- Set up the Flask server inside a Python virtual environment to serve the local model.
- Add the following keys to the `.env` file in the `Backend/functions` directory:
  ```env
  HUGGINGFACE_INFERENCE_API_KEY=<your-api-key>  # For embedding model
  PINECONE_API_KEY=<your-api-key>   # For RAG pipeline
  API=<server-url>  # For communicating with Flask server
  ```
