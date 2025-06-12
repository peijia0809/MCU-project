---
layout: post
title: AI-assisted recycling sorting system
author: [Richard Kuo]
category: [Lecture]
tags: [ai]
---

Combining AI and imaging functions to assist recycling classification system.

---
## AI-assisted recycling sorting system

### EdgeAI MCU System Diagram
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/%E5%9B%9E%E6%94%B6.png?raw=true)


### Introduciton of EdgeAI MCU Application projects 
EdgeAI MCU應用項目利用嵌入式處理器和人工智慧技術，將AI計算能力移至終端設備，以實現低延遲、高效能的智能處理。這些項目通常搭載具備高效能處理能力的微控制器（MCU），
如AMB82-mini，並結合各種感測器與通信模組，能實現即時數據處理、影像識別、語音處理等應用。EdgeAI MCU解決方案可應用於智能家居、工業自動化、健康監測等領域，具有節能、低成本、及高安全性的優勢。

### GenAI coding Design Flow

**1. System Initialization**
Setup Hardware:

Initialize the AMB82-mini development board with the Realtek RTL8735B MCU.

Configure ILI9341 TFT LCD for visual feedback.

Initialize camera module to capture images on button press.

Set up Google-TTS API for converting text into speech.

**2. Image Capture & Processing (GenAIVision.ino)**
Capture Image:

Wait for button press signal.

Capture the image using the camera module.

Store the image on the local storage (SD card or internal memory).

Send Image to Google-Gemini:

Send the captured image to Google-Gemini for analysis.

Gemini AI processes the image and generates a response based on its contents (e.g., recyclability of the item).

Receive Response from Gemini:

Parse the response from Gemini.

Extract relevant information (e.g., type of recyclable material).

Prepare a text response for the user.

**3. Text-to-Speech Conversion (GenAIVision_TTS.ino)**
Send Response to Google-TTS:

Send the text response (recyclability information) to Google-TTS API.

Ensure the message is not too long (split the text if necessary) to avoid MP3 generation issues.

Generate MP3 and Play Audio:

Google-TTS returns an MP3 file.

Use a speaker or audio output module to play the MP3 file.

**4. Display Information on LCD (GenAIVision_TTS_LCD.ino)**
Display Image on TFT LCD:

Show the captured image on the ILI9341 TFT display.

Provide a visual confirmation that the image has been processed.

Display Text on LCD:

Display the text response (e.g., recyclability status) on the screen for additional user feedback.

**5. Test and Validation**
Run GenAIVision_TTS.ino:

Capture recyclable images to test if the system correctly identifies and responds with the proper text-to-speech output.

Run GenAIVision_TTS_LCD.ino:

Conduct full system tests, including the LCD screen display and audio feedback, ensuring both are synchronized and functional.

**6. Final System Output:**
User Interaction:

User presses the button to capture an image.

System provides both an audible response (via TTS) and visual feedback (via LCD) about the recyclability of the item.

Efficiency and Optimization:

Ensure smooth operation with error handling in case of network failures or long response times.

Optimize the TTS and image recognition functions for faster and more accurate real-time responses.

### Prompts for Code Generation
提⽰詞：請根據圖⽚分辨此回收物是否壓扁


### Demo and Testing
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/cdv_photo_17455058341.jpg?raw=true)
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/cdv_photo_17455058342.jpg?raw=true)

### Conclusion
本專案「AI-assisted Recycle System」成功實現了基於 AMB82-mini 開發板的智能回收系統。系統可通過按鈕捕捉圖片，並將圖片傳送至 Google-Gemini 進行分析，生成回應信息。
該信息會透過 Google-TTS 轉換為語音並播放，同時顯示在 TFT LCD 屏幕上。經過測試，系統能準確識別回收物品並提供語音與視覺反饋，未來可進一步優化回應準確性與用戶互動體驗。





<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*

