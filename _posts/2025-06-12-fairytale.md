---
layout: post
title: fairytale
author: [Richard Kuo]
category: [Lecture]
tags: [ai]
---

This project combines AI and imaging to create fairy tales.

---
## Fairytale

### EdgeAI MCU System Diagram
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/fairytale%E7%B3%BB%E7%B5%B1%E6%96%B9%E5%A1%8A%E5%9C%96.png?raw=true)


### Introduciton of EdgeAI MCU Application projects 
1.Press button to capture an image
2.Send Image to Gemini-Vision and ask AI to tell a fairytale 
3.Send Text1 to Google-TTS and play mp3 file to speak 


### GenAI coding Design Flow
1.按下按鈕觸發
2.攝像頭捕捉圖像
3.圖像發送到 Gemini Vision API
4.AI 生成故事文本
5.文本分割成短句（重要：解決 TTS 限制）
6.通過 Google TTS 生成 MP3
7.播放童話故事

### Prompts for Code Generation
提⽰詞：請根據圖⽚編出⼤約30字的童話故事
samplecode:GenAIVision_TTS_TFT.ino


### Demo and Testing
<iframe width="482" height="857" src="https://www.youtube.com/embed/mqmCTe4TfB4" title="fairytale" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Conclusion
本專案「Fairytale Teller」利用 AMB82-mini 開發板及 Realtek RTL8735B MCU，成功實現了按下按鈕後捕捉圖片，並將圖片傳送至 Gemini-Vision 進行AI故事生成。AI返回的故事文本經過拆分成多個短句後，再將這些短句發送至 Google-TTS 進行語音合成，避免了長字串生成 MP3 文件的問題。整體系統運行順暢，能夠有效提升語音播放的流暢性與準確性，達到了預期的功能目標，未來可進一步優化故事生成與語音播放的同步效果。





<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*



