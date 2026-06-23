# Informe de Laboratorio: Gestión Colaborativa de Cambios con Git y GitHub
**Proyecto:** EduCampus LMS  
**Estudiante:** Santiago Sanabria Rubiano  
**Rol Asignado:** Equipo 5 — Guía de Contribución (`docs/contribucion.md`)

---

## 🛠️ Desarrollo de los Retos Técnicos

### 📂 Reto 1: Publicación Inicial del Trabajo
* **Rama creada:** `docs/contribucion` (Ramificada desde `develop`).
* **Archivos gestionados:** `docs/contribucion.md`
* **Flujo realizado:** Se inicializó el espacio de trabajo local aislando el entorno de desarrollo. Se redactaron las secciones exigidas en la guía y se registraron mediante commits atómicos con nomenclatura semántica (*Gitmojis*).

#### Evidencia de la Carga Inicial en GitHub:
![image-20260623131002977](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20260623131002977.png)

#### Pregunta de Reflexión:
**¿Por qué no es recomendable trabajar directamente sobre la rama principal en un proyecto colaborativo?**
> **Respuesta:** Trabajar directamente sobre la rama principal (`main`/`master`) o de integración (`develop`) destruye el principio de aislamiento de características (*Feature Branch Workflow*). Si múltiples desarrolladores introducen código de forma simultánea sin revisión previa, se pueden sobrescribir cambios, introducir errores críticos a producción y romper la estabilidad de la línea base del software.

---

### 🔄 Retos 2 y 3: Actualización e Integración con Cambios Remotos
* **Situación:** El Líder Técnico incorporó una nueva sección de *Reglas Generales* en el archivo `README.md` remoto y la rama de trabajo quedó atrasada.
* **Flujo realizado:** Se descargó el árbol de cambios del servidor remoto para actualizar el entorno local de desarrollo. Posteriormente, se integraron los cambios locales de la rama de documentación dentro de la rama principal de desarrollo mediante una fusión limpia.

#### Evidencia del Historial y Grafo de Ramas Integradas Localmente:
![image-20260623131034421](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20260623131034421.png)

#### Pregunta de Reflexión (Reto 2):
**¿Qué riesgo existe si un desarrollador trabaja varios días sin actualizar su repositorio local?**
> **Respuesta:** El principal riesgo es el desfase de historial o *drift*. Entre más tiempo pase un desarrollador aislado sin sincronizarse, mayor será la divergencia entre su código y el de la línea base. Al momento de querer integrar su trabajo, la probabilidad de enfrentarse a conflictos complejos de fusión aumenta exponencialmente, además de que podría estar construyendo lógicas sobre bases de código ya obsoletas o refactorizadas.

#### Pregunta de Reflexión (Reto 3):
**¿Qué diferencia hay entre integrar cambios generando una unión explícita de ramas y reorganizar una rama para que sus cambios parezcan construidos sobre la versión más reciente?**
> **Respuesta:**
> * La **unión explícita (`git merge`)** preserva el historial cronológico exacto tal y como sucedió en el tiempo, uniendo las ramas a través de un commit de fusión de tres vías (*merge commit*), manteniendo la bifurcación visible en el árbol.
> * La **reorganización (`git rebase`)** reescribe la historia del proyecto de forma lineal. Toma los commits de la rama de trabajo, actualiza la base con lo último de la rama destino y luego aplica tus commits encima uno por uno. Esto genera un historial limpio y perfectamente secuencial, libre de commits de mezcla innecesarios.

---

### ⚠️ Reto 4: Conflicto en Documentación General
* **Situación:** Modificación en paralelo del archivo `README.md` por el Equipo 1 y el Líder Técnico en la misma línea de descripción.
* **Flujo realizado:** Para recrear y solucionar este escenario de manera segura, se generó un punto de control en una rama alternativa para el Líder Técnico desde el commit inicial (`0601837`). Al intentar incorporar los cambios concurrentes de `main`, Git detuvo la fusión automática por colisión de líneas. Se editó el archivo manualmente, eliminando las marcas de control de Git y unificando las intenciones en una redacción técnica coherente.

