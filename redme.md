# Practica de laboratorio 6

## Funcionamiento del proyecto
El funcionamiento del proyecto consiste en implementar un contador binario de 10 bits. La salida de esto se notará en 10 leds. Este contador aumenta la cuenta cada segundo. 

Al oprimir un botón el contador cambia su modo de operación ascendente a descendente. Así mismo al presionar otro botón el contador aumenta su velocidad x2, x4, x8. Cuando la velocidad aumente una vez más después de estar en x8 entonces la velocidad regresará a x1.

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

* Al tener el archivo MAKEFILE primero utilizamos el comando make clean para verificar que no tenemos archivos .o innecesarios. Luego ejecutamos el comando make para crear los archivos .o que si se requieren. Por último escribimos el código máquina almacenado del archivo prog.bin en la memoria flash del µC mediante el comando make write.

## Diagrama electronico del circuito

![CircuitoPrac6](https://github.com/BrendaAbigailVC/Practica4/assets/109320578/57d957ac-4e26-408e-858b-940d2ad60bf6)

## Configuraciones
### Configuración de perifericos
Los pasos para configurar los puertos como entradas y salidas son:

* Habilita el reloj en el puerto requerido para ello se configura se habilita el bit en el registro RCC_APB2ENR_OFFSET según corresponda.
* Configurar el modo de funcionamiento de los puertos.
* Configurar la dirección de los pines para ello se configuran los bits correspondientes en los registros de dirección de los puertos para establecer la dirección deseada.

### Configuración del EXTI10_11
EXTI es un módulo periférico que se encarga de las interrupciones externas en el microcontrolador.
El EXTI en el Cortex-M3 es parte del controlador de interrupciones NVIC (Nested Vectored Interrupt Controller).
Proporciona la capacidad de configurar fuentes de interrupción, controlar prioridades y enmascarar interrupciones externas.
Los pasos para configurarlo son:
* Configurar EXTI_FTSR para deshabilitar la detección de flanco descendente.
* Mascara para habilitar bit 10 (EXTI10) y bit 11(EXTI11).
* Configurar EXTI_RTSR para habilitar la detección de flanco ascendente.
* Configurar EXTI_IMR para habilitar las interrupciones  externas en EXTI10 y EXTI11 correspondiente a la posición 11 y 10 en el registro EXTI_IMR.
* Habilitar la interrupción EXTI10 y EXTI11 en el NVIC. El NVIC es un registro que se utiliza para habilitar las interrupciones en el Cortex-M3.

### Configuración del SysTick
El SysTick es un temporizador integrado en los microcontroladores ARM Cortex-M. Es un componente que se encarga del control del tiempo y operaciones temporizadas. Se puede utilizar para generar interrupciones periódicas y proporcionar una referencia de tiempo en aplicaciones de tiempo real.
El Systick timer cuenta hacia abajo desde un valor de recarga (STRELOAD) hasta cero, y cuando el contador alcanza cero, se genera una interrupción. Después de la interrupción se recarga y repite la cuenta.
Los pasos para configurarlo son:
* Configurar el registro de control (SysTick control) y el estado de (SysTick_CTRL).
* Establecer el registro de valores de recarga del (Systic_LOAD) y especificar el número de ticks entre dos interrupciones.
* Borrar el registro de valor actual (SysTick_VAL).
* Configurar la prioridad de interrupción programando el SBC->SHP.
* Habilitar la interrupción del SysTick configurando el bit TICKINT (SysTick_CTRL) para habilitar SysTick IRQ y SysTick timer.

## Funciones
### main
En la primera parte se hace la configuración de los pines de salida y de entrada de la blue pill en este caso los pines del A0 al A9 funcionan como salidas y los pines A10 y A11 funcionan como entradas. 
Luego se declara la configuración del EXTI15_10 para configurar EXTI_10 y EXTI_11.
Luego se inicializan las variables en registros que serán directamente utilizados en las funciones.

### output
La función recibe como argumento la variable contador que permite representar la cuenta binaria en los leds. 

### EXTI15_10_Handler
Esta función se encarga de manejar las interrupciones del EXTI.
* Lo primero que hace es leer el estado del registro EXTI_PR para verificar si se ha producido la interrupción en el EXTI. EXTI_PR es un registro que indica que interrupciones ocurrieron el EXTI10 o EXTI11.
* Si las interrupciones ocurrieron entonces se puede leer el estado del puerto A11 o A10 para indicar si está presionado o no y a partir de eso saltar a la instrucción correspondiente de cada interrupción.

### delay

En esta función se recibe como argumento el resultado de la función check_speed, este resultado representa la cantidad de retraso en unidades de tiempo que el SysTick_Handler establece. En la función se declara un bucle que termina únicamente cuando el valor del argumento ha sido reducido a cero con ayuda de la función SysTick_Handler.

### check_speed

Contiene la lógica que permite retornar el valor de tiempo de retardo para aumentar la velocidad de encendido de los leds.

### SysTick_Initialize

Permite generar interrupciones periódicas. En esta función se inicializan los registros para habilitar el temporizador.
 
### SysTick_Handler

Permite ir decrementando el valor del retraso en unidades de tiempo en el programa y así poder notar la velocidad con la que prenden los leds.

## Hardware del sistema

Los leds se conectan con su respectiva resistencia y con los puertos de salida que en este caso van del PA0 al PA9.
Los botones se conectan con los puertos PA10 y PA11, sin embargo para eliminar el efecto rebote, de la señal de hardware, se utiliza un disparador schmitt.

![CIRCUITO](https://github.com/BrendaAbigailVC/Practica4/assets/109320578/c978f11f-75db-47fd-81e5-0f19c4269109)

El diagrama del schmitt es el siguiente:

![SN74HC14N](https://github.com/BrendaAbigailVC/Practica4/assets/109320578/a5066e69-c764-4983-873e-912c3f1cb1f4)
