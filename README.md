# CircuitPlaygroundBLE_expts
learning about Adafruit Circuit Playground BLE 

## Saving Data Across Reloads
The nRF52840 Cortex M4 processor has 2 MB of SPI Flash storage.

When using CircuitPython, the FLASH on the Circuit Playground BLE is turned into a filesystem, mountable from a Host PC on the USB drive. In addition to any storage your program wants to use, it stores your python code and libraries.

CircuitPython makes this drive writeable **EITHER** by the Host PC **OR** by CircuitPython programs, but not both.

Adafruit recommends having a file called boot.py which, if present, runs at boot time before code.py. A pin is allocated to let the Circuit Playground know whether it will be able to write the filesystem or not, and boot.py implements that feature by calling storage.remount(). Your code.py can also read this pin to see if it can write to the filesystem or not.
- For Circuit Playground this pin is board.D7 - a slide switch. This makes the scheme fairly practical.
- With the USB connector at the top, if the slide switch is to the right then it is Ground and str(slide_switch.value) returns "False". Obviously to the left is "True".

This page includes the code for boot.py that I will use:
- https://learn.adafruit.com/adafruit-circuit-playground-bluefruit/circuitpython-storage

## References
- https://www.adafruit.com/product/4333
  - https://learn.adafruit.com/adafruit-circuit-playground-bluefruit/downloads
  - nRF52840 Cortex M4 processor with Bluetooth Low Energy
  - updating loader to 0.9.2 from 0.9.0
    - https://github.com/adafruit/Adafruit_nRF52_Bootloader/releases/tag/0.9.2
    - update-circuitplayground_nrf52840_bootloader-0.9.2_nosd.uf2
  - Updating to CircuitPython 10.0.3
    - https://circuitpython.org/board/circuitplayground_bluefruit/
    - adafruit-circuitpython-circuitplayground_bluefruit-en_US-10.0.3.uf2
- https://learn.adafruit.com/adafruit-circuit-playground-bluefruit
- https://docs.circuitpython.org/projects/circuitplayground/en/latest/api.html
