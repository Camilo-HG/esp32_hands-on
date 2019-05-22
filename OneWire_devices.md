# [Controlling 1-wire devices](http://docs.micropython.org/en/latest/esp32/quickref.html#onewire-driver)

The 1-wire bus is a serial bus that uses just a single wire for communication (in addition to wires for ground and power). The **DS18B20 temperature sensor** is a very popular 1-wire device.

We have connected the data line of DS18B20 temperature sensor to `pin 5` (with a .7k Ohm resistor between them). The sensor was powered by Pin which suplies 3.3V in our board.

The OneWire driver is implemented in software and works on all pins:

```python
from machine import Pin
import onewire

ow = onewire.OneWire(Pin(5)) # create a OneWire bus on GPIO 5
ow.scan()               # return a list of devices on the bus
ow.reset()              # reset the bus
ow.readbyte()           # read a byte
ow.writebyte(0x12)      # write a byte on the bus
ow.write('123')         # write bytes on the bus
ow.select_rom(b'12345678') # select a specific device by its ROM code
```

There is a specific driver for **DS18S20** and **DS18B20** devices:

```python
import time, ds18x20
ds = ds18x20.DS18X20(ow)
roms = ds.scan()
ds.convert_temp()
time.sleep_ms(750)
for rom in roms:
    print(ds.read_temp(rom))
```

Note that the `convert_temp()` method must be called each time you want to sample the temperature.

Expected output:

```python
>>> from machine import Pin
>>> import onewire
>>> ow = onewire.OneWire(Pin(5))
>>> import time, ds18x20
>>> ds = ds18x20.DS18X20(ow)
>>> roms = ds.scan()
>>> ds.convert_temp()
>>> time.sleep_ms(750)
>>> for rom in roms:
...         print(ds.read_temp(rom))
...
90.56251
>>> roms = ds.scan()
>>> ds.convert_temp()
>>> time.sleep_ms(750)
>>> for rom in roms:
...         print(ds.read_temp(rom))
...
90.0625
```

Both temperatures `90.56251` and  `90.0625` are measured in farenheit, then in celsius they correspond to `32,53473` and `32,2569`.
