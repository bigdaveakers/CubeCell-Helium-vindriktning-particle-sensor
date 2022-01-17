<p align="center"><h2>Helium LoRaWAN connectivity for the Ikea VINDRIKTNING</h2></p>


This repository contains an ESP8266 firmware, which adds MQTT to the Ikea VINDRIKTNING PM2.5 air quality sensor.
The modification  doesn't interfere with normal operation of the device in any way.
The ESP8266 just adds another data sink beside the colored LEDs.

![half_assembled](./img/half-assembled.jpg)

Home Assistant Autodiscovery is supported.
Furthermore, the WifiManager library is used for on-the-fly configuration.
Also, ArduinoOTA is used, so that firmware updates are possible even with a reassembled device.

As the ESP8266 is 5V-tolerant, this should there shouldn't be any issues, however I haven't had time to test this for longer periods of time.
Therefore, if the ESP burns out after a while, just add a voltage divider or something.

## Prerequisites

To extend your air quality sensor, you will need

- A LoRaWAN-enabled dev board, in this case the Heltec Cubecell htcc-ab01
- Some short dupont cables
- A soldering iron
- A long PH0 Screwdriver (e.g. Wera 118022)

Fortunately, there is a lot of unused space in the enclosure, which is perfect for our ESP8266.
Also, everything we need is accessible via easy to solder testpoints.

## Hardware

To install the ESP8266, you need to unscrew the four visible screws in the back of the enclosure.

Then, there are also three screws holding the tiny PCB in place. These aren't necessary to remove since you can solder
in-place, however personally, I'd recommend taking the board out of there since it will be easier to solder without fear
of accidentally melting some plastic.

![board](./img/board.jpg)

As you can see in this image, you'll need to solder wires to GND, 5V and the Testpoint that is connected to TX of the
Particle Sensor.

Then just connect these Wires to GND, VIN (5V) and Pin 2 on the Cubecell

Done.

## Software

The firmware can be built and flashed using the Arduino IDE.

For this, you will need to add ESP8266 support to it by [using the Boards Manager](https://github.com/esp8266/Arduino#installing-with-boards-manager).

Furthermore, you will also need to install the following libraries using the Library Manager:

* ArduinoOTA 1.0.3
* ArduinoJSON 6.10.1
* PubSubClient 2.8.0
* WiFiManager 0.15.0


Just build, flash, and you're done.

When connecting everything up, you should see an open Wi-Fi Access Point to configure your Wi-Fi and MQTT credentials.

## Misc

The VINDRIKTNING consists of a custom(?) Cubic PM1006-like Sensor + another uC that does all that LED stuff, which talk
via UART. The uC simply regularly polls the sensor and displays the results.

Therefore, to add LoRaWAN connectivity, we just need to also listen to the TX of the Sensor and decode those messages.
The Ikea uC will do all that polling stuff for us.

As reported in #16, the transitions from Green to Yellow and Yellow to Red in the Ikea firmware are at around 30 and 100μg/m³.

## ToDo

- [ ] Downlink config
- [ ] Docs


## References and sources

- [Source Repo](https://github.com/Hypfer/esp8266-vindriktning-particle-sensor)
- [@haxfleisch](https://twitter.com/haxfleisch) for their teardown of the device.
- [Gabriel Valky](https://github.com/gabonator) for the incredibly useful [LA104 custom firmware + tools](https://github.com/gabonator/LA104)
