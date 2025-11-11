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