---
id: workshop-2-aeromexico
title: Workshop 2 Aeroméxico — 20-Nov-2025
description: Notas de negocio sobre contratos, recesos, compensaciones ejecutivas y viáticos para pilotos y sobrecargos de Aeroméxico.
---

# Workshop 2 Aeroméxico

Notas del segundo workshop con Aeroméxico.  
El objetivo es documentar reglas de negocio, criterios de cálculo y requerimientos funcionales (core y front-end) relacionados con:

- Determinación de tipo de contrato.
- Compensaciones adicionales (recesos, tiempo extra).
- Compensaciones ejecutivas.
- Días especiales (domingo, festivos, séptimo día).
- Viáticos.
- Entregables esperados.

---

## 1. Determinación de tipo de contrato

### 1.1 Pilotos

Para pilotos, el **tipo de contrato** se determina:

- A partir de los **últimos dos dígitos del número de empleado**.
- Utilizando un **rango** definido por Aeroméxico (pendiente detallar rangos exactos).

> **Regla:** Para pilotos, el tipo de contrato se infiere principalmente por **No. de empleado** (últimos dos dígitos + rango).

### 1.2 Sobrecargos

Para sobrecargos, el tipo de contrato se determina:

- Principalmente a partir del **crew group**.
- El crew group funciona como **complemento** a la lógica de contrato.

Notas:

- El **CrewGroup** se utiliza de forma específica dentro del módulo de **jornadas**.
- Debe haber consistencia entre:
  - Tipo de contrato.
  - Crew group.
  - Configuración de jornadas.

---

## 2. Compensaciones adicionales por receso

### 2.1 Definiciones

- **Receso**  
  Tiempo intermedio entre dos asignaciones que **no** pertenecen a la **misma jornada laboral**.

- **Receso reducido**  
  Receso que es **menor al receso mínimo planeado**.  
  - La **diferencia** contra el receso mínimo se paga como **tiempo extra**.
  - El pago se hace **al doble**, bajo el concepto correspondiente de tiempo extra de vuelo.
  - No se trata de una acreditación “normal”, sino de una compensación específica.

### 2.2 Provisión y rates de pago

Al generar la **provisión**, el sistema debe:

- Contar con el **rate de pago del tiempo extra de vuelo** (p.ej., TXV/TXU/TXDV/TXSD/TXVM, etc.).
- Facilitar el cálculo del receso reducido desglosando los componentes (tiempo en receso, diferencia contra mínimo, etc.).

**Ejemplo conceptual de cálculo (TXU = 4%)**

> *(Ejemplo esquemático basado en las notas, no definitivo)*

- Rate **TXU** = 4%
- Receso planeado: 4:00  
- Receso efectivo: 5:30  
- Total equivalente para cálculo: 5.5 horas × 4%

La lógica exacta y fórmulas deben definirse en un documento de cálculo detallado, pero la idea es que:

- Se identifiquen horas sujetas a la tasa (rate).
- Se calcule el monto multiplicando por el porcentaje correspondiente.

### 2.3 Comparación entre receso real y mínimo

Variables:

- **R real**: receso real.
- **R mín**: receso mínimo planeado.

Ejemplos de lógica de pago:

1. `R real = 27:30 - 2:00 => 25:30`  
   `R mín = 24:00 - 2:00 => 22:00`  
   - Como el receso real (25:30) es **mayor** que el mínimo (22:00), **no se paga** receso reducido.

2. `R real = 24:50 - 2:00 => 22:50`  
   - Para un mínimo de 24:00, la diferencia es `1:10`.  
   - Esa diferencia se paga **al doble** → `1:10 × 2 = 2:20` (tiempo extra al doble).

### 2.4 Rates de pago

- Algunos rates mencionados:
  - **TXDV**
  - **TXSD**
  - **TXVM**
- **TXV / TXU** se utilizan como códigos de **tiempo extra de vuelo** (según configuración acordada).

### 2.5 Datos a mapear

En el modelo de datos y reportes:

- **ISD** → Se **duplica**.
- **RR** → No se duplica.

> Nota: Se requiere precisar en qué reportes y para qué cálculos aplica esta duplicidad/no duplicidad.

### 2.6 Restricciones de descanso

- Un **descanso mínimo** **no puede sobreponerse** a:
  - Otro descanso, o
  - Una jornada ya definida.

---

## 3. Reglas especiales para receso reducido (AMX)

> **IMPORTANTE**

- Si **Piloto AMX** →  
  En receso reducido, el **tiempo extra de vuelo** se paga **al doble** bajo el código **TXV**.

- Si **Sobrecargo AMX** →  
  En receso reducido, el receso se paga como **tiempo extra de vuelo (TXV)**.

---

## 4. Duty Types y tablas de recesos/jornadas

