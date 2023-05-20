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

* Al tener el archivo MAKEFILE primero utilizamos el comando make clean para verificar que no tenemos archivos .o innecesarios. Luego ejecutamos el comando make para crear los archivos .o que si se requieren. Por último escribimos el código máquina almacenado del archivo prog.bin en la memoria flash del µC mediante el comando st-flash write prog.bin' 0x8000000

## Diagrama electronico del circuito
<img width="218" alt="n" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/ebc3bb72-86f8-41f4-b6fe-289420e53c4b">

 <img width="218" alt="m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/c040f277-fa84-4898-a1cf-c6ef15f3d2ba">
 
![Circuito](https://github.com/BrendaAbigailVC/Practica4/assets/109320578/53942ed8-f569-4dbf-abe8-075ff9149f62)

## Lógica del programa en código de alto nivel 

<img width="305" alt="CLogica" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/47f8f979-02f2-458c-8b88-8753ac085e20">


## Funciones

### wait_ms

Esta función permite generar el retraso necesario que necesita la función is_button_pressed para eliminar el efecto debouncing.

<img width="216" alt="Captura de pantalla 2023-05-18 a la(s) 10 29 41 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/a8683f94-51e4-4fa4-b51a-ef3a41b6616e">

### actions
La función detecta si se están presionando alguno de los dos botones y a partir de eso poder determinar que realizar en este caso incrementar, decrementar o resetear la variable que representa el número en binario. 

<img width="218" alt="Captura de pantalla 2023-05-19 a la(s) 11 02 02 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/68ce677c-a28a-4650-8a41-28a47c408c47">


### is_button_pressed

Esta función permite eliminar el efecto debouncing. Cuando se presiona un botón mecánico, dos contactos de metal chocan entre sí e inmediatamente rebotan un par de veces antes de fijarse. Estos rebotes producen múltiples señales en unos pocos milisegundos debido a los efectos de rebote. Existen soluciones tanto de hardware como de software para eliminar los efectos de rebote. Estas soluciones se denominan debouncing(antirrebote). La técnica de eliminación de rebotes de software más fácil es la técnica wait-and-see. Cuando el programa detecta que se presiona un botón, vuelve a examinar la señal de entrada después de un breve retraso. Si la señal de entrada aún muestra que se presionó el botón, el programa informa que el botón se presionó efectivamente.

<img width="205" alt="Captura de pantalla 2023-05-19 a la(s) 11 01 07 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/a058bd18-d7ce-4dfe-b0b2-74db23c0b1cb">

### main

En la primera parte de esta función se hace la configuración de los pines de salida y de entrada de la blue pill. 
En la segunda parte está la lógica que determina cómo deben de encender los LEDs en este caso representando una variable binaria.

<img width="216" alt="Captura de pantalla 2023-05-18 a la(s) 10 24 10 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/dcb4668d-73f3-4659-9f11-8ab587fd0e33">