#### Pregunta de Reflexión:
**¿Por qué un conflicto no debe resolverse simplemente aceptando “mi versión” o “la versión entrante” sin revisar el contenido?**
> **Respuesta:** Porque Git es una herramienta que detecta colisiones de líneas, pero carece de contexto semántico o criterio técnico. Aceptar ciegamente un lado del conflicto (*ours* o *theirs*) puede borrar lógica vital del negocio, romper la cohesión técnica de los documentos o eliminar código funcional desarrollado por otros miembros del equipo. La resolución consciente garantiza la integridad del producto final.

---

### 🧼 Reto 5: Limpieza de Commits antes del Pull Request (`Squash`)
* **Situación:** Existencia de múltiples commits desordenados o poco descriptivos en el historial local del flujo de trabajo (`cambio inicial`, `arreglo`).
* **Flujo realizado:** Se utilizó la herramienta de reajuste interactivo (`git rebase -i HEAD~2`) para compactar (*squash*) los micro-cambios locales en un único bloque significativo y limpio, aplicando el mensaje formal exigido por la rúbrica.

#### Evidencia del Historial Limpio con el Commit Unificado:
![image-20260623131111078](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20260623131111078.png)

#### Pregunta de Reflexión:
**¿Por qué es importante limpiar commits antes de solicitar una revisión en GitHub?**
> **Respuesta:** La revisión de código es una actividad humana costosa en tiempo. Presentar un Pull Request plagado de mensajes vagos o pruebas fallidas entorpece la lectura de los pares evaluadores; compactarlo en un único commit descriptivo simplifica el proceso de aprobación y optimiza la legibilidad de la bitácora histórica del software.

---

### 🍒 Retos 6 y 7: Recuperación de un Cambio Específico (`Cherry-pick`) y Conflicto
* **Situación:** El Equipo 6 posee una rama experimental con múltiples pautas, pero la dirección del proyecto solo autorizó integrar de inmediato la sección dedicada a los *Mensajes de Commit*.
* **Flujo realizado:** Se creó la rama `experimental/buenas-practicas-avanzadas` para registrar la documentación. Posteriormente, se regresó a una línea de trabajo estable y se aplicó una extracción quirúrgica mediante `git cherry-pick`, inyectando únicamente el commit aprobado de manera lineal encima de `develop` sin arrastrar el resto del trabajo inestable.

#### Evidencia del Cherry-pick Aplicado Exitosamente:
![image-20260623131150782](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20260623131150782.png)

#### Pregunta de Reflexión (Reto 6):
**¿En qué tipo de situaciones conviene traer solo un cambio específico en lugar de integrar una rama completa?**
> **Respuesta:** Conviene cuando una rama contiene múltiples experimentos o características de desarrollo a largo plazo (*work in progress*) que aún no pasan el control de calidad, pero se necesita con urgencia corregir un bug crítico o añadir una subsección específica que ya está madura y lista para producción, sin arrastrar el resto de los cambios inestables.

---

### 🏁 Reto 8: Pull Request Final
* **Situación:** Finalización formal de las tareas de documentación técnica, limpieza del árbol de commits y sincronización con el servidor remoto.
* **Flujo realizado:** Apertura del canal formal de integración en GitHub desde las ramas limpias hacia la rama integradora global.

#### Cuerpo Formal del Pull Request enviado:
```markdown
## Descripción
Se consolida la documentación técnica oficial para la plataforma EduCampus LMS, cumpliendo con la resolución de conflictos en la descripción general y la adición de buenas prácticas de desarrollo.

## Archivos modificados
- `README.md`
- `docs/contribucion.md`
- `docs/buenas-practicas.md`

## Cambios principales
- **Reto 4 & 5:** Integración unificada de la descripción académica del proyecto en el README y limpieza del historial de commits (*Squash*) bajo un mensaje semántico y claro.
- **Reto 6 & 7:** Aplicación quirúrgica (*Cherry-pick*) de las directrices estables para la redacción de mensajes de commit desde la rama experimental.

## Conflictos resueltos
- Se solucionó manualmente la colisión de líneas en el archivo README.md mediante una redacción integrada que conserva las intenciones del Líder Técnico y del Equipo 1.

## Evidencias
- Historial completamente lineal y limpio verificable mediante `git log --oneline`.
- Árbol de commits optimizado para facilitar la auditoría de código.