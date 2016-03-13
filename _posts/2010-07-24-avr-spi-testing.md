---
title: Mini High Speed Spindle
date: 2010-05-01
categories: [programming]
tags:
  - avr
  - spi
---
This is just a quick post on the problems I am having trying to initialize an [25AA1024 eeprom](http://ww1.microchip.com/downloads/en/DeviceDoc/21836B.pdf) in SPI mode with my [AT90USBKey](http://www.atmel.com/dyn/products/tools_card.asp?tool_id=3879). In the picture you can see the entire sequence:

![Massive_screenshot_small](/images/avr-spi-testing/massive_screenshot_small-scaled10001.jpg)


````
WREN: [0x06]  
Send write(2), address(0 0 0), data(1 2 3 4 5)
  [0x02 0x00 0x00 0x00 0x01 0x02 0x03 0x04 0x05]  
Send read(3), address(0 0 0). dummy(0xFF)
  [0x03 0x00 0x00 0x00 0xFF 0xFF 0xFF 0xFF 0xFF]  
````

On the input pin one would expect to receive `0x01 0x02 0x03 0x04 0x05` but I receive `0xFF` no matter what I do or write. The commands above in the '[]'s are commands from my [Bus Pirate](http://dangerousprototypes.com/docs/Bus_Pirate) which can read and write to the eeprom without any issues. I have both the AVR and the BP connected at the same time and yet the AVR cannot read or write whatsoever.  

I tried pullups internal and external with no use.This is an ongoing problem as I tried every SPI device I own and the AVR cannot communicate with any of them, I tried switching the `MOSI` and `MISO` lines and no effect either. You can see the data is setup on falling SCK and read on rising SCK without issues (the waveform is actually cleaner on the AVR than the BP)  
