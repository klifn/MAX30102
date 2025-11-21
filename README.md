# MAX30102 - Sample code to do a simple heart rate and O2 reading.

## This is a response to the challenge posted [here](https://simpleelectronics.ca/a-challenge-to-the-viewer-max30102/).


## This example is a modification of the Example8_SPO2.ino file

The original example code is included in the "SparkFun MAX3010x Pulse and Proximity Sensor Library."

I copied this to the main.cpp file in a PlatformIO project with the following platformio.ini config file:

```
[env:pico]
platform = https://github.com/maxgerhardt/platform-raspberrypi.git
board = pico
framework = arduino
board_build.core = earlephilhower

monitor_speed = 115200
monitor_port = /dev/cu.usbmodem214101

lib_deps=
    Adafruit SSD1306
    sparkfun/SparkFun MAX3010x Pulse and Proximity Sensor Library @ ^1.1.2
```

- The code was then updated to include displaying the resulting data on the SSD1306 display.
- That display code was taken from the origianl challenge code.

## Differences from the original challenge code

- The sensor and display were powered from the 3V3 OUT (Pin 36) on the Pico. 
- The heart rate in BPM was reporting about double what was expected, so I divided that value by 2 before displaying it.
- A single Wire (I2C) interface was used for both the SSD1306 display and the MAX30102 module. There didn't seem to be a conflict between the two.
- The original challenge code that was provided was attempting to trigger on the INT^ signal from the module.  I connected my scope to that pin and it seemed to be pinned low (active) at least on my module.  That seemed to keep it from triggering since it never saw the falling edge.  Because of this I opted to go with the polling Example8_SPO2 code.
- Since the output is a rolling average, you need to keep your finger on the sensor for a while (30 seconds or so) before the readings will stabilize.
