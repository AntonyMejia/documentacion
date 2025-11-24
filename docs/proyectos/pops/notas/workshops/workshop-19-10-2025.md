---
id: workshop-aeromexico-sobrecargos
title: Workshop Aeroméxico — 19-Nov-2025
description: Reglas de negocio, requerimientos funcionales y criterios de acreditación de tiempo extra para sobrecargos.
---

# Workshop Aeroméxico — Sobrecargos

Notas tomadas durante el workshop con Aeroméxico para documentar reglas de negocio, criterios de cálculo y requerimientos funcionales relacionados con contratos, jornadas y acreditación de tiempo extra para sobrecargos.

---

## 1. Tipos de contrato

### 1.1 Regla general por fecha de contratación

- **Contrato A**: tripulantes con fecha de contratación **≤ 17-Sep-2017**.  
- **Contrato B**: tripulantes con fecha de contratación **≥ 18-Sep-2017**.

> La **fecha de contratación (hired date)** puede tener inconsistencias. Para mayor fiabilidad se recomienda usar **crew group**.

### 1.2 Crew group y tipo de contrato

- El **crew group** permite determinar el tipo de contrato aplicable.
- Si un usuario no tiene crew group en RAVE → asumir **Contrato B**.
- Contratos aplican especialmente a **sobrecargos** (ejecutivo/sobrecargo).
- Contrato A puede existir con hired date inválida → preferir crew group.

**Regla clave**

> Las acreditaciones vencidas deben respetar el **tipo de contrato vigente en el periodo acreditado**, determinado por crew group.

### 1.3 Mapeo de contratos y grupos

#### Contrato A (≤ 17-Sep-2017)

**Ejecutivo**

- EJ01  
- EJ04  

**Sobrecargo**

- SO01  
- SO04  
  - (*Relación con SOB10: jornada alterna sujeta a validación.*)

#### Contrato B (≥ 18-Sep-2017)

**Ejecutivo**

- EJ10  
- EJ14 (*pendiente de validación*)

**Sobrecargo**

- SO10  
- SO14 (*pendiente de validación*)

### 1.4 Reglas relacionadas

- Debe existir un **input explícito** en el sistema para el tipo de contrato del tripulante.
- Si no se especifica → por defecto **“Contrato B — Sobrecargos”**.
- **Prohibir asignaciones** si el tripulante carece de **grupo de pago**.

---

## 2. Jornadas y día laborado

### 2.1 Definiciones de roster

- **Roster actual**: datos operados día a día.
- **Roster release**: datos planeados liberados al tripulante.

### 2.2 Contingencia operacional

El sistema debe:

- Comparar servicios operados vs eliminados.
- Seleccionar el escenario más favorable para el tripulante.
- Aplicar reglas específicas de sobrecargos.

### 2.3 Freeze

- Freeze global.  
- Freeze individual por planta (sobrecargos, pilotos).

### 2.4 Acreditaciones en días sin data

- No se generan acreditaciones sin data, excepto casos especiales (contingencias).
- Debe mostrarse visualización de días sin asignaciones.

---

## 3. Requerimientos del sistema

### 3.1 Reglas de negocio (back-end)

- Crew group como fuente principal para:
  - Tipo de contrato.
  - Tipo de jornada.
- No permitir asignaciones sin **grupo de pago**.
- Mantener coherencia del contrato en acreditaciones vigentes y vencidas.
- El **freeze de Nómina** no debe afectar el freeze independiente de acreditaciones.

### 3.2 Requerimientos Front-end / UI

#### Home Page

- Home general, visible para todos.
- Aceptación/rechazo por usuario, pero cualquier autorizado puede operar.

#### Tabla de acreditaciones

- Usar colores para señalar:
  - Cambios en tiempo de servicio.
  - Cambios en tiempo de vuelo.
- Incluir **indicador de cambios**.
- Permitir al usuario:
  - **Aceptar cambios**.
  - **Ignorar cambios**.

#### Manejo de conflictos

- Solicitar **comentario obligatorio** en caso de conflicto (ej. eliminación de trip).
- Registrar comentario junto con:
  - Estado final.
  - Identificador del servicio.

#### Datos de tripulantes en UI

- Mostrar:
  - Número de empleado.
  - Rol.
  - Tipo de contrato.

---

## 4. Acreditación de tiempo extra de sobrecargos

### 4.1 Provisión mensual

- Ajustes a nivel vuelo → para toda la tripulación.  
- Ajustes a nivel servicio → sólo para un tripulante.

La provisión aplica a:

- Pilotos.
- Sobrecargos de Aeroméxico.

#### Flujo

1. **Corte de mes**: solicitud de provisión (día 25).
2. **Generación de snapshot** + CSV con:
   - Tiempos generados.
   - Días pendientes (planeados).
3. **Cálculo de montos**:
   - Compensación + nómina.
   - Se desglosa por:
     - Tiempo de servicio.
     - Tiempo de vuelo.
4. **Conciliación**:
   - Comparar acreditaciones vs nómina.
   - Nómina decide monto final.
5. **Mejora sugerida**:
   - Crear reporte de horas adicional al CSV.

### 4.2 Inputs para salario tabular

- Salario tabular es individual.
- Parámetros:
  - Antigüedad/escalafón.
  - Categoría.
- Fuente:
  - Headcount (sobrecargos).
  - Compensación (pilotos).

El sistema debe permitir capturar y mantener estos valores.

### 4.3 Créditos por finiquitos

1. Validación de créditos pendientes.
2. Revisión de nómina (baja definitiva u otro movimiento).
3. En sistema:
   - Verificar créditos pendientes.
   - Reportarlos a Nómina.

**Sugerencias**

- Implementar estatus especial para acreditaciones de finiquito.
- No generar aclaraciones posteriores.
- Al generar archivo de nómina:
  - Cerrar acreditación o marcarla como finiquito.

**Regla de freeze**

- Freeze de nómina no debe interferir con freeze propio de acreditaciones.

---

## 5. Pendientes de validación con Aeroméxico

- Confirmar:
  - EJ14.
  - SO14.
- Revisar relación exacta SO04 / SOB10 (jornada alterna).
- Definir si se requiere módulo de **Tripulantes** o si JOC seguirá siendo fuente principal.
