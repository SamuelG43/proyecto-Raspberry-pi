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

- Realiza un programa que envíe un mensaje al pasar un segundo, dos segundos y tres segundos. Luego de esto debe volver a comenzar.
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


# Ejericio 9 tareas concurrentes (evaluación formativa)
```
void task1(){
    enum class Task1States{
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task1States task1State = Task1States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVAL = 1000;

    switch(task1State){
        case Task1States::INIT:{
            Serial.begin(115200);
            lastTime = millis();
            task1State = Task1States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task1States::WAIT_FOR_TIMEOUT:{
            // evento 1:            
            uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVAL ){
                lastTime = currentTime;
                Serial.print("mensaje a 1Hz\n");
            }
            break;
        }

        default:{
            break;
        }
    }

}
void task2(){
    enum class Task2States{
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task2States task2State = Task2States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVAL = 2000;  // Cambiado a 2000 para obtener 0.5 Hz

    switch(task2State){
        case Task2States::INIT:{
            Serial.begin(115200);
            lastTime = millis();
            task2State = Task2States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task2States::WAIT_FOR_TIMEOUT:{
            // evento 2:
            uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVAL ){
                lastTime = currentTime;
                Serial.print("mensaje a 0.5Hz\n");
            }
            break;
        }

        default:{
            break;
        }
    }
}

void task3(){
    enum class Task3States{
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task3States task3State = Task3States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVAL = 4000;  // Cambiado a 4000 para obtener 0.25 Hz

    switch(task3State){
        case Task3States::INIT:{
            Serial.begin(115200);
            lastTime = millis();
            task3State = Task3States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task3States::WAIT_FOR_TIMEOUT:{
            // evento 3:
            uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVAL ){
                lastTime = currentTime;
                Serial.print("mensaje a 0.25Hz\n");
            }
            break;
        }

        default:{
            break;
        }
    }
}

void setup()
{
    task1();
    task2();
    task3();
}

void loop()
{
    task1();
    task2();
    task3();
}
```

# Ejercicio 11: realiza algunas pruebas
- 1. Analiza el programa. ¿Por qué enviaste la letra con el botón send? ¿Qué evento verifica si ha llegado algo por el puerto serial?
      R// if(Serial.avaliable() > 0)

- 2. Analiza los números que se ven debajo de las letras. Nota que luego de la r, abajo, hay un número. ¿Qué es ese número?
      R// Este indica que no hay más palabras y que ahí se termina el mensaje que se quería transmitir en pantalla.
- 3. ¿Qué relación encuentras entre las letras y los números?
     R// Cada letra corresponde a la posición numérica de ese mismo número, es decir "6f", la "f" es la posición sexta del abecedario
- 4. ¿Qué es el 0a al final del mensaje y para qué crees que sirva?
     R// El "0a" al final del mensaje es un valor hexadecimal que representa el número 10, 0a (10) indica que después de la palabra "computador", debería comenzar una nueva línea en el mensaje.


# Ejercicio 12: Punteros
- ¿Cómo se declara un puntero?

uint32_t *pvar

- ¿Cómo se define un puntero? (cómo se inicializa)
  
  Es una variable, en la cual se almacena una dirección de memoria.

- ¿Cómo se obtiene la dirección de una variable?

uint32_t *pvar = &var;

- ¿Cómo se puede leer el contenido de una variable por medio de un puntero?

Serial.print(*pvar);

- ¿Cómo se puede escribir el contenido de una variable por medio de un puntero?

*pvar = 10;


