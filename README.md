   # Proyecto-Raspberry-pi 

# Ejercicio 5 Documentación
## Descripción

- El Raspberry Pi Pico es una placa de desarrollo económica creada por la Fundación Raspberry Pi, basada en el microcontrolador RP2040. 

## Características Principales
- Microcontrolador RP2040: Diseñado con un microcontrolador ARM Cortex-M0+, el RP2040 ofrece un rendimiento eficiente y es adecuado para diversas aplicaciones.

- Control total: Desde luces hasta sistemas eléctricos el Raspberry Pi Pico da el poder de controlar los sitemas integrados de tu hogar desde el baño, cocina o el lobby hasta operaciones industriales.

- E/S y Conectividad: Con una variedad de pines GPIO y soporte para interfaces como SPI, I2C, UART y PWM, el Pico permite la conexión de sensores, actuadores y otros dispositivos.

- Programación en C/C++ y MicroPython: Los interesados en la herramienta pueden programar el Pico utilizando el lenguaje oficial basado en C/C++ o aprovechar la simplicidad de MicroPython ( lenguaje Python ).

## Desarrollo de Software
- Raspberry Pi Pico SDK: Se proporciona información detallada sobre el SDK, que incluye herramientas y bibliotecas esenciales para el desarrollo de software.

- Programación en Ensamblador: Para aquellos con experiencia avanzada, la documentación cubre la programación en ensamblador, ofreciendo mayor control sobre el hardware.

## Personalización y Firmware
- Firmware Personalizable: La capacidad de reprogramar el firmware permite adaptar la funcionalidad del Pico según los requisitos del proyecto.

## Gestión de Energía
- Alimentación y Consumo de Energía: Se detallan estrategias para optimizar el consumo de energía, especialmente relevante para proyectos con restricciones de energía.

##Comunidad y Recursos Adicionales
- Comunidad Activa: Además de la documentación oficial, hay una comunidad en línea activa que comparte proyectos, problemas comunes y soluciones, proporcionando un valioso recurso para el aprendizaje y la resolución de problemas.

## Otras Características Relevantes
- Adaptador USB incorporado: El Pico tiene un adaptador USB incorporado, simplificando la conexión, facilitando la programación y la alimentación.

- Reloj y Temporizadores Avanzados: Ofrece capacidades avanzadas de temporización y reloj, lo que es crucial para aplicaciones que requieren sincronización precisa.

- Memoria Flash Incorporada: Equipado con memoria flash, el Pico permite almacenar programas y datos de manera eficiente.

## Especificaciónes Puntuales:
- Factor de forma: 21 mm × 51 mm
- Chip microcontrolador RP2040: Diseñado por Raspberry Pi en el Reino Unido.
- Procesador de doble núcleo Arm Cortex-M0+: Con reloj flexible de hasta 133 MHz.
- 264 kB de SRAM incorporada: En el chip.
- 2 MB de memoria flash QSPI incorporada: En la placa.
- Red LAN inalámbrica 802.11n de 2.4 GHz (solo en Raspberry Pi Pico W y Pico WH).
- Bluetooth 5.2 (solo en Raspberry Pi Pico W y Pico WH).
- 26 pines GPIO multifunción, incluyendo 3 entradas analógicas.
- 2 controladores UART, 2 controladores SPI, 2 controladores I2C y 16 canales PWM.
- Controlador USB 1.1 y PHY, con soporte para funciones de host y dispositivo.
- 8 máquinas de estado de E/S programables (PIO) para periféricos personalizados.
- Voltaje de entrada admitido: 1.8–5.5V DC.
- Temperatura de funcionamiento: -20°C a +85°C (Raspberry Pi Pico y Pico H); -20°C a +70°C (Raspberry Pi Pico W y Pico WH).
- Módulo castellado: Permite la soldadura directa en placas portadoras (solo en Raspberry Pi Pico y Pico W).
- Programación mediante arrastrar y soltar usando almacenamiento masivo a través de USB.
- Modos de bajo consumo y en espera.
- Reloj preciso en el chip.
- Sensor de temperatura.
- Bibliotecas aceleradas de enteros y punto flotante en el chip.



# Ejercicio 6 Caso de Estudio
- ¿Cómo se ejecuta este programa?

   El código en la función setup() se ejecuta una vez al inicio del programa, y el código en la función loop() se ejecuta en un bucle después de que se complete setup(). En este caso, la función setup() llama a la función task1() una vez al inicio, y la función loop() también llama a la función task1() en un loop.

- ¿Pudiste ver este mensaje: Serial.print("Task1States::WAIT_TIMEOUT\n");? ¿Por qué crees que ocurre esto?
  
   No, debido a que la instrucción de imprimir el texto está después de la instrucción de cambiar el estado de la máquina, por lo que la máquina cambiaba de estado antes de poder imprimir el mensaje, pero agregando un delay y moviendo la instrucción arriba del cambio de estado permite al micro-crontrolador procesar e imprimir el mensaje correctamente.
  
- ¿Cuántas veces se ejecuta el código en el caso Task1States::INIT?

Solo una vez, cuando se inicia el programa, posteriormente se cambia al estado de WAIT_TIMEOUT y este se queda en un loop.


# Ejercicio 7 Análisis del Programa de Prueba
- La funcion millis(); ayuda a  devolver el nuemero de milisegundos desde que el programa comienza a ejecutarse ,esto es  útil para medir el tiempo entre eventos, implementar temporizadores o cronómetros en programas para  Arduino.
