---
id: docrew-incidencia-3-cr-historia-1
title: Incidencia 3 - Historia 1 Filtros en records
---

# Historia – Filtros por tipo de evaluación y fecha en Records (Last Closed Records)

---

## Historia

**Como** usuario evaluador que consulta registros cerrados en la aplicación web  
**Quiero** poder filtrar los Records por tipo de evaluación y por fecha  
**Para** localizar rápidamente los registros cerrados que necesito revisar sin tener que recorrer toda la lista  

---

## Contexto de la pantalla

- **Módulo:** Web Evaluations  
- **Pantalla:** Records (Last Closed Records)  
- **Ubicación:** Web app → Records  
- **Descripción:**  
  La pantalla muestra los últimos registros cerrados (*Last Closed Records*). Actualmente solo permite buscar por Crew ID. Se requiere agregar los mismos filtros disponibles en *Sign Records*: filtro por tipo de evaluación y filtro por fecha, manteniendo el mismo comportamiento visual y funcional.

---

## Alcance funcional (Frontend)

### Incluye
- **Acción de UI:**  
  - Agregar en la barra superior de Records:  
    - Dropdown **Evaluation type** (opción *All* + lista de tipos de evaluación).  
    - Selector de fecha **Date** (datepicker) para filtrar por fecha de evaluación/review.
- **Interacciones:**  
  - El filtro de tipo de evaluación funciona igual que en *Sign Records*:  
    - Valor por defecto: **All**.  
    - Al seleccionar un tipo, se muestran solo los Records correspondientes.  
  - El filtro de fecha funciona igual que en *Sign Records*:  
    - Valor por defecto: sin fecha seleccionada.  
    - Al elegir una fecha, se muestran solo los Records cuya fecha coincida con la lógica usada en *Sign Records* (igualdad/rango).  
  - Los filtros deben combinarse con la búsqueda por Crew ID.  
  - Debe existir una forma de limpiar los filtros (volver a All y remover fecha).  
- **Estados de UI:**  
  - Inicial: lista de Records sin filtros (equivalente a *All* + sin fecha).  
  - Cargando: estado visible al aplicar filtros o cambiar paginación.  
  - Proceso asíncrono: consulta al endpoint con parámetros.  
  - Éxito / Error: mensajes estándar cuando la consulta termina.  
- **Validaciones:**  
  - Valores válidos de “Evaluation type” deben ser los mismos que en *Sign Records*.  
  - La fecha debe cumplir el formato del datepicker estándar.  
- **Integración con API:**  
  - Reutilizar el endpoint actual de Records.  
  - Agregar parámetros de filtro (evaluationType, date) siguiendo la misma estructura que *Sign Records*.  
  - Mantener paginación existente (page, items per page, etc.).  

### No Incluye
- Creación de nuevos endpoints  
- Cambios en la estructura de los cards de Records  
- Modificación de reglas de negocio de cierre de registros  
- Cambios en la lógica interna de *Sign Records* (solo se replica comportamiento)  

---

## Alcance técnico (Frontend)

- **Framework:** Angular (web app)  
- **Componentes nuevos:**  
  - Opcional: componente de filtro reutilizable (si no se puede usar el de *Sign Records* directamente).  
- **Componentes reutilizados:**  
  - Input de búsqueda por Crew ID  
  - Dropdown y datepicker de *Sign Records*  
- **Estado / Store:**  
  - Manejo de filtros en estado local o store existente (NgRx/servicio), manteniendo sincronización con paginación/URL si aplica.  
- **Manejo de errores:**  
  - Uso del mecanismo estándar de la app (toasts/banners/modales).  
- **Accesibilidad:**  
  - Labels claros: “Evaluation type”, “Date”.  
  - Navegación por teclado y soporte a screen readers alineado con *Sign Records*.  

---

## Criterios de aceptación (Funcionales)

1. Al abrir Records, se muestran los filtros *Evaluation type* (All) y *Date* (vacío).  
2. Seleccionar un tipo filtra correctamente los Records por tipo.  
3. Seleccionar una fecha filtra la lista usando la misma lógica que *Sign Records*.  
4. La combinación Crew ID + tipo + fecha filtra el conjunto adecuadamente.  
5. Al limpiar la fecha y regresar el tipo a All, la vista vuelve al estado inicial.  
6. La app muestra loading y errores cuando el servicio falla sin romper la UI.  
7. La vista Records es responsiva en todos los tamaños soportados.  

---

## Criterios de aceptación (Técnicos)

1. La llamada al endpoint incluye los parámetros `evaluationType` y `date` con el mismo formato que *Sign Records*.  ← Posible mejora, se puede filtrar con la data local 
2. La lógica de filtros aplicada en Records es equivalente a la de *Sign Records* sin duplicar código innecesario.  
3. No se generan errores de TypeScript ni fallos de lint.  
4. La integración con paginación funciona correctamente con filtros activos.  
5. Errores de API se gestionan con el mecanismo estándar de logging/mensajes.  

---

## Tareas técnicas (Checklist)

### Desarrollo
- [ ] Revisar implementación de filtros en *Sign Records* (dropdown y datepicker).  
- [ ] Extraer o reutilizar componentes/lógica para Records.  
- [ ] Agregar controles de filtro en la barra superior.  
- [ ] Conectar los filtros al servicio de Records (query params/body).  
- [ ] Asegurar compatibilidad con búsqueda por Crew ID.  
- [ ] Probar escenarios combinados de filtros y paginación.  

### QA
- [ ] Comparar funcionamiento con *Sign Records* (mismos filtros y resultados).  
- [ ] Probar filtro solo por tipo.  
- [ ] Probar filtro solo por fecha.  
- [ ] Probar combinación tipo + fecha.  
- [ ] Probar combinación Crew ID + tipo + fecha.  
- [ ] Validar paginación + filtros.  
- [ ] Validar navegadores/resoluciones soportadas.  

---

## Pruebas (QA)

- [ ] Validar que los filtros se muestran y funcionan correctamente.  
- [ ] Validar que el payload al backend contiene los parámetros adecuados.  
- [ ] Validar que los resultados coinciden con los filtros aplicados.  
- [ ] Validar navegación, recarga y persistencia/reseteo de filtros según comportamiento actual.  
- [ ] Validar manejo de errores 4xx/5xx.  
- [ ] Validar responsividad.  

---

## Dependencias

### Backend
- Endpoint actual de Records que soporte los parámetros de filtro (o requiera ajuste mínimo).

### Diseño
- Uso de los mismos componentes/estilos ya definidos en *Sign Records*.  

### Otros módulos
- Lógica actual de filtros en *Sign Records*.  

---

## Recursos relacionados
- Código fuente de *Sign Records*  
- Documentación de servicios web de Records / Sign Records  
- Historias previas del módulo Evaluations  

---
