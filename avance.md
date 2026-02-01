# **Sistema de Control de Presión Diferencial**
## **Informe de Avances**



**Marcos Joel Dias Peñaranda – Padrón 111423**
**Nicolas Alejandro Potenza – Padrón 97024**
**Andres Giacomelli – Padrón 111038**
**Fecha: 01/02/2026**

**2do cuatrimestre / 2025**

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
