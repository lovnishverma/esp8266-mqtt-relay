# 🔌 ESP8266 MQTT Relay Control via HiveMQ Cloud

[![ESP8266](https://img.shields.io/badge/Board-ESP8266-blue)](https://www.espressif.com/en/products/socs/esp8266)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![MQTT](https://img.shields.io/badge/MQTT-HiveMQ-orange)](https://www.hivemq.com/mqtt-cloud-broker/)
[![WebSocket](https://img.shields.io/badge/WebSocket-WSS-blueviolet)](https://www.hivemq.com/blog/mqtt-over-websockets-with-hivemq-cloud/)

Control a relay module connected to an ESP8266 using secure **MQTT over TLS** with **HiveMQ Cloud**. This project also includes:
- A built-in ESP8266 **web server**
- A **remote HTML/JS dashboard** using MQTT.js via **WebSocket**
- Buzzer feedback for user actions

---

## 📦 Features

- 🔒 Secure MQTT with TLS (port 8883)
- 🌐 Built-in ESP Web Interface (`/` and `/toggle`)
- 📱 MQTT.js-powered HTML + JS Web Dashboard (WSS over port 8884)
- 🎵 Beep on every toggle (Buzzer Feedback)
- 🔁 Auto reconnect on MQTT disconnection

---

## 📷 Screenshots

| ESP8266 Local Web Interface | HTML + JS MQTT Dashboard        |
|-----------------------------|---------------------------------|
| <img width="469" height="337" alt="image" src="https://github.com/user-attachments/assets/ade27c7b-5541-43a5-93be-5965d72ecb51" />
           | ![Web Dashboard](https://github.com/user-attachments/assets/04ce9176-eb2e-4a38-a297-87d658c7f6f6) |

---

## 🛠️ Hardware Required

| Component        | Quantity | Description           |
|------------------|----------|-----------------------|
| ESP8266 NodeMCU  | 1        | Wi-Fi microcontroller |
| Relay Module     | 1        | For controlling loads |
| Buzzer           | 1        | Feedback on toggle    |
| Jumper Wires     | —        | For wiring            |

### ⚙️ ESP8266 Pin Mapping

| Device   | GPIO | NodeMCU Pin |
|----------|------|-------------|
| Relay    | 4    | D2          |
| Buzzer   | 5    | D1          |

---

## 🌐 HiveMQ Cloud Setup

<details>
<summary>Steps to Configure HiveMQ Cloud Broker</summary>

1. Sign up at 👉 [HiveMQ Cloud](https://www.hivemq.com/mqtt-cloud-broker/)
2. Create a **free cluster**
3. Enable **WebSocket (TLS)**
4. Create **credentials (username & password)**
5. Use:
   - MQTT Broker for ESP8266: `mqtt.example.s1.eu.hivemq.cloud` (port `8883`)
   - WSS Broker for frontend: `wss://mqtt.example.s1.eu.hivemq.cloud:8884/mqtt`

</details>

---

## 🔧 Code Overview

### Arduino (`esp8266_mqtt_relay.ino`)

- Connects to Wi-Fi and HiveMQ Cloud securely
- Subscribes to topic and listens for toggle commands
- Runs a simple web server on `/` and `/toggle`
- Publishes state when relay is toggled locally

```cpp
client.publish(mqttTopic, relayState ? "0" : "1");
````

### HTML + JS Dashboard (`index.html`)

* MQTT.js client via `mqtt.min.js`
* Uses WSS connection to HiveMQ
* Simple toggle button and relay state display

```js
const client = mqtt.connect("wss://mqtt.example.s1.eu.hivemq.cloud:8884/mqtt", options);
client.publish("212", "0"); // Turn ON
client.publish("212", "1"); // Turn OFF
```

---

## 📁 File Structure

```bash
.
├── esp8266_mqtt_relay.ino     # Arduino sketch
├── index.html                 # Web dashboard (HTML + MQTT.js)
└── README.md                  # GitHub documentation
```

---

## 🚀 Getting Started

<details>
<summary>🧠 Step-by-Step Instructions</summary>

### ✅ Arduino Setup

1. Open the sketch in Arduino IDE
2. Install required libraries:

   * ESP8266WiFi
   * WiFiClientSecure
   * PubSubClient
   * ESP8266WebServer
   * ESP8266mDNS
3. Update Wi-Fi and MQTT credentials
4. Upload to ESP8266

### 🌍 Access Local Web Interface

* Check serial monitor for IP address
* Visit: `http://<ESP-IP>/`

### 🌐 Use Web Dashboard

* Open `index.html` in browser
* Relay toggles via WSS MQTT messages

</details>

---

## 🔐 Security Notes

* SSL Certificate verification is disabled on ESP8266 via `espClient.setInsecure();`
* Use proper access controls if deploying the dashboard online
* Consider securing MQTT topics with ACLs on HiveMQ

---

## 👨‍💻 Author

**Lovnish Verma**
[🌐 Portfolio](https://lovnishverma.github.io/)
[📬 Contact on WhatsApp](https://wa.me/918894869371)

---

## 📜 License

MIT License
Feel free to use, modify, and distribute.

---

## 💡 Future Improvements

* Use real-time status sync from ESP8266 to frontend
* Add Firebase / ThingsBoard integration
* Enable voice control using Google Assistant or Alexa

---

> 🎓 “With great MQTT comes great IoT control.”

---

