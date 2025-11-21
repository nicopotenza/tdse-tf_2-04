# **Control para salas de aislados**

## **Autores:**

*Marcos Joel Dias Peñaranda (111.423)*  
*Andrés Giacomelli (111.038)*  
*Nicolás Alejandro Potenza (97.024)*

**Fecha: 2do cuatrimestre 2025**

---

### **1. Selección del proyecto a implementar**

#### **1.1 Contexto general**

Las salas de aislados son áreas hospitalarias críticas diseñadas para mantener condiciones de presión diferencial controlada que eviten la transferencia de contaminantes entre ambientes. Su correcto funcionamiento es esencial tanto para proteger a pacientes inmunosuprimidos (evitando el ingreso de patógenos) como para contener agentes infecciosos (evitando su salida al pasillo o áreas comunes). 

El control de presión diferencial entre la **sala**, la **antesala** y el **pasillo** se realiza habitualmente mediante sistemas de ventilación con compuertas y ventiladores regulados. En este proyecto se propone desarrollar un **control embebido autónomo** que gestione la presión diferencial actuando sobre una **persiana modulante de retorno**, utilizando sensores de presión diferencial y una interfaz de usuario local. 

Persiana modulante de retorno: Una persiana modulante de retorno es una compuerta en el circuito de aire de retorno que se abre y cierra de forma proporcional, para regular y equilibrar el flujo de aire en un sistema de climatización.
Cumple las funciones de:
- Mantener el equilibrio de presiones en el sistema.
- Mezclar aire de retorno con aire exterior (ventilación).

Se busca así un sistema confiable, de bajo costo y fácilmente replicable en entornos hospitalarios, cumpliendo con determinados lineamientos de seguridad establecidos por normativas internacionales como **ASHRAE 170 (Ventilation of Health Care Facilities)** y las recomendaciones de la **CDC (Centers for Disease Control and Prevention)** para ambientes críticos.

Sin embargo no es el objetivo, al menos en esta primera instancia, llegar a un producto comercializable, ya que para eso sería necesario realizar una revisión mucho más exhaustiva a las normativas nacionales/internacionales vigentes, y realizar controles mucho más estrictos en el funcionamiento de este primer prototipo, lo que estalaría la complejidad por encima de lo buscado en un proyecto de mitad de carrera de ingeniería.

#### **1.2 Objetivo del proyecto y resultados esperados**

El objetivo principal es **diseñar e implementar un sistema de control de presión diferencial para salas de aislados** que permita operar en tres modos: **INMUNO, INFECTO y SET_UP**. Cada modo ajusta automáticamente la consigna y el comportamiento del control según las condiciones de seguridad requeridas.

**Objetivos específicos:**
- Medir en tiempo real la presión diferencial entre sala–antesala y antesala–pasillo.
- Mantener una diferencia de presión mínima de **50 Pa** entre sala y antesala según el modo seleccionado.
- Asegurar que la antesala mantenga siempre presión positiva respecto del pasillo.
- Accionar una persiana de retorno modulante para estabilizar la presión.
- Proveer alarmas visuales y sonoras ante condiciones anómalas.
- Permitir calibración y configuración de parámetros en modo SET_UP.


**Resultados esperados:**
- Prototipo funcional del sistema embebido basado en microcontrolador ARM.
- Interfaz local mediante display y teclado.
- Ensayo en maqueta o banco de pruebas que simule los gradientes de presión.
- Documentación técnica completa del hardware, firmware y lógica de control.

#### **1.3 Proyectos similares y justificación**

Existen sistemas comerciales de control de presión hospitalaria, como los módulos de monitoreo **Setra Lite**, **Tsi Pressure Monitor** o **Honeywell Room Pressure Monitor**, que ofrecen medición y alarmas visuales, pero suelen tener un alto costo y escasa posibilidad de personalización.  

El proyecto propuesto busca **una alternativa académica y didáctica**, implementable con hardware de disponibilidad local (sensores analógicos, controladores ARM STM32, actuadores estándar), reduciendo costos y fomentando la comprensión del lazo de control.

#### **1.4 Selección del proyecto**

