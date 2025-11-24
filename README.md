#  Documentación de Proyectos

Repositorio centralizado para notas técnicas, plantillas, documentación funcional y organizacional de los proyectos **POPS**, **Docrew** y otros módulos internos.

Este sitio está construido con **Docusaurus**, usa **PNPM** como gestor de paquetes y se despliega automáticamente en **GitHub Pages** mediante GitHub Actions.

---

## Características principales

* Documentación estructurada por proyectos y categorías
* Plantillas reutilizables (Historias de usuario, Pull Requests, README estándar)
* Notas, workshops y registro de actividades por equipo
* Navegación mediante sidebars autogenerados
* Estilos y resaltado de código optimizado (Prism themes)
* Despliegue continuo vía GitHub Actions
* Uso de PNPM para dependencias (más rápido y eficiente)

---

## Requisitos Previos

Asegúrate de tener instalado:

| Herramienta | Versión recomendada   | Uso                     |
| ----------- | --------------------- | ----------------------- |
| **Node.js** | 18+ o 20+             | Ejecución del proyecto  |
| **PNPM**    | 8+ / 9+ / 10+         | Gestión de dependencias |
| **Git**     | Última versión        | Control de versiones    |
| **VS Code** | Opcional, recomendado | Editor                  |

### Instalar PNPM

```bash
npm install -g pnpm
pnpm -v
```

---

## Clonado del Proyecto

```bash
git clone https://github.com/AntonyMejia/documentacion.git
cd documentacion
```

---

## Instalación de Dependencias

```bash
pnpm install
```

---

## 🛠️ Ejecución en Modo Desarrollo

```bash
pnpm start
```

Servidor local por defecto: [http://localhost:3000](http://localhost:3000)

---

## Build de Producción

```bash
pnpm build
```

La salida se genera en `/build`.

---

## Despliegue (GitHub Actions + GitHub Pages)

El despliegue se realiza automáticamente al hacer push a `main`.

Workflow utilizado:

```
.github/workflows/deploy.yml
```

Este workflow:

1. Instala PNPM
2. Construye Docusaurus
3. Publica el contenido en la rama `gh-pages`
4. GitHub Pages sirve el sitio desde esa rama

Asegúrate de que en:

**Settings → Pages → Build and Deployment**

* Source: **Deploy from a branch**
* Branch: **gh-pages**
* Folder: `/ (root)`

---

## Estructura General del Proyecto

```
documentacion/
├── .github/
│   └── workflows/
│       └── deploy.yml         # Workflow de despliegue
├── docs/                      # Documentación principal
│   ├── proyectos/
│   │   ├── pops/
│   │   │   ├── actividades/
│   │   │   ├── notas/
│   │   │   └── index.md
│   │   └── docrew/
│   ├── plantillas/
│   │   ├── plantilla-historia-frontend.md
│   │   ├── plantilla-pr-frontend.md
│   │   └── plantilla-readme-frontend.md
│   └── index.md
├── src/
│   └── css/
│       └── custom.css         # Personalización de estilos
├── static/                    # Assets estáticos
├── sidebars.js                # Sidebars autogenerados
├── docusaurus.config.js       # Configuración principal
├── package.json               # Dependencias y scripts
├── pnpm-lock.yaml             # Lockfile de PNPM
└── README.md                  # Este archivo
```

---

## Estructura de Contenido Documentado

La documentación se organiza así:

### **1. Proyectos**

Cada proyecto (POPS, Docrew) contiene:

* **Notas técnicas**
* **Workshops**
* **Ideas y mejoras**
* **Actividades por miembro del equipo**
* **Integraciones futuras**
* **Referencias**

### **2. Plantillas**

Plantillas listas para reutilizar:

* Historias de usuario (FE)
* Pull Requests (FE)
* README base
* Estructura de commits
* Flujo de merge / branching

### **3. Actividades**

* Miembros del equipo asignados
* Actividades en curso
* Posibles implementaciones
* Enlaces a tarjetas de ClickUp (si aplica)

---

## Comandos Útiles

| Comando      | Descripción                          |
| ------------ | ------------------------------------ |
| `pnpm start` | Inicia servidor de desarrollo        |
| `pnpm build` | Genera build de producción           |
| `pnpm serve` | Sirve localmente la build            |
| `pnpm lint`  | Ejecuta linter (si está configurado) |

---

## Notas Finales

* No almacenar información sensible en el repositorio.
* Las configuraciones internas deben mantenerse solo en entornos controlados.
* Para nuevas features o ideas, usa la sección “Ideas y mejoras”.
