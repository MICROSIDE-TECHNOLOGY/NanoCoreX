[![en](https://img.shields.io/badge/lang-english-red.svg)](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/README.md)
[![es](https://img.shields.io/badge/lang-español-blue.svg)](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/README.es-419.md)

# NanoCoreX

An Arduino core for ATmega4809, ATmega4808, ATmega3209, ATmega3208, ATmega1609, ATmega1608, ATmega809 and ATmega808 developed by [MCUdude](https://github.com/MCUdude/MegaCoreX) and modified by [Microside Technology](https://microside.com/) for its development board [NanoX](#) which is fully compatible with any Arduino Nano based project. Some of their key features include multiple serial ports, SPI and i2c interfaces, built-in programmable logic, up to 16 analog input pins, and an analog comparator with a built-in programmable voltage reference and hysteresis and much more!

Compared to Arduino Nano board it also has an accurate internal oscillators which can provide base frequencies of 16 and 20 MHz. These can then be divided down internally to reduce the processor speed and power consumption. You can read more about clocks and clock frequencies in the [Supported clock frequencies](#supported-clock-frequencies) section.

For programming, a USB to serial adapter is used with Optiboot bootloader, if also allows the use of a UPDI programming interface. This is a bi-directional single wire interface and requires a programmer that supports UPDI.  Read more about UPDI and bootloaders in the [Programming](#programming) section below.


<!-- #### UPDI programmers
If you're looking for well-designed, reliable UPDI programmers that also acts as a USB to serial adapters, check out the [microUPDI](https://www.tindie.com/products/MCUdude/microupdi-programmer/) and the [SerialUPDI](https://www.tindie.com/products/mcudude/serialupdi-programmer/) programmers I sell on [Tindie](https://www.tindie.com/stores/mcudude/)!
They're small programmers with excellent software support, and can be used with Arduino IDE or PlatformIO. The SerialUPDI programmer comes fully assembled (except for the 6-pin programming connector), but the microUPDI needs to be soldered to an Arduino Pro Micro board.

| [microUPDI](https://www.tindie.com/products/MCUdude/microupdi-programmer/) | [SerialUPDI](https://www.tindie.com/products/mcudude/serialupdi-programmer/) | Pinout                                                   |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------|
| <img src="https://i.imgur.com/Xn0pMuk.jpg" width="250">                    | <img src="https://i.imgur.com/fBV4Jfy.jpg" width="250">                      | <img src="https://i.imgur.com/12xKn3P.png" width="270">  |
| <img src="https://i.imgur.com/tF0KQS9.jpg" width="250">                    | <img src="https://i.imgur.com/SfP6ym8.jpg" width="250">                      |                                                          | -->

# Table of contents
* [Supported microcontrollers](#supported-microcontrollers)
* [Programming](#programming)
  - [Using a bootloader](#using-a-bootloader)
  - [Using a UPDI programmer](#using-a-updi-programmer)
    - [SerialUPDI](#serialupdi)
* [Supported clock frequencies](#supported-clock-frequencies)
* [BOD option](#bod-option)
* [EEPROM retain option](#eeprom-option)
* [Reset pin](#reset-pin)
* [Printf support](#printf-support)
* [Fast IO](#fast-io)
* [Pin macros](#pin-macros)
* [Write to own flash](#write-to-own-flash)
* [Memory-mapped flash](#memory-mapped-flash)
* [Identifying NanoCoreX](#identifying-nanocorex)
* [Timer used for millis and micros](#timer-used-for-millis-and-micros)
* [Pinout](#pinout)
* [Hardware features](#hardware-features)
  - [PWM output](#pwm-output)
  - [Configurable Custom Logic (CCL)](#configurable-custom-logic-ccl)
  - [Analog Comparator (AC)](#analog-comparator-ac)
  - [Event System (EVSYS)](#event-system-evsys)
  - [Peripheral pin swapping](#peripheral-pin-swapping)
* [How to install](#how-to-install)
  - [Boards Manager Installation](#boards-manager-installation)
  - [Manual Installation](#manual-installation)
  - [PlatformIO](#platformio)
* [Getting your hardware working](#getting-your-hardware-working)
  - [4809 Xplained Pro](#NanoX)


## Supported microcontrollers

|                  | Mega4809                   | Mega4808                          | Mega3209          | Mega3208                          | Mega1609          | Mega1608                          | Mega809           | Mega808                           |
|------------------|----------------------------|-----------------------------------|-------------------|-----------------------------------|-------------------|-----------------------------------|-------------------|-----------------------------------|
| **Flash**        | 48 kiB                     | 48 kiB                            | 32 kiB            | 32 kiB                            | 16 kiB            | 16 kiB                            | 8 kiB             | 8 kiB                             |
| **RAM**          | 6 kiB                      | 6 kiB                             | 4 kiB             | 4 kiB                             | 2 kiB             | 2 kiB                             | 1 kiB             | 1 kiB                             |
| **EEPROM**       | 256 B +<br/>64 B†          | 256 B +<br/>64 B†                 | 256 B +<br/>64 B† | 256 B +<br/>64 B†                 | 256 B +<br/>64 B† | 256 B +<br/>64 B†                 | 256 B +<br/>64 B† | 256 B +<br/>64 B†                 |
| **Serial ports** | 4                          | 3                                 | 4                 | 3                                 | 4                 | 3                                 | 4                 | 3                                 |
| **IO pins**      | 41<br/>33&ast;&ast;&ast;   | 27&ast;<br/>24&ast;&ast;          | 41                | 27&ast;<br/>24&ast;&ast;          | 41                | 27&ast;<br/>24&ast;&ast;          | 41                | 27&ast;<br/>24&ast;&ast;          |
| **Packages**     | TQFP48<br/>QFN48<br/>DIP40 | TQFP32<br/>QFN32<br/>SSOP28       | TQFP48<br/>QFN48  | TQFP32<br/>QFN32<br/>SSOP28       | TQFP48<br/>QFN48  | TQFP32<br/>QFN32<br/>SSOP28       | TQFP48<br/>QFN48  | TQFP32<br/>QFN32<br/>SSOP28       |

<b>†</b> 64 bytes of USERROW, accessible from address 256 to 319 using the EEPROM.h library <br/>
<b>&ast;</b> TQFP32/QFN32 package <br/>
<b>&ast;&ast;</b> SSOP28 package <br/>
<b>&ast;&ast;&ast;</b> DIP40 package


## Programming
### Using a bootloader
Programming can be done using the [Optiboot bootloader](https://github.com/optiboot/Optiboot). It is available for all megaAVR-0 chips and can be used with all hardware serial ports including alternative pin positions. By default the UART port to use is UART0, which is connected to the built-in USB-Serial interface on the NanoX dev board, therefore this can't be changed. The bootloader will flash an LED connected to PA7 (digital pin 7 on most pinouts) twice when a hardware reset occurs. The upload speed is 115200 baud. Note that that the reset pin cannot be used as a GPIO when using a bootloader. It doesn't matter if you have set the reset pin as GPIO in the tools menu or not; the bootloader option will override this setting.
If the bootloader is accidentally deleted, it can be burned again, select the UDPI programmer from the menu and click "Burn bootloader".

### Using a UPDI programmer
Programming must be done with a UPDI compatible programmer, such as the [microUPDI](https://github.com/MCUdude/microUPDI), [JTAG2UPDI](https://github.com/ElTangas/jtag2updi) SerialUPDI or an official Atmel/Microchip UPDI compatible programmer.

Unlike the *Arduino megaAVR boards* package, NanoCoreX does not auto-detect the programmer you're using. You'll have to select the correct programmer in the *Programmers* menu.

#### SerialUPDI
SerialUPDI is a programmer that utilize a simple serial connection. It uses a USB serial adapter which can be turned into a UPDI programmer by adding a few passive components. You can read more about the details on how the SerualUPDI works [here](https://github.com/SpenceKonde/AVR-Guidance/blob/master/UPDI/jtag2updi.md).

## Supported clock frequencies
NanoCoreX lets you choose what clock frequency you want to run your microcontroller at.

| Frequency | Oscillator type     | Other                    |
|-----------|---------------------|--------------------------|
| 20 MHz    | Internal oscillator |                          |
| 16 MHz    | Internal oscillator | **Default option**       |
| 10 MHz    | Internal oscillator | Derived from 20 MHz osc. |
| 8 MHz     | Internal oscillator | Derived from 16 MHz osc. |
| 5 MHz     | Internal oscillator | Derived from 20 MHz osc. |
| 4 MHz     | Internal oscillator | Derived from 16 MHz osc. |
| 2 MHz     | Internal oscillator | Derived from 16 MHz osc. |
| 1 MHz     | Internal oscillator | Derived from 16 MHz osc. |

Note that unlike other AVRs **none of these chips can drive an external crystal or resonator**. If you need an external oscillator it has to be one with a driven clock output. Also the development board NanoX does not have the oscilator pins exposed.
The microcontroller will freeze if the external clock suddenly drops out. If not present on boot, it will automatically choose the 16 MHz internal oscillator instead.

Another thing to watch out for is the lowest possible baudrates when running at 16 or 20 MHz. At 16 MHz the lowest baud rate is 1200 baud. When running at 20 MHz the lowest is 2400 baud. This is caused by the way the internal baud rate generator works. If you need lower baud rates you'll either have to reduce the main clock speed or use the software serial library instead.


## BOD option
Brownout detection or BOD for short lets the microcontroller sense the input voltage and shut down if the voltage goes below the brownout setting. Below is a table that shows the available BOD options:

| BOD threshold                  |
|--------------------------------|
| 4.3 V                          |
| 4.0 V                          |
| 3.7 V                          |
| 3.3 V                          |
| 2.9 V                          |
| **2.6 V** (default option)     |
| 2.1 V                          |
| 1.8 V                          |
| Disabled                       |


## EEPROM option
If you want the EEPROM to be erased every time you burn the bootloader or upload using a programmer, you can turn off this option. A UPDI programmer is needed to enable or disable EEPROM retain. Note that when uploading using a bootloader, the EEPROM will always be retained.


## Reset pin
None of the megaAVR-0 microcontrollers needs the reset line to be reprogrammed over the UPDI interface. This means that the reset pin can be used as a GPIO pin instead! There's no need for a high voltage programmer to turn that pin into a reset pin again either. If you have a development board you can instead use the reset button as a general-purpose button for your project. Note that the reset pin cannot be used as a GPIO when using a bootloader.


## Printf support
Unlike the official Arduino core, MegaCoreX has printf support out of the box and by extension NanoCoreX has it as well. If you're not familiar with printf you should probably [read this first](https://www.tutorialspoint.com/c_standard_library/c_function_printf.htm). It's added to the Print class and will work with all libraries that inherit Print. Printf is a standard C function that lets you format text much easier than using Arduino's built-in print and println. 

[**See the extended API documentation for more information!**](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/tree/master/Extended-API.md#printf-support)


## Fast IO
For timing critical applications the standard `digitalRead()` and `digitalWrite()` functions may be too slow. To solve this, NanoCoreX also incorporates `digitalReadFast(myPin)` and `digitalWriteFast(mypin, state)` which compiles down to a single instruction.

[**See the extended API documentation for more information!**](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/tree/master/Extended-API.md#fast-io)


## Pin macros
Note that you don't have to use the digital pin numbers to refer to the pins. You can also use some predefined macros that map "Arduino pins" to the port and port number. Check the schematic of the development board for this information.

```c++
// Use PIN_PA0 macro to refer to pin PA0 (Arduino pin 0)
digitalWrite(PIN_PA0, HIGH);

// Results in the exact same compiled code
digitalWrite(0, HIGH);
```


## Write to own flash
As an alternative for UPDI, NanoCoreX uses Optiboot Flash, a bootloader that supports flash writing within the running application, thanks to the work of [@majekw](https://github.com/majekw).
This means that content from e.g. a sensor can be stored in the flash memory directly without the need of external memory. Flash memory is much faster than EEPROM, and can handle at least 10 000 write cycles before wear becomes an issue.
For more information on how it works and how you can use this in your own application, check out the [Serial_read_write](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/megaavr/libraries/Optiboot_flasher/examples/Serial_read_write/Serial_read_write.ino) for a simple proof-of-concept demo, and
[Flash_put_get](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/megaavr/libraries/Optiboot_flasher/examples/Flash_put_get/Flash_put_get.ino) + [Flash_iterate](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/megaavr/libraries/Optiboot_flasher/examples/Flash_iterate/Flash_iterate.ino) for useful examples on how you can store strings, structs and variables to flash and retrieve then afterwards.


## Memory-mapped flash
Unlike classic AVRs, the flash memory on these parts is within the same address space as the main memory. This means that functions like `pgm_read_byte()` and `pgm_read_word()` is not needed to read directly from flash. Because of this, the compiler automatically puts any variable declared const into PROGMEM, and accesses it appropriately - you no longer need to explicitly declare them as PROGMEM!
This also includes quoted strings, which means that the string in `Serial.print("Hello World");` won't consume any RAM. This means that the `F()` macro is not needed when working with MegaCoreX.

If you explicitly declare a variable PROGMEM, you must still use the `pgm_read_byte()/pgm_read_word()` functions, just like on "classic" AVRs. Do note that declaring things PROGMEM or using the `F()` macro works fine, but it is slower and consumes more flash than simply declaring something `const` or omitting the `F()` macro.


# Identifying NanoCoreX
If you're writing code that has NanoCoreX specific code in it, you can identify this core and its pinouts with the list of macros below.


| Core specific       | Pinout specific                  | Chip family specific | Chip specific        |
|---------------------|----------------------------------|----------------------|----------------------|
| `NANOCOREX`         | `NANOCOREX_DEFAULT_NANOX_PINOUT` | `__AVR_ATmegax09__`  | `__AVR_ATmega4809__` |
| `MICROSIDE_NANOCOREX` || `__AVR_ATmegax08__`  | `__AVR_ATmega4808__` |
|||| `__AVR_ATmega3209__` |
|||                      | `__AVR_ATmega3208__` |
|                     |                                  |                      | `__AVR_ATmega1609__` |
|                     |                                  |                      | `__AVR_ATmega1608__` |
|                     |                                  |                      | `__AVR_ATmega809__`  |
|                     |                                  |                      | `__AVR_ATmega808__`  |

Usage:
```c++
#if defined(NANOCOREX)
// Code
#endif
```


# Timer used for millis and micros
The 28 and 32 pin part have a total of three TCB timers, while the 40 and 48 pin parts have four. In order to create default pinouts with as may PWM output pins as possible, different pinout uses different timers for millis and micros.  
The default timer can be changed by adding `-DMILLIS_USE_TIMERBx` to the platformio.ini build flags, where *x* represents the TCB timer number from 0 to 3. Alternatively, the respective *pins_arduino.h* file can be modified if using Arduino IDE. Be aware that changing this will result in a pinout not fully compatible with Arduino Nano legacy projects.

| Pinout          | Timer used for millis and micros |
|-----------------|----------------------------------|
|      NanoX      | TCB2                             |


## Pinout
This core provides just the NanoX Arduino pin mapping, design specifically to match 1-to-1 pin mapping of the Arduino Nano development board. If you want to work with a different platform we encourage you to check out [MegaCoreX](https://github.com/MCUdude/MegaCoreX) by [MCUdude](https://github.com/MCUdude)
- **NanoX pin standard**: This pinout is 1-to-1 compatible with Arduino Nano pinout. The only pin swap done by default is the PWM output pins & the I2C port. This however causes "collisions" between the ADC peripheral and the I2C peripheral. Activating the I2C peripheral will set the PF3 & PF4 pins to high impedance state, after this trying to read from this pins will cause wrong behavior on I2C, be aware of this limitation.

Please have a look at the pins_arduino.h files for detailed info.<br/> <br/>
>>TODO: Insert board pinout
<!-- <b>Click to enlarge:</b>

| **MegaCoreX ATmega809/1609/3209/4809 pinout**                                                                                                                                 | **MegaCoreX ATmega808/1608/3208/4808 pinout**                                                                                                                                                                                        |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|<img src="https://i.imgur.com/3PUBB6H.jpg" width="350"><br/><img src="https://i.imgur.com/QbOOOTd.png" width="350"><br/><img src="https://i.imgur.com/Hp21SXJ.jpg" width="350">|<img src="https://i.imgur.com/2YlmE8p.png" width="350"><br/><br/><br/><br/><img src="https://i.imgur.com/zXyWvi1.png" width="350"><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>| -->


## Hardware features
Here's some hardware specific features that differ from the older AVR families.

### Interrupt pins
Unlike older AVRs the megaAVR-0 microcontrollers have fully featured interrupts on every pin.
Supported states are *RISING*, *FALLING*, *CHANGE*, *HIGH* and *LOW*. This means there's no need to use the `digitalPinToInterrupt` macro. Simply call attachInterrupt like this:

```c
attachInterrupt(myPin, myInterruptFunction, RISING);
```

### PWM output
PWM output, `analogWrite()`, is available for the following pins:

| Pinout            | Number of PWM pins | Available PWM pins                 |
|-------------------|--------------------|------------------------------------|
| |
| *NanoX pin standard* | 8                  | 3, 5, 6, 9, 10, 14, 15, 16       |

The repeat frequency for the pulses on all PWM outputs can be changed with the new function `analogWriteFrequency(kHz)`, where `kHz` values of 1 (default), 4, 8, 16, 32 and 64 are supported. Note that these values are very approximate. The best effort within the constraints of the hardware will be made to match the request.

Note also that tone() will use TCB1, so the corresponding PWM output is not available if it is used.

### Configurable Custom Logic (CCL)
The megaAVR-0 microcontrollers are equipped with four independent configurable logic blocks that can be used to improve speed and performance. The CCL pins are marked on all pinout diagrams in a dark blue/grey color. The logic blocks can be used independently from each other, connected together or generate an interrupt to the CPU. MCUdude developed a [light weight, high-level library](https://github.com/MCUdude/MegaCoreX/tree/master/megaavr/libraries/Logic) for easy integration with the CCL hardware.

>Note: If you find a bug with this library while using it with NanoCoreX, please report it first on the [NanoCoreX repository](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX), because it might be a bug related with this fork specifics.

### Analog Comparator (AC)
The megaAVR-0 microcontrollers are equipped with an analog comparator. It compares the voltage levels on two inputs and gives a digital output based on this comparison. The megAVR chip has four positive AC pins and three negative. There's also a configurable internal voltage reference that can be used on the negative comparator pin instead of an external voltage.
Try out the [Comparator library](https://github.com/MCUdude/MegaCoreX/tree/master/megaavr/libraries/Comparator) for more information, library reference and examples.

>Note: If you find a bug with this library while using it with NanoCoreX, please report it first on the [NanoCoreX repository](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX), because it might be a bug related with this fork specifics.

### Event System (EVSYS)
The Event System (EVSYS) enables direct peripheral-to-peripheral signaling. It allows a change in one peripheral (the event generator) to trigger actions in other peripherals (the event users) through event channels, without using the CPU. It is designed to provide short and predictable response times between peripherals, allowing for autonomous peripheral control and interaction, and also for synchronized timing of actions in several peripheral modules. It is thus a powerful tool for reducing the complexity, size, and execution time of the software. Give the [Event library](https://github.com/MCUdude/MegaCoreX/tree/master/megaavr/libraries/Event) a try! There you'll find documentation and useful library examples.

>Note: If you find a bug with this library while using it with NanoCoreX, please report it first on the [NanoCoreX repository](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX), because it might be a bug related with this fork specifics.

### Peripheral pin swapping
The megaAVR-0 microcontrollers support alternative pin assignments for some of their built-in peripherals.<br/>
NanoCoreX currently supports pinswapping the SPI, i2c and UART peripheral pins.

[**See the extended API documentation for more information!**](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/tree/master/Extended-API.md#peripheral-pin-swapping)


## How to install
#### Boards Manager Installation
* Open Arduino IDE.
* Open the **File > Preferences** menu item.
* Enter the following URL in **Additional Boards Manager URLs**:
    ```
    TODO: url
    ```
* Separate the URLs using a comma ( **,** ) if you have more than one URL
* Open the **Tools > Board > Boards Manager...** menu item.
* Wait for the platform indexes to finish downloading.
* Scroll down until you see the **NanoCoreX** entry and click on it.
* Click **Install**.
* After installation is complete close the **Boards Manager** window.

#### Manual Installation
Click on the "Download ZIP" button. Extract the ZIP file, and move the extracted folder to the location "**~/Documents/Arduino/hardware**". Create the "hardware" folder if it doesn't exist.
Open Arduino IDE and a new category in the boards menu called "NanoCoreX" will show up.

#### PlatformIO
[PlatformIO](http://platformio.org) is an open source ecosystem for IoT development, support for NanoCoreX is coming soon.

**See [PlatformIO.md](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/PlatformIO.md) for more information.**

## Getting your hardware working

### NanoX
The NanoX, is Microside's Arduino compatible board with the Nano form factor. It uses an ATmega4808 and the *only* supported pinout is *NanoX*. Printing to the serial monitor on your PC is done by initializing `Serial.begin(baud)`. The NanoX uses a bootloader by default on UART0, no other option is available in the tools menu. If you're having upload issues, connect a dedicated UPDI programmer to the UPDI pin, select it in the programmers menu and burn the bootloader.
Click to enlarge:

>>TODO: Board pinout image
<!-- <img src="https://i.imgur.com/ykebGW7.png" width="400"> -->