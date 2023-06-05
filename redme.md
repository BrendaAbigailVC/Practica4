# Practica de laboratorio 6

## Funcionamiento del proyecto

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
*
## Compilación del software

* Lectura: Lee la tarjeta de desarrollo blue pill mediante el comando: st-flash read dummy.bin 0 0xFFFF

* Al tener el archivo MAKEFILE primero utilizamos el comando make clean para verificar que no tenemos archivos .o innecesarios. Luego ejecutamos el comando make para crear los archivos .o que si se requieren. Por último escribimos el código máquina almacenado del archivo prog.bin en la memoria flash del µC mediante el comando st-flash write prog.bin' 0x8000000

## Diagrama electronico del circuito

![CircuitoPrac6](https://github.com/BrendaAbigailVC/Practica4/assets/109320578/57d957ac-4e26-408e-858b-940d2ad60bf6)

## Lógica del programa en código de alto nivel 

## Configuraciones
### Configuración de perifericos
* Primero se habilita el reloj en el puerto A.
* Configura los pines del 0 al 7 en el puerto GPIOA_CRL.
* Configura los pines del 8 al 15 en GPIOA_CRH.

### Configuración del EXTI10
* Configurar EXTI_FTSR para deshabilitar la detección de flanco descendente.
* Mascara para habilitar bit 10 (EXTI10) y bit 11(EXTIO11).
* Configurar EXTI_RTSR para habilitar la detección de flanco ascendente.
* Configurar EXTI_IMR para habilitar interrupciones en EXTI10 y EXTI11.
* Habilitar la interrupción EXTI10 y EXTI11 en el NVIC.

### Configuración del SysTick
* Configurar el registro de control (SysTick control) y el estado de (SysTick_CTRL).
* Establecer el registro de valores de recarga del (Systic_LOAD) y especificar el numero de ticks entre dos interrupciones.
* Borrar el registro de valor actual (SysTick_VAL).
* Configurar la prioridad de interrupción programando el SBC->SHP.
* Habilitar la interrupción del SysTick cconfigurando el bit TICKINT (SysTick_CTRL) para habilitar SysTick IRQ y SysTick timer.

## Funciones
### main

En la primera parte de esta función se hace la configuración de los pines de salida y de entrada de la blue pill. 
En la segunda parte está la lógica que determina cómo deben de encender los LEDs en este caso representando una variable binaria.

### main