Tras evaluar alternativas, se selecciona el proyecto **“Control de presión para salas de aislados”** por las siguientes razones:

- Alta relevancia sanitaria y aplicabilidad práctica.
- Posibilidad de implementación con hardware disponible en el mercado argentino.
- Alcance de complejidad adecuado para un proyecto integrador de sistemas embebidos.
- Interés personal y profesional de los autores en el área de automatización y control ambiental.

###### **1.4.1 Diagrama en bloques**

*(Ver Figura 2 - Sección 5)*

---

### **2. Elicitación de requisitos y casos de uso**

#### **2.1 Requisitos funcionales (RF)**

- **RF1:** El sistema debe medir la presión diferencial entre la sala y la antesala mediante un sensor analógico.
- **RF2:** El sistema debe accionar **únicamente la persiana modulante de retorno de la sala** mediante una salida PWM para mantener la presión objetivo.
- **RF3:** El sistema debe mostrar en pantalla el estado actual de la sala (presiones y modo de operación).
- **RF4:** El sistema debe generar alarmas visuales y sonoras ante condiciones fuera de rango.
- **RF5:** El usuario debe poder cambiar el modo de operación mediante el teclado.
- **RF6:** En modo SET_UP el sistema debe permitir calibrar sensores, modificar setpoints y probar salidas.
- **RF7:** El sistema debe registrar y mantener en memoria no volátil el último modo de operación.
- **RF8:** La **diferencia de presión entre sala y antesala** debe ser **≥ 50 Pa** en los modos automáticos; si cae por debajo de 50 Pa se genera alarma.
- **RF9:** La **antesala debe mantener presión positiva respecto del pasillo** en todo momento; la **presión diferencial sala–antesala no puede caer por debajo de 50 Pa**, y si la diferencia cae de ese valor se genera alarma.

#### **2.2 Requisitos no funcionales (RNF)**

- **RNF1:** El firmware se desarrollará en lenguaje C sobre una plataforma ARM Cortex‑M.
- **RNF2:** El tiempo de respuesta ante una desviación de presión no debe superar los 2 segundos.
- **RNF3:** La interfaz debe ser intuitiva y usable por personal no técnico.
- **RNF4:** Los componentes deben ser de disponibilidad local.

---

### **3. Modos de operación del sistema**

El sistema dispone de **tres modos de operación**, seleccionables mediante el teclado del panel de control.

#### **3.1 Modo INMUNO**

Diseñado para pacientes inmunosuprimidos, donde se requiere evitar el ingreso de contaminantes desde el exterior.

- **Objetivo:** Mantener **presión positiva** en la sala respecto de la antesala.
- **Setpoint normativo del proyecto:** **≥ +50 Pa** (se admite banda muerta con histeresis).
- **Control:** **modulación de la persiana de retorno de la sala** (no se actúa sobre inyección).
- **Alarmas:**  
  - Si la **presión diferencial sala–antesala cae por debajo de 50 Pa**, se activa alarma visual y sonora.  
  - Si la **presión antesala–pasillo es menor o igual a 0 Pa**, se activa alarma por pérdida de gradiente hacia el pasillo.

#### **3.2 Modo INFECTO**

Destinado al aislamiento de pacientes infecciosos, evitando la salida de aire contaminado.

- **Objetivo:** Mantener **presión negativa** en la sala respecto de la antesala.
- **Setpoint normativo del proyecto:** **≤ −50 Pa** (se admite banda muerta con histeresis).
- **Control:** **modulación de la persiana de retorno de la sala** (no se actúa sobre inyección).
- **Alarmas:**  
  - Si la **presión diferencial sala–antesala sube por encima de −50 Pa**, se activa alarma visual y sonora.  
  - Si la **presión antesala–pasillo es menor o igual a 0 Pa**, se activa alarma por pérdida de gradiente hacia el pasillo.

#### **3.3 Modo SET_UP**

Utilizado para calibración y mantenimiento técnico.

- **Objetivo:** Permitir configuración de parámetros sin control activo.
- **Funciones:**  
  - Calibrar sensores.  
  - Ajustar setpoints y límites.  
  - Probar actuadores y alarmas.  
  - Monitorear lecturas en tiempo real.
