---
id: plantilla-historia-frontend
title: Plantilla – Historia de Usuario Frontend
sidebar_label: Plantilla Historia FE
description: Plantilla estándar para registrar historias de usuario destinadas al Frontend.
---

# Plantilla – Historia de Usuario (Frontend)

import TOCInline from '@theme/TOCInline';

---

## Historia

**Como** \<rol del usuario\>  
**Quiero** \<acción o funcionalidad\>  
**Para** \<beneficio que se obtiene\>

---

## Contexto de la pantalla

- **Módulo:** \<Nombre del módulo\>  
- **Pantalla:** \<Nombre de la vista\>  
- **Ubicación:** \<Ruta o menú\>  
- **Descripción:**  
  \<Explica brevemente la funcionalidad dentro del flujo\>

---

## Alcance funcional (Frontend)

### Incluye
- Acción de UI: \<botón, modal, menú, ícono, etc.\>
- Interacciones:
  - \<Describe cada interacción relevante\>
- Estados de UI:  
  - Inicial  
  - Cargando  
  - Proceso asíncrono  
  - Éxito / Error
- Validaciones:
  - \<Campos obligatorios, formatos\>
- Integración con API:
  - Endpoint a consumir  
  - Parámetros requeridos  
  - Manejo de `process_id`  
  - Descarga de archivo (si aplica)

### No Incluye
- Cambios en lógica de backend  
- Cálculos complejos  
- Nuevos endpoints  
- Alteración de reglas de negocio

---

## Alcance técnico (Frontend)

- Framework: \<Angular/React/Ionic/etc.\>
- Componentes nuevos:
  - \<NombreComponente\>
- Componentes reutilizados:
  - \<Inputs, modales, dropdowns, etc.\>
- Estado / Store:
  - \<NgRx, Context, Zustand, etc.\>
- Manejo de errores:
  - Estándar del proyecto
- Accesibilidad:
  - Labels, navegación por teclado, roles ARIA

---

## Criterios de aceptación (Funcionales)

1. El usuario puede iniciar la acción desde la pantalla indicada.
2. Se muestran correctamente los inputs requeridos.
3. La acción no inicia si faltan campos obligatorios.
4. El UI refleja el estado en tiempo real (loading, etc.).
5. Se muestra notificación de éxito o error.
6. El archivo se descarga correctamente (si aplica).
7. La vista es responsiva a distintos tamaños.

---

## Criterios de aceptación (Técnicos)

1. La llamada al endpoint incluye todos los parámetros.
2. El `process_id` se guarda correctamente en el estado.
3. El polling o consulta manual sigue el estándar del proyecto.
4. No se generan errores de TypeScript ni fallos de lint.
5. Logs correctos en caso de error.

---

## Tareas técnicas (Checklist)

### Desarrollo
- [ ] Crear/actualizar componentes necesarios  
- [ ] Implementar formulario o modal  
- [ ] Integrar servicio/llamada HTTP  
- [ ] Manejo de estado  
- [ ] Implementar descarga (si aplica)  
- [ ] Mensajes de validación  
- [ ] Mensajes de error y toasts

### QA
- [ ] Pruebas unitarias  
- [ ] Pruebas de UI  
- [ ] Validación de interacciones  
- [ ] Validación de permisos  
- [ ] Validación en desktop/tablet

---

## Pruebas (QA)

- [ ] Validar que la UI muestra correctamente inputs y estados  
- [ ] Validar `process_id`  
- [ ] Validar actualización de estatus  
- [ ] Validar navegación y flujos  
- [ ] Validar accesibilidad  
- [ ] Validar errores 4xx/5xx  
- [ ] Validar responsividad

---

## Dependencias

- Backend:  
  - Endpoint de generación  
  - Endpoint de estatus  
  - Endpoint de descarga  
- Diseño:
  - Mockups o Figma asociados  
- Otros módulos:
  - \<Indicar si depende de otra vista u otro feature\>

---

## Recursos relacionados

- Documentación técnica Backend  
- Especificaciones de diseño  
- Historias relacionadas

---