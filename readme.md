# **Control para Salas de Aislados**

## **Autores:**

*Marcos Joel Dias Peñaranda (111.423)*  
*Andrés Giacomelli (111.038)*  
*Nicolás Alejandro Potenza (97.024)*

**Fecha: 2do cuatrimestre 2025**

---

## Resumen Ejecutivo Técnico

El proyecto propone el desarrollo de un sistema embebido de control automático de presión diferencial para salas de aislamiento hospitalario, capaz de operar en tres modos: INMUNO, INFECTO y SET_UP.
Su objetivo es garantizar el mantenimiento de un gradiente de presión estable entre sala, antesala y pasillo, evitando tanto la entrada de contaminantes en salas de pacientes inmunosuprimidos como la fuga de aire potencialmente infeccioso hacia áreas comunes.

El sistema mide en tiempo real la presión diferencial mediante sensores analógicos y actúa sobre una persiana modulante de retorno para estabilizar la presión. Incluye alarmas visuales y sonoras, display informativo y teclado para selección de modo y calibración.

En los modos automáticos se exige una diferencia mínima de ±50 Pa entre sala y antesala; si la presión cae por debajo de este valor, se generan alarmas y el sistema intenta restablecer el equilibrio. La antesala mantiene siempre presión positiva respecto del pasillo, conforme a las recomendaciones sanitarias internacionales.

El diseño se basa en un microcontrolador ARM Cortex-M, con firmware en lenguaje C y respuesta dinámica inferior a 2 s. El modo SET_UP permite calibración de sensores, prueba de actuadores y ajuste de parámetros sin afectar el control automático.

## Partes del Proyecto

### Definición de Requisitos y Casos de Uso del Trabajo Final
[Documento](https://github.com/nicopotenza/tdse-tf_2-04/blob/main/Definici%C3%B3n%20de%20Requisitos%20y%20Casos%20de%20Uso%20del%20Trabajo%20Final/Definici%C3%B3n%20de%20Requisitos%20y%20Casos%20de%20Uso%20del%20Trabajo%20Final.md)


## **Informe de Avances**

A continuación se detalla el informe de avances del sistema a partir de los requerimientos definidos.

| Estado | Descripción |
|-----|---------------------|
| 🟢 | Ya implementado |
| 🟡 | En proceso de implementación |
| 🔴 | No implementado |

---

#### **Medición de Presión**

| Req ID | Descripción | Estado |
|--------|-------------|--------|
| 1.1 | El sistema debe medir la presión diferencial entre la sala y la antesala mediante un sensor analógico. | 🟡 |
| 1.2 | La antesala debe mantener presión positiva respecto del pasillo en todo momento. | 🟡 |

---

#### **Activación de Persiana**

| Req ID | Descripción | Estado |
|--------|-------------|--------|
| 2.1 | El sistema debe accionar únicamente la persiana modulante de retorno de la sala mediante una salida PWM para mantener la presión objetivo. | 🟢 |

---

#### **Alarma**

| Req ID | Descripción | Estado |
|--------|-------------|--------|
| 3.1 | El sistema debe generar alarmas visuales y sonoras ante condiciones fuera de rango. | 🟡 |

---

#### **Configuración y Calibración**

| Req ID | Descripción | Estado |
|--------|-------------|--------|
| 4.1 | En modo **SET_UP** el sistema debe permitir calibrar sensores, modificar setpoints y probar salidas. | 🟡 |
| 4.2 | El sistema debe registrar y mantener en memoria no volátil el último modo de operación. | 🟢 |
| 4.3 | La diferencia de presión entre sala y antesala debe ser ≥ 50 Pa en los modos automáticos. | 🟢 |
| 4.4 | El tiempo de respuesta ante una desviación de presión no debe superar los 2 segundos. | 🟢 |

---

#### **Display y Teclado**

| Req ID | Descripción | Estado |
|--------|-------------|--------|
| 5.1 | El sistema debe mostrar en pantalla el estado actual de la sala. | 🟢 |
| 5.2 | El usuario debe poder cambiar el modo de operación mediante el teclado. | 🟢 |
| 5.3 | Deben existir tres modos de operación: **INMUNO**, **INFECTO** y **SET_UP**, seleccionables mediante el teclado. | 🟢 |

04/blob/main/Definici%C3%B3n%20de%20Requisitos%20y%20Casos%20de%20Uso%20del%20Trabajo%20Final/Definici%C3%B3n%20de%20Requisitos%20y%20Casos%20de%20Uso%20del%20Trabajo%20Final.md)