- **Condición especial:** No se ejecutan acciones automáticas de control.

---

### **4. Casos de uso**

| **Actor** | **Caso de Uso** | **Descripción** |
|------------|----------------|----------------|
| Operador técnico | Seleccionar modo de operación | El usuario elige entre INMUNO, INFECTO o SET_UP desde el teclado. |
| Sistema automático | Ajustar comportamiento según modo | El microcontrolador cambia parámetros de control y alarmas según el modo activo. |
| Operador técnico | Ver estado de sala | El usuario consulta en el display el estado de presión y alarmas. |
| Sistema automático | Regular presión | El controlador modula la persiana para mantener la presión estable. |
| Sistema automático | Generar alarma | Se activa alarma sonora/visual al detectar condiciones fuera de rango. |
| Operador técnico | Calibrar sensores y salidas | En modo SET_UP, el técnico calibra sensores y prueba actuadores. |

---

### **5. Diagramas del sistema**

- **Figura 1:** Diagrama de planta del sistema  
![SalaAislados](https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/Definici%C3%B3n%20de%20Requisitos%20y%20Casos%20de%20Uso%20del%20Trabajo%20Final/Im%C3%A1genes/SalaAislados.png)

- **Figura 2:** Diagrama de bloques funcional  
![DiagramaDeBloques](https://raw.githubusercontent.com/nicopotenza/tdse-tf_2-04/refs/heads/main/Definici%C3%B3n%20de%20Requisitos%20y%20Casos%20de%20Uso%20del%20Trabajo%20Final/Im%C3%A1genes/DiagramaDeBloques.png)

---

### **6. Conclusiones preliminares**

El sistema propuesto permite el control automático de salas de aislamiento bajo tres modos de operación, garantizando un gradiente de presión mínimo de **±50 Pa** entre ambientes contiguos y manteniendo la antesala siempre positiva respecto del pasillo. La arquitectura embebida garantiza flexibilidad, bajo costo y robustez, cumpliendo con los requisitos técnicos y funcionales establecidos.

---

### **7. Cumplimiento normativo (ASHRAE 170 / CDC)**

El diseño propuesto toma como referencia las normas **ASHRAE 170 – Ventilation of Health Care Facilities** y las **recomendaciones de la CDC** para ambientes hospitalarios críticos, cumpliendo con los siguientes puntos principales:

| **Aspecto** | **Referencia normativa** | **Cumplimiento en el diseño** |
|--------------|---------------------------|-------------------------------|
| **Diferencia mínima de presión** | ASHRAE 170: ±25 Pa mínimo | El sistema mantiene ±50 Pa como mínimo, excediendo la norma. |
| **Modos de operación** | CDC: Aislamiento protector (positivo) / infeccioso (negativo) | Modos INMUNO (positivo) e INFECTO (negativo) replican ambos tipos de aislamiento. |
| **Monitoreo continuo y alarmas** | ASHRAE 170 §7.2.1: monitoreo visual/sonoro continuo | RF4, RF8 y RF9 incluyen alarmas visuales y sonoras ante pérdida de gradiente. |
| **Presión positiva en antesala** | CDC: antesala siempre positiva respecto del pasillo | RF9 garantiza este gradiente en todos los modos. |
| **Calibración y verificación periódica** | CDC: sistemas deben ser verificables y calibrables | Modo SET_UP permite calibrar sensores y probar alarmas. |
| **Indicadores visuales locales** | ASHRAE 170: visualización del estado en accesos | El display muestra presiones y modo activo antes del ingreso. |
| **Continuidad de control y respuesta rápida** | ASHRAE 170: restablecimiento rápido tras perturbaciones | RNF2 define un tiempo de respuesta ≤ 2 s ante variaciones de presión. |

En conjunto, el proyecto **cumple o supera** los criterios mencionados de diseño y seguridad establecidos por ambas normas para entornos hospitalarios de aislamiento. Sin embargo, como se mencionó anteriormente, deberían analizarte las normativas vigentes en mayor profundidas para llevar, en una próxima instancia, el prototipo a un producto comercializable.
