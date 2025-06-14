---
layout: post
title: Emotion-sensing music player
author: [Richard Kuo]
category: [Lecture]
tags: [ai]
---

Emotion-sensing music player


---
## Emotion-sensing music player

### EdgeAI MCU System Diagram
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/%E6%83%85%E7%B7%92%E6%84%9F%E7%9F%A5.jpg?raw=true)


### Introduciton of EdgeAI MCU Application projects 
本專題以 AMB82-mini 為平台，建構一套具備情緒辨識與音樂推薦功能的 EdgeAI 系統。
透過鏡頭擷取影像，使用 Gemini Vision 分析使用者的情緒，再由 Gemini LLM 根據 SD 卡中既有的音樂檔名，推薦符合情緒的音樂，並播放對應 MP3。
實現即時互動、在地運算的智慧情境娛樂應用。


### GenAI coding Design Flow
**1.初始化模組**

初始化 Wi-Fi、SD 卡、攝影機與 MP3 播放器模組。

擷取影像

按下按鈕或定時擷取影像，儲存為 JPG 格式。

**2.情緒辨識**

將影像傳送至 Gemini Vision 分析表情，取得情緒類別（如開心、生氣、悲傷等）。

**3.歌曲推薦**

提供 SD 卡中既有音樂檔名清單給 Gemini LLM，請其根據情緒推薦最適合的歌曲名稱。

**4.MP3 播放**

根據 Gemini 回傳的歌曲名稱，從 SD 卡播放對應的 MP3 音樂檔。

### Prompts for Code Generation
Prompt:
請判斷這張圖片代表的情緒，如果是喜輸出APT, 怒輸出AstroBunny, 哀輸出gTTS, 樂輸出IBelieve&quot


### Demo and Testing
喜:
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/cdv_photo_17479310501(1)(1).jpg?raw=true)
怒:
<iframe width="315" height="460" src="https://www.youtube.com/embed/LZqWnEe04Is" title="怒" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
哀:<iframe width="315" height="460" src="https://www.youtube.com/embed/0xQjPVIYdNM" title="哀" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
樂:
<iframe width="315" height="460" src="https://www.youtube.com/embed/3SlPfRxvroo" title="樂" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


### Conclusion
本系統成功結合 EdgeAI 與多媒體應用，利用情緒辨識與語意理解技術達到情緒導向的音樂推薦功能。
透過 AMB82-mini 執行即時分析與本地音樂播放，實現低延遲、高互動性的智慧娛樂裝置，展現 AIoT 在日常生活中的應用潛力。


### Code
/*
    Emotion-to-MP3 Player with Gemini Vision
    功能：
    - 拍照分析情緒（喜怒哀樂）
    - 根據情緒播放對應 MP3 檔案（需存於 /mp3/ 資料夾中）
    - 顯示在 LCD 螢幕上
*/
String openAI_key = &quot;&quot;;  // 如果有用 openAI 可放 key，但此程式未用
String Gemini_key = &quot;AIzaSyAak7JerV3mz1e2h6XZmFSyKeJvw-5TALA&quot;;
String Llama_key = &quot;&quot;;
char wifi_ssid[] = &quot;Xiaomi 12X&quot;;
char wifi_pass[] = &quot;0902367789&quot;;
#include &lt;WiFi.h&gt;
#include &lt;WiFiUdp.h&gt;
#include &quot;GenAI.h&quot;
#include &quot;VideoStream.h&quot;
#include &quot;SPI.h&quot;
#include &quot;AmebaILI9341.h&quot;
#include &quot;TJpg_Decoder.h&quot;
#include &quot;AmebaFatFS.h&quot;
WiFiSSLClient client;
GenAI llm;
AmebaFatFS fs;
String mp3BasePath = &quot;/mp3/&quot;;
VideoSetting config(768, 768, CAM_FPS, VIDEO_JPEG, 1);
#define CHANNEL 0
uint32_t img_addr = 0;
uint32_t img_len = 0;
const int buttonPin = 1;
String prompt_msg = &quot;請判斷這張圖片代表的情緒，如果是喜輸出APT, 怒輸出
AstroBunny, 哀輸出gTTS, 樂輸出IBelieve&quot;;
#define TFT_RESET 5
#define TFT_DC    4
#define TFT_CS    SPI_SS
AmebaILI9341 tft = AmebaILI9341(TFT_CS, TFT_DC, TFT_RESET);
#define ILI9341_SPI_FREQUENCY 20000000

