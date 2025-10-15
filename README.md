## **🏠 Smart Home Automation Architecture Diagram**

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      HOME NETWORK (WiFi)                               │
│                                                                         │
│  ┌─────────────────┐                                                    │
│  │   ESP32 BOARD   │                                                    │
│  │                 │                                                    │
│  │  ┌───────────┐  │    ┌─────────────┐    ┌─────────────┐             │
│  │  │   WiFi    │◄─┼───►│  DHT11      │    │ 4-CHANNEL   │             │
│  │  │ Module    │  │    │ SENSOR      │    │   RELAY     │             │
│  │  └───────────┘  │    └─────────────┘    └─────────────┘             │
│  │         │        │          │                   │                   │
│  │  ┌───────────┐  │    ┌─────────────┐    ┌─────────────┐             │
│  │  │ Arduino   │  │    │ Temperature│    │    Relay 1   │───► LIGHT   │
│  │  │  Code     │  │    │ & Humidity │    │    Relay 2   │───► FAN     │
│  │  └───────────┘  │    │   Data     │    │    Relay 3   │───► TV      │
│  └─────────────────┘    └─────────────┘    │    Relay 4   │───► CHARGER │
│          ▲                                 └─────────────┘             │
│          │                                                             │
└──────────┼─────────────────────────────────────────────────────────────┘
           │
           │ Internet
           │
           ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        BLYNK CLOUD PLATFORM                            │
│                                                                         │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐     │
│  │ Authentication  │    │   Command       │    │   Real-time     │     │
│  │    Server       │    │   Processing    │    │   Data Sync     │     │
│  │                 │    │                 │    │                 │     │
│  │ - Token Verify  │    │ - Control Logic │    │ - Temp/Humidity │     │
│  │ - User Auth     │    │ - Device State  │    │ - Device Status │     │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘     │
│          ▲                                 ▲                           │
└──────────┼─────────────────────────────────┼───────────────────────────┘
           │                                 │
           │                                 │
           ▼                                 ▼
┌───────────────────┐              ┌───────────────────┐
│   MOBILE APP      │              │   WEB DASHBOARD   │
│   (Blynk IoT)     │              │                   │
│                   │              │                   │
│ ┌───────────────┐ │              │ ┌───────────────┐ │
│ │  Control      │ │              │ │  Analytics    │ │
│ │   Buttons     │ │              │ │   Dashboard   │ │
│ │               │ │              │ │               │ │
│ │ ○ Light ON/OFF│ │              │ │ 📊 Temp Graph │ │
│ │ ○ Fan ON/OFF  │ │              │ │ 📈 Hum Graph  │ │
│ │ ○ TV ON/OFF   │ │              │ │ ⚡ Power Usage │ │
│ │ ○ Charger CTL │ │              │ └───────────────┘ │
│ └───────────────┘ │              │                   │
│                   │              │                   │
│ ┌───────────────┐ │              └───────────────────┘
│ │  Sensor       │ │
│ │   Display     │ │
│ │               │ │
│ │ 🌡️ 25°C      │ │
│ │ 💧 45% RH     │ │
│ └───────────────┘ │
└───────────────────┘
```

## **🔌 ESP32 Circuit Connection Diagram:**

```
ESP32 MICROCONTROLLER
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  GPIO PINS:                                             │
│  ┌───────┬──────────┬─────────────────────────────────┐ │
│  │ PIN   │ CONNECTS TO │ FUNCTION                    │ │
│  ├───────┼──────────┼─────────────────────────────────┤ │
│  │ GPIO5 │───► Relay IN1 │ Control Light            │ │
│  │ GPIO18│───► Relay IN2 │ Control Fan              │ │
│  │ GPIO19│───► Relay IN3 │ Control TV               │ │
│  │ GPIO21│───► Relay IN4 │ Control Charger          │ │
│  │ GPIO4 │───► DHT11 Data │ Read Temp/Humidity      │ │
│  │ 3.3V  │───► DHT11 VCC  │ Power Sensor            │ │
│  │ GND   │───► DHT11 GND  │ Ground                  │ │
│  └───────┴──────────┴─────────────────────────────────┘ │
│                                                         │
└─────────────────────────────────────────────────────────┘
         │               │               │
         ▼               ▼               ▼
┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│ 4-CHANNEL   │   │   DHT11     │   │   WiFi      │
│   RELAY     │   │  SENSOR     │   │  MODULE     │
│             │   │             │   │             │
│ IN1 IN2 IN3 │   │ Temp: 25°C  │   │ Connected   │
│ IN4         │   │ Hum:  45%   │   │ to Router   │
└─────────────┘   └─────────────┘   └─────────────┘
    │  │  │  │
    ▼  ▼  ▼  ▼
   💡🎐📺🔌
