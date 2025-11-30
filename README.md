# CircuitPlaygroundBLE_expts
Learning about Adafruit Circuit Playground BLE

**Table Of Contents**
* [Top](#circuitplaygroundble_expts "Top")
* [Preparation of Circuit Playground Express Bluetooth](#preparation-of-circuit-playground-express-bluetooth "Preparation of Circuit Playground Express Bluetooth")
* [Things I had Trouble With](#things-i-had-trouble-with "Things I had Trouble With")
  * [Running out of RAM loading Sequential Images](#running-out-of-ram-loading-sequential-images "Running out of RAM loading Sequential Images")
  * [Saving Data across Reboots](#saving-data-across-reboots "Saving Data across Reboots")
  * [For now I am doing this](#for-now-i-am-doing-this "For now I am doing this")
* [References](#references "References")
  * [From Nordic](#from-nordic "From Nordic")
  * [From Adafruit](#from-adafruit "From Adafruit")
    * [Update to Latest Firmware and CircuitPython](#update-to-latest-firmware-and-circuitpython "Update to Latest Firmware and CircuitPython")
    * [Learn the basics of Circuit Playground Bluefruit](#learn-the-basics-of-circuit-playground-bluefruit "Learn the basics of Circuit Playground Bluefruit")
    * [The big cahuna - the API registry.](#the-big-cahuna-\--the-api-registry "The big cahuna - the API registry.")
  * [Non-Official but Useful](#non\-official-but-useful "Non-Official but Useful")

## Preparation of Circuit Playground Express Bluetooth
[Top](#circuitplaygroundble_expts "Top")<br>
First - go here [Update to Latest Firmware and CircuitPython](#update-to-latest-firmware-and-circuitpython "Update to Latest Firmware and CircuitPython")<br>
Next - assemble with TFT Gizmo

## Things I had Trouble With
[Top](#circuitplaygroundble_expts "Top")<br>

### Running out of RAM loading Sequential Images
[Top](#circuitplaygroundble_expts "Top")<br>
I tried multiple things to load a sequence of images without rebooting, but they all eventually got an error saying out of RAM. I did not try every possible combination of these.
- displayio.release_displays()
- display.root_group = None
- snow_bmp = None
- bg_palette = None
- bg_bitmap = None
- gc.collect()

### Saving Data across Reboots
[Top](#circuitplaygroundble_expts "Top")<br>
Since I was having trouble loading images successively without rebooting, I decided to reboot in order to load different images. I was going to save the current image across reboot so I could cycle through the images, but I have not gotten CIRCUITPY file system writes to succeed yet...
- therefore cannot save things across reboot
- therefore have not implemented the "sequential" image mode
- I guess that will be version 2

When using CircuitPython, the FLASH on the Circuit Playground BLE is turned into a filesystem, mountable from a Host PC on the USB drive. In addition to any storage your program wants to use, it stores your python code and libraries.

CircuitPython makes this drive writeable **EITHER** by the Host PC **OR** by CircuitPython programs, but not both.

Adafruit recommends having a file called boot.py which, if present, runs at boot time before code.py. A pin is allocated to let the Circuit Playground know whether it will be able to write the filesystem or not, and boot.py implements that feature by calling storage.remount(). Your code.py can also read this pin to see if it can write to the filesystem or not.
- For Circuit Playground this pin is board.D7 - a slide switch. This makes the scheme fairly practical.
- With the USB connector at the top, if the slide switch is to the right then it is Ground and str(slide_switch.value) returns "False". Obviously to the left is "True".

This page includes the code for boot.py that I will use:
- https://learn.adafruit.com/adafruit-circuit-playground-bluefruit/circuitpython-storage

Unfortunately I have not yet been able to make this work.

### For now I am doing this
[Top](#circuitplaygroundble_expts "Top")<br>
I use a random number based on the LSBs of the number of nanoseconds since power-on to choose the image to reload after power-on or reboot.

For choosing the image to load, I like this better than using randrange() as in the rest of the code. Psuedo-random number generators that are not seeded with a truly random seed have a potential problem of giving the same sequence over and over. I figured I would have a better chance at a random sequence using my nanosecond scheme.

On the other hand, randrange() is perfectly fine for controlling the many snowflakes falling through the image. There are so many of them that no one would notice if it always followed a pattern.

## References
[Top](#circuitplaygroundble_expts "Top")<br>

### From Nordic
[Top](#circuitplaygroundble_expts "Top")<br>
**nRF52840 Specs**
- https://docs.nordicsemi.com/bundle/ps_nrf52840/page/keyfeatures_html5.html
- https://docs-be.nordicsemi.com/bundle/ps_nrf52840/attach/nRF52840_PS_v1.11.pdf?_LANG=enus

### From Adafruit
[Top](#circuitplaygroundble_expts "Top")<br>

#### Circuit Playground Bluefruit - Bluetooth Low Energy
[Top](#circuitplaygroundble_expts "Top")<br>
- https://www.adafruit.com/product/4333
  - nRF52840 Cortex M4 processor with Bluetooth Low Energy
    - On-chip according to specification: 1 MBbyte FLASH and 256 kB RAM
    - When using CircuitPython, there is a 2 MByte USB filesystem FLASH disk. Maybe some other FLASH on the board.
  - https://learn.adafruit.com/adafruit-circuit-playground-bluefruit/guided-tour
  - https://learn.adafruit.com/adafruit-circuit-playground-bluefruit/pinouts

#### Circuit Playground TFT Gizmo
[Top](#circuitplaygroundble_expts "Top")<br>
- https://www.adafruit.com/product/4367

#### Update to Latest Firmware and CircuitPython
[Top](#circuitplaygroundble_expts "Top")<br>
Good idea to do this when getting a new Circuit Playground Bluefruit.
- https://learn.adafruit.com/adafruit-circuit-playground-bluefruit/downloads
- updating loader to 0.9.2 from 0.9.0
  - https://github.com/adafruit/Adafruit_nRF52_Bootloader/releases/tag/0.9.2
  - update-circuitplayground_nrf52840_bootloader-0.9.2_nosd.uf2
- Updating to CircuitPython 10.0.3
  - https://circuitpython.org/board/circuitplayground_bluefruit/
  - adafruit-circuitpython-circuitplayground_bluefruit-en_US-10.0.3.uf2

#### Learn the basics of Circuit Playground Bluefruit
[Top](#circuitplaygroundble_expts "Top")<br>
From the Adafruit Learning System
- https://learn.adafruit.com/adafruit-circuit-playground-bluefruit

#### The big cahuna - the API registry.
[Top](#circuitplaygroundble_expts "Top")<br>
It is for all Circuit Playground and includes a section on the Bluefruit model.
- https://docs.circuitpython.org/projects/circuitplayground/en/latest/api.html

### Non-Official but Useful
Quick Reference Guide - Adafruit Circuit Playground Bluefruit (from Carnegie Mellon University)
- https://courses.ideate.cmu.edu/16-376/s2022/ref/text/code/cpb-quickref.html
