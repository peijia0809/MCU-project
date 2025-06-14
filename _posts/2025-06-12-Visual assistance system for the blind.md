---
layout: post
title: Visual assistance system for the blind
author: [Richard Kuo]
category: [Lecture]
tags: [ai]
---

Visual assistance system for the blind


---
## Visual assistance system for the blind

### EdgeAI MCU System Diagram
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/%E7%9B%B2%E4%BA%BA%E8%BC%94%E5%8A%A9.jpg?raw=true)


### Introduciton of EdgeAI MCU Application projects 
本專題以 AMB82-mini 為開發平台，整合觸控輸入、影像辨識、時間語意理解與語音辨識等功能。
系統可偵測觸控輸入後啟動影像擷取與場景分析，透過 RTC 提供時間資訊給 AI 理解場景背景，並可錄音上傳給 Gemini 進行語音轉文字，
最後透過 TTS 播放語音回饋，實現一套具備感知、語意與語音互動的智慧裝置。


### GenAI coding Design Flow
**1.初始化系統元件**

啟動 Wi-Fi、RTC、ADC（觸控）、Camera、SD 卡、TTS 播放器、麥克風。

**2.觸控偵測（ADC**

利用類比輸入讀取觸控值，啟動後續互動流程。

**3.影像分析**

拍照並上傳至 Gemini Vision，詢問場景內容（如：這是什麼地方？）

**4.時間語意理解**

取得 RTC 時間，提供給 Gemini LLM，產生與時間相關的說明文字。

**5.語音辨識與回應**

錄製使用者語音，上傳至 Gemini，取得轉換文字，再傳送給 Google TTS 播放語音。


### Prompts for Code Generation
**1.觸控功能（ADC）**

使用 AMB82-mini 撰寫程式，讀取類比觸控輸入（如光筆或手指）並觸發事件。

**2.拍照並辨識場景**

請幫我撰寫 Arduino 程式，在按鈕觸發後拍照，並將圖片傳送至 Gemini Vision，回傳場景描述文字。

**3.RTC 時間語意理解**

撰寫程式取得 RTC 時間，傳送時間字串給 Gemini LLM，請它描述這個時間可能發生的事情。

**4.語音錄音與轉文字**

請幫我用 AMB82-mini 撰寫錄音程式，錄製使用者語音並傳送給 Gemini Speech-to-Text API，取得轉文字結果。

**5.文字轉語音**

把上一段轉文字結果送到 Google TTS，生成 MP3 並播放。


### Demo and Testing

<iframe width="315" height="460" src="https://www.youtube.com/embed/GJzu59XZvNU" title="Visual assistance system for the blind-1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="315" height="460" src="https://www.youtube.com/embed/vmNmOjd-VwQ" title="Visual assistance system for the blind-2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Conclusion
本專題透過 AMB82-mini 實現結合觸控、影像辨識、語音辨識與語音回饋的多模態 AI 系統。
藉由 EdgeAI 技術，能即時感測與理解環境並回應使用者，展現 AI 在智慧互動與人機介面領域的應用潛力，適合應用於智慧玩具、語音助理與教育裝置等場景。




<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*