bool tft_output(int16_t x, int16_t y, uint16_t w, uint16_t h, uint16_t
*bitmap) {
    tft.drawBitmap(x, y, w, h, bitmap);
    return 1;
}
void initWiFi() {
    for (int i = 0; i &lt; 2; i++) {
        WiFi.begin(wifi_ssid, wifi_pass);
        delay(1000);
        Serial.print(&quot;Connecting to &quot;);
        Serial.println(wifi_ssid);
        uint32_t StartTime = millis();
        while (WiFi.status() != WL_CONNECTED) {
            delay(500);
            if ((StartTime + 5000) &lt; millis()) {
                break;
            }
        }
        if (WiFi.status() == WL_CONNECTED) {
            Serial.println(&quot;STAIP address: &quot;);
            Serial.println(WiFi.localIP());
            break;
        }
    }
}
void init_tft() {
    tft.begin();
    tft.setRotation(2);
    tft.clr();
    tft.setCursor(0, 0);
    tft.setForeground(ILI9341_GREEN);
    tft.setFontSize(2);
    tft.println(&quot;GenAIVision_TTS_LCD&quot;);
}
void setup() {
    Serial.begin(115200);
    SPI.setDefaultFrequency(ILI9341_SPI_FREQUENCY);
    initWiFi();
    config.setRotation(0);
    Camera.configVideoChannel(CHANNEL, config);
    Camera.videoInit();

    Camera.channelBegin(CHANNEL);
    Camera.printInfo();
    pinMode(buttonPin, INPUT);
    pinMode(LED_B, OUTPUT);
    init_tft();
    TJpgDec.setJpgScale(2);
    TJpgDec.setCallback(tft_output);
}
void loop() {
    tft.setCursor(0, 1);
    tft.println(&quot;press button to capture image&quot;);
    if ((digitalRead(buttonPin)) == 1) {
        tft.println(&quot;Capture Image&quot;);
        for (int count = 0; count &lt; 3; count++) {
            digitalWrite(LED_B, HIGH);
            delay(500);
            digitalWrite(LED_B, LOW);
            delay(500);
        }
        // 拍照
        Camera.getImage(0, &amp;img_addr, &amp;img_len);
        // 顯示照片
        TJpgDec.getJpgSize(0, 0, (uint8_t *)img_addr, img_len);
        TJpgDec.drawJpg(0, 0, (uint8_t *)img_addr, img_len);
        // 圖像辨識
        String text = llm.geminivision(Gemini_key, &quot;gemini-2.0-flash&quot;,
prompt_msg, img_addr, img_len, client);
        Serial.print(&quot;Gemini 回傳：&quot;);
        Serial.println(text);
        // 顯示結果
        tft.clr();
        tft.setCursor(0, 0);
        tft.setForeground(ILI9341_CYAN);
        tft.println(&quot;Playing Music:&quot;);
        tft.setForeground(ILI9341_WHITE);
        tft.println(text);
        // 撥放對應音樂

        String musicFile = mp3BasePath + text + &quot;.mp3&quot;;
        sdPlayMP3(musicFile);
    }
}
void sdPlayMP3(String filename) {
    fs.begin();
    String filepath = String(fs.getRootPath()) + filename;
    if (!fs.exists(filename)) {
        Serial.println(&quot;MP3 file not found: &quot; + filepath);
        tft.setForeground(ILI9341_RED);
        tft.println(&quot;MP3 not found!&quot;);
        fs.end();
        return;
    }
    File file = fs.open(filepath, MP3);
    file.setMp3DigitalVol(175);
    file.playMp3();
    file.close();
    fs.end();
}


<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*



