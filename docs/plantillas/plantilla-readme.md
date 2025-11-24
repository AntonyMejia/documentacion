---
id: plantilla-readme-frontend
title: Plantilla README Frontend
sidebar_label: Plantilla README FE
description: Plantilla estándar para proyectos frontend.
---

# Nombre del Proyecto

Descripción breve del proyecto.  
Ejemplo: "Aplicación web desarrollada con Angular/React/Ionic para gestionar \<función principal\>."

---

## Requisitos Previos

- Node.js versión mínima recomendada.
- npm / pnpm / yarn.
- CLI del framework (si aplica).
- Git.
- Editor sugerido (VS Code).

---

## Instalación del Gestor de Paquetes

Si utilizas PNPM, instala globalmente con:

```
npm install -g pnpm
```

Verificar instalación:

```
pnpm -v
```

---

## Clonado del Proyecto

```
git clone <url-del-repositorio>
cd <carpeta-del-proyecto>
```

---

## Instalación de Dependencias

```
pnpm install
```

---

## Configuración del Entorno Local

El proyecto puede requerir archivos como:

```
src/environments/
environment.ts
environment.local.ts
environment.stage.ts
```

**Importante:**  
El archivo `environment.local.ts` contiene configuraciones sensibles y debe pedirse al equipo de desarrollo.

---

## Ejecutar en Modo Desarrollo

```
pnpm start
```

Servidor local (por defecto):

```
[http://localhost:4200](http://localhost:4200)
```

Cambiar puerto:

```
pnpm start --port 4600
```

---

## Tecnologías Principales

Lista las herramientas base del proyecto. Ejemplo:

| Tecnología      | Uso principal                                                    |
| --------------- | ---------------------------------------------------------------- |
| **Angular**     | Framework principal, arquitectura modular                        |
| **PrimeNG**     | Componentes UI dinámicos (tablas, diálogos, inputs, etc.)        |
| **TailwindCSS** | Sistema de utilidades para diseño responsivo y estilizado rápido |
| **PNPM**        | Gestión eficiente de dependencias (monorepo-friendly)            |

---

## Documentación del Proyecto

Toda la documentación extendida debe estar en:

```
/docs
```

Incluye lineamientos internos como:

- Convenciones de ramas
- Convenciones de commits
- Flujo de trabajo (workflow)
- Versionado
- Ambientes

---

## Estructura Básica del Proyecto

```
PROJECT/
├── .angular/                   # Configuración interna generada por Angular CLI
├── .github/                    # Workflows, acciones automáticas y pipelines de CI/CD
├── .vscode/                    # Configuración del entorno de desarrollo en VS Code
├── public/                     # Recursos públicos accesibles (favicon, íconos, etc.)
│   ├── assets/
│   ├── icons/
│   └── favicon.ico
├── src/
│   ├── app/
│   │   ├── core/               # Servicios globales, interceptors, guards, layouts
│   │   ├── features/           # Módulos funcionales del sistema (por dominio o sección)
│   │   ├── info/               # Componentes informativos o de ayuda
│   │   ├── services/           # Servicios generales (API, utilidades, controladores)
│   │   └── shared/             # Componentes, pipes, directivas y helpers reutilizables
│   ├── app.config.ts           # Configuración base de la aplicación
│   ├── app.routes.ts           # Rutas principales de la aplicación
│   ├── app.ts                  # Módulo raíz de Angular
│   ├── app.scss                # Estilos globales del proyecto
│   ├── index.html              # Documento HTML principal
│   ├── main.ts                 # Punto de entrada de la aplicación Angular
│   └── environments/           # Archivos de configuración por entorno
│       ├── environment.ts
│       ├── environment.local.ts         # Requerido para entorno local (no versionado)
│       └── environment.stage.template.ts
├── pops-preset.ts              # Presets o configuración base del proyecto
├── postcss.config.js           # Configuración de PostCSS (TailwindCSS, autoprefixer, etc.)
├── primeng-es.translation.ts   # Traducciones personalizadas o extensiones de PrimeNG
├── styles.scss                 # Estilos globales y configuración principal de Tailwind
├── tailwind.config.js          # Configuración de TailwindCSS
├── angular.json                # Configuración global de Angular (builds, serve, assets)
├── package.json                # Dependencias, scripts y metadatos del proyecto
├── pnpm-lock.yaml              # Bloqueo de versiones exactas de dependencias
├── tsconfig.json               # Configuración principal de TypeScript del proyecto
├── tsconfig.app.json           # Configuración TypeScript específica para la app Angular
├── tsconfig.spec.json          # Configuración TypeScript para pruebas unitarias
├── .editorconfig               # Reglas de formato y estilo de código
├── .gitignore                  # Archivos y carpetas ignoradas por Git
└── README.md                   # Documentación principal del proyecto
```

---

## Comandos Útiles

```
pnpm start
pnpm build
pnpm test
```

---

## Notas Finales

- No compartir archivos de entorno fuera del equipo autorizado.
- Documentación interna debe mantenerse en `/docs`.
- Para desarrolladores nuevos, revisar primero el documento de instalación del proyecto.

---