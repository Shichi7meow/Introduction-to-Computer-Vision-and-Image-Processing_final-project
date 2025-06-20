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
