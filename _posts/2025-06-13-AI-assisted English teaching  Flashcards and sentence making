---
layout: post
title: AI-assisted English teaching ~ Flashcards and sentence making
author: [Richard Kuo]
category: [Lecture]
tags: [ai]
---

This project combines AI and imaging to create AI-assisted English teaching ~ Flashcards and sentence making.

---
## AI-assisted English teaching ~ Flashcards and sentence making

### EdgeAI MCU System Diagram
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/%E8%AE%80%E5%8D%A1.jpg?raw=true)


### Introduciton of EdgeAI MCU Application projects 
1.Press button to capture an image
2.Send Image to Gemini-Vision to read the word card
3.Send Text1 to Google-TTS and play mp3 file to speak 
4.Send Text1 to Gemini-LLM to make a sentence
5.Send Text2 to Google-TTS and play mp3 file to speak 


### GenAI coding Design Flow
**系統初始化**

初始化 Wi-Fi、RTC、TFT 螢幕與 SD 卡

載入必要函式庫與 API Key（Gemini、Google-TTS）

**按鈕觸發事件**

偵測按鈕按下（digitalRead）

執行拍照並儲存為 JPG 圖檔

**Gemini Vision 辨識**

上傳圖片至 Gemini Vision

取得文字結果（Text1），如單字卡上的單字

Google TTS 播放 Text1

傳送 Text1 給 Google TTS

**播放語音（MP3）**

Gemini LLM 生成例句

將 Text1 傳送至 Gemini LLM

**取得生成句子（Text2）**

Google TTS 播放 Text2

傳送 Text2 給 Google TTS

播放語音（MP3）

**顯示與記錄**

在 ILI9341 螢幕顯示 Text1 與 Text2

選擇性儲存圖片與文字至 SD 卡


### Prompts for Code Generation
提⽰詞：請協助我撰寫一份專題報告，主題為「AI-assisted Educational System」，內容包含以下重點：

硬體平台：使用 AMB82-mini（MCU: Realtek RTL8735B）

系統功能：

按下按鈕拍照

將影像傳送至 Gemini Vision 辨識單字卡，取得 Text1

將 Text1 傳送至 Google TTS 播放語音

將 Text1 傳送至 Gemini LLM 生成例句 Text2

再將 Text2 傳送至 Google TTS 播放語音

程式參考範例：GenAIVision.ino、TextToSpeech.ino、ILI9341_TFTLCD_Text.ino

測試項目：執行 GenAIVision_TTS_Text_ReadWordCard.ino 完成語音測試

samplecode:1.GenAIVision_TTS.ino
2.GenAIVision_TTS_Text_ReadWordCard.ino


### Demo and Testing
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/cdv_photo_17461116411.jpg?raw=true)
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/cdv_photo_17461116412.jpg?raw=true)

### Conclusion
本專題以 AMB82-mini 為核心，結合 Gemini Vision 與 Google TTS 實現智慧學習系統。透過按鍵拍照辨識單字卡，再由語音合成朗讀，並延伸生成例句與語音播報。系統整合 GenAIVision 與 TTS 技術，提升互動式學習效果，達成教育輔助自動化目標。





<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*



