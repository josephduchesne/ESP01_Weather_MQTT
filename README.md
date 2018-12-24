# ESP01_Weather_MQTT
Weather station using the ESP01 and DHT11

Features:

If it can't connect to WiFi (or if you press reset twice in 3 seconds - slowly) you can connect to the ESP01's hosted wifi network and use the web interface captive portal to set wifi and MQTT configuration options.
This works very well from a phone.

Once configured, it will connect to wifi, then connect to the given MQTT server with the given username (this currently uses the user as client id as well, but that could be changed).
Once connected every 10s the temperature (°C), humidity (%), and RSSI (arbitrary units) they will be broadcast to /sensors/**MQTTUSER**/[temp,humidity,rssi].

This can be integrated into most smart home / online MQTT dashboards. For example in Home Assistant's configuration.yaml:

```
# Sensors
sensor:
  - platform: mqtt
    state_topic: /sensors/temp01/temp
    name: 'Temperature 01'
    unit_of_measurement: '°C'
  - platform: mqtt
    state_topic: /sensors/temp01/humidity
    name: 'Humidity 01'
    unit_of_measurement: '%'
```

Based on demo code from the libraries used:

- [WiFiManager](https://github.com/tzapu/WiFiManager) for WiFi/MQTT configuration management (MIT license)
- [ESP8266/Arduino](https://github.com/esp8266/Arduino) Version 2.5-beta2 or higher (GP{L v2.1)
- [ArduinoJson](https://github.com/bblanchon/ArduinoJson) (MIT License) for loading/saving preferences
- [DoubleResetDetect](https://github.com/jenscski/DoubleResetDetect) (MIT License) to allow reconfiguration on reset double-click

Hardware:
1x "ESP01S DHT11 v1.0 TB: IOTMCV" temperature/humidity breakout for the ESP01
1x ESP01 esp8266 breakout board
1x male-female 10cm ribbon cable (or the ESP heats up the DHT11 throwing readings way off)
1x 3.7-12V DC source (used an old USB cable with a dead micro-usb connector and crimped on female 2.54mm pin headers)

Arduino Board options:
- (Defaults for all else)
- Generic ESP8266 Module
- Flash Size 1M 64k SPIFFS (important, since prefs won't save correctly without SPIFFS enabled)

Programmed using an FTDI USB to serial dev board, a breadboard, some pull up resistors, and an extra 3.3V DC-DC converter to supply enough current for the ESP01 (the FTD1232 wasn't enough on its own).
