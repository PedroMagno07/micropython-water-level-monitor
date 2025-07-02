# ESP32 Dam Alert System

An IoT-based system designed to monitor a dam's water level and local humidity, issuing tiered alerts via LEDs, a buzzer, and MQTT messages based on predefined risk levels. This project was developed using MicroPython on an ESP32 board.

![Circuit Diagram](img/circuit-diagram.png)
## Features

-   **Real-time Monitoring:** Continuously measures water level using an HC-SR04 ultrasonic sensor and humidity with a DHT22 sensor.
-   **Tiered Alert System:** Provides visual and audible alerts based on risk severity:
    -   **Green LED:** Normal conditions.
    -   **Yellow LED & Buzzer:** High humidity (potential for rain).
    * **Red LED & Buzzer:** Critical water level.
    * **Red + Yellow LEDs & High-Intensity Buzzer:** Critical water level and high humidity (imminent flood risk).
-   **MQTT Integration:** Publishes sensor data to a public broker and subscribes to a command topic, allowing for remote control and monitoring.
-   **Remote Control:** Supports switching between `automatic` and `manual` modes via MQTT commands, allowing direct control over LEDs and the buzzer.

## Hardware Components

-   ESP32 Development Board
-   HC-SR04 Ultrasonic Sensor (for water level)
-   DHT22 Temperature and Humidity Sensor
-   Piezo Buzzer
-   LEDs (1x Red, 1x Yellow, 1x Green)
-   Resistors (3x 220Ω or similar)
-   Breadboard and Jumper Wires

## Software & Setup

### Firmware
The project runs on **MicroPython** for the ESP32.

### Libraries
-   `dht`: For the humidity sensor.
-   `umqtt.simple`: For MQTT communication.

### Configuration
Before running, you need to configure the following variables inside `main.py`:

-   **Wi-Fi Credentials:**
    ```python
    sta_if.connect('YOUR_WIFI_SSID', 'YOUR_WIFI_PASSWORD')
    ```
-   **MQTT Broker:** The project is pre-configured to use `broker.mqttdashboard.com`.
    ```python
    MQTT_BROKER = "broker.mqttdashboard.com"
    ```

## How It Works

### Automatic Mode
In automatic mode, the ESP32 reads data from the ultrasonic and humidity sensors every few seconds. Based on the values, it determines the current risk level and triggers the appropriate alert.

-   **Normal:** `distancia > 100` and `umid < 70` -> Green LED on.
-   **High Humidity:** `umid > 70` -> Yellow LED blinks with a sound alert.
-   **High Water Level:** `distancia < 100` -> Red LED blinks with a sound alert.
-   **Critical Danger:** `distancia < 100` and `umid > 70` -> Red and Yellow LEDs blink with a high-intensity alarm.

### MQTT Communication
The system uses MQTT to publish real-time data and receive commands.

-   **Published Topics:**
    -   `barragem/distancia`: Current water distance in cm.
    -   `barragem/umidade`: Current humidity in %.
    -   `barragem/status`: A human-readable status message (e.g., "✅ Tudo normal", "🚨 Barragem está alagando!").
-   **Subscribed Topic:**
    -   `barragem/comandos`: Listens for commands to control the system remotely.
-   **Example Commands:** `liga_verde`, `desliga_verde`, `liga_buzzer`, `modo_manual`, `modo_automatico`.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