# Ejercicio 14: retrieval practice (evaluación formativa)
```
static void changeVar(uint32_t *pdata, uint32_t *pdata2)
{
    uint32_t temp = *pdata;
    *pdata = *pdata2;
    *pdata2 = temp;
}

static void printVar(uint32_t value)
{
    Serial.print("var content: ");
    Serial.print(value);
    Serial.print('\n');
}

void task1()
{
    enum class Task1States    {
        INIT,
        WAIT_DATA
    };
    static Task1States task1State = Task1States::INIT;

    switch (task1State)
    {
    case Task1States::INIT:
    {
        Serial.begin(115200);
        task1State = Task1States::WAIT_DATA;
        break;
    }

    case Task1States::WAIT_DATA:
    {
        // evento 1:        // Ha llegado al menos un dato por el puerto serial?        
     if (Serial.available() > 0)
        {
            Serial.read();
            uint32_t var = 0;
            uint32_t var2 = 1;
            uint32_t *pvar = &var;
            uint32_t *pvar2 = &var2;
            
            printVar(*pvar);
            printVar(*pvar2);
            changeVar(pvar, pvar2);
            printVar(var);
            printVar(var2);
        }
        break;
    }

    default:
    {
        break;
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

# Ejercicio 15: Punteros y arreglos

- ¿Por qué es necesario declarar rxData static? y si no es static ¿Qué pasa? ESTO ES IMPORTANTE, MUCHO.

Con static:

Al ser estático, rxData mantiene su valor entre llamadas a la función task1(). Esto significa que los datos almacenados en rxData no se reinician a cero cada vez que se llama a la función task1(). Si, por ejemplo, la función es llamada repetidamente en el bucle principal (loop()), rxData retiene su estado entre llamadas, lo que es crucial para acumular los datos recibidos hasta que se complete un conjunto de 5 bytes. Sin static:

Si rxData no se declara como static, se comportará como una variable local dentro de la función task1(). Esto significa que cada vez que se llama a la función, se crea una nueva instancia de rxData y su contenido se reinicia. Si, por ejemplo, estás esperando recibir 5 bytes y acumularlos en rxData, sin el calificador static, cada vez que se llame a la función task1(), rxData se reiniciará, y no se acumularán los datos como se espera.

dataCounter se define static y se inicializa en 0. Cada vez que se ingrese a la función loop dataCounter se inicializa a 0? ¿Por qué es necesario declararlo static?

el funcionamiento es similar a rxData, cada vez que ingresa a el loop no se inizializa en 0, dataCounter conserva su valor entre llamadas de funiones debido a que se declara static, esto sirve para poder llevar seguimiento de la cantidad de datos ingresados

Finalmente, la constante 0x30 en (pData[i] - 0x30) ¿Por qué es necesaria?

La expresión pData[i] - 0x30 se utiliza para convertir un carácter numérico ASCII en su equivalente numérico. En el conjunto de caracteres ASCII, los dígitos numéricos del 0 al 9 tienen códigos ASCII consecutivos desde 0x30 hasta 0x39. Al restar 0x30, se realiza una conversión para obtener el valor numérico correspondiente.

Por ejemplo:

El carácter '0' tiene un código ASCII de 0x30. Restar 0x30 resultará en 0. El carácter '1' tiene un código ASCII de 0x31. Restar 0x30 resultará en 1. El carácter '2' tiene un código ASCII de 0x32. Restar 0x30 resultará en 2. Y así sucesivamente.

# Ejercicio 16: análisis del api serial (investigación: hipótesis-pruebas)

- ¿Qué pasa cuando hago un Serial.available()?

Verifico si la placa recibio algun dato

- ¿Qué pasa cuando hago un Serial.read()?

Leo el dato que recibio la placa

- ¿Qué pasa cuando hago un Serial.read() y no hay nada en el buffer de recepción?

Cuando ejecutas Serial.read() y no hay nada en el buffer de recepción (Serial.available() devuelve 0), la función Serial.read() devuelve -1. Esta es una indicación de que no se ha recibido ningún dato.

- Un patrón común al trabajar con el puerto serial es este:

*if*(Serial.available() > 0){ int dataRx = Serial.read() }

- ¿Cuántos datos lee Serial.read()?

La función Serial.read() en Arduino lee un solo byte de datos del buffer de recepción serie. En términos de tipo de datos, devuelve un valor de tipo int, que representa el byte leído como un valor entre 0 y 255.

- ¿Y si quiero leer más de un dato? No olvides que no se pueden leer más datos de los disponibles en el buffer de recepción porque no hay más datos que los que tenga allí.

Se podria utilizar un arreglo como en el codigo del punto 15

- ¿Qué pasa si te envían datos por serial y se te olvida llamar Serial.read()?

Si se te olvida llamar a Serial.read() después de verificar que hay datos disponibles (Serial.available() > 0) en la función task1(), los datos que se envían por el puerto serie no se leerán y se acumularán en el búfer de recepción serial, si la comunicación serial está configurada para esperar la recepción de ciertos datos antes de continuar con la ejecución del programa, el código puede quedar bloqueado indefinidamente en el estado WAIT_DATA, ya que nunca se lee y procesa correctamente la información. Ademas, Si el código asume que siempre habrá datos disponibles y no se implementa una lógica adecuada para manejar la falta de datos, el programa puede comportarse de manera impredecible o generar errores.
