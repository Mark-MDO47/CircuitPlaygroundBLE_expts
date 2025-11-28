# CircuitPlaygroundBLE_expts
learning about Adafruit Circuit Playground BLE 

## Saving Data Across Reloads
Read this:
- https://learn.adafruit.com/adafruit-circuit-playground-bluefruit/circuitpython-storage

The FLASH on the Circuit Playground BLE is turned into a filesystem, mountable from a Host PC on the USB drive.

CircuitPython makes this drive writeable --EITHER-- by the Host PC --OR-- by CircuitPython programs, but not both.

They recommend having a file called boot.py which, if present, runs at boot time before code.py. A pin (board.D7 on Circuit Playground) is allocated to let the Circuit Playground know whether it will be able to write the filesystem or not, and boot.py implements that. This is apparently the only way to have storage retained across reloads.

That pin "board.D7" is the slide switch on the Circuit Playground BLE, so that is fairly practical.

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
