#### Robotron EC1834 POST 60h ISA card

In this project I would like to introduce you to an ISA Port 60h card for the EC1834.

This Project was inspired by:
https://bbright.tripod.com/information/postcard.htm

The part list differs from that 




Original text from that site:

Build your own P.O.S.T. card 

P.O.S.T. Diagnostics Card  
  
January 1998  
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

        Whenever a computer boots up, the BIOS does a self-check of the system to verify that all components of the system are operating correctly. For instance, the BIOS will check that the memory is functional, that the disk drives are working, that the video card is in place, and many other things. As the BIOS works its way through the list of things that need to be checked, it sends out 'messages' which indicate the status of the testing process. These 'messages' are sent out to I/O port 80H. This is where the P.O.S.T. card comes in. It plugs into the ISA expansion bus of the PC and monitors all data sent to port 80H. Each 'message' is a single byte. The P.O.S.T. card reads this byte off the bus and displays it on its 7-segment displays. In this manner, it is possible to observe which test the computer is performing. This is very useful as a diagnostics tool for a non-functional computer because the code associated with the last attempted test is left on the P.O.S.T. card display even after the computer locks up.

        For example, a computer may quickly check its memory, disk drives, etc. These codes will flash by fairly quickly on the P.O.S.T. display if these systems are functional. Let's say there is a problem with the video system, however. The computer will hang up here, but it will already have sent out the numerical code indicating that the video test is about to be performed. When the computer hangs up, you simply need to look at the P.O.S.T. card to see at what point the BIOS tests failed. In this case, the code will indicate the video test. Now you can replace the video card. The point is, the P.O.S.T. card can tell you where the problem lies. It narrows down the search for faulty components of your computer system.  
  
        There are several major BIOS manufacturers, and each has its own unique P.O.S.T. codes. The same P.O.S.T. card can be used for different BIOS types; the error codes simply mean different things. For example: If you have AMI (American Megatrends) BIOS, a P.O.S.T. code of 12 means that the 64K of base memory passed its test. However, a P.O.S.T. code of 12 on a computer equipped with Quadtel BIOS means that the 8237 DMA Controller test is being performed. All you need to do is refer to the P.O.S.T. code list for the BIOS which you have.  
  
        An excellent source for P.O.S.T. code lists is the FAQ for the comp.sys.ibm.pc.hardware newsgroup. This FAQ is divided into 5 parts. All 5 parts have useful information, but Part 4 contains the P.O.S.T. codes for several different BIOS types. The FAQ is available at

