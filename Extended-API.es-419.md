[![en](https://img.shields.io/badge/lang-english-red.svg)](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/Extended-API.md)
[![es](https://img.shields.io/badge/lang-español-blue.svg)](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/Extended-API.es-419.md)

# Extended Arduino API

NanoCoreX es completamente compatible con Arduino Nano, pero también incluye funcionalidades adicionales que no se encuentran disponibles en el núcleo oficial "megaavr" de Arduino.

## Table of contents
* [Resolución de analog read](#resolución-de-analog-read)
* [IO Rápido](#io-rápido)
* [Cambio de los pines de los periféricos](#cambio-de-los-pines-de-los-periféricos)
* [pinConfigure](#pinConfigure---configuración-extendida-de-pines)
* [Soporte de Printf](#soporte-de-printf)

## Resolución de analog read
La resolución por defecto de estos chips es de 10 bits, lo que te permite leer valores entre 0 a 1023. Si necesita una resolución menor, puede configurarla a 8 bits, lo que permite leer valores entre 0 a 255.
Utilice la función `analogReadResolution`:
```c
analogReadResolution(8);  // Set resolution to 8 bits
analogReadResolution(10); // Set resolution to 10 bits
```

## IO Rápido
Para aplicaciones donde el tiempo de respuesta es crítico la función `digitalRead()` y `digitalWrite()` podrían ser demasiado lentas. Para solucionar esto, NanoCoreX incluye una variante que se compila a una sola instrucción de la CPU.
Llame a la función `digitalReadFast(myPin)` o `digitalWriteFast(mypin, state)`.<br/>
**Nota el número de pin y el estado del pin deben conocerse en tiempo de compilación!**

### Declaración
```c++
uint8_t digitalReadFast(uint8_t pinNumber);
void digitalWriteFast(uint8_t pinNumber, uint8_t state);
```

## Cambio de los pines de los periféricos
Los microcontroladores megaAVR-0 permiten el uso de pines alternativos para algunos de sus periféricos.
Utilice la función `swap()` o `pins()` antes de llamar al método `begin()` del periférico.
La función regresará `true` si el cambio se realizó correctamente.
Para el periférico `Serial` el método es `pins(tx,rx)`, para `Wire` es `pins(sda,scl)` y para `SPI` es `pins(mosi,miso,sck,ss)`.
(Nota: esta es la misma secuencia utilizada por el método `pins` del ESP8266, pero diferente al método que SoftwareSerial utiliza.)

Observe que `swap()` y `pins()` hacen exactamente lo mismo, pero `swap()` es orientado a la multiplexión, mientras que `pins()` es orientado a los pines (revise la hoja de datos del chip).

Si desea utilizar esta característica para implementar comunicación con dos dispositivos externos conectados a pines diferentes, pero que compartan el mismo periférico interno, la forma apropiada es invocar primero a la método `end()` para finalizar la comunicación correctamente, después utilizar  `swap()` o `pins()` para cambiar los pines, y finalmente invocar nuevamente al método `begin()`.

`swap()` o `pins()` se invocan de la siguiente manera. **Utilice `swap()` o `pins()`, pero no ambos!**

``` c++
// UART pin swapping
Serial3.swap(1);
Serial3.begin(9600);

// Wire pin swapping
Wire.swap(1);
Wire.begin();

// SPI pin swapping
SPI.swap(1);
SPI.begin();
```

Combinaciones de pines disponibles para la tarjeta *NanoX* son:

| Periférico | Por defecto                        | Alternativo                      |
|------------|--------------------------------|----------------------------------|
| Serial     | swap(0)  **o**  pins(0,1)     | swap(1)  **o**  pins(4,5)       |
| Serial1    | swap(0)  **o**  pins(8,9)     |                                  |
| Serial2    | swap(0)  **o**  pins(20,21)   | swap(1)  **o**  pins(24,25)     |
| Wire       | swap(0)  **o**  pins(2,3)     | swap(1)  **o**  pins(10,11)     |
| SPI        | swap(0)  **o**  pins(4,5,6,7) | swap(1)  **o**  pins(8,9,10,11) |

**Nota: considere que el periférico i2c está multiplexado por defecto, lo que significa que los pines**

## pinConfigure - configuración extendida de pines
pinConfigure es una función plantilla que permite configurar todas las funciones de un pin en una sola línea. El primer argumento es el número de pin a configurar, del segundo al enésimo parámetro son las posibles configuraciones del pin. El orden de los argumentos no importa. La función configurará el pin incluso si la configuración carece de sentido, tenga cuidado de ello. Si habilita las interrupciones y no existe una función que administre esa interrupción el microcontrolador fallará.

### Declaración
```c++
template <typename... MODES>
void pinConfigure(const uint8_t digital_pin, const pin_configure_t mode, const MODES&... modes);
```

### Ejemplo
```c++
// Set pin PA0 to output and drive the pin high
pinConfigure(PIN_PA0, PIN_DIR_OUTPUT, PIN_OUT_HIGH);
// Disable pin PA1 completely to save power
pinConfigure(PIN_PA1, PIN_INPUT_DISABLE);
```

### Tabla de modos válidos de pines
La siguiente configuración puede utilizarse:
| Funcionalidad |   Habilitar  | Deshabilitar            | Conmutar |
|---------------|-------|---------------------|--------------------|
| Dirección, pinMode() | `PIN_DIR_OUTPUT`<br/>`PIN_DIR_OUT`<br/>`PIN_DIRSET` | `PIN_DIR_INPUT`<br/>`PIN_DIR_IN`<br/>`PIN_DIRCLR`       | `PIN_DIR_TOGGLE`<br/>`PIN_DIRTGL` |
| Pin output, `HIGH` o LOW | `PIN_OUT_HIGH`<br/>`PIN_OUTSET`         | `PIN_OUT_LOW`<br/>`PIN_OUTCLR`          | `PIN_OUT_TOGGLE`<br/>`PIN_OUTTGL`       |
| Pullup Interno | `PIN_PULLUP_ON`<br/>`PIN_PULLUP`        | `PIN_PULLUP_OFF`<br/>`PIN_NOPULLUP`       | `PIN_PULLUP_TGL`       |
| Invertir `HIGH` y `LOW` |`PIN_INVERT_ON`        | `PIN_INVERT_OFF`       | `PIN_INVERT_TGL`       |
| Buffer de entrada Digital | `PIN_INPUT_ENABLE` o<br/> `PIN_ISC_ENABLE`    | `PIN_ISC_DISABLE` o<br/>`PIN_INPUT_DISABLE`    | No  soportado |
| Interrupt on change | `PIN_ISC_ENABLE` o<br/> `PIN_INPUT_ENABLE`       | `PIN_ISC_ENABLE` or<br/>`PIN_ISC_DISABLE`     | No aplica |
| Interrupt on Rise  | `PIN_ISC_RISE` o<br/> `PIN_INT_RISE`         | `PIN_ISC_ENABLE` or<br/>`PIN_ISC_DISABLE`     | No aplica |
| Interrupt on Fall  | `PIN_ISC_FALL` o<br/> `PIN_INT_FALL` | `PIN_ISC_ENABLE` or<br/>`PIN_ISC_DISABLE`      | No aplica |
| Interrupt on LOW  | `PIN_ISC_LEVEL`  o<br/> `PIN_INT_LEVEL` | `PIN_ISC_ENABLE` or<br/>`PIN_ISC_DISABLE`      | No aplica |

## Soporte de printf
A diferencia del núcleo oficial de Arduino, MegaCoreX tiene soporte para printf, y por extensión NanoCoreX también la tiene. Si no está familiarizado con esta función puede aprender más [en este enlace](https://www.it.uc3m.es/pbasanta/asng/course_notes/input_output_printf_es.html). Está añadida a cualquier clase que extienda de la clase Print. Printf es una función estándar en C que le permite dar formato al texto de manera más sencilla en comparación a las funciones print y println de Arduino.

Observe que la implementación por defecto de printf NO mostrará valores flotantes o doubles de manera nativa. Esta limitación extiende de la implementación de printf en avr-libc, pero puede habilitarse con un parámetro de compilación. Esto es sencillo si está utilizando [PlatformIO](https://github.com/MICROSIDE-TECHNOLOGY/NanoCoreX/blob/master/PlatformIO.md).

Si está utilizando el monitor serial, puede simplemente llamar a la función de la siguiente manera `Serial.printf("Milliseconds since start: %ld\n", millis());`. Otras librerías que hereden de la clase Print como SoftwareSerial, pueden llamar a la función printf sin realizar configuraciones, incluyendo a la librería LiquidCrystal LCD, u8g2 librería grafica LCD.