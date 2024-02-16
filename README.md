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
- La funcion millis(); ayuda a  devolver el nuemero de milisegundos desde que el programa comienza a ejecutarse, esto es  útil para medir el tiempo entre eventos, implementar temporizadores o cronómetros en programas para  Arduino.

# Ejercicio 8 Retrieval practice (evaluación formativa)
```
void task1()
{
    // Definición de estados y variable de estado
    enum class Task1States
    {
        INIT,
        WAIT_TIMEOUT1,
        WAIT_TIMEOUT2,
        WAIT_TIMEOUT3
    };
    static Task1States task1State = Task1States::INIT;

    // Definición de variables static (conservan su valor entre llamadas a task1)
    static uint32_t lastTime = 0;

    // Constantes
    constexpr uint32_t INTERVAL1 = 1000;
    constexpr uint32_t INTERVAL2 = 2000;
    constexpr uint32_t INTERVAL3 = 3000;

    // MÁQUINA de ESTADOS
    switch (task1State)
    {
    case Task1States::INIT:
    {
        // Acciones:
        Serial.begin(115200);

        // Garantiza los valores iniciales para el siguiente estado.
        lastTime = millis();
        task1State = Task1States::WAIT_TIMEOUT1;

        break;
    }
    case Task1States::WAIT_TIMEOUT1:
    {
        uint32_t currentTime = millis();

        // Evento
        if ((currentTime - lastTime) >= INTERVAL1)
        {
            // Acciones:
            lastTime = currentTime;
            Serial.print(currentTime);
            Serial.print("hola este es el primer mensaje");
            Serial.print('\n');
            task1State = Task1States::WAIT_TIMEOUT2;
        }
        break;
    }


    case Task1States::WAIT_TIMEOUT2:
    {

      uint32_t currentTime = millis();
  
          // Evento
          if ((currentTime - lastTime) >= INTERVAL2)
          {
              // Acciones:
              lastTime = currentTime;
              Serial.print(currentTime);
              Serial.print("hola este es el segundo mensaje");
              Serial.print('\n');
              task1State = Task1States::WAIT_TIMEOUT3;
          }
          break;
    }
      



    case Task1States::WAIT_TIMEOUT3:
    {

      uint32_t currentTime = millis();
  
          // Evento
          if ((currentTime - lastTime) >= INTERVAL3)
          {
              // Acciones:
              lastTime = currentTime;
              Serial.print(currentTime);
              Serial.print("hola este es el 3 mensaje");
              Serial.print('\n');
              task1State = Task1States::WAIT_TIMEOUT1;
              
              
          }
          break;
      
    }
    
    
    default:
    {
        Serial.println("Error");
    }
    }
}

void setup()
{
    task1();
}

void loop()
{
    task1();
}
```

- Estados del Programa:
   INIT: Estado inicial donde se realiza la inicialización y configuración inicial del programa.
   WAIT_TIMEOUT1: Espera un intervalo de tiempo (INTERVAL1) antes de pasar al siguiente estado.
   WAIT_TIMEOUT2: Espera un intervalo de tiempo (INTERVAL2) antes de pasar al siguiente estado.
   WAIT_TIMEOUT3: Espera un intervalo de tiempo (INTERVAL3) antes de volver al estado inicial.
  
- Eventos:
   WAIT_TIMEOUT1 a WAIT_TIMEOUT2: Ocurre cuando ha transcurrido el tiempo definido por INTERVAL1.
   WAIT_TIMEOUT2 a WAIT_TIMEOUT3: Ocurre cuando ha transcurrido el tiempo definido por INTERVAL2.
   WAIT_TIMEOUT3 a WAIT_TIMEOUT1: Ocurre cuando ha transcurrido el tiempo definido por INTERVAL3.
  
- Acciones:
   INIT: Inicialización del estado y configuración inicial, incluyendo la inicialización de la comunicación serial y la configuración de la variable lastTime.
   WAIT_TIMEOUT1: Muestra un mensaje en la comunicación serial después de que ha transcurrido el tiempo definido por INTERVAL1.
   WAIT_TIMEOUT2: Muestra otro mensaje en la comunicación serial después de que ha transcurrido el tiempo definido por INTERVAL2.
   WAIT_TIMEOUT3: Muestra un tercer mensaje en la comunicación serial después de que ha transcurrido el tiempo definido por INTERVAL3.
