# [MicroPython](http://micropython.org/)

From documentation:

> **MicroPython** is a lean and efficient implementation of the **Python 3** programming language that includes a small subset of the **Python standard library** and is optimised to run on microcontrollers and in constrained environments.

> **MicroPython** is packed full of advanced features such as an interactive prompt, arbitrary precision integers, closures, list comprehension, generators, exception handling and more. Yet **it is compact enough to fit and run within just 256k of code space and 16k of RAM**.
**MicroPython** aims to be as compatible with normal Python as possible to **allow you to transfer code** with ease **from the desktop to a microcontroller** or embedded system.

> **MicroPython is a full Python compiler and runtime** that runs on the bare-metal. You get an **interactive prompt (the REPL)** to execute commands immediately, along with the ability **to run and import scripts from the built-in filesystem**. The REPL has history, tab completion, auto-indent and paste mode for a great user experience.

## MicroPython for ESP32

Take a look on [Quick reference for the ESP32](http://docs.micropython.org/en/latest/esp32/quickref.html).

We are gonna follow [Getting started with MicroPython on the ESP32](http://docs.micropython.org/en/latest/esp32/tutorial/intro.html)

Download the [firmware for the ESP32](https://micropython.org/download#esp32).

#### First: erase your device with [esptool.py](https://github.com/espressif/esptool/)

As `sudo`, run:

```
# pip install esptool
```

Then

```
# esptool.py --chip esp32 --port /dev/ttyUSB0 erase_flash
```

And finally

```
# esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 460800 write_flash -z 0x1000 esp32-version.bin
```

Expected output:

```
esptool.py v2.5.0
Serial port /dev/ttyUSB0
Connecting......
Chip is ESP32D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse
MAC: 30:ae:a4:c1:2b:94
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Configuring flash size...
Auto-detected Flash size: 4MB
Compressed 1549888 bytes to 970594...
Wrote 1549888 bytes (970594 compressed) at 0x00001000 in 22.9 seconds (effective 541.2 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
```

## Getting a MicroPython REPL prompt

**REPL** stands for **Read Evaluate Print Loop**, and is the name given to the **interactive MicroPython prompt** that you can access on the **ESP8266** and **ESP32**. Using the **REPL** is the easiest way to test out the code and run commands. Since both, **ESP8266** and **ESP32**, are two **Espressif chips** that are very similar when it comes to using **MicroPython** on them, one must follow the [ESP8266 tutorial - Getting a MicroPython REPL prompt](http://docs.micropython.org/en/latest/esp8266/esp8266/tutorial/repl.html)

The REPL is always available on the UART0 serial peripheral, which is connected to the pins GPIO1 for TX and GPIO3 for RX. The baudrate of the REPL is 115200. If your board has a USB-serial convertor on it then you should be able to access the REPL directly from your PC. Otherwise you will need to have a way of communicating with the UART.

To access the prompt over USB-serial you need to use a terminal emulator program. First, install **picocom:**

```
# apt install picocom
```

Then, run:

```
# picocom /dev/ttyUSB0 -b115200
```

Expected output looks like:

```
picocom v3.1

port is        : /dev/ttyUSB0
flowcontrol    : none
baudrate is    : 115200
parity is      : none
databits are   : 8
stopbits are   : 1
escape is      : C-a
local echo is  : no
noinit is      : no
noreset is     : no
hangup is      : no
nolock is      : no
send_cmd is    : sz -vv
receive_cmd is : rz -vv -E
imap is        :
omap is        :
emap is        : crcrlf,delbs,
logfile is     : none
initstring     : none
exit_after is  : not set
exit is        : no

Type [C-a] [C-h] to see available commands
Terminal ready
```

Then press `Enter` a few times. You should see the **Python REPL prompt**, indicated by `>>>`. **Then you can work!!**

But be careful. Let's say you want to *turn on* the `LED_BUILTIN` attached in your **ESP32**. In the root web page [MicroPython](http://micropython.org/), you will see next example you can run in **REPL prompt**:

```python
import pyb

# turn on an LED
pyb.LED(1).on()

# print some text to the serial console
print('Hello MicroPython!')
```

But when you try:

```python
>>> import pyb
```

Output is:

```python
>>> import pyb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: no module named 'pyb'
```

This happens because the `pyb module` exists for **PyBoard** in the **MicroPython** family of ports. So, the **MicroPython compiled for ESP32** does not have a `pyb module` and you must use the `machine module` instead:

```python
>>> import machine
>>> pin = machine.Pin(2, machine.Pin.OUT)
>>> pin.on()
>>> pin.off()
```

In **LOLIN32 lite**, the `LED_BUILTIN` is on `pin 22`, then last code must change to:

```python
>>> import machine
>>> pin = machine.Pin(22, machine.Pin.OUT)
>>> pin.on()
>>> pin.off()
```

Note that `on` method of a Pin **might** turn the LED **off** and `off` might turn it **on** (or vice versa), depending on how the LED is wired on your board. To resolve this, `machine.Signal class` is provided.

## [class Signal – control and sense external I/O devices](https://docs.micropython.org/en/latest/library/machine.Signal.html)

...

## WiFi

After a fresh install and boot the device configures itself as a **WiFi access point (AP)** that you can connect to. The **ESSID** is of the form **MicroPython-xxxxxx** where the **x’s** are replaced with part of the **MAC (Media Access Control) address** of your device (so will be the same everytime, and most likely different for all **ESP8266** chips). The `password` for the **WiFi** is `micropythoN` (note the upper-case N). Its IP address will be `192.168.4.1` once you connect to its network.
