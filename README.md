# ESP32 hands-on
Hands on **ESP32**

## Board used

The board used in this project was [Wemos LOLIN32 Lite](https://wiki.wemos.cc/products:lolin32:lolin32_lite).

The **LOLIN32 Lite** is a low-budget **ESP32** board that has a 32-bit dual core microcontroller that runs at 240MHz, has 4MB of flash storage, and can be programmed in a range of languages, including **MicroPython**, **LUA** and **Arduino**. An important feature is that Linux will detect the board out of the box.

### Power suply

The **LOLIN32 Lite** can be powered via **micro USB** or **lithium polymer batteries** with a **500mA max charging current**.

### Documentation:

See the [schematic](https://wiki.wemos.cc/_media/products:lolin32:sch_lolin32_lite_v1.0.0.pdf)

### Technical specs

| Microcontroller   |	ESP-32   |
| ----------------- | -------- |
| Operating Voltage	| 3.3 V    |
| Digital I/O Pins	| 19       |
| Analog Input Pins	| 6        |
| Clock Speed(Max)	| 240 Mhz  |
| Flash	            | 4M bytes |
| Length	          | 5 cm     |
| Width	            | 2.54 cm  |
| Weight	          | 4 g      |

## Language used: MicroPython

Take a look on `MicroPython.md` to see process followed.

Once the board was connected to serial port, I followed instructions in `serial_port.md` to catch device's port info.
