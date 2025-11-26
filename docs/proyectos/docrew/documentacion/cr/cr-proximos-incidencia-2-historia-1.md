---
id: docrew-incidencia-2-cr-historia-1
title: Incidencia 2 - Historia 1 (Δ en Level)
---

# Historia 1 - Captura y almacenamiento del símbolo Δ en Level

---

## Historia

**Como** usuario evaluador
**Quiero** poder ingresar letras griegas (por ejemplo la letra Δ) en el campo Level de *Evaluation Information*  
**Para** registrar el nivel usando la nomenclatura solicitada por el cliente y que la información se conserve correctamente en el sistema  

---

## Contexto de la pantalla

* **Módulo:** Evaluations - Móvil
* **Pantalla:** Evaluation 7.7 - Simulator - Evaluation Information
* **Ubicación:** Evaluations → Seleccionar evaluación 7.7 de Simulador → Sección *Evaluation Information*
* **Descripción:**
  En esta pantalla el evaluador captura los datos generales de la evaluación en simulador. Dentro de esta sección existe el campo de texto **Level**, donde se requiere poder escribir letras griegas (particularmente Δ) sin que sean eliminadas por validaciones o servicios de sanitización al guardar la evaluación.

---

## Alcance funcional (Frontend)

### Incluye

* Acción de UI: Edición del campo de texto **Level** dentro de *Evaluation Information*.
* Interacciones:

  * El usuario puede escribir caracteres alfanuméricos y letras griegas (ej. Δ) desde el teclado del dispositivo.
  * Al abandonar el campo o guardar la evaluación, el valor se mantiene exactamente como fue capturado (ej. `D Δ`).
* Estados de UI:

  * Inicial: formulario cargado con el valor almacenado actualmente en Level (si existe).
  * Cargando: al guardar la evaluación se muestra el estado de envío estándar de la pantalla.
  * Proceso asíncrono: guardado de la evaluación mediante el endpoint actual.
  * Éxito / Error: se muestran los mensajes estándar de guardado o error.
* Validaciones:

  * El campo **Level** mantiene las validaciones actuales (obligatorio/opcional, longitud máxima, etc.).
  * No se debe bloquear ni limpiar el símbolo Δ ni otras letras griegas por reglas de validación en frontend.
* Integración con API:

  * Endpoint a consumir: endpoint actual de guardado de la evaluación 7.7 de simulador.
  * Parámetros requeridos: payload estándar de evaluación incluyendo el campo `level` con el valor capturado (ej. `"level": "D Δ"`).

### No Incluye

* Cambios en el layout o diseño de la pantalla.
* Nuevos campos o secciones en la evaluación.
* Cambios en reglas de negocio de la evaluación.
* Nuevos endpoints; se reutiliza el existente.

---

## Alcance técnico (Frontend)

* Framework: Ionic + Angular (app de Evaluations).
* Componentes nuevos:

  * No se crean componentes nuevos; solo se actualiza el componente de la pantalla de Evaluation 7.7.
* Componentes reutilizados:

  * Input de texto existente para el campo **Level**.
* Estado / Store:

  * Se mantiene la estrategia actual (servicios / estado local) asegurando que el binding del campo **Level** acepte caracteres Unicode (ej. Δ).
* Manejo de errores:

  * Se utiliza el manejo estándar del proyecto (toasts, alerts, etc.).
* Accesibilidad:

  * Se conserva el label y la asociación con el campo (`Level:`), así como la navegación por teclado sin cambios.

---

## Criterios de aceptación (Funcionales)

1. El evaluador puede acceder a la evaluación 7.7 de simulador y ver el campo **Level** en *Evaluation Information*.
2. El evaluador puede escribir la letra Δ en el campo **Level** sin que sea bloqueada.
3. Al escribir un valor como `D Δ`, guardar la evaluación y volver a abrirla, el texto se muestra exactamente igual (`D Δ`).
4. El flujo de guardado muestra los estados correspondientes (loading, éxito, error) sin cambios de comportamiento.
5. Si el backend retorna error, se muestra el mensaje estándar de error sin pérdida del valor capturado en **Level**.
6. El comportamiento es consistente en orientación horizontal/vertical y tamaños de pantalla soportados.

---

## Criterios de aceptación (Técnicos)

1. La llamada al endpoint de guardado incluye el campo **Level** con el valor Unicode (Δ) sin ser alterado por validaciones de frontend.
2. El valor de **Level** recibido en la respuesta del backend coincide con el enviado (no se reemplaza por `?`, espacio u otro carácter).
3. El servicio de sanitización usado en el frontend no elimina ni modifica la letra Δ; solo aplica reglas de seguridad ya definidas para caracteres no permitidos.
4. No se generan errores de TypeScript ni fallos de lint relacionados con cambios de tipo o encoding.
5. Se registran logs de error estándar si ocurre un fallo en la llamada al backend.
6. Al entrar en modo lectura el campo correspondiente muestra la letra Δ dentro de la evaluación correspondiente.

---

## Tareas técnicas (Checklist)

### Desarrollo

* [ ] Revisar la implementación actual del campo **Level** en Evaluation 7.7.
* [ ] Verificar/ajustar la sanitización en frontend para permitir letras griegas (Δ) en **Level**.
* [ ] Verificar que el binding y el modelo de datos manejen correctamente caracteres Unicode.
* [ ] Probar guardar y recargar evaluaciones con valores que incluyan Δ.
* [ ] Ajustar mensajes de error/validación si fuera necesario.

### QA (¿Pendiente?)

* [ ] Pruebas unitarias de componente (captura y binding del campo **Level**).
* [ ] Pruebas de UI escribiendo Δ, `D Δ` y combinaciones con otros caracteres.
* [ ] Validación de que no se pierda el símbolo al guardar y reabrir la evaluación.
* [ ] Validación de permisos/roles del usuario evaluador.
* [ ] Validación en los dispositivos/formatos soportados.

---

## Pruebas (QA) (¿Pendiente?)

* [ ] Validar que la UI muestra correctamente el campo **Level** y acepta Δ.
* [ ] Validar que el payload enviado al backend contiene el símbolo Δ en el campo `level`.
* [ ] Validar que al consultar de nuevo la evaluación se reciba y muestre Δ sin cambios.
* [ ] Validar la navegación y el flujo completo de captura/guardado.
* [ ] Validar accesibilidad básica (labels, focus).
* [ ] Validar manejo de errores 4xx/5xx.
* [ ] Validar responsividad en los dispositivos objetivo.

---

## Dependencias

* Backend:

  * Servicio/endpoint de guardado de evaluación 7.7 (incluye lógica de sanitización del lado servidor).
* Diseño:

  * No se requieren nuevos diseños; se mantiene la UI actual.
* Otros módulos:

  * Servicio de sanitización compartido que procesa los campos de texto antes de enviarlos al backend.

---

## Recursos relacionados

* Documentación técnica del endpoint de evaluación 7.7.
* CR / Documento funcional donde se solicita el soporte de letras griegas.
* Historias relacionadas con generación de PDF de evaluaciones.

---