### 4.1 Duty type operativo

**DUTY TYPE OPERATIVO**

  - `PR`
  - `DR`

> Asociados a **tabla de recesos**.

### 4.2 Duty type acreditado

- **DUTY TYPE ACREDITADO**
  - Asociado a **Jornadas**:
    - Códigos de acreditación como **TXS** (por ejemplo).

### 4.3 Tablas a implementar

- **Tabla de recesos operativos**
  - Similar a la tabla de jornadas actual.
  - Debe estar ligada a **duty type**.
  - Basada en la combinación:
    - **Servicio efectuado** / **Servicio a realizar**.

- **Tabla de descansos**
  - También basada en la combinación:
    - **Servicio efectuado** / **Servicio a realizar**.
  - Debe reflejar límites de descanso, mínimos y reglas de invasión.

### 4.4 Lógica de descanso (a futuro)

- El **descanso** es el **último duty** del trip.
- El descanso **solo “invade”** (o se considera en la lógica posterior) **si y solo si**:
  - El siguiente servicio es **on-duty** (no aplica si el siguiente estado es “intocable”, vacaciones, etc.).
- Este comportamiento está planeado como **funcionalidad futura**.

---

## 5. Compensaciones ejecutivas (CMP)

### 5.1 Definición

Las **compensaciones ejecutivas** se generan cuando:

- En un vuelo **no hay ejecutivas suficientes**, o
- **No viaja ningún ejecutivo**.

Por escalafón/antigüedad:

- Las sobrecargos de mayor antigüedad cubren a las más jóvenes o faltantes.
- Cualquier sobrecargo puede llegar a cubrir la posición de ejecutivo.

Hay un portal interno para:

- **Asignar** ejecutivas.
- **Eliminar** ejecutivas.

### 5.2 Reglas de negocio

- **Bloquear** la compensación ejecutiva **solo** para los vuelos donde **no vaya un ejecutivo**.
- En **POPS** debe existir la funcionalidad para:
  - **Realizar la compensación de sobrecargo** (asignar como ejecutivo).
  - **Asignar y desasignar** posiciones de sobrecargos:
    - Varios ejecutivos pueden solicitar la misma posición, por lo que se requiere control de asignación.

### 5.3 Requerimientos Front-end / UI

- **Alerta**:
  - Mostrar un mensaje que **impida solicitar ser ejecutivo** si el sistema detecta que **ya existe un ejecutivo** asignado en ese vuelo/posición.
- **Gestión de posiciones**:
  - UI para:
    - Ver posiciones de sobrecargos.
    - Asignar/desasignar ejecutivos en cada posición.
  - Control de conflictos cuando varios ejecutivos solicitan la misma posición.

---

## 6. Días especiales y compensaciones adicionales

### 6.1 Prima dominical

- Aplica cuando el sobrecargo **trabaja en domingo**.
- Se acredita como un **evento**.
- Si la jornada toca el domingo, también se incluye el **debriefing** como parte del día.
- **No se considera el receso** para la prima dominical.

### 6.2 Séptimo día (7D)

- Aplica cuando el tripulante **trabaja 7 días consecutivos**.
- El conteo inicia:
  - Desde el momento en que comienza el **duty**.
- También se consideran:
  - **Pernoctas** (el tripulante sigue a disposición de la aerolínea).
  - Cualquier tipo de **asignación** dentro del periodo.

### 6.3 Días festivos (DFS)

Existen diferentes tipos de días festivos:

- **Día festivo doble**
  - Se considera como **día de servicio doble** a partir de **1:31** horas.
  - Si el servicio **excede** ese límite (≥ 1:31), se acredita como **doble**.
  - Si no alcanza ese tiempo, se acredita como **sencillo**.

- **Día festivo sencillo**
  - Si el servicio se realiza **antes de 1:31**, el día se acredita como festivo **sencillo**.

- **Día festivo en receso**
  - Aplica cuando el tripulante está en **receso** (por ejemplo, en una gira/pernocta).
  - Aunque esté en receso, sigue al servicio de la empresa, por lo que se reconoce el festivo.

### 6.4 Configuración de días festivos

- El sistema debe permitir que los usuarios **den de alta los días festivos**, por tipo:
  - **Conect**
  - **Sobrecargos**
  - **Pilotos**
- El sistema utiliza también los días festivos de **México**, pero:
  - Por tipo de contrato, estos pueden **variar**.
  - Puede ser necesario segmentar:
    - Por **empresa** (AMX/Conect).
    - Por **tipo de tripulante** (piloto/sobrecargo).

---

## 7. Horas de garantía

### 7.1 Definición

- Las **horas de garantía** están **capadas a 80 horas**.
- Se paga el **diferencial** para que el tripulante llegue a esas 80 horas.

