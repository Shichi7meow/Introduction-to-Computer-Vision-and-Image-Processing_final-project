# Introduction-to-Computer-Vision-and-Image-Processing_final-project
Real-Time Building Detection and Memory Overlay Using Haar Cascade Classifier

# Real-Time Building Detection and Memory Overlay  
**Final Project Report for YZU 113-2 EEB215A**  
**Course:** Introduction to Computer Vision and Image Processing  
**Student:** Chen, Chien-Yi (1100336)

---

## 🔍 Abstract
The objective of this project is to develop a Haar cascade classifier for detecting a specific building—called the "Seventh Building"—using computer vision techniques. By collecting image data, annotating positive and negative samples, training a Haar cascade classifier, and implementing a real-time detection system using a camera, we aim to accurately identify the building in live video streams. The project also explores the practical challenges and limitations of Haar-based object detection.

**Keywords:** Haar, YZU_building, OpenCV, webcam video detect

---

## 🧩 Introduction
(簡要介紹專案背景與動機，例如你來校園迷路的經驗、資料收集流程的概觀...)

---

## 🛠️ System Design
**A. Sample Generation**
- Collected positive samples (Seventh Building) from multiple angles and lighting conditions.
- Collected negative samples (other buildings and unrelated scenes).
- Resized all images to ~500×500 or 640×480 for consistency.

**B. Annotation**
- Used **LabelImg** to annotate bounding boxes, converted XML to positive.txt for Haar training.

**C. `.vec` File Creation**
- Used `opencv_createsamples` to generate `positive.vec`:
  ```bash
  opencv_createsamples -info positive.txt -num 200 -w 24 -h 24 -vec positive.vec
D. Training the Classifier

Trained using opencv_traincascade:

bash
複製程式碼
opencv_traincascade \
  -data classifier_yzu1 \
  -vec positives.vec \
  -bg YZU_First_building_negative.txt \
  -numPos 90 -numNeg 200 \
  -numStages 10 -w 24 -h 24
Highlighted that this requires OpenCV 3.4.16, as newer versions lack createsamples/traincascade tools.

Outputs include cascade.xml and stage files.

E. Real-Time Detection

Implemented in Seven_building_detect_with_Camera.py, using camera input (USB webcam or DroidCam).

Detects only the Seventh Building and overlays a personal message when detected.

🎥 Demo & Code
Video Demonstration: [YouTube Link]
GitHub Repository: https://github.com/Shichi7meow/Introduction-to-Computer-Vision-and-Image-Processing_final-project

🧠 Discussion
Strengths: Successful under controlled conditions (good lighting, clear framing).

Challenges: poor lighting, occlusion, angle variance, resizing distortion, false positives.

Solutions: increase data size/diversity, higher training resolution, adopt deep-learning models (YOLO, SSD), multi-building detection.

📝 Conclusion
Classical Haar-based building detection is feasible but limited in variability handling and robustness. Future improvements should focus on more varied data, modern models, and multi-object support.