[ftp://rtfm.mit.edu/pub/usenet/news.answers/pc-hardware-faq/](ftp://rtfm.mit.edu/pub/usenet/news.answers/pc-hardware-faq/)

* * *

* * *

  
**Construction:  
  
**        Parts List:  
  

Part

Qty.

7404 Hex Inverter

2

7430 8-input NAND

1

7432 Quad OR

1

74374 Octal latch

1

2732 EPROM

2

Common Anode 7-Seg. Display

2

**[Click here to view schematic for this circuit](post.gif)**

  
        The P.O.S.T. card can be constructed fairly easily. For the design in this document, an EPROM programmer must be available. EPROMs are used as combinational logic to go from a 4-bit binary number to the pins on a 7-segment display. If you do not have an EPROM programmer, check this web site, as there will eventually be information about building your own programmer very inexpensively.  
        The P.O.S.T. card hardware is composed of basically three sections:  

1.  Address and control signal decoding  
    
2.  Data register  
    
3.  Binary-to-7-segment conversion and display  
    

                The address and control signal decoding is accomplished with IC1, IC2, IC3, IC4 in the schematic. When the address 80 hexadecimal is sent out on the address lines, and AEN is low, and IOW pulses low, these three conditions cause pin 11 of IC4 to go low, clocking the data into the register (IC5). From here, the upper 4 bits of the data (which comprise the most significant hex digit) are sent to an EPROM and 7-segment display, while the lower 4 bits are sent to the other EPROM and display.

       Each EPROM is a combinational logic block which converts a 4-bit binary number ( a single hex digit) into the 7 lines needed for a 7-segment display. Those familiar with the 7447 chip (BCD-to-7-segment decoder) will be familiar with this concept; the only difference is that the 7447 takes a Binary Coded Decimal number as input and drives a 7-segment display on the output, while the EPROM takes a single hex digit (4-bit binary number) as the input and drives a 7-segment display on the output. I could not locate a chip to do the job, so I used EPROMs instead. The 4-bit binary number is fed to the address lines of the EPROM, and the corresponding 7-segment code then appears on the EPROM data lines. You can see that this is a 'look-up table' approach of logic conversion.

      A .HEX file contains the data that needs to be programmed into the EPROM in order to implement this binary-to-7-segment decoder. The file is called [POST.HEX](post.hex) This same file can be used to program both of the EPROMS. The information is also included in address-data form in the table below, just in case you want to make your own .HEX file instead of using the one provided. The column on the left is the hex digit that must be displayed on the 7-segment display. This corresponds to the EPROM address. The next column contains the display segments which must be illuminated in order to display that number. The next column contains the binary data that will illuminate the required segments. The last column contains the same data in hexadecimal form. Therefore, you can see that the first and last columns contain the addresses and data for programming an EPROM.  
  
\*\*\*Note: The data is for an common anode display (as opposed to common cathode). In other words, the output data line goes low when the corresponding display segment is to be turned on. The EPROM completes the path to ground through the display segments. The data bits are associated with the display segments as follows:

D7 = not used  
D6 = a  
D5 = b  
D4 = c  
D3 = d  
D2 = e  
D1 = f  
D0 = g

Hex Input  
(EPROM Address)

Display Segments  
Turned On

7-Seg. Codes  
(EPROM Data)

Hex EPROM  
Data

0

a b c d e f

1000 0001

81

1

b c

1100 1111

CF

2

a b g e d

1001 0010

92

3

a b c d g

1000 0110

86

4

f g b c

1100 1100

CC

5

a f g c d

1010 0100

A4

6

a f g e c d

1010 0000

A0

7

a b c

1000 1111

8F

8

a b c d e f g

1000 0000

80

9

a b g f c d

1000 0100

84

A

a b g f c d

1000 1000

88

B

a b g f c e

1110 0000

E0

C

a f e d

1011 0001

B1

D

b c d e g

1100 0010

C2

E

a f g e d

1011 0000

B0

F

a f g e

1011 1000

B8

                The following diagram relates the letter names of the segments to the position of the segment on the 7-segment display.

  
![](Display.gif)  

  
        For those who have access to programmable logic devices, the number of integrated circuits required can be greatly reduced. One simple PLD such as the GAL22V10 can replace all the address and control signal decoding logic. In fact, it is possible to implement the entire design (except the 7-segment displays, of course!) on just a couple simple PLD's. More information on getting started with PLD's is coming to this web site. There are now sources for inexpensive PLD's which do not require expensive programmers; they can be programmed with a simple download cable attached to your PC. There is also free logic compiler software available?but more on that later.  
    
      One of the more expensive items involved in building an ISA bus card such as this P.O.S.T. card reader is the prototyping perf board itself. Several companies offer ISA prototyping boards on which the user can construct custom circuitry. These are rather expensive, however ($15 and up). The method I use is much less expensive, and I have achieved good results with it. I simply take an old ISA card that I don't need anymore (most people have a few of those these days!) and use that for my project. I carefully cut the circuit board traces that go to the finger contacts of the board. I then cut away a nice chunk of the card with a pair of tin snips and replace that section with standard hobbyist perf board which you can buy many places. I then solder small wires onto the card edge finger contacts that I need to use, and I run these wires into the perf board area and connect them to the appropriate integrated circuit pins. Remember, the +5 volts needed for the integrated circuits is also available on the ISA bus. Just run some power wires from the ISA bus to the perf board. I have built several boards using this 'home-brew' ISA card construction technique, and it works perfectly. Just BE SURE to cut ALL of the original traces going to the card edge fingers. I have found that the thing cutting wheel that comes with Dremel tools works very well for cutting traces quickly and cleanly. You want to TOTALLY disconnect the old card circuitry from the bus. A pinout of the ISA bus is available at The Hardware Book web site. The address is

[http://www.sunsite.auc.dk/hwb/co\_ISA.html](http://www.sunsite.auc.dk/hwb/co_ISA.html)

Additional technical information about the ISA bus signals is available at

[http://www.sunsite.auc.dk/hwb/co\_ISA\_Tech.html  
  
](http://www.sunsite.auc.dk/hwb/co_ISA_Tech.html)Also, see the additional information on this web site about home-brew ISA cards (Coming Soon).  

* * *

**[Click here to view schematic for the P.O.S.T. circuit](post.gif)**

* * *

If I have left anything out, did not describe something very clearly, or if you have any questions, feel free to send me email at: [\[email protected\]](/cdn-cgi/l/email-protection#51332338363925221126383f373e2335343f367f323e3c)
