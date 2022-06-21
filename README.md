# Robotron EC1834 POST 60h (and not only port 60) ISA card

Anleitung in [Deutsch](README-DE.md)

In this project I would like to introduce you to an ISA Port 60h card for the EC1834.

![POST Platine](photos/POST_by_rowikla.jpg)

![POST Schema](photos/post-schematics.png)

This project was inspired by:
https://bbright.tripod.com/information/postcard.htm
and by the ELV 1995-04 ISA-BIOS-POST Karte

## Description

### Decoding
The board uses 2 EPROMS for decoding the bus byte to the two 7 segment LED displays.
There may be better ways to do it (BCD to 7seg hex encoder or PAL ICs). Both techniques are obsolete and not/hardly available (as the used 2732/27C32 EPROMs are).
But to keep it as simple as possible, the EPROMs are the solution.

### Configuration
To support 74LS series ICs (the 74LS688), I used only pull up mechanisms for the configuration.
So the address lines NOT wanted, have to be switched **ON** at the DIP switch 
(which is - by the way - prepared to be replaced by a 2x10 PIN header),
when the address is to be defined.
For jumpers - these have to be set then.

Example:

* Adress 0x80 - Lines A0-A6,A8-A9 need to be set to **ON**; A7 to **OFF**
* Adress 0x60 - Lines A0-A4,A7-A9 must be **ON**; A5,A6 have to be **OFF**

The address range is configurable from 0x000-0x3ff

The display can be configured, to display read requests, instead of write requests. The Jumper J3 needs to be set to "Read".
Default should be "Write".

### Connectors
The board has 2 feature connectors.
The power connector J4 shall to be used later for a power monitoring. It provides access to all voltage (+5V, -5V, +12V, -12V, GND) lines on the ISA BUS.

Connector J2 provides easy access for a Logic analyzer. The bus data presents read and write access to IO ports on the configured port.
The ongoing transaction can be monitored on the /IOR or /IOW line, which is active low, depending on the bus transaction.

J0 can be installed as a DIN41612 64pin (a+c) connector, as no access to the 16bit data bus is needed.

## BOM

| Ref | Value               |
|-----|---------------------|
| C1  | 100n                |
| C2  | 100n                |
| C3  | 100n                |
| C4  | 100n                |
| C5  | 100n                |
| C6  | 100n                |
| C7  | 100uF               |
| C8  | 100n                |
| J0  | DIN 41612C 64(a+c) or 96 pin (abc) <br />female 90° THT connector  |
| J2  | Conn_02x07_Odd_Even |
| J3  | Conn_01x03          |
| J4  | Conn_02x06_Odd_Even |
| Q1  | BC337               |
| Q2  | BC337               |
| R1  | 2K2                 |
| R2  | 10k                 |
| R3  | 10k                 |
| R4  | 1k                  |
| R5  | 1k                  |
| R6  | 220                 |
| RN1 | SIL9-8 10K          |
| RN2 | SIL5-4 1,5K or 4x1,5K R |
| RN3 | SIL5-4 1,5K or 4x1,5K R |
| RN4 | SIL5-4 1,5K or 4x1,5K R |
| RN5 | SIL5-4 1,5K or 4x1,5K R |
| SW1 | SW_DIP_x10          |
| U1  | 74LS688             |
| U2  | 74LS86              |
| U3  | 7432                |
| U4  | 74HC374             |
| U5  | 2732                |
| U6  | 2732                |
| U7  | SA39-11EWA          |
| U8  | SA39-11EWA          |
| U9  | 74HC374             |

