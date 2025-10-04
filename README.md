# 🔌 ESP8266 MQTT Relay Control via HiveMQ Cloud

[![ESP8266](https://img.shields.io/badge/Board-ESP8266-blue)](https://www.espressif.com/en/products/socs/esp8266)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![MQTT](https://img.shields.io/badge/MQTT-HiveMQ-orange)](https://www.hivemq.com/mqtt-cloud-broker/)
[![WebSocket](https://img.shields.io/badge/WebSocket-WSS-blueviolet)](https://www.hivemq.com/blog/mqtt-over-websockets-with-hivemq-cloud/)
[![Voice Control](https://img.shields.io/badge/Voice-Enabled-success)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)

Control a relay module connected to an ESP8266 using secure **MQTT over TLS** with **HiveMQ Cloud**. This project includes:
- A built-in ESP8266 **web server**
- A **remote HTML/JS dashboard** using MQTT.js via **WebSocket**
- **Voice recognition** for hands-free control
- Buzzer feedback for user actions
- Real-time relay status monitoring

---

## 📦 Features

- 🔒 Secure MQTT with TLS (port 8883)
- 🌐 Built-in ESP Web Interface (`/` and `/toggle`)
- 📱 MQTT.js-powered HTML + JS Web Dashboard (WSS over port 8884)
- 🎤 **Voice Control** - Control relay using voice commands
- 🗣️ **Text-to-Speech Feedback** - Audio confirmation for actions
- 🎵 Beep on every toggle (Buzzer Feedback)
- 🔁 Auto reconnect on MQTT disconnection
- 📊 Session statistics (toggle count, uptime)
- 🎨 Modern gradient UI with smooth animations
- 📱 Responsive design for mobile devices

---

## 📷 Screenshots

| ESP8266 Local Web Interface | HTML + JS MQTT Dashboard |
|------------------------------------------------------------------|----------------------------------------------------------------|
| ![Local Dashboard](https://github.com/user-attachments/assets/ade27c7b-5541-43a5-93be-5965d72ecb51) | ![Web Dashboard](https://github.com/user-attachments/assets/342d6d4a-6e42-4118-bc84-e7c460b16a70) |

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
   - MQTT Broker for ESP8266: `2332bf283a3042789deec54af864c4d4.s1.eu.hivemq.cloud` (port `8883`)
   - WSS Broker for frontend: `wss://2332bf283a3042789deec54af864c4d4.s1.eu.hivemq.cloud:8884/mqtt`
6. Configure MQTT Topics:
   - Control Topic: `212`
   - Status Topic: `212/status`

</details>

---

## 🎤 Voice Commands

The web dashboard supports hands-free voice control with the following commands:

| Command | Action |
|---------|--------|
| "Turn on" / "On" | Turns relay ON |
| "Turn off" / "Off" | Turns relay OFF |
| "Check status" / "Status" | Announces current relay state |
| "Time" | Announces current time |
| "Hello" | Greets the user |
| "How are you" | Friendly response |
| "Tell me a joke" | Tells a joke |
| "About you" | Describes the app |
| "Developer" | Credits NIELIT Chandigarh |
| "Thank you" | Acknowledges appreciation |

### Using Voice Control

1. Click the **"🎤 Start Voice Recognition"** button
2. The button will turn red and pulse when active
3. Speak any supported command
4. The system will respond with voice feedback
5. Click again to stop voice recognition

**Note:** Voice recognition requires:
- Chrome, Edge, or Safari browser
- Microphone permissions
- Active internet connection

---

## 🔧 Code Overview

### Arduino (`esp8266_mqtt_relay.ino`)

- Connects to Wi-Fi and HiveMQ Cloud securely
- Subscribes to topic and listens for toggle commands
- Runs a simple web server on `/` and `/toggle`
- Publishes state when relay is toggled locally

```cpp
// Publish relay state
client.publish(mqttTopic, relayState ? "1" : "0");

// Publish to status topic
client.publish("212/status", relayState ? "1" : "0");
```

### HTML + JS Dashboard (`index.html`)

* MQTT.js client via `mqtt.min.js`
* Uses WSS connection to HiveMQ
* Web Speech API for voice recognition
* Speech Synthesis API for audio feedback
* Real-time status updates via MQTT subscription

```js
const client = mqtt.connect("wss://2332bf283a3042789deec54af864c4d4.s1.eu.hivemq.cloud:8884/mqtt", options);

// Control relay
client.publish("212", "1"); // Turn ON
client.publish("212", "0"); // Turn OFF

// Subscribe to status updates
client.subscribe("212/status");
```

---

## 📁 File Structure

```bash
.
├── esp8266_mqtt_relay.ino     # Arduino sketch
├── index.html                 # Web dashboard (HTML + MQTT.js + Voice Control)
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
3. Update Wi-Fi and MQTT credentials in the code:
   ```cpp
   const char* ssid = "YOUR_WIFI_SSID";
   const char* password = "YOUR_WIFI_PASSWORD";
   const char* mqttServer = "2332bf283a3042789deec54af864c4d4.s1.eu.hivemq.cloud";
   const char* mqttUser = "nielit212";
   const char* mqttPass = "iloveMqtt212";
   ```
4. Upload to ESP8266

### 🌍 Access Local Web Interface

* Check serial monitor for IP address
* Visit: `http://<ESP-IP>/`

### 🌐 Use Web Dashboard

* Open `index.html` in a modern browser (Chrome/Edge/Safari recommended)
* The dashboard will automatically connect to HiveMQ Cloud
* Use manual toggle button or voice commands to control the relay
* Monitor real-time status updates

</details>

---

## 🔐 Security Notes

* SSL Certificate verification is disabled on ESP8266 via `espClient.setInsecure();`
* **Important:** Change default MQTT credentials before deployment
* Use proper access controls if deploying the dashboard online
* Consider securing MQTT topics with ACLs on HiveMQ
* Voice recognition data is processed locally by the browser (not sent to external servers)

---

## 🐛 Troubleshooting

<details>
<summary>Common Issues and Solutions</summary>

### Voice Recognition Not Working
- Ensure you're using Chrome, Edge, or Safari
- Grant microphone permissions when prompted
- Check if your browser supports Web Speech API
- Make sure you have an active internet connection

### MQTT Connection Failed
- Verify HiveMQ Cloud credentials
- Check if WebSocket port 8884 is accessible
- Ensure your firewall allows WSS connections
- Verify the broker URL is correct

### Relay Not Responding
- Check ESP8266 serial monitor for errors
- Verify MQTT topic matches between ESP8266 and dashboard
- Ensure ESP8266 is connected to Wi-Fi and MQTT broker
- Check relay wiring and power supply

### Voice Commands Not Recognized
- Speak clearly and avoid background noise
- Wait for the button to turn red before speaking
- Try different phrasings of commands
- Check browser console for error messages

</details>

---

## 👨‍💻 Author

**Created by NIELIT Chandigarh**

**Lovnish Verma**  
[🌐 Portfolio](https://lovnishverma.github.io/)  
[📬 Contact on WhatsApp](https://wa.me/918894869371)

---

## 📜 License

MIT License  
Feel free to use, modify, and distribute.

---

## 💡 Future Improvements

* ✅ ~~Add voice control using Web Speech API~~ (Implemented)
* 🔄 Real-time bidirectional status sync between ESP8266 and frontend
* 🔥 Add Firebase / ThingsBoard integration
* 📊 Historical data logging and charts
* 🔔 Push notifications for relay state changes
* 🌍 Multi-language support for voice commands
* 🎨 Dark mode toggle
* 📱 Progressive Web App (PWA) support
* 🏠 Google Assistant / Alexa integration
* 🔐 Implement proper SSL certificate verification on ESP8266
* 👥 Multi-user access control

---

## 🙏 Acknowledgments

- **HiveMQ** for providing free MQTT cloud broker
- **MQTT.js** for JavaScript MQTT client library
- **ESP8266 Community** for Arduino libraries
- **Web Speech API** for voice recognition capabilities
- **NIELIT Chandigarh** for project development

---

## 📊 Project Statistics

- **Lines of Code:** ~800+
- **Technologies Used:** 6 (ESP8266, MQTT, WebSocket, HTML/CSS/JS, Voice Recognition, TTS)
- **Features Implemented:** 15+
- **Browser Compatibility:** Chrome, Edge, Safari

---

## 🔗 Related Links

- [MQTT.js Documentation](https://github.com/mqttjs/MQTT.js)
- [HiveMQ Cloud](https://www.hivemq.com/mqtt-cloud-broker/)
- [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)
- [ESP8266 Arduino Core](https://github.com/esp8266/Arduino)

---

> 🎓 "With great MQTT comes great IoT control."  
> 🎤 "With voice control comes hands-free convenience."

---

⭐ **Star this repository if you found it helpful!**
