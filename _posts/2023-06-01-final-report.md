---
layout: post
title: PID遙控車車
author: [PEI JIA HUNG]
category: [Lecture]
tags: [jekyll, ai]
---

This project is to implement a bluetooth remote controlled robotcar.

---
## PID遙控小車
![](https://github.com/rkuo2023/MCU-project/blob/main/images/ESP32_RoboCar.jpg?raw=true)


### 應用功能說明
1. WIFI remote control App 
2. two-wheel robocar



### 設計考量與相關技術
**系統設計考量：**<br>
1. 操作方式:藍芽遙控
2. 移動方式:兩輪 
3. 供電方式:鋰電池 3.7V x2
4. 聯網方式:藍芽

**所需相關技術：**
1. Arduino IDE 2.1.0
2. Arduino程式設計

**所需相關套件:**
![](https://image.ruten.com.tw/g2/8/d4/16/21440347657238_872.jpg)

### 系統方塊圖
![](https://github.com/peijia0809/MCU-project/blob/main/_posts/%E9%9B%BB%E5%8B%95%E8%BB%8A%E8%BB%8A.png?raw=true)

### 成果展示

Forward:
<iframe width="329" height="586" src="https://www.youtube.com/embed/cEgKL3nPrZs" title="forward" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Bakward:
<iframe width="329" height="586" src="https://www.youtube.com/embed/D0CN3ujb_G0" title="backward" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### coding

#include <WiFi.h>
#include "DHT.h"

#define DHTPIN 19     // NodeMCU pin D6 connected to DHT11 pin Data
DHT dht(DHTPIN, DHT11, 15);

const char* ssid     = "PinLe的iPhone";
const char* password = "00001111";


const char* host = "api.thingspeak.com";
const char* thingspeak_key = "6BQ13YRF3BJ97VZR";

void turnOff(int pin) {
  pinMode(pin, OUTPUT);
  digitalWrite(pin, 1);
}

void setup() {
  Serial.begin(115200);

  // disable all output to save power
  turnOff(0);
  turnOff(2);
  turnOff(4);
  turnOff(5);
  turnOff(12);
  turnOff(13);
  turnOff(14);
  turnOff(15);

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
  delay(1*60*1000);
}

***

<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*


