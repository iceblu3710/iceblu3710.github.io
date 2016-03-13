---
title: Hacking a Linksys WMCE54AG
date: 2011-07-01T00:00:00.000Z
categories: []
tags:
  - teardown
  - electronics
  - hacking
---
  ![Wmce54ag](/images/hacking-a-linksys-wmce54ag/wmce54ag-scaled500.jpg)
---

So I was parusing kijiji looking for free printers to steal parts from when I happened upon an add for a Linksys Media Center Extender (WMCE54AG). This device was originally created to route audio and video from your XP Media Center to your TV so that you did not need a computer in your living room. I remember when these first came out I saw them at memoryexpress.com from a chunk of change. Well I scored this one in working condition minus the remote for $10 !!!

As with most project I was to into it and forgot pictures of the unit before it was stripped. Here are some I stole from the internets:

[![Caseoff](/images/hacking-a-linksys-wmce54ag/caseoff-scaled1000.jpg?w=300)](/images/hacking-a-linksys-wmce54ag/caseoff-scaled1000.jpg)
[![Chips](/images/hacking-a-linksys-wmce54ag/chips-scaled1000.jpg?w=300)](/images/hacking-a-linksys-wmce54ag/chips-scaled1000.jpg)

(The picture with the chips circled is a handy reference from [here](http://www.dd-wrt.com/phpBB2/viewtopic.php?p=387187))

Main processor under the heatsink is a ATI Xilleon 225

1. [ATI Xilleon 225 ](http://www.mips.com/products/cores/hard-ip-cores/4kc-hard-ip-core/)  
215H25AGA13  
GC7312.1  
0433SS

2. [Ethernet Controler, PCI interface](http://www.realtek.com.tw/products/productsView.aspx?Langid=1&PFid=6&Level=5&Conn=4&ProdID=15)  
RTL8101L  
48298Q1  

3. [Serial EEPROM from MAC storage](http://www.atmel.com/dyn/resources/prod_documents/doc0172.pdf)  
AT93C46  

4. Mini-PCI A/G WiFi card  
Atheros  
AR5213A-00  
A20254C  
2304

5. [128Mb SDRAM for the BlackFin DSP](http://www.alldatasheet.com/datasheet-pdf/pdf/101123/SAMSUNG/K4S281632F-TC75.html)  
SAMSUNG 440  
K4S281632F-TC75

6. [600MHz BlackFin DSP](http://www.analog.com/en/embedded-processing-dsp/blackfin/ADSP-BF533/processors/product.html) ($28 for one of these babbies in TQFP)  
ANALOG DEVICES  
ADSP-BF533  
SKBCE10  
559595.1 0.3  
0445 SINGAPORE  BLACK

7. [128Mb (8M x 16) GDDR SDRAM](http://www.alldatasheet.com/datasheet-pdf/pdf/105585/HYNIX/HY5DU281622ET-5.html)  
HYNIX 430P  
HY5DU281622ET-5

8. 16M x 8 bit NAND flash memory  
SAMSUNG 446  
K9F2808U0C  
YCB0  
UPH737DCC

9. [2ch 1% regulation LDO](http://www.linear.com/product/ltc1628)  
LT 0433  
LTC1628CG  
N27185

10. [4Mb Flash](http://www.alldatasheet.com/datasheet-pdf/pdf/74500/MCNIX/MX29LV040QC-70.html)  
MXJ043504  
29LV040QC-70  
1F9588

9. [8-bit AVR, maybe IR controler?](http://www.google.ca/url?sa=t&source=web&cd=1&ved=0CB8QFjAA&url=http%3A%2F%2Fwww.chipcatalog.com%2FAtmel%2FAT90SC3232CS.htm&rct=j&q=at90sc3232cs&ei=DDuVTfuEIcfw0gGX1fHkCw&usg=AFQjCNFFnTPfGm4_PdlyoDd4F8rUXkki7w&sig2=OqwqJGTzG_n1BOEdVbSpeg&cad=rja)  
ATMEL  
AT90SC3232CS  
0443  
4S5691

---

Another fun chip is on the back of the front controls. Its a [16bit I2C port expander from NXP](http://www.nxp.com/pip/PCA9555.html) It runs the front controls and the tri color LED ring on the navigational pad. I had some fun with blinking lights before continuing.

So I set out looking at the board and discovered some things. Their is a MFG/WDT jumper on the board, no idea what the WDT jumper does but I presume its something to do with the watchdog timer. Maybe during chip flashing they need to halt the Xillions WDT so it wont interfear. In the one corner I found a "Debug Port" with an IC not in place. Their where only two wires so I immediatly thought serial port and was right. I soldered in a header after carefully drilling the via's out to accept a standard sized headder. The copper jumpers across the pins there is because I do not need to use the level shifting IC that they would have put on. My BusPirate runs at 3V3/5V0 so I just bridged the pins and hooked her up.  

[![Tools](/images/hacking-a-linksys-wmce54ag/tools-scaled1000.jpg?w=300)](/images/hacking-a-linksys-wmce54ag/tools-scaled1000.jpg)  
[![Debug_port](/images/hacking-a-linksys-wmce54ag/debug_port-scaled1000.jpg?w=300)](/images/hacking-a-linksys-wmce54ag/debug_port-scaled1000.jpg)  
[![Mobo](/images/hacking-a-linksys-wmce54ag/mobo-scaled1000.jpg?w=300)](/images/hacking-a-linksys-wmce54ag/mobo-scaled1000.jpg)  

115200bps, 8N1 seemed like a good place to start and out poped the boot sequence! Initialy it loads the "bios" or "IPL" that inits the hardware and discovers boot devices which are the Flash and NAND Flash then asks for a command. Without interfearence it will boot the WindowsCE kernel and start up a bunch of tasks like re-mapping the address space to a virtual one then unloads the debug shell. I somewhat remember from my WinCE v5 licence days that the IDE had some shell debugging apps that would hook the kernel to keep the debug ports open but I will need to dig up my CD's before I can be sure. For now I will just poke around.

So far I have leared this; The MFG jumper puts the unit into infinate self check and typing "y" at the ++CMD prompt will dump you to a PROM console those who have ran SGI/SUN machines may remember. Below are some test files with the output of a few boots.  

---

### Boot Info

[Boot](/images/hacking-a-linksys-wmce54ag/Boot.txt)  
[SelfTest](/images/hacking-a-linksys-wmce54ag/SelfTest.txt)
