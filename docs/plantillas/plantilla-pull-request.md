---
id: plantilla-pr-frontend
title: Plantilla Pull Request FE
description: Plantilla estándar para PR de frontend, enfocada en vistas, componentes y ajustes de UI.
---

# Plantilla de Pull Request — Frontend

## Resumen

Describe brevemente el propósito del PR y qué problema resuelve.

- ...

---

## Contexto / Ticket

- ID o referencia: `[#ID]`
- Módulo o pantalla afectada: `...`

---

## Tipo de cambio

Selecciona lo que aplica:

- [ ] Nueva pantalla o vista
- [ ] Nuevo componente
- [ ] Refactor de componente existente
- [ ] Ajuste de estilos (CSS/SCSS/Tailwind)
- [ ] Corrección visual
- [ ] Lógica de UI (estados, validaciones, interacciones)
- [ ] Otro: `...`

---

## Detalles de implementación

Explica los cambios técnicos relevantes:

- Se agregó: `NombreDelComponente`
- Se modificó:
  - `src/...`
  - `src/...`
- Se actualizó lógica de:
  - Hook / servicio / store / context: `...`

---

## Cambios visuales

Indica si hubo cambios en la UI.

- [ ] Sin cambios visuales  
- [ ] Con cambios visuales  

Si sí hubo cambios, agrega:

**Antes**  
_(captura o descripción)_

**Después**  
_(captura o descripción)_

---

## Responsividad

- [ ] Probado en desktop  
- [ ] Probado en tablet  
- [ ] Probado en mobile  

Notas:

- ...

---

## Accesibilidad (si aplica)

- [ ] Etiquetas correctas en inputs  
- [ ] Navegación por teclado  
- [ ] Uso adecuado de roles/ARIA  
- [ ] Contraste correcto  

Notas:

- ...

---

## Pruebas realizadas

### Manual

- [ ] Flujo principal validado  
- [ ] Validaciones y estados vacíos  
- [ ] Estados de carga y deshabilitado  

Casos verificados:

1. `...`
2. `...`

### Automáticas

- [ ] Tests unitarios actualizados  
- [ ] Tests de integración/end-to-end (si aplica)  

Comando ejecutado:

```bash
npm test
````

Resultado:

* ...

---

## Impacto en otros módulos

* [ ] Cambios aislados
* [ ] Puede impactar:

  * [ ] Navegación / rutas
  * [ ] Componentes compartidos
  * [ ] Estilos globales
  * [ ] Estado global
  * [ ] API / estructura de datos

Detalles:

* ...

---

## Riesgos y consideraciones

* Riesgos potenciales:

  * ...
* Plan de reversión:

  * ...

---

## Checklist final

* [ ] Sin `console.log` innecesarios
* [ ] Sin TODOs no documentados
* [ ] Código formateado (lint/prettier)
* [ ] Nombres de variables y componentes claros
* [ ] Documentación actualizada (si aplica)

