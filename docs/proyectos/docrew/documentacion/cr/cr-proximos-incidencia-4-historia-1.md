---
id: docrew-incidencia-4-cr-historia-1
title: Incidencia 4 - Historia 1 First sync date
---

# Historia – Mostrar fecha de primera sincronización en tarjetas de Sign Records

---

## Historia

**Como** usuario evaluador
**Quiero** que la fecha mostrada en la tarjeta de la evaluación corresponda a la primera sincronización de la misma  
**Para** asegurar que, en caso de auditoría, la fecha visible refleje el momento en que la evaluación se registró por primera vez en el sistema y no solo la fecha de creación del record  

---

## Contexto de la pantalla

- **Módulo:** Web 
- **Pantalla:** Sign Records / Records waiting for signature  
- **Ubicación:** Web app → Sign Records  
- **Descripción:**  
  En esta pantalla se muestran tarjetas de evaluaciones pendientes de firma, con datos como Nickname, Crew ID, License Number, Date y RecordID. Actualmente, el campo **Date** muestra la fecha de creación del record. Se requiere que esta fecha pase a ser la fecha de **primera sincronización** de la evaluación (primer envío desde dispositivo) para fines de trazabilidad y auditoría.

---

## Alcance funcional (Frontend)

### Incluye
- **Acción de UI:**  
  - Mostrar en el campo **Date** de la tarjeta el valor enviado por backend que represente la fecha de primera sincronización.  
- **Interacciones:**  
  - El usuario sigue visualizando las tarjetas y puede firmar como hasta ahora.  
  - No cambia el flujo de navegación ni las acciones de “SIGN RECORD” o firma.  
- **Estados de UI:**  
  - Inicial: tarjetas cargadas con la nueva fecha de primera sincronización.  
  - Cargando / Proceso asíncrono: igual que en la versión actual.  
  - Éxito / Error: mismos mensajes estándar.  
- **Integración con API:**  
  - Ajustar el mapeo para que el campo **Date** use el atributo de primera sincronización (ej. `firstSyncDate`).  
  - Mantener sin cambios los demás campos de la tarjeta.  

### No Incluye
- Nuevos componentes o cambios de layout.  
- Cambios en el flujo de firma o reglas de negocio.  
- Nuevas llamadas a API (se reutiliza el endpoint actual).  
- Cálculo de la fecha de sincronización (responsabilidad de backend).  

---

## Alcance técnico (Frontend)

- **Framework:** Angular (aplicación web).  
- **Componentes nuevos:** Ninguno.  
- **Componentes reutilizados:** Card de evaluación en Sign Records.  
- **Estado / Store:**  
  - Actualizar la interfaz/modelo para consumir el nuevo campo enviado por backend (`firstSyncDate`).  
- **Manejo de errores:**  
  - Uso del mecanismo estándar (toasts/alerts).  
- **Accesibilidad:**  
  - Sin cambios; se mantiene la estructura actual.  

---

## Criterios de aceptación (Funcionales)

1. Al acceder a Sign Records, el campo **Date** de cada tarjeta corresponde a la fecha de primera sincronización enviada por backend.  
2. Al firmar una evaluación, la fecha mostrada es la misma usada como referencia en auditoría.  
3. Si la evaluación fue creada en una fecha y sincronizada en otra, se muestra **la de primera sincronización**, no la de creación.  
4. El resto de la información de la tarjeta permanece igual.  
5. No se altera el flujo de firma ni la navegación hacia SIGN RECORD.  
6. La vista es responsiva y funciona en los navegadores soportados.  

---

## Criterios de aceptación (Técnicos)

1. El frontend usa el atributo de fecha proporcionado por backend (`firstSyncDate` o equivalente).  
2. Si backend aún no envía ese campo, se plantea la posble implementación dentro del registro de los records para almacenar la información.  
3. No se generan errores de TypeScript ni fallos de lint al modificar modelos o servicios.  
4. Si backend envía un valor nulo o formato inválido, se registra error con el mecanismo estándar y se muestra mensaje genérico si aplica.  
5. El formato visible de la fecha en la tarjeta se mantiene igual al actual (ej. `YYYY-MM-DD`).  

---

## Tareas técnicas (Checklist)

### Desarrollo
- [ ] Revisar modelo de datos actual de Sign Records (interfaz de Record/DTO).  
- [ ] Confirmar con backend el nombre y formato del campo de primera sincronización (`firstSyncDate`).  
- [ ] Actualizar el servicio que consume Records para mapear este campo.  
- [ ] Actualizar el componente de la tarjeta para mostrar esta fecha en el campo **Date**.  
- [ ] Implementar fallback/documentación en caso de ausencia del campo.  
- [ ] Probar manualmente casos donde difieran la fecha de creación y la fecha de primera sincronización.  

### QA
- [ ] Validar un record con fecha de primera sincronización distinta a la fecha de creación.  
- [ ] Validar records donde ambas fechas coincidan.  
- [ ] Validar que antes/después de firmar la fecha mostrada se mantenga consistente.  
- [ ] Validar comportamiento cuando backend no envía el campo (si se simula).  
- [ ] Validar navegadores/resoluciones soportadas.  

---

## Pruebas (QA)

- [ ] Verificar que la UI muestra correctamente la fecha de primera sincronización.  
- [ ] Comparar contra la información almacenada en base de datos/logs (si QA tiene acceso).  
- [ ] Validar flujo completo: cargar Sign Records → revisar tarjeta → firmar → recargar y verificar fecha.  
- [ ] Validar manejo de errores 4xx/5xx.  
- [ ] Validar responsividad y legibilidad de la fecha.  

---

## Dependencias

### Backend
- Actualizar servicio/modelo para:  
  - Calcular y almacenar la fecha de primera sincronización.  
  - Enviar ese valor en la respuesta del endpoint (`firstSyncDate`).  

### Otros módulos
- Procesos móviles/web que registran la primera sincronización.  

---
