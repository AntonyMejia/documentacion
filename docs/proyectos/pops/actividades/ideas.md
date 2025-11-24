---
title: Ideas y mejoras por implementar
description: Registro de ideas, mejoras técnicas, reorganización arquitectónica y procesos a implementar en el proyecto.
---

# Ideas y mejoras por implementar

Este documento reúne ideas, mejoras y propuestas técnicas para la evolución del proyecto, organizadas por categorías.  
Las iniciativas aquí listadas pueden derivarse en tareas, historias de usuario o spikes técnicos.

---

## 1. Arquitectura y organización del código

### 1.1 Migración a monorepo (NX)
**Contexto:**  
El repositorio actual contiene múltiples módulos y un crecimiento acelerado de archivos, lo cual dificulta mantenimiento y escalabilidad.

**Propuesta:**  
Evaluar una migración a **NX** para convertir el proyecto en un monorepo estructurado.

**Beneficios esperados:**
- Mejor segmentación por dominios y módulos.
- Builds y deploys más rápidos con **caching inteligente**.
- Control más claro sobre dependencias internas.
- Estructura más limpia para futuros módulos.

**Consideraciones / riesgos:**
- Amplify requiere ajustes para reconocer un monorepo.
- Validar pipelines y rutas de compilación.
- Es necesario hacer pruebas de funcionamiento completas tras la migración.

---

## 2. Documentación y procesos

### 2.1 Documentación formal con Docusaurus
**Objetivo:**  
Integrar la documentación técnica, funcional y de procesos en Docusaurus.

**Líneas de trabajo:**
- Documentación del código (arquitectura, módulos, decisiones técnicas).
- Documentación de features (requerimientos, flujos, pantallas).
- Manuales internos para devs (plantillas de historias de usuario, PR y README).
- Crear un **mapa del sitio** dentro de Docusaurus para navegación general.

### 2.2 Historias de usuario y Pull Requests obligatorios
- Definir un proceso estándar para historias de usuario.
- Estructurar PRs con las plantillas definidas.
- Cualquier cambio debe:
  - Adjuntar su historia de usuario.
  - Abrir PR.
  - Seguir la plantilla establecida.

### 2.3 Estandarizar estructura de commits
Posibles estrategias:
- Conventional Commits (`feat:`, `fix:`, `docs:`, etc.)
- Generación automática de changelogs.
- Facilitar releases controlados.

---

## 3. Pruebas y calidad del código

### 3.1 División del trabajo de pruebas
**Propuesta:**
- **Unitarias** → responsabilidad de cada dev al implementar su feature.
- **E2E** → responsabilidad del dev encargado de documentación/QA.

### 3.2 Herramientas
- **Unitarias:** Jasmine / Karma (Angular)
- **E2E:** Cypress para validar flujos completos y pruebas visuales si aplica.

### 3.3 Integración CI/CD futura
- Validar pruebas automáticas antes del deploy.
- Reglas para bloquear merges sin tests.

---

## 4. Exploración tecnológica

### 4.1 Evaluar integración de Vue con Angular
**Estado:** idea en exploración.  
Se evalúa si:
- Vue podría mejorar la flexibilidad del front.
- Existen limitaciones de integración Angular + Vue en entornos corporativos.
- El costo/beneficio es viable.

### 4.2 Revisión de nuevas librerías o frameworks de UI
- Evaluar reducción de complejidad en components.
- Buscar alternativas a componentes repetitivos actuales.

---

## 5. Gestión y asignación de actividades

### 5.1 Reasignación y definición de responsabilidades
Propuesta de roles por área:

| Área | Responsable | Descripción |
|------|-------------|-------------|
| Actualización de dependencias | Dev asignado | Revisión periódica + impacto |
| Pruebas unitarias | Dev por feature | Asegurarlas antes de PR |
| Pruebas E2E | QA / Dev documental | Flujos completos |
| Documentación (Docusaurus) | Dev documental | Funcional + técnica |
| Arquitectura | Tech Lead | Revisión estructura general |
| Mapa del sitio | Dev documental | Estructura navegable en Docusaurus |

### 5.2 Seguimiento de dependencias
- Revisar Dependabot.
- Revisar seguridad de paquetes.
- Plan de actualización trimestral o mensual.

---

## 6. Ideas adicionales por explorar

- Implementar un tablero visual en Docusaurus para roadmap interno.
- Integrar Storybook para centralizar componentes UI.
- Automatizar generación de documentación a partir de comentarios TS.
- Crear script interno para validar estructura de módulos.
- Mejorar performance con lazy loading y módulos dinámicos.
- Analizar migración de Node 16 → 18/20 dependiendo del soporte AWS.

---

## 7. Experimentos por ejecutar

### Experimento 1: Migración gradual a monorepo
- **Hipótesis:** La estructura NX reducirá tiempo de build y facilitará la mantenibilidad.
- **Cómo medir:** Comparar tiempos de build, complejidad de PRs, dependencias y estructura final.
- **Resultado:** *(pendiente)*

### Experimento 2: Prototipo de documentación feature → Docusaurus
- **Hipótesis:** La documentación cercana al código mejora onboarding y calidad.
- **Cómo medir:** Uso, claridad, retroalimentación del equipo.
- **Resultado:** *(pendiente)*
