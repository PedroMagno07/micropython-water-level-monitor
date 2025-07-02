# ESP32 Dam Alert System

An IoT-based system designed to monitor a dam's water level and local humidity, issuing tiered alerts via LEDs, a buzzer, and MQTT messages based on predefined risk levels. This project was developed using MicroPython on an ESP32 board.

![Circuit Diagram](img/circuit-diagram.png)

## Features

-   **Real-time Monitoring:** Continuously measures water level using an HC-SR04 ultrasonic sensor and humidity with a DHT22 sensor.
-   **Tiered Alert System:** Provides visual and audible alerts based on risk severity.
-   **MQTT Integration:** Allows for remote control and monitoring of the system.
-   **Live Simulation:** Fully interactive and runnable online via Wokwi.

## Hardware Components

-   ESP32 Development Board
-   HC-SR04 Ultrasonic Sensor
-   DHT22 Temperature and Humidity Sensor
-   Piezo Buzzer
-   LEDs (1x Red, 1x Yellow, 1x Green) & Resistors

---

## Live Demo & How to Run

This project can be tested live without needing any physical hardware, using the Wokwi online simulator.

1.  **Click on the link below** to open the project in Wokwi:
    -   **[Run the ESP32 Dam Alert System Simulation](https://wokwi.com/projects/433233310642725889)**

2.  Wait for the project to load, then press the **green "play" button** to start the simulation.

### Interacting with the Simulation

-   **To Change Water Level:** Click on the **ultrasonic sensor (HC-SR04)** and use the slider to change the distance. A smaller distance simulates a higher water level.
-   **To Change Humidity:** Click on the **DHT22 sensor** and use the slider to adjust the humidity level.
-   **To Send Remote Commands:** Use any MQTT client (like MQTTX) connected to `broker.mqttdashboard.com` to send commands to the `barragem/comandos` topic (e.g., `modo_manual`, `liga_verde`).

---

## How It Works

### Automatic Mode
In automatic mode, the ESP32 reads data from the sensors and evaluates the risk level to trigger the appropriate alert.

-   **Normal:** `distance > 100cm` and `humidity < 70%` -> Green LED on.
-   **High Humidity:** `humidity > 70%` -> Yellow LED blinks with a sound alert.
-   **High Water Level:** `distance < 100cm` -> Red LED blinks with a sound alert.
-   **Critical Danger:** `distance < 100cm` and `humidity > 70%` -> Red and Yellow LEDs blink with a high-intensity alarm.

### MQTT Communication
The system uses MQTT to publish real-time data and receive commands.

-   **Published Topics:** `barragem/distancia`, `barragem/umidade`, `barragem/status`.
-   **Subscribed Topic:** `barragem/comandos`.
