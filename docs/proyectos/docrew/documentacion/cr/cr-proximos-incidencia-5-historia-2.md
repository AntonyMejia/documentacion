---
id: docrew-incidencia-5-cr-historia-2
title: Incidencia 5 - Comentario PDF Back
---

# Historia de Usuario – Backend / PDF  
## Almacenamiento y render de comentarios de revisión en la evaluación

---

## Historia

**Como** ????  
**Quiero** que los comentarios de revisión capturados al firmar un record se almacenen y se incluyan automáticamente en el PDF final de la evaluación  
**Para** contar con un respaldo formal de las aclaraciones o correcciones realizadas sobre la evaluación en caso de auditorías  

---

## Contexto de servicios

- **Ámbito:**  
  - API de firma de records en Web  
  - Base de datos de evaluaciones  
  - Lambda / servicio encargado de generar los PDFs de formatos 7.7 y 7.1  
- **Descripción:**  
  Cuando el revisor firma un record en la aplicación web, puede enviar un comentario opcional junto con sus datos de usuario.  
  El backend debe:  
  - Recibir y validar esta información.  
  - Guardarla de forma persistente en la BD.  
  - Exponerla para que la lambda de generación de PDF la recupere.  
  - Incluir una nueva página en el PDF con el comentario (si existe).  
  Si no hay comentario, el PDF se mantiene igual al actual.

---

## Alcance funcional (Backend)

### Incluye
- **Nuevo endpoint de creación de comentario para recibir:**  
  - `comment` (texto libre opcional)  
  - Datos del usuario firmante: `signedById`, `signedByName` (o equivalentes)  
- **Persistencia:**  
  - Guardar comentario y metadatos asociados:  
    - usuario, fecha/hora, recordId  
  - Modelo flexible para permitir comentarios futuros (columnas en `records`)  
- **Exposición de datos:**  
  - Asegurar que la lambda pueda consultar el comentario mediante endpoint, evento o payload ya existente  
- **Generación de PDF:**  
  - Actualizar lambda para:  
    - Verificar si el record tiene comentario  
    - Agregar una **nueva página** después de las firmas con:  
      - Título “Comentario del revisor”  
      - Texto del comentario  
      - Opcional: revisor + fecha/hora  
    - Si no hay comentario → no cambiar diseño actual  
- **Seguridad y cumplimiento:**  
  - Sanitización de entrada y salida  
  - Evitar inyección de contenido en el PDF  

### No Incluye
- Cambios en la lógica de negocio de evaluación  
- Historial complejo de versiones de comentarios  
- Mostrar comentarios en otras interfaces (solo PDF)  

---

## Alcance técnico (Backend)

### APIs / Servicios
- Crear endpoint de guardado de comentarios para:  
  - Aceptar `comment` y datos de usuario  
  - Validar longitud/tipo  

### Base de datos
Opciones de diseño:

**Opción A – Nuevas columnas en `records`:**  
- lastCommentText, lastCommentUserId, etc.

**Opción B – Nueva tabla `record_comments`:**  
- id, recordId, commentText, userId, userName, createdAt  

Crear migraciones y versionamiento del esquema.

### Lambda / Generación de PDF
- Actualizar lógica de PDFs 7.7 y 7.1 para:  
  - Consultar comentario por recordId  
  - Renderizar página adicional:  
    - Título  
    - Texto multilínea UTF-8  
    - Usuario y fecha (opcional)  
- Respetar plantillas actuales sin romperlas  

### Logging / Observabilidad
- Registrar:  
  - Comentarios recibidos  
  - Fallos de persistencia  
  - Errores al generar PDF  

### Seguridad y datos
- Sanitizar `commentText`  
- Verificar permisos del usuario que firma  

---

## Criterios de aceptación (Funcionales – Backend)

1. El endpoint de guardar comentarios solo acepta peticiones con comentario y datos del usuario.  
2. Si hay comentario, se almacena correctamente en la BD asociado al recordId.  
3. El PDF generado con comentario incluye una página después de las firmas con texto del comentario.  
4. El PDF generado sin comentario permanece igual al actual.  
5. El comentario almacenado puede consultarse para regenerar PDFs sin errores.  

---

## Criterios de aceptación (Técnicos – Backend)

1. Las migraciones de BD se aplican correctamente en prueba y producción.  
2. El endpoint mantiene compatibilidad hacia atrás: clientes sin comentario siguen funcionando.  
3. La lambda maneja correctamente casos de:  
   - Comentario vacío  
   - Comentario largo  
   - Caracteres especiales UTF-8  
4. No se generan errores no controlados durante firma o generación de PDF.  
5. Las pruebas automáticas cubren:  
   - Firma con comentario  
   - Firma sin comentario  
   - PDF con comentario  
   - PDF sin comentario  

---

## Tareas técnicas (Checklist – Backend)

### API / BD
- [ ] Diseñar modelo para almacenar comentarios  
- [ ] Crear migración (tabla nueva o columnas nuevas)  
- [ ] Actualizar endpoint de firma para aceptar `comment` y datos de usuario  
- [ ] Implementar persistencia del comentario  
- [ ] Actualizar documentación del API  

### Lambda / PDF
- [ ] Actualizar lambda para consultar comentario por recordId  
- [ ] Agregar página adicional al PDF cuando exista comentario  
- [ ] Implementar formato y soporte de texto multilínea UTF-8  
- [ ] Realizar pruebas de performance por generación masiva  

### QA / Pruebas Backend
- [ ] Pruebas unitarias del endpoint (con y sin comentario)  
- [ ] Pruebas de persistencia y lectura en BD  
- [ ] Pruebas de PDF: sin comentario, con comentario corto y con comentario largo  
- [ ] Pruebas de regresión para verificar que otros PDFs no se rompan  

---

## Dependencias

- Coordinación con Frontend para definir nombres de campos y validar payloads.  
- DevOps para desplegar migraciones y actualizar lambdas.  

---

## Recursos relacionados

- Esquema actual de BD de evaluaciones/records  
- Código y plantillas de lambda de PDF (7.7 y 7.1)  
- Documentación del flujo de firma de evaluaciones  

---

