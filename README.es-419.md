[![en](https://img.shields.io/badge/lang-english-red.svg)](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/README.md)
[![es](https://img.shields.io/badge/lang-español-blue.svg)](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/README.es-419.md)

# NanoCoreX

Un núcleo Arduino para ATmega4809, ATmega4808, ATmega3209, ATmega3208, ATmega1609, ATmega1608, ATmega809 y ATmega808 desarrollado por [MCUdude](https://github.com/MCUdude/MegaCoreX) y modificado por [Microside Technology](https://microside.com/) para su tarjeta de desarrollo [NanoX](#) que es 100% compatible con cualquier proyecto basado en la tarjeta Arduino Nano. Algunas de sus caracteristicas incluyen múltipes puertos seriales, interfaces SPI e i2c, lógica programable, hasta 16 canales analógicos, y un comparador analógica con lógica programable, voltaje de referencia y mucho más!

Comparado con un Arduino Nano, también incluye un oscilador interno frecuencias base de 16 y 20 MHz. Que después pueden dividirse internamente para reducir la velocidad del procesador y reducir el consumo de energía. Puedes leer más acerca de la configuración del reloj en la sección[Frecuencias de reloj compatibles](#frecuencias-de-reloj-compatibles).

Para programarla, se incluye un adaptador USB a serial en conjunto con el gestor de arranque Optiboot, también permite el uso de un programador UPDI. UPDI es una interface bidireccional de un solo cable.  En la sección de programación[Programación](#programación) se detalla acerca del uso de UPDI y el gestor de arranque.


<!-- #### UPDI programmers
If you're looking for well-designed, reliable UPDI programmers that also acts as a USB to serial adapters, check out the [microUPDI](https://www.tindie.com/products/MCUdude/microupdi-programmer/) and the [SerialUPDI](https://www.tindie.com/products/mcudude/serialupdi-programmer/) programmers I sell on [Tindie](https://www.tindie.com/stores/mcudude/)!
They're small programmers with excellent software support, and can be used with Arduino IDE or PlatformIO. The SerialUPDI programmer comes fully assembled (except for the 6-pin programming connector), but the microUPDI needs to be soldered to an Arduino Pro Micro board.

| [microUPDI](https://www.tindie.com/products/MCUdude/microupdi-programmer/) | [SerialUPDI](https://www.tindie.com/products/mcudude/serialupdi-programmer/) | Pinout                                                   |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------|
| <img src="https://i.imgur.com/Xn0pMuk.jpg" width="250">                    | <img src="https://i.imgur.com/fBV4Jfy.jpg" width="250">                      | <img src="https://i.imgur.com/12xKn3P.png" width="270">  |
| <img src="https://i.imgur.com/tF0KQS9.jpg" width="250">                    | <img src="https://i.imgur.com/SfP6ym8.jpg" width="250">                      |                                                          | -->

# Tabla de contenidos
* [Microcontroladores compatibles](#microcontroladores-compatibles)
* [Programación](#programación)
  - [Uso con el bootloader](#uso-con-el-bootloader)
  - [Uso con un programador UPDI](#uso-con-un-programador-updi)
    - [SerialUPDI](#serialupdi)
* [Frecuencias de reloj compatibles](#frecuencias-de-reloj-compatibles)
* [Opciones BOD](#opciones-bod)
* [Opciones de retención EEPROM](#opciones-eeprom)
* [Pin Reset](#pin-reset)
* [Soporte Printf](#soporte-printf)
* [IO rápido](#io-rápido)
* [Macros para pins](#macros-para-pins)
* [Escritura a memoria Flash](#escritura-a-memoria-flash)
* [Mapeo de memoria Flash](#mapeo-de-memoria-flash)
* [Identificar a NanoCoreX](#identificar-a-nanocorex)
* [Temporizador utilizado para millis y micros](#temporizador-utilizado-para-millis-y-micros)
* [Pinout](#pinout)
* [Características de hardware](#características-de-hardware)
  - [Pines de interrupción](#pines-de-interrupción)
  - [Salidas PWM](#salidas-pwm)
  - [Lógica personalizada configurable (CCL)](#lógica-personalizada-configurable-ccl)
[Comparador analógico (AC)](#comparador-analógico-ac)
  - [Eventos de sistema (EVSYS)](#eventos-de-sistema-evsys)
  - [Cambio de los pines de los periféricos](#cambio-de-los-pines-de-los-periféricos)
* [Guía de instalación](#guía-de-instalación)
  - [Instalar a través del gestor de tarjetas](#instalar-a-través-del-gestor-de-tarjetas)
  - [Instalación manual](#instalación-manual)
  - [PlatformIO](#platformio)
* [Preparación del hardware](#preparación-del-hardware)
  - [NanoX](#NanoX)


## Microcontroladores compatibles

|                  | Mega4809                   | Mega4808                          | Mega3209          | Mega3208                          | Mega1609          | Mega1608                          | Mega809           | Mega808                           |
|------------------|----------------------------|-----------------------------------|-------------------|-----------------------------------|-------------------|-----------------------------------|-------------------|-----------------------------------|
| **Flash**        | 48 kiB                     | 48 kiB                            | 32 kiB            | 32 kiB                            | 16 kiB            | 16 kiB                            | 8 kiB             | 8 kiB                             |
| **RAM**          | 6 kiB                      | 6 kiB                             | 4 kiB             | 4 kiB                             | 2 kiB             | 2 kiB                             | 1 kiB             | 1 kiB                             |
| **EEPROM**       | 256 B +<br/>64 B†          | 256 B +<br/>64 B†                 | 256 B +<br/>64 B† | 256 B +<br/>64 B†                 | 256 B +<br/>64 B† | 256 B +<br/>64 B†                 | 256 B +<br/>64 B† | 256 B +<br/>64 B†                 |
| **Puertos seriales** | 4                          | 3                                 | 4                 | 3                                 | 4                 | 3                                 | 4                 | 3                                 |
| **IO pins**      | 41<br/>33&ast;&ast;&ast;   | 27&ast;<br/>24&ast;&ast;          | 41                | 27&ast;<br/>24&ast;&ast;          | 41                | 27&ast;<br/>24&ast;&ast;          | 41                | 27&ast;<br/>24&ast;&ast;          |
| **Encapsulado**     | TQFP48<br/>QFN48<br/>DIP40 | TQFP32<br/>QFN32<br/>SSOP28       | TQFP48<br/>QFN48  | TQFP32<br/>QFN32<br/>SSOP28       | TQFP48<br/>QFN48  | TQFP32<br/>QFN32<br/>SSOP28       | TQFP48<br/>QFN48  | TQFP32<br/>QFN32<br/>SSOP28       |

<b>†</b> 64 bytes de USERROW, accesible desde la dirección 256 a 319 utilizando la librería EEPROM.h <br/>
<b>&ast;</b> TQFP32/QFN32 <br/>
<b>&ast;&ast;</b> SSOP28 <br/>
<b>&ast;&ast;&ast;</b> DIP40 


## Programación
### Uso con el bootloader
La programación se realiza mediante el gestor de arranque [Optiboot](https://github.com/optiboot/Optiboot). Disponible para todos los chips megaAVR-0 puede utilizarse en cualquier puerto serial incluyendo posiciones de pines alternativos. Sin embargo, en la tarjeta NanoX el puerto utilizado es UART0, conectado al adaptador USB a Serial inlcuido en la tarjeta de desarrollo, por lo que la opción para seleccionar el puerto de programación fue deshabilitada. El gestor de arranque iluminará el LED conectado al pin PA7 (pin digital 13) dos veces cuando se reinicia el hardware. La velocidad de subida es 115200. Nota, el pin de reset no puede utilizarse como un GPIO cuando se utiliza el gestor de arranque. Sin importar si se configuró como un GPIO en el menú; el gestor de arranque omitirá esta configuración.
Si accidentalmente borró el gestor de arranque, puede volver a cargarlo, seleccione un programador UDPI y presione la opción "Burn bootloader".

### Uso con un programador UPDI
La programación por UPDI requiere de un programador compatible con este estandar, por ejemplo [microUPDI](https://github.com/MCUdude/microUPDI), [JTAG2UPDI](https://github.com/ElTangas/jtag2updi) SerialUPDI o cualquier programador UPDI oficial de Atmel/Microchip.

A diferencia del paquete *Arduino megaAVR boards*, NanoCoreX no detecta automáticamente el programador que esté utilizazndo. Deberá seleccionar el programador adecuado en la sección *Programmers* en el menú.

#### SerialUPDI
SerialUPDI que utiliza una simple conexión serial. Utiliza un adaptador USB a UART que puede convertirse en un programador UPDI añadiendo componentes pasivos. Más detalles pueden leerse en [SerualUPDI](https://github.com/SpenceKonde/AVR-Guidance/blob/master/UPDI/jtag2updi.md) (recurso en inglés).

## Frecuencias de reloj compatibles
NanoCoreX permite seleccionar la velocidad de reloj a utilizar, tenga en cuenta que al utilizar el gestor de arranque esta configuración no puede cambiarse, sin embargo, puede seleccionar una frecuencia de reloj especifica y después programar el gestor de arranque utilizando UPDI, los programas que suba a continuación usarán la nueva frecuencia de reloj. Por defecto el gestor de arranque está configurado para funcionar a 20MHz.

| Frecuencia| Tipo de oscilador   | Info                     |
|-----------|---------------------|--------------------------|
| 20 MHz    | Oscilador interno   | **Opción por defecto**   |
| 16 MHz    | Oscilador interno   | **Opción en arranque**   |
| 10 MHz    | Oscilador interno   | Derivado del osc. 20 MHz |
| 8 MHz     | Oscilador interno   | Derivado del osc. 16 MHz |
| 5 MHz     | Oscilador interno   | Derivado del osc. 20 MHz |
| 4 MHz     | Oscilador interno   | Derivado del osc. 16 MHz |
| 2 MHz     | Oscilador interno   | Derivado del osc. 16 MHz |
| 1 MHz     | Oscilador interno   | Derivado del osc. 16 MHz |

A diferencia de otros AVRs **estos chips no pueden dirigir un cristal externo**. Si requiere utilizar un oscilador externo, debe ser este el que controle la CPU. Además en la tarjeta de desarrollo NanoX, los pines del oscilador no está expuestos.
El microcontrolador fallará si se desconecta la señal de reloj externa. Si está no está presente en el arranque y se configuró para usar una señal externa, cambiará automáticamente al oscilador interno de 16 MHz.

Considere que a 16 o 20 MHz existen limites a la velocidad minima de comunicación serial. A 16 MHz la velocidad minima es de 1200 bps. A 20 MHz la velocidad minima es 2400 bps. Esto es debido al funcionamiento del generador UART de hardware. Si necesita valores más bajos considere utilizar una librería de comunicación serial por software.


## Opciones BOD
Las opciones brownout le permiten al microcontrolador medir el valor de voltaje y apagarse si cae por debajo de un valor definido por esta opción:

| Limite BOD                     |
|--------------------------------|
| 4.3 V                          |
| 4.0 V                          |
| 3.7 V                          |
| 3.3 V                          |
| 2.9 V                          |
| **2.6 V** (opción por defecto) |
| 2.1 V                          |
| 1.8 V                          |
| Deshabilitado                  |


## Opciones EEPROM
Si desea borrar la EEPROM cada vez que carga el gestor de arranque o cuando utilice un programador puede deshabilitar esta opción. Un programador UPDI se requiere para borrar la EEPROM. Al utilizar el gestor de arranque la EEPROM no se borrará.


## Pin Reset
Ningún microcontrolador megaAVR-0 requiere una línea de reset para programarse sobre una interfaz UPDI. Lo que significa que puede utilizar el pin como un GPIO. Tampoco se requiere un programador de alto voltaje para reprogramar el Pin de Reset. Nota, al utilizar el gestor de arranque el pin de reset no puede utilizarse como un GPIO.


## Soporte Printf
A diferencia del núcleo oficial de Arduino, MegaCoreX tiene sopore para printf, y por extensión NanoCoreX también la tiene. Si no está familiarizado con esta función puede aprender más [en este enlace](https://www.it.uc3m.es/pbasanta/asng/course_notes/input_output_printf_es.html). Está añadida a cualquier clase que extienda de la clase Print. Printf es una función estandar en C que le permite dar formato al texto de manera más sencilla en comparación a las funciones print y println de Arduino. 

[**Lea la documentación Extendida de la API para obtener más información**](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/tree/master/Extended-API.md#printf-support)


## IO rápido
En aplicaciones donde los tiempos de respuesta son criticos las funciones `digitalRead()` y `digitalWrite()` podrían ser demasiado lentas. Para solucionar esta problemática, NanoCoreX incorpora `digitalReadFast(myPin)` y `digitalWriteFast(mypin, state)` que se compila a una instrucción.

[**Lea la documentación Extendida de la API para obtener más información**](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/tree/master/Extended-API.md#fast-io)


## Macros para pins
Además de utilizar el número de pin digital, también puede referirse a los pines por el puerto al que pertenecen. Revise el esquemático de la tarjeta de desarrollo para obtener más información. Ejemplo de uso:

```c++
// Use PIN_PA0 macro to refer to pin PA0 (Arduino pin 0)
digitalWrite(PIN_PA0, HIGH);

// Results in the exact same compiled code
digitalWrite(0, HIGH);
```


## Escritura a memoria Flash
NanoCoreX utiliza Optiboot Flash, un gestor de arranque con soporte para escritura en memoría flash, gracias al trabajo de [@majekw](https://github.com/majekw).
Esto significa que el contenido de, por ejemplo, un sensor puede almacenarse en la memoría flash, directamente, sin la necesidad de una memoría externa. La memoría flash es mucho más rápida que la memoría EEPROM, y puede realizar al menos 10 000 ciclos de escritura antes de que el desgaste represente un problema.
Para más información sobre como utilizar esta función en su aplicación, revise [Serial_read_write](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/megaavr/libraries/Optiboot_flasher/examples/Serial_read_write/Serial_read_write.ino) para un ejemplo sencillo, y
[Flash_put_get](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/megaavr/libraries/Optiboot_flasher/examples/Flash_put_get/Flash_put_get.ino) + [Flash_iterate](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/megaavr/libraries/Optiboot_flasher/examples/Flash_iterate/Flash_iterate.ino) para ejemplos sobre como almacenar texto, estructuras y variables en flash y después leer estos datos.


## Mapeo de memoria Flash
A diferencia de los AVRs clasicos, la memoría flash de estos componentes comparte el espacio de memoría con la memoría principal. Lo que significa que funciones como `pgm_read_byte()` y `pgm_read_word()` no es requerida para leer directamente de la memoría flash. Debido a esto, el compilador aloja automáticamente cualquier variable declarada como constante (const) en PROGMEM, y accede a ellos apropiadamente - por lo que no es necesario declararlas como PROGMEM.
Esto inclye a texto declarado entre comillas, es decir el texto en `Serial.print("Hola mundo");` no consume RAM. Por lo que la macro `F()` no es requerida en NanoCoreX.

Si declara explicitamente una variable como PROGMEM, aún tendrá que utilizar las funciones `pgm_read_byte()/pgm_read_word()`, como se haría en AVRs "clasicos". Considere que al declarar cosas con PROGMEM o utilizando `F()` aunque funciona, es bastante más lento y consume más flash que simplemente declararlo como `const`.


# Identificar a NanoCoreX
Si está escribiendo código especifico para NanoCoreX en un proyecto multiplataforma, puede identificar este núcleo y su pinout con la siguiente lista de macros.


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

Uso:
```c++
#if defined(NANOCOREX)
// Code
#endif
```


# Temporizador utilizado para millis y micros
La tarjeta NanoX tiene tres temporizadores TCB timers. Para maximizar la cantidad de salidas PWM, diferentes pines utilizan diferentes temporizadorespara millis y micros.  
El temporizador por defecto puede cambiarse añadiendo la opción `-DMILLIS_USE_TIMERBx` en el archivo platformio.ini, donde *x* representa el temporizador TCB de 0 a 3. Alternativamente, el archivo *pins_arduino.h* puede modificarse al utilizar el IDE de Arduino. Considere que realizar estos cambios resultará en un pinout incompatible con algunos proyectos basados en Arduino Nano.

| Pinout          | Temporizador para millis and micros |
|-----------------|-------------------------------------|
|      NanoX      | TCB2                                |


## Pinout
Este núcleo provee solo el mapeo de pines de la tarjeta NanoX, diseñada especificamente para coincidir 1-a-1 con el pinout de la tarjeta de desarrollo Arduino Nano. Si desea trabajar con una plataforma diferente considere revisar el proyecto [MegaCoreX](https://github.com/MCUdude/MegaCoreX) por [MCUdude](https://github.com/MCUdude)
- **NanoX pin standard**: Este pinout es compatible 1-a-1 con el pinout de Arduino Nano. Para obtener este resultado los pines de salidas PWM e I2C se multiplexaron. Sin embargo, esto puede ocasionar "colisiones" entre el periferico ADC y el periferico I2C. Activar el periferico I2C cambiara los pines PF3 & PF4 a un estado de alta impedancia, si después intenta leer/escribir de estos pines causará un comportamiento indefinido en el periferico I2C, considere esta limitación al trabajar con la tarjeta de desarrollo.

Para más información revise el archivo pins_arduino.h para información detallada.<br/> <br/>
>>TODO: Insert board pinout
<!-- <b>Click to enlarge:</b>

| **MegaCoreX ATmega809/1609/3209/4809 pinout**                                                                                                                                 | **MegaCoreX ATmega808/1608/3208/4808 pinout**                                                                                                                                                                                        |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|<img src="https://i.imgur.com/3PUBB6H.jpg" width="350"><br/><img src="https://i.imgur.com/QbOOOTd.png" width="350"><br/><img src="https://i.imgur.com/Hp21SXJ.jpg" width="350">|<img src="https://i.imgur.com/2YlmE8p.png" width="350"><br/><br/><br/><br/><img src="https://i.imgur.com/zXyWvi1.png" width="350"><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>| -->


## Características de hardware
A continuación se describen caracteristicas de hardware que difieren la familia AVR.

### Pines de interrupción
A diferencia de la antigua familia AVR, los megaAVR-0 tienen interrupciones en todos los pines.
Estados compatibles son *RISING*, *FALLING*, *CHANGE*, *HIGH* y *LOW*. Esto significa que el uso de `digitalPinToInterrupt` no es requerido, pero su uso es requerido para mantener la compatibilidad de su código con otras tarjetas, si no planea crear un proyecto multiplataforma puede llamar a la rutina de interrupciones de la siguiente manera:

```c
attachInterrupt(myPin, myInterruptFunction, RISING);
```

### Salidas PWM
Salida PWM, `analogWrite()`, esta disponible para los siguientes pines:

| Pinout            | Número de pines PWM | Pines PWM                 |
|-------------------|--------------------|------------------------------------|
| |
| *NanoX pin standard* | 8                  | 3, 5, 6, 9, 10, 14, 15, 16       |

La frecuencia de las salidas PWM puede cambiarse utilizando la función `analogWriteFrequency(kHz)`, donde `kHz` los valores 1 (default), 4, 8, 16, 32 y 64 están permitidos. Considere que estos valores son aproximados. La función tratará de obtener el valor indicado en base a las limitaciones del propio hardware.

Consideré que la función tone() utiliza TCB1, asi que las salidas PWM correspondientes no estáran disponibles al utilizarla, por defecto el pin digital 3.

### Lógica personalizada configurable (CCL)
Los microcontroladores megaAVR-0 están equipados con cuatro bloques independientes de lógica configurable, que pueden utilizarse para mejorar la velocidad y desempeño. Los pines CCL están marcados en el diagrama pinout. Los bloques lógicos pueden utilizarse de manera independient unos de otros, conectarse juntos o generar interrupciones a la CPU. MCUdude desarrolló una  [librería ligera, y de alto nivel](https://github.com/MCUdude/MegaCoreX/tree/master/megaavr/libraries/Logic) para el uso del hardware CCL.

>Note: Si encuentra problemas al usar esta librería junto al núcleo NanoCoreX, por favor realize el reporte [NanoCoreX repository](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX) primero, ya que puede tratarse de un error especifico de este fork.

### Comparador analógico (AC)
Los microcontroladores megaAVR-0 están equipados con un comparador analógico de hardware. Compara valores de voltaje de dos entradas y genera una salida digital basada en esta comparación. El chip megAVR tiene cuatro pines AC positivos y tres pines AC negativos. También integra un generador de voltage de referencia interno que puede utilizarse en lugar de una referencia externa.
revise [la librería Comparator](https://github.com/MCUdude/MegaCoreX/tree/master/megaavr/libraries/Comparator) para más información y ejemplos.

>Note: Si encuentra problemas al usar esta librería junto al núcleo NanoCoreX, por favor realize el reporte [NanoCoreX repository](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX) primero, ya que puede tratarse de un error especifico de este fork.

### Eventos de sistema (EVSYS)
Los eventos de sistema (EVSYS) permite señalización directa periférico a periférico. Permite que el cambio en un periférico (el generador de evento) dispare una acción en otros periféricos (el usuario de evento) a través de canales de eventos, sin utilizar la CPU. Está diseñada para proveer respuestas predecibles en cortos intervalos de tiempo entre periféricos, permitiendo control e interacción autonoma entre periféricos. Es una poderosa herramienta para redicor la complejidad, tamaño y tiempo de ejeción del software. Revise la [librería Event](https://github.com/MCUdude/MegaCoreX/tree/master/megaavr/libraries/Event), allí encontrará documentación y ejemplos de uso.

>Note: Si encuentra problemas al usar esta librería junto al núcleo NanoCoreX, por favor realize el reporte [NanoCoreX repository](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX) primero, ya que puede tratarse de un error especifico de este fork.

### Cambio de los pines de los periféricos
El microcontrolador megaAVR-0 permite asignar pines alternativos en sus periféricos de hardware.<br/>
NanoCoreX actualmente soporta el intercambio de pines en los periféricos SPI, i2c y UART.

[**Lea la documentación Extendida de la API para obtener más información**](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/tree/master/Extended-API.md#peripheral-pin-swapping)


## Guía de instalación
#### Instalar a través del gestor de tarjetas
* Ejecute el IDE de Arduino.
* Despliegue la opción **Archivo > Preferencias** del menú.
* Ingrese la siguiente URL en **Gestor de URLs Adicionales de Tarjetas**:
    ```
    TODO: url
    ```
* Separe las URLs utilizando commas ( **,** ) si tiene más de una URL
* Selecciona la opción **Herramientas > Placa > Gestor de tarjetas...** del menu.
* Espere que la descarga de indice de plataformas termine.
* Navegue hasta la opción **NanoCoreX** y haga clic en ella.
* Haga clic en **Instalar**.
* Después de que termine la instalación cierre la ventana del **Gestor de tarjetas**.

#### PlatformIO
[PlatformIO](http://platformio.org) es un ecosistema para el desarrollo de IoT de código abierto, soporte para NanoCoreX en camino.

**Vea [PlatformIO.md](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/PlatformIO.md) para más información.**

## Preparación del hardware

### NanoX
La NanoX, es la tarjeta compatible con Arduino Nano de Microside. Utiliza el ATmega4808 y el único *only* pinout compatible es *NanoX*. La NanoX utiliza un bootloader que utiliza el periférico UART0, aunque es posible utilizar otros puertos UART estas opciones están deshabilitadas del menú para evitar confusiones. Si tiene problemas para cargar programas a su tarjeta, conecte un programador UPDI al pin UPDI de la NanoX, seleccione el programador que está utilizando en el menu y haga clic en la opción "Quemar bootloader".
Clic para maximizar:

>>TODO: Board pinout image
<!-- <img src="https://i.imgur.com/ykebGW7.png" width="400"> -->