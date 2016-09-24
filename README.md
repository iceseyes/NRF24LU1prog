NRF24LU1prog
============

Program bricked, bootloader-less or code protected [NRF24LU1+][1] via SPI.  
Works with 16kB and 32kB versions of the chip.


---
## NEWS

### STATUS 2016/09/22:

  Board:

    [X] serial comm (PC)
    [X] serial buffer dump
    [X] serial buffer write
    [X] SPI comm (NRF)
    [X] NRF24 mem read
    [X] NRF24 mem prog
    [ ] NRF24 mem verify
    [X] NRF24 erase
    [X] NRF24 FSR read
    [X] NRF24 flash operation wait
    [ ] automatic pin selections for different boards
    [ ] ...

  PC:    
    
    [ ] PC SW, Python
    [X] lot of checkboxes missing
    [ ] ...



### CHANGES 2016/09/XX:

    - initial upload; not much to see, for now...


---
## REQUIREMENTS

  - a 3.3V "Arduino-compatible" board with SPI functionality (see below)
  - [PlatformIO][2], [CLI][3] or [IDE][4]
  - [Python][5] interpreter
  - a handful of wires


### BOARDS

#### BUILT-IN

  NRF24LU1prog should [tm] run on a lot of 3.3V Arduino-compatible boards.  
  Currently implemented are:

  - [ChipKIT Max32][20]
  - [Teensy LC][21]

#### ADDING YOUR OWN BOARD

  ...


---
## HW-SETUP

### CONNECTIONS

  To program an NRF24LU1+ via [SPI][6], at least 5 pins are required:

    - MOSI
    - MISO
    - SCLK
    - CS
    - PROG

  In very rare cases, it might be necessary to additionally wire the RESET pin, e.g.
  when the NRF24 is powered externally and can't be turned on or off.  
  But that is usually [tm] not required.

  The NRF24LU1 has an integrated voltage regulator, so it can either be used with
  a 5V or a 3V3 power supply.  
  Notice: EITHER, not BOTH:
  
    5V   VBUS  *recommended*
    3V3  VDD

  NOTE 1:
  
    Notice that the logic pins, including the SPI connections require 3.3V

  NOTE 2: 

    The serial in- and output pins need to be cross wired.
    MOSI of your board to MISO of the NRF24 and vice versa.
  
  ...
  
  For now, you need to set the connections in the file 'config.h' manually, e.g.:

    // config.h
    
    #define PIN_PROG     77
    #define PIN_RESET    76
    #define PIN_CS       75
    

#### DEFAULT PINS FOR CHIPKIT MAX32

    MOSI   SP1-1
    MISO   SP1-4
    SCLK   SP1-3
    
    CS     75
    PROG   77
    RESET  76
    
    5V    
    GND    SP1-6, or see labels
    


#### DEFAULT PINS FOR TEENSY LC

  ...
  

---
## COMPILE & INSTALL

### PLATFORMIO

  Beside the many ways and platforms, described in the the official [installation instruction][8],
  the easiest way to install PlatformIO is doing that via [PIP][10]:

    pip install -U platformio

  Pip is usually [tm] available via your system's package manager.  
  E.g. for Ubuntu, it can be installed via
  
    sudo apt-get install python-pip 


### BOARD

  Inside the 'board' directory, type
  
    platformio run
  
  to compile or
  
    platformio run --target upload
  
  to compile and upload the firmware.
  
  You might wish to edit the targets (boards) in the 'platformio.ini' file:

    [env:mega_pic32]
    platform = microchippic32
    board = mega_pic32
    framework = arduino
    
    ;[env:teensylc]
    ;platform = teensy
    ;board = teensylc
    ;framework = arduino

  Just comment out the stuff you don't need with a leading semicolon.  
  Depending on your selection, PlatformIO will automatically install the proper toolchains, required
  to compile the software for the board you selected.

### PC

  ...


---
## USAGE

  NOTE 1:
  
    Most of those already programmed NRF24LU1s (e.g. those from eBay :-)
    reset the RDISMB/IP bits upon a power, a reset or a PROG pin cycle.
    
    ########################################################################
    You can ONLY read back the NRF24's freshly updated memory, directly
    after programming it and before a reset, PROG pin change or power cycle.
    ########################################################################
    
    The only way to solve that is to write 0x00 to address 0x23 of the InfoPage,
    which is (currently) not supported

  NOTE 2:

    Programming or erasing the InfoPage is (currently) disabled.
    This will brick your NRF24LU1 if the InfoPage cannot be read due to an
    already issued RDISIP command. That is usually the case if you got your
    chips from far east eBay sellers...

  NOTE 3:
    
    Some boards, e.g. the ChipKIT Max32, perform a reset after
    the serial port on the PC was opened and spend some seconds
    in the bootloader first.


### TERMINAL MODE

#### COMMANDS

  ...

  For the ease of it, all commands can be send/read via a serial UART/VCP terminal.  
  Serial settings are "8N1":

    - 115200 bits/s
    - 8 data bits
    - 1 stop bit
    
  

  So far implemented, are:
  
  
    CMD DATA DESCRIPTION   
     ?       show help
     0       set NRF memory page address (n*512) to zero
     +       increase NRF memory page address; max is 63
     -       decrease NRF memory page address
     i       initialize NRF via RESET and PROG pin cycle
     S       show flash memory status (in human readable form)
     s       show flash memory status (FSR register value in HEX)
     W       enable  write or erase of flash memory
     w       disable write or erase of flash memory
     E       erase all; requires W (enable flash memory write/erase)
     r       read memory of current selected NRF memory page and store in buffer
     P       program NRF memory from buffer
     d       dump memory buffer to console (in HEX)
     b  34   write to memory buffer, followed by 34 HEX digits (1B addr + 16B data)


##### (?) HELP

  ...

##### (i) INIT; RESET and PROGADJUST NRF24 MEMORY POINTER

  ...

##### (0/+/-) ADJUST NRF24 MEMORY POINTER

  ...

##### (s/S) SHOW FLASH MEMORY STATUS

  ...
  
##### (w/W) ENABLE/DISABLE WRITE/ERASE FLASH MEMORY

  ...

##### (E) ERASE ALL

  ...

##### (r) READ MEMORY FROM NRF24

  ...

##### (P) PROGRAM NRF MEMORY

  ...
  
##### (d) DUMP MEMORY BUFFER TO CONSOLE

  ...

##### (b) WRITE TO MEMORY BUFFER

  ...
  


### PROGRAMMING

  ...



---
## LICENSE

  [CC BY 4.0, Attribution 4.0 International][7]

  You are free to:

  Share — copy and redistribute the material in any medium or format  
  Adapt — remix, transform, and build upon the material for any purpose, even commercially.
  
  The licensor cannot revoke these freedoms as long as you follow the license terms.


---
Have fun  
FMMT666(ASkr)  


[1]:  https://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24LU1P
[2]:  http://platformio.org/
[3]:  http://platformio.org/get-started/cli
[4]:  http://platformio.org/platformio-ide
[5]:  https://www.python.org/
[6]:  https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus
[7]:  https://creativecommons.org/licenses/by/4.0/
[8]:  http://docs.platformio.org/en/stable/installation.html
[9]:  https://en.wikipedia.org/wiki/WTFPL
[10]: https://en.wikipedia.org/wiki/Pip_(package_manager)


[20]: https://reference.digilentinc.com/chipkit_max32/refmanual
[21]: https://www.pjrc.com/teensy/teensyLC.html
