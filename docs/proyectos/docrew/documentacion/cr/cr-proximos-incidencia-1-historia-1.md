---
id: docrew-incidencia-1-cr-historia-1
title: Incidencia 1 - Historia 1 Validación en 7.7
---

# Historia – Validaciones de Inglés en Formato 7.7

---

## Historia

**Como** usuario evaluador  
**Quiero** que los campos de habilidades lingüísticas en inglés del formato 7.7 tengan un valor predeterminado “–” y validaciones al momento de firmar  
**Para** asegurar que estos campos no queden sin evaluar cuando el examen lo requiera y evitar guardar evaluaciones incompletas  

---

## Contexto de la pantalla

- **Módulo:** Evaluations - Movil
- **Pantalla:** Resultado de la evaluación – Formato 7.7  
- **Ubicación:** Evaluations → Evaluación 7.7 → Sección Resultado de la evaluación  
- **Descripción:**  
  En esta sección el evaluador indica si el piloto demuestra habilidades lingüísticas en inglés. Dos campos específicos deben iniciar en “–” y deben validarse al momento de firmar según el tipo de sincronización (solo evaluación o evaluación + examen).

---

## Alcance funcional (Frontend)

### Incluye
- **Acción de UI:**  
  Campos desplegables (select) con valores “–”, “YES”, “NO”.
- **Interacciones:**  
  - Los campos deben cargar con el valor predeterminado “–”.  
  - Si el usuario intenta firmar con “–”, se muestra una advertencia.  
- **Estados de UI:**  
  - Inicial: “–” como valor default.  
  - Cargando: estado normal de guardado/firma.  
  - Proceso asíncrono: envío de datos al backend.  
  - Éxito / Error: mensajes estándar.  
- **Validaciones:**  
  - Si el proceso es solo sincronización de evaluación:  
    Se permite firmar, pero se lanza advertencia.  
  - Si el proceso incluye sincronización del examen:  
    Los campos NO pueden estar en “–”; se bloquea la firma.  
- **Integración con API:**  
  - Uso del endpoint actual de firma/guardado.  
  - Envío de valores seleccionados (incluyendo “–”).  

### No Incluye
- Nuevos endpoints  
- Cambios en reglas de negocio fuera de esta sección  
- Cambios en el diseño del PDF  
- Alteración de otras secciones del 7.7  

---

## Alcance técnico (Frontend)

- **Framework:** Ionic + Angular  
- **Componentes nuevos:** Ninguno  
- **Componentes reutilizados:** Selects actuales de habilidades lingüísticas en inglés  
- **Estado / Store:** Se mantiene el estado actual usado por la evaluación 7.7  
- **Manejo de errores:** Manejo estándar (alerts, toasts, modales)  
- **Accesibilidad:** Labels claros y navegación por teclado  

---

## Criterios de aceptación (Funcionales)

1. Los dos campos de habilidades lingüísticas cargan siempre con el valor “–”.  
2. Al modificar la evaluación, los selects permiten elegir “YES” o “NO”.  
3. Al intentar firmar con uno o ambos campos en “–”:  
   - Si solo es evaluación: se muestra advertencia, pero se permite firmar.  
   - Si incluye examen: NO se permite firmar.  
4. Durante la firma del examen, los campos aparecen bloqueados (gris).  
5. Los mensajes de advertencia cumplen el estilo estándar del proyecto.  
6. El flujo de guardado muestra sus estados loading/éxito/error.  
7. La vista es responsiva en todos los dispositivos soportados.  

---

## Criterios de aceptación (Técnicos)

1. El formulario inicializa los campos con “–” sin errores de binding.  
2. El valor “–” se envía correctamente al backend cuando aplica.  
3. El frontend valida correctamente el tipo de sincronización (evaluación vs examen).  
4. No se generan errores de TypeScript ni advertencias de lint.  
5. Ante error del servicio, se registran logs y se muestran mensajes adecuados.  

---

## Tareas técnicas (Checklist)

### Desarrollo
- [ ] Añadir valor default “–” a ambos selects.  
- [ ] Implementar validación al momento de firmar según reglas del CR.  
- [ ] Mostrar advertencia cuando aplique.  
- [ ] Bloquear firma cuando el proceso incluya examen y existan valores “–”.  
- [ ] Ajustar lógica de UI para bloquear campos durante el examen.  
- [ ] Probar flujos: firma sin examen, firma con examen, edición.  

### QA
- [ ] Verificar carga inicial con “–”.  
- [ ] Verificar advertencia en firma de evaluación.  
- [ ] Verificar bloqueo en firma de examen.  
- [ ] Validar que los selects cambian entre “–”, “YES”, “NO”.  
- [ ] Validar comportamiento en móviles/tablets.  
- [ ] Validar casos de error del backend.  

---

## Pruebas (QA)

- [ ] Probar que los campos cargan siempre con “–”.  
- [ ] Intentar firmar solo evaluación → debe permitirlo con advertencia.  
- [ ] Intentar firmar examen → debe bloquear si hay “–”.  
- [ ] Validar campos bloqueados durante sincronización de examen.  
- [ ] Confirmar permisos y roles.  
- [ ] Validar responsividad.  
- [ ] Validar errores 4xx/5xx del API.  

---

## Dependencias

### Backend
- Servicio actual de firma y guardado de evaluación/examen.

### Otros módulos
- Lógica que determina tipo de sincronización.  
- Alertas/modales de advertencia según estándar.

### Diseño
- Tooltip/alerta/modal de advertencia (si aplica).  

---

## Recursos relacionados
- Documento del formato 7.7  
- Flujos de firma de evaluación y examen  
- Historias relacionadas del módulo Evaluations  

---
