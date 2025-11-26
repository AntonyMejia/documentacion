---
id: docrew-incidencia-2-cr-historia-2
title: Incidencia 2 - Historia 2 (Δ en PDF)
---

# Historia 2 - Render del símbolo Δ en el PDF de la evaluación

---

## Historia

**Como** usuario evaluador
**Quiero** que el valor capturado en el campo Level (incluyendo letras griegas como Δ) se muestre correctamente en el PDF generado de la evaluación 7.7  
**Para** que el documento oficial refleje exactamente la evaluación registrada y sea aceptado por el cliente y autoridades  

---

## Contexto de la pantalla

- **Módulo:** Evaluations - Web
- **Pantalla:** Records - Vista de generación/consulta de PDF

- **Ubicación:** Evaluations → Seleccionar evaluación 7.7 → Acción de generación o consulta de PDF  
- **Descripción:**  
  Desde esta pantalla/acción se genera el PDF de la evaluación. El campo Level de *Evaluation Information* debe mostrarse en el PDF respetando letras griegas (ej. Δ), sin sustituciones ni pérdida de información.

---

## Alcance funcional (Frontend)

### Incluye
- Acción de UI: Botón/acción actual que dispara la generación/visualización del PDF de la evaluación.
- Interacciones:
  - El usuario genera el PDF de una evaluación que contiene un valor con Δ en Level.
  - El usuario visualiza/descarga el PDF y el campo Level muestra el mismo valor (ej. D Δ).
- Estados de UI:  
  - Inicial: evaluación cargada con datos ya guardados.  
  - Cargando/Proceso: generación de PDF según flujo actual.  
  - Éxito/Error: mensaje estándar cuando el PDF se genera o existe un fallo.  
- Validaciones:
  - No se modifica el valor de Level antes de enviarlo al servicio que genera el PDF.
- Integración con API:
  - Endpoint a consumir: endpoint actual de generación de PDF.
  - Parámetros requeridos: datos completos de la evaluación, incluyendo `level` con Δ sin alteraciones.
  - Descarga de archivo: Incluida según comportamiento actual.

### No Incluye
- Cambios en el diseño o estructura del PDF.  
- Nuevas secciones o campos en el PDF.  
- Cambios en reglas de negocio de la evaluación.  
- Nuevos endpoints; se reutiliza el existente.

---

## Alcance técnico 

---

## Criterios de aceptación (Funcionales)

1. El evaluador genera el PDF de una evaluación donde el campo Level contiene la letra Δ.  
2. En el PDF, el campo Level muestra el mismo contenido capturado (Δ, D Δ, etc.) sin caracteres extraños ni espacios vacíos.  
3. La generación del PDF mantiene los estados (loading, éxito, error) ya definidos.  
4. El PDF se puede descargar/visualizar sin cambios en el flujo actual.  
5. El comportamiento es consistente en visores soportados (iOS, Adobe Reader, navegadores).  

---

## Criterios de aceptación (Técnicos)

1. La data enviada al servicio de generación de PDF contiene el valor de Level con Δ sin ser modificado por sanitización o encoding en frontend.  
2. El backend/servicio de PDF devuelve un archivo donde el campo Level respeta la codificación Unicode.  
3. No se introducen errores de compilación ni advertencias de lint.  
4. En caso de error del servicio de PDF, se registran logs y se muestran mensajes estándar al usuario.  

---

## Tareas técnicas (Checklist)

### Desarrollo
- [ ] Revisar payload actual enviado al servicio de PDF y confirmar inclusión de Level.  
- [ ] Validar/ajustar que Level se envíe como texto Unicode sin transformación previa.  
- [ ] Probar generación de PDF con distintos valores de Level que incluyan Δ.  

### QA (¿Pendiente?)
- [ ] Generar PDFs con ejemplos: Δ, D Δ, A Δ, etc.  
- [ ] Verificar visualmente que el símbolo Δ se muestre correctamente.  
- [ ] Validar manejo de errores en generación (time-out, error 4xx/5xx).  
- [ ] Validar descarga/visualización en los visores objetivo.  

---

## Pruebas (QA) (¿Pendiente?)

- [ ] Validar que la UI permite generar el PDF desde la evaluación 7.7.  
- [ ] Validar que el símbolo Δ llega al backend para la generación del PDF.  
- [ ] Validar que el PDF muestra el símbolo correctamente.  
- [ ] Validar navegación entre evaluación y vista de PDF.  
- [ ] Validar manejo de errores y mensajes.  

---

## Dependencias

- **Backend:**  
  - Servicio de generación de PDF (incluye sanitización y soporte Unicode).

- **Otros módulos:**  
  - Servicio de sanitización compartido que procesa datos antes de la creación del PDF.

---

## Recursos relacionados

- Documentación técnica del servicio de generación de PDF.  
- CR/Historias relacionadas con sanitización y soporte Unicode.  

---
