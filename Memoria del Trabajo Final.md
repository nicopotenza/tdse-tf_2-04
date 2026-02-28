<img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/fiubaLogo.png" alt="Logo Fiuba" width="50%">  

**UNIVERSIDAD DE BUENOS AIRES**  
**Facultad de Ingeniería**  
**TA133 - Taller de Sistemas Embebidos**

Memoria del Trabajo Final:

## **Control para Salas de Aislados**

**Autores:**

### **Marcos Joel Dias Peñaranda**  
Legajo: 111.423  

### **Andrés Giacomelli**  
Legajo: 111.038  

### **Nicolás Alejandro Potenza**  
Legajo: 97.024

*Este trabajo fue realizado en la Ciudad Autónoma de Buenos Aires,  
entre noviembre de 2025 y febrero de 2026.*

---

## Resumen



# Índice General

- [**Registro de versiones**](#registro-de-versiones)
- [**Introducción general**](#introducción-general)
  - [1.1 Selección del proyecto a implementar](#11-selección-del-proyecto-a-implementar)
  - [1.2 Objetivo del proyecto y resultados esperados](#12-objetivo-del-proyecto-y-resultados-esperados)
  - [1.3 Proyectos similares y justificación](#13-proyectos-similares-y-justificación)
  - [1.4 Selección del proyecto](#14-selección-del-proyecto)
- [**Introducción específica**](#introducción-específica)
  - [2.1 Requisitos](#21-requisitos)
  - [2.2 Modos de operación del sistema](#22-modos-de-operación-del-sistema)
    - [2.2.1 Modo INMUNO](#221-modo-inmuno)
    - [2.2.2 Modo INFECTO](#222-modo-infecto)
    - [2.2.3 Modo SETUP](#223-modo-set_up)
  - [2.3. Casos de uso](#23-casos-de-uso)
  - [2.4. Descripción de los módulos del sistema](#24-descripción-de-los-módulos-del-sistema)
- [**Diseño e implementación**](#diseño-e-implementación)
  - [3.1 Sistema](#31-sistema)
    - [3.1.1 Firmware](#311-firmware)
  - [3.2 Teclado](#32-teclado)
    - [3.2.1 Hardware](#321-hardware)
    - [3.2.2 Firmware](#322-firmware)
  - [3.3 Alarma visual](#33-alarma-visual)
    - [3.3.1 Hardware](#331-hardware)
    - [3.3.2 Firmware](#332-firmware)
  - [3.4 Alarma sonora](#34-alarma-sonora)
    - [3.4.1 Hardware](#341-hardware)
    - [3.4.2 Firmware](#342-firmware)
  - [3.5 Sensores](#35-sensores)
    - [3.5.1 Hardware](#351-hardware)
    - [3.5.2 Firmware](#352-firmware)
  - [3.6 Actuador de persiana](#36-actuador-de-persiana)
    - [3.6.1 Hardware](#361-hardware)
    - [3.6.2 Firmware](#362-firmware)
  - [3.7 Memoria EEPROM](#37-memoria-eeprom)
    - [3.7.1 Hardware](#371-hardware)
    - [3.7.2 Firmware](#372-firmware)
  - [3.8 Display LCD](#38-display-lcd)
    - [3.8.1 Hardware](#381-hardware)
    - [3.8.2 Firmware](#382-firmware)
- [**Ensayos y resultados**](#ensayos-y-resultados)
  - [4.1 Pruebas funcionales del hardware](#)
  - [4.2 Pruebas funcionales del firmware](#)
  - [4.3 Pruebas de integración](#)
  - [4.4 Comparación con otros sistemas similares](#)
- [**Conclusiones**](#conclusiones)
  - [5.1 Resultados obtenidos](#)
  - [5.2 Próximos pasos](#)
- [**Bibliografía**](#bibliografía)

---

# Registro de versiones

| Revisión | Cambios realizados | Fecha |
| :---: | ----- | ----- |
| 1.0 | Creación del documento | 27/02/2026 |

---

# **Capítulo 1**  
# **Introducción general** 

## **1.1 Selección del proyecto a implementar**

Las salas de aislados son áreas hospitalarias críticas diseñadas para mantener condiciones de presión diferencial controlada que eviten la transferencia de contaminantes entre ambientes. Su correcto funcionamiento es esencial tanto para proteger a pacientes inmunosuprimidos (evitando el ingreso de patógenos) como para contener agentes infecciosos (evitando su salida al pasillo o áreas comunes). 

El control de presión diferencial entre la **habitación**, la **sala previa** y el **pasillo** se realiza habitualmente mediante sistemas de ventilación con compuertas y ventiladores regulados. En este proyecto se propone desarrollar un **control embebido autónomo** que gestione la presión diferencial actuando sobre una **persiana modulante de retorno**, utilizando sensores de presión diferencial y una interfaz de usuario local. 

Persiana modulante de retorno: Una persiana modulante de retorno es una compuerta en el circuito de aire de retorno que se abre y cierra de forma proporcional, para regular y equilibrar el flujo de aire en un sistema de climatización.
Cumple las funciones de:
- Mantener el equilibrio de presiones en el sistema.
- Mezclar aire de retorno con aire exterior (ventilación).

Se busca así un sistema confiable, de bajo costo y fácilmente replicable en entornos hospitalarios, cumpliendo con los lineamientos de seguridad establecidos por normativas internacionales como **ASHRAE 170 (Ventilation of Health Care Facilities)** y las recomendaciones de la **CDC (Centers for Disease Control and Prevention)** para ambientes críticos.

## **1.2 Objetivo del proyecto y resultados esperados**

El objetivo principal es **diseñar e implementar un sistema de control de presión diferencial para salas de aislados** que permita operar en tres modos: **INMUNO, INFECTO y SET_UP**. Cada modo ajusta automáticamente la consigna y el comportamiento del control según las condiciones de seguridad requeridas.

**Objetivos específicos:**
- Medir en tiempo real la presión diferencial entre habitación-sala y sala–pasillo.
- Mantener una diferencia de presión mínima de **50 Pa** entre habitación y sala según el modo seleccionado.
- Asegurar que la sala mantenga siempre presión positiva respecto del pasillo.
- Accionar una persiana de retorno modulante para estabilizar la presión.
- Proveer alarmas visuales y sonoras ante condiciones anómalas.
- Permitir calibración y configuración de parámetros en modo SET_UP.

**Resultados esperados:**
- Prototipo funcional del sistema embebido basado en microcontrolador ARM.
- Interfaz local mediante display y teclado.
- Ensayo en maqueta o banco de pruebas que simule los gradientes de presión.
- Documentación técnica completa del hardware, firmware y lógica de control.

En la Figura 1.1 se ejemmplifica la vista superior de una planta de salas de aislados, con las ubicaciones de los elementos de campo del sistema.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/SalaAislados.png" alt="planta" width="90%"><br>
<em><b>Figura 1.1:</b> Diagrama de planta del sistema</em></p>


## **1.3 Proyectos similares y justificación**

Existen sistemas comerciales de control de presión hospitalaria, como los módulos de monitoreo **Setra Lite**, **Tsi Pressure Monitor** o **Honeywell Room Pressure Monitor**, que ofrecen medición y alarmas visuales, pero suelen tener un alto costo y escasa posibilidad de personalización.  

El proyecto propuesto busca **una alternativa académica y didáctica**, implementable con hardware de disponibilidad local (sensores analógicos, controladores ARM o STM32, actuadores estándar), reduciendo costos y fomentando la comprensión del lazo de control.

## **1.4 Selección del proyecto**

Tras evaluar alternativas, se selecciona el proyecto **“Control de presión para salas de aislados”** por las siguientes razones:

- Alta relevancia sanitaria y aplicabilidad práctica.
- Posibilidad de implementación con hardware disponible en el mercado argentino.
- Alcance de complejidad adecuado para un proyecto integrador de sistemas embebidos.
- Interés personal y profesional de los autores en el área de automatización y control ambiental.

A continuación, en la Figura 1.2, puede visualizarse el diagrama de bloques funcional del proyecto.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/DiagramaDeBloques.png" alt="planta" width="90%"><br>
<em><b>Figura 1.2:</b> Diagrama de bloques funcional</em></p>

---

# **Capítulo 2**  
# **Introducción específica** 

## **2.1 Requisitos**

Habiendo analizado las características principales del sistema, se definieron los principales requisitos para que el sistema cumpla con su función de forma correcta y resulte útil para su propósito. Para esto, se realizó una tabla definiendo los principales requisitos a implementar, mostrado en la Tabla 2.1.

| ID | Descripción |
| :-: | - |
| 1 | El sistema debe medir la presión diferencial entre la sala y la habitación mediante un sensor analógico. |
| 2 | El sistema debe accionar **únicamente la persiana modulante de retorno de la habitación** mediante una salida PWM para mantener la presión objetivo. |
| 3 | El sistema debe mostrar en pantalla el estado actual de las salas (presiones y modo de operación). |
| 4 | El sistema debe generar alarmas visuales y sonoras ante condiciones fuera de rango. |
| 5 | El usuario debe poder cambiar el modo de operación mediante el teclado. |
| 6 | En modo SET_UP el sistema debe permitir calibrar sensores, modificar setpoints y probar salidas. |
| 7 | El sistema debe registrar y mantener en memoria no volátil el último modo de operación. |
| 8 | La **diferencia de presión entre sala y habitación** debe ser **≥ 50 Pa** en los modos automáticos; si cae por debajo de 50 Pa se genera alarma. |
| 9 | La **sala debe mantener presión positiva respecto del pasillo** en todo momento; la **presión diferencial sala-habitación no puede caer por debajo de 50 Pa**, y si la diferencia cae de ese valor se genera alarma. |

<p align="center"><em>Tabla 2.1: Requisitos del proyecto</em></p>

## **2.2 Modos de operación del sistema**

El sistema dispone de **tres modos de operación**, seleccionables mediante el teclado del panel de control.

### **2.2.1 Modo INMUNO**

Diseñado para pacientes inmunosuprimidos, donde se requiere evitar el ingreso de contaminantes desde el exterior.

- **Objetivo:** Mantener **presión positiva** en la habitación respecto de la sala.
- **Setpoint:** Debe mantenerse la presión diferencial (positiva) seteada, con una banda de desvío aceptable (también seteable).
- **Control:** Mediante la modulación de la persiana de retorno de la habitación. Esta modulación se realiza de forma lineal dentro de los extremos del desvío seteado alrededor del seteo de la presión diferencial de la sala-habitación.  
- **Alarmas:**  
  - Si la presión diferencial sala-habitación se sitúa por fuera del seteo ± desvío, se activa alarma visual intermitente por 3 segundos. Si luego de ese tiempo se mantiene el desvío, se activa una alarma visual fija y una alarma sonora intermitente.
  - Si la presión diferencial sala-pasillo se sitúa por fuera del seteo ± desvío, se activa alarma visual intermitente por 3 segundos. Si luego de ese tiempo se mantiene el desvío, se activa una alarma visual fija y una alarma sonora intermitente.  

### **2.2.2 Modo INFECTO**

Destinado al aislamiento de pacientes infecciosos, evitando la salida de aire contaminado.

- **Objetivo:** Mantener **presión negativa** en la habitación respecto de la sala.
- **Setpoint:** Debe mantenerse la presión diferencial (negativa) seteada, con una banda de desvío aceptable (también seteable).
- **Control:** Mediante la modulación de la persiana de retorno de la habitación. Esta modulación se realiza de forma lineal dentro de los extremos del desvío seteado alrededor del seteo de la presión diferencial de la sala-habitación.  
- **Alarmas:**  
  - Si la presión diferencial sala-habitación se sitúa por fuera del seteo ± desvío, se activa alarma visual intermitente por 3 segundos. Si luego de ese tiempo se mantiene el desvío, se activa una alarma visual fija y una alarma sonora intermitente.
  - Si la presión diferencial sala-pasillo se sitúa por fuera del seteo ± desvío, se activa alarma visual intermitente por 3 segundos. Si luego de ese tiempo se mantiene el desvío, se activa una alarma visual fija y una alarma sonora intermitente.  

### **2.2.3 Modo SET_UP**

Utilizado para calibración y mantenimiento técnico.

- **Objetivo:** Permitir configuración de parámetros sin control activo.
- **Funciones:**  
  - Seleccionar el modo de operación.
  - Calibrar sensores. Se permite ajustar un valor de offset.  
  - Ajustar setpoints y desvíos.  
  - Probar actuadores.  
  - Monitorear lecturas en tiempo real.
- **Condición especial:** No se ejecutan acciones automáticas de control.

## **2.3. Casos de uso**

En la tabla 2.2 se presentan casos de uso para ejemplificar el uso del sistema.

| **Actor** | **Caso de Uso** | **Descripción** |
| ------------ | -------------- | ----------------|
| Operador técnico | Seleccionar modo de operación | El usuario elige entre INMUNO, INFECTO o SET_UP desde el teclado. |
| Sistema automático | Ajustar comportamiento según modo | El microcontrolador cambia parámetros de control y alarmas según el modo activo. |
| Operador técnico | Ver estado de sala | El usuario consulta en el display el estado de presión y alarmas. |
| Sistema automático | Regular presión | El controlador modula la persiana para mantener la presión estable. |
| Sistema automático | Generar alarma | Se activa alarma sonora/visual al detectar condiciones fuera de rango. |
| Operador técnico | Calibrar sensores y salidas | En modo SET_UP, el técnico calibra sensores y prueba actuadores. |

<p align="center"><em>Tabla 2.2: Casos de uso</em></p>

## **2.4. Descripción de los módulos del sistema**

### **2.4.1 Microcontrolador**
Como controlador principal del sistema se utiliza la placa NUCLEO-F103RB (Figura 2.1). La elección de esta placa recayó exclusivamente en la disponibilidad, teniendo además como requerimiento la cantidad de memoria, pines y periféricos de la placa.  
Durante la cátedra se trabajó con esta placa y algunos de los dispositivos que luego se mencionan. Como documentación de cabecera se utilizaron las prácticas de clase y los enlaces que se indican en la bibliografía [1] y [2].  
La placa se programó en C++ a través de la plataforma STM32CubeIDE y se muestra en la Figura 2.1.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/microcontrolador.png" alt="microcontrolador" width="50%"><br>
<em><b>Figura 2.1:</b> Imagen de la placa Nucleo-F103RB</em></p>

### **2.4.2 Display LCD + Módulo I²C**
Es un módulo de visualización LCD alfanumérico monocromático utilizado en sistemas embebidos para mostrar información básica al usuario, como texto, números y símbolos. Los caracteres se organizan en filas y columnas y se generan mediante una matriz interna de puntos. Opera típicamente con una alimentación de 5 V.

Para su control se empleó un módulo adaptador basado en el circuito integrado PCF8574, que actúa como expansor de entradas/salidas digitales mediante interfaz I²C. Este dispositivo permite manejar la pantalla LCD utilizando únicamente dos líneas de comunicación del microcontrolador (SDA y SCL), reduciendo la cantidad de pines necesarios.

En la Figura 2.2 se pueden observar ambos dispositivos.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/display_i2c.png" alt="display" width="50%"><br>
<em><b>Figura 2.2:</b> Imagen del display con su adaptador a I²C</em></p>

### **2.4.3 Memoria EEPROM**
La memoria EEPROM (Electrically Erasable Programmable Read-Only Memory) es un dispositivo de almacenamiento no volátil utilizado en sistemas embebidos para guardar datos que deben conservarse aun cuando se interrumpe la alimentación, como configuraciones, parámetros o registros del sistema. (importante en el caso de que haya una interrupción de la alimentacion que los valores de presion, estados,etc sean guardados) 

Se comunica comúnmente con el microcontrolador mediante interfaces seriales como I²C o SPI y opera con tensiones de alimentación típicas de sistemas electrónicos digitales.

Esta es la encargada de almacenar los datos de los seteos en caso de una interrupción de la energía.

En la Figura 2.3 se puede observar una imagen del módulo de memoria utilizado.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/memoria.png" alt="memoria" width="50%"><br>
<em><b>Figura 2.3:</b> Imagen de la memoria EEPROM AT24C08</em></p>

### **2.4.4 Servomotor**
El servomotor es un actuador electromecánico para controlar con precisión la posición angular de un eje. Integra un motor de corriente continua, un sistema de engranajes y un circuito de control interno que ajusta la posición según una señal eléctrica de mando. 

Se controla mediante una señal PWM proveniente del microcontrolador, que determina el ángulo de giro dentro de un rango limitado (típicamente entre 0° y 180°).  

Fue utilizado para controlar la persiana modulante de retorno de la habitacion.

En la Figura 2.4 se puede observar una imagen del servomotor utilizado.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/sg90.png" alt="memoria" width="50%"><br>
<em><b>Figura 2.4:</b> Imagen del servomotor SG90</em></p>

### **2.4.5  Pulsadores**
Los pulsadores son dispositivos de entrada para permitir la interacción del usuario con el equipo. Funcionan como interruptores momentáneos que, al ser presionados, abren o cierran un circuito eléctrico generando una señal digital que el microcontrolador puede detectar. 

Se emplearon para acciones como teclado para avanzar, retroceder, subir y/o bajar en las opciones del sistema, y fueron conectadas a pines de entrada configurados con resistencias de pull-up para garantizar un estado lógico definido cuando no están presionados.

En la Figura 2.5 se puede observar una imagen del módulo de botones utilizado.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/botones.png" alt="memoria" width="50%"><br>
<em><b>Figura 2.5:</b> Imagen del módulo de 4 botones</em></p>

### **2.4.6  Luces Leds**
Los LEDs (diodos emisores de luz) son dispositivos de salida para indicar visualmente estados o condiciones de funcionamiento del sistema. Emiten luz cuando circula corriente a través de ellos en polarización directa. 

Se emplean comúnmente como indicadores de encendido, alerta o actividad. En nuestro caso fue utilizada para permitir al sistema generar indicaciones visuales rápidas ante condiciones dentro o fuera de rango. Requirieron una resistencia limitadora de corriente en serie para evitar daños por exceso de corriente. Se controlan desde pines de salida digitales del microcontrolador.

En la Figura 2.6 se puede observar una imagen de leds similares a los utilizados.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/leds.png" alt="leds" width="50%"><br>
<em><b>Figura 2.6:</b> Imagen de LEDs</em></p>

### **2.4.7  Buzzer**
El buzzer es un dispositivo de salida sonora utilizado en sistemas embebidos para generar señales acústicas de aviso, alerta o confirmación. Convierte una señal eléctrica en sonido mediante un elemento piezoeléctrico o electromecánico interno. 

Se controla desde un pin de salida del microcontrolador, ya sea mediante una señal digital o una señal PWM para variar el tono o el patrón de sonido, y se empleó para la indicación sonora de valores medidos fuera de rango.

En la Figura 2.6 se puede observar una imagen del buzzer utilizado.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/buzzer.png" alt="buzzer" width="50%"><br>
<em><b>Figura 2.6:</b> Imagen de LEDs</em></p>

### **2.4.8  Sensores**
Los sensores de presión diferencial son dispositivos de entrada utilizados en sistemas embebidos para medir la diferencia de presión entre dos puntos de un sistema, en lugar de la presión absoluta respecto al ambiente. Esta medicion fue empliada para controlar la presion entre las dos habitaciones. 

El sensor convierte la diferencia de presión en una señal eléctrica, analógica o digital, que el microcontrolador puede procesar para supervisar condiciones de funcionamiento o activar acciones de control dentro del sistema (en nuestro caso, reposicionar el servomotor).

En la Figura 2.7 se puede observar una ejemplo de un sensor de presión diferencial comúnmente utilizado.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/sensor.png" alt="sensor" width="50%"><br>
<em><b>Figura 2.7:</b> Imagen de sensor Dwyer MSX-W20-PA</em></p>

En este caso se utilizaron en su lugar potenciómetros para simplificar las pruebas. El potenciómetro permite convertir la posición de una resistencia variable en un valor de tensión medible por el microcontrolador.

En la Figura 2.8 se puede observar una imagen del potenciometro utilizado.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/potenciometros.png" alt="potenciometros" width="50%"><br>
<em><b>Figura 2.8:</b> Imagen de Potenciómetro</em></p>

---

# **Capítulo 3**  
# **Diseño e implementación** 

## **3.1 Sistema**
### **3.1.1 Firmware**
Utilizando la arquitectura de aplicación con FSM de la cátedra, se adaptó para poder generar tres modos de operación (SET_UP, INFECTO e INMUNO).

En la inicialización de sistema, se realiza una consulta a la memoria EEPROM de la información de seteo almacenada. Al no contar aún con la información de arranque, se pasa al loop indicando que no hay estado definido.

En el loop, si no hay estado definido, se espera la respuesta de la memoria EEPROM y se procesa la información devuelta por esta. Con esa información, el estado para al modo de funcionamiento cargado.

Con el estado ya definido, el programa invoca a la función de la máquina INMUNO o INFECTO, según el estado de task_system_mode.

Si en algún momento se indicó que debe ingresarse al SET_UP, se cambia el modo a SET_UP.
De igual modo, al salir del SET_UP, se vuelve a cualquiera de los dos modos de funcionamiento normal.

#### **3.1.1.1 Modo SET_UP**

Dentro de este modo, el primer paso es leer la información cargada de la memoria EEPROM. Una vez hecho esto, se la procesa y se pasa a los modos de configuración, en el siguiente orden:

* **Modo de operación:** para seleccionar si el sistema funciona bajo el concepto de aislados infectocontagiosos o inmunosuprimidos.
* **Presión sala-pasillo:** permite configurar la presión positiva entre la sala y el pasillo.
* **Offset de presión sala-pasillo:** permite configurar el offset del sensor de presión. Útil para calibrar el sensor.
* **Desvío de presión sala-pasillo:** permite configurar el desvío aceptable de la presión actual respecto de la seteada.
* **Presión sala-habitación:** permite configurar la presión entre la sala y la habitación. Dependiendo si el modo seleccionado anteriormente es INFECTO o INMUNO, esta toma los valores positivos o negativos.
* **Offset de presión sala-habitación:** permite configurar el offset del sensor de presión. Útil para calibrar el sensor.
* **Desvío de presión sala-habitación:** permite configurar el desvío aceptable de la presión actual respecto de la seteada.
* **Prueba de actuador:** permite probar la posición del actuador. Útil para calibración del mismo durante la instalación y mantenimiento.

Durante este modo se dispone de 4 botones útiles para operar:
- **Back:** para retroceder al menú anterior. Si está en **Modo*, vuelve al modo de operación actual sin cambios.
- **Enter:** Para pasar al siguiente menú. Si está en **Prueba de actuador**, almacena los valores nuevos en la memoria y pasa al modo seleccionado.
- **Up:** para aumentar el valor actual.
- **Down:** para disminuir el valor actual.

#### **3.1.1.2 Modos INFECTO e INMUNO**

Dentro de este modo, el primer paso es permitir que las presiones se estabilicen. Para esto se esperan 5 segundos, durante los cuales el actuador se posiciona en función del valor medido por el sensor de presión sala-habitación.

Luego, pasa a alguno de los modos siguientes:

* **Normal:** si las presiones están dentro de los rangos de setpoint ± desvío, está en este modo. Acá, la alarma sonora está apagada y los indicadores visuales verdes están encendidos. Si en algún momento una de las presiones sale de rango, se pasa a modo **Alerta*.

* **Alerta:** si alguna de las presiones está fuera del rango deseado, se indica en la pantalla. La alarma sonora se activa en modo intermitente y la indicación visual de la presión fuera de rango pasa de verde a rojo intermitente. Si esto se corrige, vuelve al modo **Normal**. En caso de superar 3 segundos sin corrección, pasa a **Alarma**.

* **Alarma:** si alguna de las presiones está fuera del rango deseado por más de 3 segundos, se indica en la pantalla. La alarma sonora se enciende de forma fija y la indicación visual de la presión fuera de rango pasa rojo intermitente a fijo. Si esto se corrige, vuelve al modo **Normal**.

En cualquier modo, si se presiona el botón **Enter**, pasa al modo de *SET_UP*.

La diferencia entre los modos está dada por la indicación del modo en la pantalla. A futuro, es posible utilizar los modos para funcionamientos diferenciados, como en el control de los lazos para los actuadores de persiana.

Durante estos modos, se controla para posición del actuador de retorno de la habitación para intentar mantener la presión objetivo.

## **3.2 Teclado**
### **3.2.1 Hardware**
El hardware de los botones que componen el teclado se detalla en la Figura 3.1.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/circuito_botones.png" alt="circuito" width="30%"><br>
<em><b>Figura 3.1:</b> Conexionado Botones</em></p>

### **3.2.2 Firmware**
Utilizando las máquinas de estado para el control de pulsadores vistas en la cátedra, se realizó una máquina para relevar el estado de los 4 pulsadores.  
Dependiendo del estado del botón físico, se indica al sistema si está presionado o no.  
Adicionalmente, tiene incorporada la lógica de antirebote para evitar falsas pulsaciones.

## **3.3 Alarma visual**
### **3.3.1 Hardware**
El hardware de los LED que componen las indicaciones visuales se detalla en la Figura 3.2.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/circuito_leds.png" alt="circuito" width="35%"><br>
<em><b>Figura 3.2:</b> Conexionado Leds</em></p>

### **3.3.2 Firmware**
Utilizando las máquinas de estado para el control de LEDs vistas en la cátedra, se realizó una máquina para controlar el estado de los 4 leds. Los modos de funcionamiento que tiene desarrollados son:

* **ST_LED_OFF:** el LED permanece apagado.
* **ST_LED_ON:** el LED permanece encendido.
* **ST_LED_BLINK_OFF y ST_LED_BLINK_ON:** el LED parpadea. Cambia entre estos estados mientras lo hace.
* **ST_LED_PULSE:** el LED realiza un pulso y vuelve al estado de apagado.

## **3.4 Alarma sonora**
### **3.4.1 Hardware**
El hardware del buzzer que compone la alarma sonora se detalla en la Figura 3.3.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/circuito_buzzer.png" alt="circuito" width="30%"><br>
<em><b>Figura 3.3:</b> Conexionado Buzzer</em></p>

### **3.4.2 Firmware**
Este firmware tiene el mismo comportamiento que el desarrollado para los Leds. Solamente se definió en forma independiente para su análisis y entendimiento del código.

## **3.5 Sensores**
### **3.5.1 Hardware**
El hardware de los potenciómetro que componen la simulación de los sensores se detalla en la Figura 3.4.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/circuito_potenciometros.png" alt="circuito" width="40%"><br>
<em><b>Figura 3.4:</b> Conexionado potenciómetros</em></p>

### **3.5.2 Firmware**
utilizando la arquitectura de FSM de la cátedra se adaptó para leer dos entradas analógicas del procesador.  
Cuando inicia el loop, la tarea queda en el estado de inactivo. Al recibirse un pedido de lectura, para al estado de espera (ST_TXPRESION_WAIT).  
En el estado de espera, se espera una medición y al obtenerse de indica mediante un evento al sistema que ya está disponible la lectura.  
Esta lectura se adapta de un valor medido por el ADC (0 a 4095) a un valor de pascales consistente con el valor que transmite el sensor (en este caso -125 a 125 Pa). De esa forma, el procesamiento del sensor queda encapsulado y el sistema ya tiene el valor listo para utilizarlo.

## **3.6 Actuador de persiana**
### **3.6.1 Hardware**
El hardware del servomotor utilizado como actuador de persiana se detalla en la Figura 3.5.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/circuito_servo.png" alt="circuito" width="50%"><br>
<em><b>Figura 3.5:</b> Conexionado servomotor</em></p>

### **3.6.2 Firmware**
utilizando la arquitectura de FSM de la cátedra se adaptó para controlar un servomotor por PWM.  
Cuando inicia el loop, el servomotor está deshabilitado (ST_SERVO_DISABLED). En este estado no se controla su posición.
Al pasar al estado de inactivo (ST_SERVO_IDLE), el servomotor está habilitado y comienza a controlar su posición.  
Si se cambia el valor del ángulo (indicado en porcentaje), pasa al estado de movimiento (ST_SERVO_MOVING), el cual mueve el servomotor en forma progresiva hasta la nueva posición. De esta forma se evitan los movimientos bruscos, que en el caso de persianas mecánicas podrían generar inestabilidad en el sistema e incluso dañarlas.  
Cuando llega a la nueva posición, vuelve al estado de inactivo.

## **3.7 Memoria EEPROM**
### **3.7.1 Hardware**
El hardware de la memoria EEPROM se detalla en la Figura 3.6.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/circuito_eeprom.png" alt="circuito" width="40%"><br>
<em><b>Figura 3.6:</b> Conexionado memoria EEPROM</em></p>

### **3.7.2 Firmware**
utilizando la arquitectura de FSM de la cátedra se adaptó para trabajar con una memoria EEPROM mediante I²C. Para esto se consultó la lista de comandos en la bibliografía del fabricante [3].  
Para su funcionamiento, tiene una tabla del tamaño de la memoria, la cual permite que se lea o escriban los datos ahí para luego ser enviados a la memoria o procesados por el sistema.  
Por defecto, el loop queda en el estado de espera. Cuando se recibe un evento, empieza la el proceso de escritura (ST_MEM_WRITE) o lectura (ST_MEM_SET_ADDRESS).  

#### **3.7.2.1 Escritura**
Para escribir en la memoria, primero se consulta si la interfaz I²C está disponible. En caso afirmativo, se procesa la posición de memoria que debe leerle, se envía a la memoria EEPROM la indicación de que se va a escribir y pasa al estado de escribiendo ST_MEM_WRITING.  
En el estado de escribiendo, se espera que finalice ese envío de instrucción a la memoria. Cuando se termina, pasa a escribir la posición, y luego de finalizar vuelve al estado de espera.

#### **3.7.2.2 Lectura**
Para leer en la memoria, primero se consulta si la interfaz I²C está disponible. En caso afirmativo, se procede a indicarle a la memoria la posición que desea ser leída. Una vez hecho esto, se espera la respuesta de la memoria en ST_MEM_READ y se cuando está listo para leerse pasa a ST_MEM_READING, donde espera a que se finalice la lectura. El valor almacenado queda la memoria creada como interfaz esperando ser usada por el sistema.

## **3.8 Display LCD**
### **3.8.1 Hardware**
El hardware del display LCD y su adaptador a I²C se detalla en la Figura 3.7.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/Circuito_lcd.png" alt="circuito" width="60%"><br>
<em><b>Figura 3.6:</b> Conexionado Display LCD</em></p>

### **3.8.2 Firmware**  
utilizando la arquitectura de FSM de la cátedra se adaptó para trabajar con un Display LCD mediante I²C. Para esto se consultó la lista de comandos en la bibliografía del fabricante [4][5].  

En su funcionamiento básico, cuenta con una memoria temporal que almacena una matriz correspondiente al tamaño del display. Los pedidos de escritura modifican esa matriz, y luego, cuando se recibe el evento de escritura del display, se vuelca esta información a la pantalla física.

De esa forma es posible escribir una pantalla de cualquier tamaño, sin importar el tiempo que lleve ni afectar al resto del procesamiento.

Para eso, se realizó una máquina de estados que en su inicialización configure la inicialización del display LCD. Cómo esto no forma parte del loop, se decidió que esta tarea tenga código bloqueante, para asegurar la correcta configuración del módulo.

Una vez en el loop, el display esta inactivo hasta recibir un evento de escritura o modificación del backlight. En el caso de ser un evento de escritura, los estados ST_DIS_ACTIVE_01 y ST_DIS_ACTIVE_02 son los encargados de desplazarse entre filas (ST_DIS_ACTIVE_01) y columnas (ST_DIS_ACTIVE_02) enviando el valor almacenado en la matriz a la pantalla.

```cpp
case ST_DIS_ACTIVE_01:

		if (row >= MAX_ROWS) {
			p_task_display_dta->state = ST_DIS_IDLE;
		} else {
			col = 0;
			LCD_SetCursor(row, col);
			p_task_display_dta->tick = DEL_DIS_MAX;
			p_task_display_dta->state = ST_DIS_ACTIVE_02;
		}

		break;

	case ST_DIS_ACTIVE_02:

		if (p_task_display_dta->tick > DEL_DIS_MIN) {
			p_task_display_dta->tick--;
		} else {
			LCD_Send_Data(display_memory_ram[row][col]);
			p_task_display_dta->tick = DEL_DIS_MAX;

			col++;

			if (col >= MAX_COLUMNS) {
				row++;
				p_task_display_dta->tick = DEL_DIS_MIN;
				p_task_display_dta->state = ST_DIS_ACTIVE_01;
			}
		}

		break;
```

Una vez finalizado, vuelve al estado de reposo.

---

# **Capítulo 4**
# **Ensayos y resultados**

## **4.1 Pruebas funcionales del hardware**

El circuito definitivo puede observarse en la Figura 4.1.


<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/circuito_completo.png" alt="completo" width="100%"><br>
<em><b>Figura 4.1:</b> Circuito completo</em></p>

Dado que se realizó la disposición de pines con anterioridad al inicio de las pruebas (la misma se observa en la Tabla 4.1), se realizó un único protoboard para todas las pruebas, utilizando sólo lo necesario para cada prueba.

| Pin_Arduino | Pin | Tipo | Descripción |
| :-: | :-: | :--: | :---------: |
| D2 | PA_10 | Salida Digital | LED_1 |
| D4 | PB_5 | Salida Digital | LED_2 |
| D5 | PB_4 | Salida Digital | LED_3 |
| D6 | PB_10 | Salida PWM | Servomotor |
| D7 | PA_8 | Salida Digital | Buzzer |
| D8 | PA_9 | Salida Digital | LED_4 |
| D9 | PC_7 | Entrada Digital | BTN_BACK |
| D10 | PB_6 | Entrada Digital | BTN_ENTER |
| D11 | PA_7 | Entrada Digital | BTN_UP |
| D12 | PA_6 | Entrada Digital | BTN_DOWN |
| D14 | PB_9 | Comunicación I²C | I2C1_SDA |
| D15 | PB_8 | Comunicación I²C | I2C1_SCL |
| A0 | PC_0 | Entrada Analógica | Sensor_1 |
| A1 | PC_1 | Entrada Analógica | Sensor_2 |

<p align="left"><em>Tabla 4.1: Disposición de pines del sistema</em></p>

Adicionalmente, se midieron los consumos de corriente de cada uno de los dispositivos. Esto se detalla en la Tabla 4.2.

| Dispositivo | Consumo (mA) |
| :---- | ---: |
| Buzzer | 11,2 |
| Display LCD + Adaptador I²C | 42,9 |
| Pulsador | - |
| Potenciómetro | 0,377 |
| Servomotor (medio) | 33,9 |
| Led | 1,3 |
| Memoria EEPROM | 0,267 |
| Nucleo-F103RB | 33,9 |

<p align="left"><em>Tabla 4.2: Disposición de pines del sistema</em></p>

## **4.2 Pruebas funcionales del firmware**
El firmware se probó por etapas. En un primer lugar se probaron la comunicación I2C con el display y con la memoria EEPROM por separado.

Luego, se probaron en conjunto para asegurar que no haya interferencia entre ellos.

Una vez realizado esto, y con la FSM del system ya realizada, se fueron agregando las partes (del sistema), las cuales fueron previamente probadas por separado.

En la Tabla 4.3 se detalla el *WCET* de cada máquina de estados y el *WCEP* del sistema.

| Tarea | WCET |
| ----- | ---- |
| Buzzer | 7 |
| Display | 13 |
| Pulsadores | 14 |
| Sensores | 17 |
| Servomotor | 25 |
| Leds | 27 |
| Memoria | 270 |
| Sistema | 228 |
|  |  |
| Total (WCEP) | 339 |
| Total (Suma WCET) | 601 |

<p align="left"><em>Tabla 4.3: Detalle de los tiempos de ejecución</em></p>

De la tabla anterior podemos ver el Factor de Uso está en un 33,9% de la capacidad del procesador.  
Incluso, considerando el caso extremo (si todo estuviera en su peor condición al mismo tiempo), el factor de uso sería de del 60,1%, muy por debajo del límite.  
Esto nos permite determinar que es posible reemplazar el procesador por uno de menores prestaciones.  
Adicionalmente, hay acciones que podrían optimizarse para disminuir aún más el procesamiento. Por ejemplo, en la realidad las variables ambientales suelen ser lentas, del orden del segundo o superior. Esto permitiría que algunas tareas que hoy se ejecutan en cada milisegundo, pasen a ejecutarse cada 0,5 segundos o incluso más tiempo.

| Region | Start address | End address | Size | Free | Used | Usage (%) |
| - | - | - | - | - | - | - |
| RAM   | 0x20000000 | 0x2004ffff | 20 KB  | 15,64 KB | 4,36 KB  | 21,80 % |
| FLASH | 0x08000000 | 0x0801ffff | 128 KB | 70,79 KB | 57,21 KB | 44,70 % |

<p align="center"><em>Tabla 4.4: Detalle de Build Analyzer - Memory Region</em></p>

En la Figura 4.x se observa el detalle de la salida de *Console* luego de la compilación final.

<p align="center"><img src="https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/MaterialApoyo/Im%C3%A1genes/console.png" alt="console" width="90%"><br>

<p align="center"><em>Figura 4.4: Detalle de Console</em></p>

## **4.3 Pruebas de integración**
Para las pruebas de integración se fueron incorporando las partes del proyecto y dando funcionalidad dentro del esquema general.

Dado que la disposición de pines fue definida con anterioridad, no fue necesario reubicar dispositivos. Eso ahorró tiempo al no requerir reacomodar partes en función de los módulos internos requeridos y los pines disponibles para su uso.

En la Tabla 4.5 se visualiza el estado actual del trabajo. 

| ID | Descripción | Estado | Observaciones |
| :-: | :-- | :-: | :-- |
| 1 | El sistema debe medir la presión diferencial entre la sala y la habitación mediante un sensor analógico. | Completado | - |
| 2 | El sistema debe accionar **únicamente la persiana modulante de retorno de la habitación** mediante una salida PWM para mantener la presión objetivo. | Completado | - |
| 3 | El sistema debe mostrar en pantalla el estado actual de las salas (presiones y modo de operación). | Completado | - |
| 4 | El sistema debe generar alarmas visuales y sonoras ante condiciones fuera de rango. | Completado | - |
| 5 | El usuario debe poder cambiar el modo de operación mediante el teclado. | Completado | - |
| 6 | En modo SET_UP el sistema debe permitir calibrar sensores, modificar setpoints y probar salidas. | Completado | - |
| 7 | El sistema debe registrar y mantener en memoria no volátil el último modo de operación. | Completado | - |
| 8 | La **diferencia de presión entre sala y habitación** debe ser **≥ 50 Pa** en los modos automáticos; si cae por debajo de 50 Pa se genera alarma. | Completado | Se puede cambiar el valor y se creó una ventaja de desvío aceptable. |
| 9 | La **sala debe mantener presión positiva respecto del pasillo** en todo momento; la **presión diferencial sala-habitación no puede caer por debajo de 50 Pa**, y si la diferencia cae de ese valor se genera alarma. | Completado | Se puede cambiar el valor de 50 Pa para adaptarlo a lo requerido en la instalación. |

<p align="center"><em>Tabla 4.5: Estado del trabajo</em></p>

En el siguiente enlace es posible observar una de las pruebas de funcionamiento.

https://youtu.be/VONTj9oOafY

## **4.4 Comparación con otros sistemas similares**
Si bien este producto no alcanza un grado de desarrollo apto para su comercialización en el momento, y requiere un análisis profundo y pruebas de estabilidad acordes a la criticidad del proceso que debe controlar, preliminarmente, y en base a experiencias personales, se puede apreciar que el costo de esta resolución es significativamente inferior a los sistemas comercializados.

---

# **Capítulo 5**
# **Conclusiones**

## **5.1 Resultados obtenidos**
El trabajo presentado logró implementar la totalidad de las funcionalidades previstas dentro de los plazos establecidos, incorporando criterios de robustez, modularidad y adaptabilidad que permiten su futura migración a microcontroladores de menores recursos.

Los principales resultados obtenidos pueden sintetizarse en los siguientes puntos:

* Implementación de control de velocidad de motor mediante modulación por ancho de pulso (PWM).
* Diseño e implementación de máquinas de estados finitos (FSM) tanto para la operación individual de los módulos como para la coordinación e interacción entre ellos.
* Integración de múltiples dispositivos en un mismo bus I²C (memoria EEPROM y display LCD), garantizando correcta gestión de direccionamiento y comunicación.
* Desarrollo de una máquina de estados para el control del display LCD independiente de sus dimensiones físicas, asegurando portabilidad y reutilización del código.
* Diseño de una arquitectura escalable y adaptable, orientada a su reutilización en futuros desarrollos embebidos.

Adicionalmente, durante el desarrollo se incorporó el uso de herramientas basadas en inteligencia artificial como soporte para la consulta y análisis de documentación técnica, verificación de enfoques de implementación y revisión de fragmentos de código. La utilización de estas herramientas permitió agilizar la interpretación de datasheets, bibliotecas y estructuras de programación, optimizando los tiempos de desarrollo. No obstante, su rol fue estrictamente complementario: las decisiones de diseño, la arquitectura del sistema, la validación del funcionamiento y la resolución final de problemas permanecieron bajo criterio técnico del equipo desarrollador. En este sentido, la inteligencia artificial se empleó como una herramienta de apoyo y no como un reemplazo del proceso de ingeniería ni del programador.

Se concluye que, al finalizar este trabajo, se ha adquirido un conjunto amplio e integrado de competencias aplicables al desarrollo de trabajos de ingeniería electrónica. Este trabajo permitió abordar de manera integral aspectos teóricos y prácticos del diseño de sistemas embebidos, proporcionando una visión completa de las etapas críticas del ciclo de desarrollo y consolidando la capacidad de transformar un concepto en un producto funcional, validado y técnicamente consistente.

## **5.2 Próximos pasos**

Se puede extender el trabajo aquí realizado con las siguientes implementaciones adicionales:
1. Implementar de un actuador por cada persiana de retorno, para controlar la presión en cada sala.
2. Incorporar de una segunda habitación al sistema de aislados.
3. Reemplazar los sensores actuales (potenciómetros) por sensores de presión normalmente utilizados en estos sistemas.
4. Migrar la electrónica realizada a una plaqueta diseñada para tal fin.  
5. Incorporar desvios por alta y baja presión diferenciados.
6. Incorporar mínimos y máximos de apertura de los actuadores.
7. Reemplazar los servomotores a PWM por motores controlados por tensión (0-10V) o corriente (4-20mA).
8. Cambiar el control lineal por un lazo PID.
9. Analizar en profundidad las normativas mencionadas en la introducción general para corregir y/o incorporar los requerimientos necesarios para su pasaje a producción.

---

# **Bibliografía**

[1]  Description of STM32F1 HAL and low-layer drivers. [Online]. Disponible: https://www.st.com/resource/en/user_manual/um1850-description-of-stm32f1-hal-and-lowlayer-drivers-stmicroelectronics.pdf

[2] Manual de usuario NUCLEO-F103RB [Online]. Disponible: https://www.st.com/en/evaluation-tools/nucleo-f103rb.html

[3] AT24C04C/AT24C08C - I²C-Compatible (Two-Wire) Serial EEPROM 4‑Kbit (512 x 8), 8‑Kbit (1,024 x 8). [Online]. Disponible: https://ww1.microchip.com/downloads/en/DeviceDoc/AT24C04C-AT24C08C-I2C-Compatible-%20Two-Wire-Serial-EEPROM-4-Kbit-8-Kbit-20006127A.pdf

[4] LCD-016N002B-CFH-ET - 16 x 2 Character LCD [Online]. Disponible: https://www.vishay.com/docs/37484/lcd016n002bcfhet.pdf

[5] PCF8574 Remote 8-Bit I/O Expander for I2C Bus. [Online]. Disponible: https://www.ti.com/lit/ds/symlink/pcf8574.pdf
