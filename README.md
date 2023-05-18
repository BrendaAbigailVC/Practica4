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

