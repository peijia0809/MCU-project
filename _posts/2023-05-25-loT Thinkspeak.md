---
layout: post
title: Innovative Project Proposal
author: [PEI JIA HUNG]
category: [Lecture]
tags: [jekyll, ai]
---

這次的作業是用ESP32接上溫度濕度感測器，再藉由手機熱點上傳至thinkspeak顯示數據

---

**Contents:**<br>
* **應用與功能說明**
  - 利用溫度濕度感測器偵測數據，並且將數據顯示在thinkspeak的網站上
* **設計考量與所需相關技術**
  - 需要連接手機熱點wifi，才能將數據上傳到thinkspeak
* **系統方塊圖**
  - Draw a System Block Diagram

---
## loT ThinkSpeak

### 應用功能說明
利用溫度感測器將溫度濕度數據傳送到ThinkSpeak的網站上
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/346170580_183638597624377_3858830159712200539_n.png?raw=true)

### 設計考量與相關技術
**系統設計考量：**<br>
硬體元件：準備一個溫度感測器，以及ESP32
程式碼軟件：Arduino
編寫代碼：使用Arduino語言，並將其發送到ThingSpeak的網站上

### 系統方塊圖
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/thinkspeak.png?raw=true)                     


### code程式碼
#include <WiFi.h>
#include "DHT.h"

#define DHTPIN 19     // NodeMCU pin D6 connected to DHT11 pin Data
DHT dht(DHTPIN, DHT11, 15);

const char* ssid     = "pj";
const char* password = "oojj12345";


const char* host = "api.thingspeak.com";
const char* thingspeak_key = "F0HOM6A0TEJTZ5TG";

void turnOff(int pin) {
  pinMode(pin, OUTPUT);
  digitalWrite(pin, 1);
}

void setup() {
  turnOff(2);
  turnOff(4);
  turnOff(5);
  turnOff(12);
  turnOff(13);
  turnOff(14);
  turnOff(15);

  Serial.begin(115200);

  // disable all output to save power
  turnOff(0);
  dht.begin();
  delay(10);
  

  // We start by connecting to a WiFi network

  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
  
  WiFi.begin(ssid, password);
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected");  
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

int value = 0;

void loop() {
  delay(5000);
  ++value;

  Serial.print("connecting to ");
  Serial.println(host);
  
  // Use WiFiClient class to create TCP connections
  WiFiClient client;
  const int httpPort = 80;
  if (!client.connect(host, httpPort)) {
    Serial.println("connection failed");
    return;
  }

  String temp = String(dht.readTemperature());
  String humidity = String(dht.readHumidity());
  //String voltage = String(system_get_free_heap_size());
  String url = "/update?key=";
  url += thingspeak_key;
  url += "&field1=";
  url += temp;
  url += "&field2=";
  url += humidity;
  
  Serial.print("Requesting URL: ");
  Serial.println(url);
  
  // This will send the request to the server
  client.print(String("GET ") + url + " HTTP/1.1\r\n" +
               "Host: " + host + "\r\n" + 
               "Connection: close\r\n\r\n");
  delay(10);
  
  // Read all the lines of the reply from server and print them to Serial
  while(client.available()){
    String line = client.readStringUntil('\r');
    Serial.print(line);
  }
  
  Serial.println();
  Serial.println("closing connection. going to sleep...");
  delay(1000);
  // go to deepsleep for 1 minutes
  //system_deep_sleep_set_option(0);
  //system_deep_sleep(1 * 60 * 1000000);
  delay(1*10*1000);
}

                                                                                                                                                             
                                                                                         
                                                                                       
                                                                                         
<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*


