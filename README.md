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

## Lógica del programa en código de alto nivel 

<img width="438" alt="CodigoLogica" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/30b4a4d4-ab0c-4799-855e-0c65814f692e">

## Funciones

### wait_ms
Cuando se presiona un botón mecánico, dos contactos de metal chocan entre sí e inmediatamente rebotan un par de veces antes de fijarse. Estos rebotes producen múltiples señales en unos pocos milisegundos debido a los efectos de rebote.
Existen soluciones tanto de hardware como de software para eliminar los efectos de rebote. Estas soluciones se denominan debouncing(antirrebote).
La técnica de eliminación de rebotes de software más fácil es la técnica wait-and-see. Cuando el programa detecta que se presiona un botón, vuelve a examinar la señal de entrada después de un breve retraso, generalmente entre 20 y 50 ms. Si la señal de entrada aún muestra que se presionó el botón, el programa informa que el botón se presionó efectivamente. 
<img width="216" alt="Captura de pantalla 2023-05-18 a la(s) 10 29 41 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/e616c9e8-b26e-4d35-b01b-f0d5f17fd7ff">


### is_button_pressed
La función detecta si se están presionando alguno de los dos botones y a partir de eso poder determinar que realizar en este caso incrementar, decrementar o resetear la variable que representa el número en binario. 
<img width="226" alt="Captura de pantalla 2023-05-18 a la(s) 10 27 24 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/76a1c423-21f7-4a2b-8041-a5ca113ba0f1">


### setup

En la primera parte de esta función se hace la configuración de los pines de salida y de entrada de la blue pill. 
En la segunda parte está la lógica que determina cómo deben de encender los LEDs en este caso representando una variable binaria.
<img width="216" alt="Captura de pantalla 2023-05-18 a la(s) 10 24 10 a m" src="https://github.com/BrendaAbigailVC/Practica4/assets/109320578/6a5c72f0-441c-4959-b69c-db3a7de0d6b0">


