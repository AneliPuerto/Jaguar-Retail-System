# Proceso de desarrollo

## 1. Descripción

El proyecto Jaguar Retail System se desarrolla mediante un proceso iterativo que contempla el análisis de requisitos, planificación, diseño, desarrollo, validación y documentación.


---

## 2. Etapas del proceso

### 2.1 Análisis de requisitos
 
**Septiembre**
En esta etapa se identifican y documentan los requisitos funcionales (RF) y no funcionales (RNF) a partir de la información proporcionada por el cliente.

**Octubre**
Durante la segunda etapa se refina y se confirma el alcance del proyecto. El cual se concentra exclusivamente en el punto de venta físico (POS), el control de inventario inter-campus y la gestión de cuentas por cobrar (Interuady). Toda la funcionalidad relativa a E-commerce y pasarela web (antigua EPIC-03) y recolección BOPIS fue declarada fuera de alcance.

### 2.2 Priorización

Los requisitos identificados se priorizan mediante una Matriz de Priorización
Ponderada considerando los criterios de:

- Importancia: 60 %
- Factibilidad: 40 %

El método de priorización se encuentra documentado en `requisitos.md`.

### 2.3 Planeación

Los requisitos priorizados se organizan en el Product Backlog y se dividen en épicas y actividades de trabajo. Estructurados bajo las siguientes épicas y ámbitos:
* **EPIC-01: Gestión de Inventario y Catálogo:** Registro de productos por CDU, código de barras único, control de existencias por bodega y traspaso de mercancía.
* **EPIC-02: Punto de Venta Físico (POS):** Búsqueda predictiva/lectura USB (Keyboard Wedge), descuento automático de stock, pagos (efectivo, tarjeta, Interuady), formulario obligatorio Interuady y ticket contable térmico con desglose de IVA.
* **EPIC-04: Cuentas por Cobrar (Interuady):** Envío automático de transacciones institucionales y módulo de consulta/liquidación restringido al Administrador Central.
* **TRANSVERSAL: Rendimiento y Arquitectura:** Tiempo de respuesta ≤ 1.5s en base de datos POS y disponibilidad del 99% en infraestructura UADY/Nube.

### 2.4 Análisis y diseño

Se elaboran y refinan los modelos y diagramas necesarios para representar la estructura y comportamiento del sistema:

* **Diagrama de Casos de Uso:** Representación de las interacciones de los 4 roles principales (Administrador Central, Ventas CDU, Ventas Sociales y Ventas Matemáticas).
* **Diagrama de Clases UML:** Modelado técnico de entidades comerciales, relaciones, atributos y métodos del sistema.
* **Arquitectura del Sistema:** Definición de la estructura cliente-servidor y base de datos centralizada.

### 2.5 Desarrollo

Las funcionalidades se implementan a partir de los requisitos y diseños
previamente definidos.

El código fuente se mantiene dentro del repositorio del proyecto.

### 2.6 Validación

Las funcionalidades y requisitos se revisan para verificar que correspondan con las necesidades definidas por el cliente.

Los cambios, decisiones e incidencias se registran en la bitácora.

### 2.7 Seguimiento

El avance del proyecto se monitorea mediante métricas relacionadas con las actividades realizadas, requisitos, contribuciones individuales y avance del
desarrollo.

---

## 3. Organización del Equipo y Roles

Para garantizar una gestión ágil y ordenada a lo largo del ciclo de vida del **Jaguar Retail System**, los integrantes asumen las siguientes responsabilidades generales dentro del proyecto:

## 3. Organización del Equipo y Roles

| Integrante | Rol en el Proyecto | Responsabilidades Principales |
| :--- | :--- | :--- |
| **Axel** | Product Owner | Definición de la visión del producto, alcance, propuesta de valor, especificación de Requisitos Funcionales (RF) y No Funcionales (RNF), priorización y argumentación de competencias. |
| **Mariana** | Scrum Master | Definición y seguimiento del marco de trabajo ágil, registro de bitácoras de reuniones, control de la Métrica de Contribución Individual y organización del repositorio en GitHub. |
| **Hiram** | Diseñador (UX/UI y Presentación) | Modelado del Diagrama de Casos de Uso del sistema, especificación de actores, flujos de interacción, criterios de aceptación, diseño de la estructura visual e interfaz (UI) para exposiciones y material de apoyo. |
| **Rodrigo** | Diseñador (UML) | Modelado del Diagrama de Clases UML, especificación de entidades del dominio y creación de assets gráficos para la documentación técnica. |
| **Salomón** | Diseñador (Arquitectura) | Co-diseño del Diagrama de Clases UML, definición de atributos, métodos, relaciones de entidades y apoyo en la estructuración de la arquitectura técnica. |

---

## 4. Adaptación de la Metodología Ágil (Scrum)

El proyecto adopta un enfoque Ágil basado en Scrum, adaptado al entorno académico:

1. **Sprint Planning:** Al inicio de cada fase se revisa la rúbrica y se desglosan las tareas asignándolas a los responsables.
2. **Reuniones de Sincronización:** Sesiones breves sincrónicas/asincrónicas facilitadas por la Scrum Master para resolver bloqueos.
3. **Sprint Review:** Revisión integral de la documentación en Markdown (`.md`), diagramas y presentación previo a cada entrega.

### Herramientas de Monitoreo
El equipo utiliza **GitHub Projects** estructurado en las siguientes columnas: *Por hacer (To Do)*, *En proceso (In Progress)*, *En revisión (In Review)* y *Completado (Done)*.

> **Enlace al tablero de seguimiento:** [https://github.com/users/AneliPuerto/projects/2/views/1]
---

## 5. Control del Proyecto y Documentación

El repositorio de GitHub se utiliza para:

- Control de versiones.
- Gestión del código fuente.
- Documentación.
- Registro de cambios.
- Organización de actividades.
- Seguimiento del trabajo del equipo.

La información relacionada con el seguimiento del proceso se encuentra
distribuida en:

- `requisitos.md` — requisitos y priorización.
- `bitacora.md` — actividades, decisiones e incidencias.
- `metricas.md` — métricas y contribuciones.
- `diagramas/` — modelos y diagramas del sistema.