```

## **🔄 Data Flow & Control System:**

```
WORKING PRINCIPLE FLOWCHART:
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   USER      │    │   BLYNK     │    │    ESP32    │    │   HOME      │
│   INPUT     │    │   CLOUD     │    │  CONTROLLER │    │  DEVICES    │
│             │    │             │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │                   │
       │ 1. Tap Button     │                   │                   │
       │    (Light ON)     │                   │                   │
       │──────────────────►│                   │                   │
       │                   │                   │                   │
       │                   │ 2. Send Command   │                   │
       │                   │    via Internet   │                   │
       │                   │──────────────────►│                   │
       │                   │                   │                   │
       │                   │                   │ 3. Process &      │
       │                   │                   │    Activate GPIO  │
       │                   │                   │──────────────────►│
       │                   │                   │                   │
       │                   │                   │                   │ 4. Relay
       │                   │                   │                   │    Switches
       │                   │                   │                   │───► 💡 ON
       │                   │                   │                   │
       │                   │ 6. Update UI      │ 5. Read Sensor    │
       │                   │    Status         │    Data           │
       │◄──────────────────│◄──────────────────│◄──────────────────│
       │                   │                   │                   │
```

## **📱 Blynk Mobile App Interface:**

```
BLYNK MOBILE APP DASHBOARD
┌─────────────────────────────────────────────────────────┐
│                    HOME AUTOMATION                      │
│                                                         │
│  ┌─────────────────┐    ┌─────────────────┐            │
│  │    CONTROL      │    │   SENSOR DATA   │            │
│  │    PANEL        │    │    DISPLAY      │            │
│  │                 │    │                 │            │
│  │  ┌───────────┐  │    │  ┌───────────┐  │            │
│  │  │   💡      │  │    │  │  🌡️ 25°C  │  │            │
│  │  │  LIGHT    │  │    │  └───────────┘  │            │
│  │  │   [ON]    │  │    │                 │            │
│  │  └───────────┘  │    │  ┌───────────┐  │            │
│  │                 │    │  │  💧 45%   │  │            │
│  │  ┌───────────┐  │    │  │ HUMIDITY  │  │            │
│  │  │   🎐      │  │    │  └───────────┘  │            │
│  │  │   FAN     │  │    └─────────────────┘            │
│  │  │   [OFF]   │  │                                  │
│  │  └───────────┘  │    ┌─────────────────┐            │
│  │                 │    │   AUTOMATION    │            │
│  │  ┌───────────┐  │    │     RULES       │            │
│  │  │   📺      │  │    │                 │            │
│  │  │    TV     │  │    │ IF TEMP > 30°C  │            │
│  │  │   [OFF]   │  │    │ THEN FAN → ON   │            │
│  │  └───────────┘  │    │                 │            │
│  │                 │    └─────────────────┘            │
│  │  ┌───────────┐  │                                  │
│  │  │   🔌      │  │                                  │
│  │  │ CHARGER   │  │                                  │
│  │  │   [ON]    │  │                                  │
│  │  └───────────┘  │                                  │
│  └─────────────────┘                                  │
└─────────────────────────────────────────────────────────┘
```

## **⚡ Power Management System:**

```
POWER DISTRIBUTION:
┌─────────────────────────────────────────────────────────┐
│                     POWER SUPPLY                       │
│                    (5V DC Adapter)                     │
└─────────────────────────┬───────────────────────────────┘
                          │
              ┌───────────┼───────────┐
              ▼           ▼           ▼
      ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
      │    ESP32    │ │   RELAY     │ │   DHT11     │
      │             │ │   MODULE    │ │   SENSOR    │
      │  3.3V Reg  │ │             │ │             │
      │  Internal   │ │  5V Input   │ │  3.3V Input │
      └─────────────┘ └─────────────┘ └─────────────┘
              │               │
              │               ▼
              │       ┌─────────────┐
              │       │  HOUSEHOLD  │
              │       │  APPLIANCES │
              │       │             │
              │       │  💡 🎐 📺 🔌 │
              │       └─────────────┘
              │
              ▼
      ┌─────────────┐
      │   WiFi      │
      │ CONNECTION  │
      └─────────────┘
```

## **🔧 System Specifications:**

```
TECHNICAL SPECIFICATIONS:
┌─────────────────┬─────────────────────────────────────────┐
│ Component       │ Specification                          │
├─────────────────┼─────────────────────────────────────────┤
│ Microcontroller │ ESP32 (Dual-core, 240MHz)             │
│ Wireless        │ WiFi 802.11 b/g/n                     │
│ Range           │ Up to 100m (indoors)                  │
│ Power           │ 5V DC, 2A Adapter                     │
│ Relay Rating    │ 10A @ 250V AC, 10A @ 30V DC           │
│ Sensor Range    │ Temp: 0-50°C, Humidity: 20-90% RH     │
│ Response Time   │ < 2 seconds                            │
│ Mobile App      │ Blynk IoT (Android/iOS)               │
└─────────────────┴─────────────────────────────────────────┘
```
