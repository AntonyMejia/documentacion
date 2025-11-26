---
id: docrew-incidencia-5-cr-historia-1
title: Incidencia 5 - Comentario PDF Front
---

# Historia de Usuario – Campo de texto libre antes de firmar un record (Frontend)

---

## Historia

**Como** revisor
**Quiero** ver un modal con un campo de texto libre al momento de firmar definitivamente un record  
**Para** poder registrar comentarios de corrección o aclaración que se envíen al backend y se reflejen en el PDF final de la evaluación  

---

## Contexto de la pantalla

- **Módulo:** Web 
- **Pantalla:** View PDF / Sign Record  
- **Ubicación:** Web app → Sign Records → View PDF
- **Descripción:**  
  En la vista de detalle de un record, el revisor puede revisar la información y firmar la evaluación. Antes de ejecutar la firma final, se debe mostrar un modal que permita capturar un comentario opcional. Al confirmar, el comentario y los datos del usuario logueado se envían al backend junto con la petición de firma.

---

## Alcance funcional (Frontend)

### Incluye
- **Acción de UI:**  
  - Al hacer clic en *Sign Record*, mostrar un botón que permita abrir un modal para guardar comentarios de la evaluación, previo a la firma.  
- **Interacciones:**  
  - El modal contiene:  
    - Título y mensaje explicando que el comentario es opcional.  
    - `textarea` para escribir el comentario.  
    - Botones: **Confirmar** y **Cancelar**.  
  - El usuario puede:  
    - Cerrar el modal sin escribir comentario.  
    - Escribir comentario y guardar → firmar.  
- **Estados de UI:**  
  - Inicial: botón *Sign Record* disponible.  
  - Inicial: botón *Create comment* disponible.  
  - Modal: textarea habilitado y botones activos (Solo cancelar, guardar se habilita cuando el textarea no este vacia).  
  - Cargando: al confirmar, deshabilitar controles y mostrar indicador.  
  - Éxito: cerrar modal.  
  - Error: mostrar mensaje y permitir reintentar o cancelar.  
- **Validaciones:**  
  - Longitud máxima (ej. 2000 caracteres).  
  - Manejo de caracteres especiales (Unicode permitido).  
  - Comentario no obligatorio.  
- **Integración con API:**  
  - Implementar petición post para guardar comentario:  
    - `comment`  
    - Identificadores del usuario logueado (`userId`, `userName`, etc.)  

### No Incluye
- Edición/eliminación de comentario después de la firma.  
- Historial de múltiples comentarios por record.  
- Cambios en la UI móvil.  
- Cambios en layout del PDF (se resuelve en backend).  

---

## Alcance técnico (Frontend)

- **Framework:** Angular  
- **Componentes nuevos:**  
  - `CommentSignRecordModalComponent` (o equivalente).  
- **Componentes reutilizados:**  
  - Estilos, botones y base de modales existentes.  
- **Autenticación / usuario actual:**  
  - Reutilizar servicio actual para obtener: `userId`, `userName`.  
- **Gestión de estado:**  
  - Comentario manejado localmente dentro del modal.  
- **Manejo de errores:**  
  - Usar toasts/alerts estándar.  
  - Modal solo se cierra si la firma fue exitosa.  
- **Accesibilidad:**  
  - Focus inicial en textarea.  
  - Cerrar con tecla ESC.  
  - Navegación por teclado y roles ARIA.  

---

## Criterios de aceptación (Funcionales – Frontend)

1. Al pulsar *Create comment*, se abre un modal con textarea y botones Confirmar/Cancelar.  
2. El usuario puede firmar sin escribir comentario.  
3. Si escribe un comentario y confirma, el comentario se envía correctamente al backend.  
4. Si el usuario cancela, el modal se cierra sin firmar.  
5. El modal muestra estados de carga y mensajes en caso de error.  
6. El modal funciona correctamente en resoluciones soportadas.  

---

## Criterios de aceptación (Técnicos – Frontend)

1. La llamada al endpoint de firma incluye `comment` y los datos del usuario.  
2. No existen errores de TypeScript ni fallos de lint.  
3. Ante un error HTTP (4xx/5xx), se muestra mensaje estándar y el modal queda disponible para reintentar.  
4. El comentario no se pierde mientras el modal esté abierto (solo se limpia al confirmar o cancelar).  

---

## Tareas técnicas (Checklist – Frontend)

### Desarrollo
- [ ] Crear componente de modal para comentario antes de firmar.  
- [ ] Integrar apertura del modal al hacer clic en *Create comment*.  
- [ ] Ajustar servicio de firma para enviar `comment` + datos de usuario.  
- [ ] Manejar estados de carga y error dentro del modal.  
- [ ] Revisar accesibilidad (focus, teclado, ARIA).  

### QA
- [ ] Probar firma sin comentario.  
- [ ] Probar firma con comentario corto/largo/saltos de línea/caracteres especiales.  
- [ ] Probar botón Cancelar.  
- [ ] Validar payload enviado al backend.  
- [ ] Validar en navegadores soportados.  

---

## Dependencias

- Backend que acepte y persista el comentario y datos del usuario.  
- Servicio de generación de PDF que use esta información.  

---

## Recursos relacionados

- Documentación del endpoint de firma de record.  
- Guías de diseño de modales del proyecto.  
- Historias de usuario de firma de evaluaciones Web.  

---
