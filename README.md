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

# 🔧 System Setup & Workflow
Real-Time Building Detection Using Haar Cascade Classifier

# 🖥️ 1. Environment & Tools
工具	 :說明
Python :3.x	用於整個專案開發

OpenCV :3.4.16 (with CLI tools)	用於 opencv_createsamples 和 opencv_traincascade

LabelImg:	用於手動標註正樣本 bounding boxes

DroidCam / USB Webcam:	測試階段所用的即時攝影機輸入

Windows 10 / Ubuntu (可選):	系統執行環境皆可

IDE / 編輯器:	建議使用 PyCharm、VS Code

# 2. Data Preparation
(1) 正樣本收集

拍攝目標建築「元智大學第七棟」的多張照片。

照片需涵蓋 不同角度、時間與光照。

Resize 所有圖片至標準尺寸（如 500x500 或 640x480）。

(2) 負樣本收集

拍攝校園其他建築或隨機背景圖。

不含第七棟建築物。

用於產生 negative.txt。

(3) 標註正樣本
使用 LabelImg 對每張正樣本圖片畫出建築物的 bounding box。

轉換產生 positive.txt（OpenCV 格式），格式如下：

pgsql
複製程式碼
path/to/image.jpg 1 x y w h

# 3. 資料檔案生成
(1) 建立 .vec 檔
使用以下指令產生 .vec：

bash
複製程式碼
opencv_createsamples -info positive.txt -num 200 -w 24 -h 24 -vec positives.vec
注意 positive.txt 格式與最後一行不要有錯（否則會產生 parse error）。

# 4. 模型訓練 (Haar Classifier)
使用 opencv_traincascade 指令訓練模型：

bash
複製程式碼
opencv_traincascade \
  -data classifier_yzu1 \
  -vec positives.vec \
  -bg negative.txt \
  -numPos 90 -numNeg 200 \
  -numStages 10 -w 24 -h 24
輸出目錄（如 classifier_yzu1/）將包含：

cascade.xml → 最終模型（用於實際偵測）

stage0.xml~stage9.xml → 每階段中間模型（除錯用）

# 5. 實時偵測應用
執行 Seven_building_detect_with_Camera.py

載入 cascade.xml 進行建築物辨識

當辨識出第七棟時，於畫面上顯示你對該建築的個人回憶文字

支援裝置：

筆電內建攝影機

USB 外接攝影機

手機相機 (經由 DroidCam)

# 6. 測試與評估
在不同環境（光線、角度、遮蔽情況）下測試效果

紀錄成功與失敗案例

評估準確率與即時反應效果

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
# D. Training the Classifier

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

# E. Real-Time Detection

Implemented in Seven_building_detect_with_Camera.py, using camera input (USB webcam or DroidCam).

Detects only the Seventh Building and overlays a personal message when detected.

# Demo & Code
Video Demonstration: [[YouTube Link](https://youtu.be/ZjWS8C7co9k)]
GitHub Repository: https://github.com/Shichi7meow/Introduction-to-Computer-Vision-and-Image-Processing_final-project

# Discussion
Strengths: Successful under controlled conditions (good lighting, clear framing).

Challenges: poor lighting, occlusion, angle variance, resizing distortion, false positives.

Solutions: increase data size/diversity, higher training resolution, adopt deep-learning models (YOLO, SSD), multi-building detection.

#  Conclusion
Classical Haar-based building detection is feasible but limited in variability handling and robustness. Future improvements should focus on more varied data, modern models, and multi-object support.
