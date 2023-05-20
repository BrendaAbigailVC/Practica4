# Practica de laboratorio 5

## Funcionamiento del proyecto
En el proyecto los LEDs muestran el valor binario de una variable. Está formado por dos push button que cumplen con las siguientes funciones.

* Si se oprime el push button A se incrementa una unidad el valor de la variable.
 
* Si se oprime el push button B se decrementa una unidad al valor de la variable. 

* Si se oprime ambos botones el valor de la variable se resetea.

## Paquetes necesarios:
Para el funcionamiento de la blue-pill se necesita la instalación del siguiente software necesario.

* Paquete del compilador cruzado para generar el código máquina para microcontroladores: gcc-arm-none-eabi. 

* Paquete para grabar un microcontrolador STM32 mediante el dispositivo ST-Link V2: stlink-tools. 

* Paquete con los controladores que detectan la conexión con el ST-Link V2: libusb-1.0-0-dev.

## Alias necesarios
* alias arm-gcc=arm-none-eabi-gcc

* alias arm-as=arm-none-eabi-as

* alias arm-objdump=arm-none-eabi-objdump

* alias arm-objcopy=arm-none-eabi-objcopy
## Compilación del software

* Lectura: Lee la tarjeta de desarrollo blue pill mediante el comando: st-flash read dummy.bin 0 0xFFFF

* Ensamble: Se ensambla el contenido del archivo blink.s mediante el comando: arm-as blink.s -o blink.o 

* Objeto binario: Se construye un objeto binario a partir del objeto blink.o mediante el comando: arm-objcopy -O binary blink.o blink.bin

* Código máquina: Escribe el código máquina almacenado del archivo blink.bin en la memoria flash del µC mediante el comando st-flash write 'blink.bin' 0x8000000

## Diagrama electronico del circuito
<img width="218" alt="n" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/ebc3bb72-86f8-41f4-b6fe-289420e53c4b">

 <img width="218" alt="m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/c040f277-fa84-4898-a1cf-c6ef15f3d2ba">

![Circuito](https://github.com/BrendaAbigailVC/Practica4/assets/109320578/ac36ea0d-788c-4ba7-a563-743dc852c441)

## Lógica del programa en código de alto nivel 

<img width="305" alt="CLogica" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/fde6bcd2-c8b5-49cd-8cb0-9b5a10832b53">

## Funciones

### wait_ms
Esta función permite generar el retraso necesario que necesita la función is_button_pressed para eliminar el efecto debouncing.

<img width="216" alt="Captura de pantalla 2023-05-18 a la(s) 10 29 41 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/7b81d8cb-8ef6-4674-beab-6838e7218fa7">

### actions
La función detecta si se están presionando alguno de los dos botones y a partir de eso poder determinar que realizar en este caso incrementar, decrementar o resetear la variable que representa el número en binario. 

<img width="218" alt="Captura de pantalla 2023-05-19 a la(s) 11 02 02 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/bb307f67-6438-4e5b-a821-f396c4766926">

### is_button_pressed
Esta funcion permite eliminar el efecto debouncing. Cuando se presiona un botón mecánico, dos contactos de metal chocan entre sí e inmediatamente rebotan un par de veces antes de fijarse. Estos rebotes producen múltiples señales en unos pocos milisegundos debido a los efectos de rebote. Existen soluciones tanto de hardware como de software para eliminar los efectos de rebote. Estas soluciones se denominan debouncing(antirrebote). La técnica de eliminación de rebotes de software más fácil es la técnica wait-and-see. Cuando el programa detecta que se presiona un botón, vuelve a examinar la señal de entrada después de un breve retraso. Si la señal de entrada aún muestra que se presionó el botón, el programa informa que el botón se presionó efectivamente.

<img width="205" alt="Captura de pantalla 2023-05-19 a la(s) 11 01 07 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/5ca9d3a9-d7ae-493f-a150-2f8f3996ac39">

### setup

En la primera parte de esta función se hace la configuración de los pines de salida y de entrada de la blue pill. 
En la segunda parte está la lógica que determina cómo deben de encender los LEDs en este caso representando una variable binaria.

<img width="216" alt="Captura de pantalla 2023-05-18 a la(s) 10 24 10 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/700d2901-3df7-4e43-8298-021ea5af53f5">