### 7.2 Alcance

- Aplica sólo para:
  - **Sobrecargos/ejecutivos** con **Contrato A**.
  - Ejecutivos contratados **antes de 2014**.

- Este pago equivale a:
  - **15 horas de tiempo extra de vuelo** al mes.
  - **Excepción**: En **febrero**, sólo se consideran **10 horas** de tiempo extra de vuelo.

### 7.3 Exclusiones

No entran en el beneficio de horas de garantía:

- Personas con **incapacidad general**.
- Personas con **sanciones** (por ejemplo, suspensión).
- Tripulantes que hayan tenido:
  - **Vacaciones** dentro del periodo.
  - **Permiso sin goce de sueldo**.
- La cláusula **no aplica** si el tripulante fue contratado **después de 2014**.

### 7.4 Requerimiento Front-end

- Implementar las **horas de garantía** como una **card** o sección visible en la UI, donde:
  - Se muestre quién genera las horas de garantía.
  - Se pueda identificar fácilmente:
    - Quién sí aplica.
    - Quién no aplica (por exclusiones).

---

## 8. Viáticos

### 8.1 Tipos de viáticos

- Existen **2 tipos de viáticos**, según la moneda:
  - Viáticos en **pesos**.
  - Viáticos en **dólares**.

- Agrupación:
  - Los viáticos en pesos se actualizan de forma **trimestral**.
  - Los viáticos en dólares se actualizan de forma **anual**.
- Ambas configuraciones deben poder **actualizarse desde el sistema**.

### 8.2 Estaciones y tablas de viáticos

- Se deben poder **dar de alta estaciones**.
- Cada estación debe enlazarse con la **tabla de viáticos** correspondiente.

### 8.3 Líneas horarias y reglas por tripulante

Las líneas horarias de referencia (en **hora México**) son:

- 08:30 
- 14:30
- 20:30 

La cotejación se hace **siempre contra horario México**, aunque el vuelo esté en otra zona horaria.

Reglas:

- **Sobrecargos**:
  - Debe pasar **al menos 1 minuto** después de la hora de referencia para asignar un viático.
- **Pilotos**:
  - A partir del momento en que se **rebasa la hora** (por ejemplo, desde las 14:30 en adelante) se asigna el viático.

### 8.4 Momento de pago

- El pago se da **al llegar a la estación**.
- Si el tripulante está **en tierra** al momento de llegar al lugar:
  - Se **cambia el tipo de moneda** de acuerdo con la estación (MXN / USD).
- Existen viáticos:
  - **En tierra**.
  - **En vuelo**.
- Si el tripulante está en tierra, se deben pagar los viáticos **de la estación** correspondiente.

### 8.5 Segmentación de montos

Los montos de viáticos se segmentan por:

- **Compañía**.
- **Categoría / crewCategory**:
  - Pilotos.
  - Sobrecargos.
- Cada monto puede estar asociado a una o varias **zonas**.

### 8.6 Devoluciones

- En devoluciones, el monto se **cobra al tripulante** al que se le pagó el viático.
- Si el pago se hizo en **dólares** y hay devolución:
  - La devolución se cobra también en **dólares**.

### 8.7 Alcance de viáticos

- **Sí** se pagan viáticos para:
  - Vuelos.
  - Simuladores.
- **No** se pagan viáticos para:
  - Sesiones de **teoría**.

- Actualmente, los usuarios pueden decidir **si se pagan o no** viáticos, pero:
  - **Sólo lo que está registrado en el sistema** se considera para la **generación de viáticos**.

---

## 9. Entregables esperados

Lista de entregables mencionados en el workshop:

1. **Base de datos de sobrecargos**
   - Inicialmente puede ser estática (idealmente dinámica).
   - Contiene información de sobrecargos (contratos, crew group, etc.).

2. **Jornadas de servicio y vuelo (sobrecargos)**
   - Definición y parametrización de jornadas:
     - Tiempos de servicio.
     - Tiempos de vuelo.
     - Reglas de receso y descanso.

3. **Tipo de contrato para sobrecargos y pilotos**
   - Configuración de:
     - Sobrecargos con esquema 6x4.
     - Ejecutivos (por ejemplo EJ04).
     - Mapeo con **crew group**.

4. **Séptimo día, receso reducido y descanso reducido (Pilotos/Sobrecargos AMX)**
   - Implementación de lógica de:
     - Séptimo día.
     - Receso reducido.
     - Descanso reducido.
   - Validación de que se **lee correctamente la data** de operación.

5. **Viáticos**
   - Límites de pago por concepto:
     - **Desayuno / Comida / Cena**.
   - Listado de pago para **Non-fly** (tripulantes que no vuelan pero generan viáticos por otros motivos).

---
