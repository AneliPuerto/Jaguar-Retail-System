## 1. Lista Requisitos Funcionales y No Funcionales

| ID | Épica / ámbito | Descripción del PBI / requisito | Imp. | Fact. | Puntaje | Prioridad | Estado / validación |
|---|---|---|---:|---:|---:|---|---|
| RF-1.1 | EPIC-01 | Autenticación y autorización basada en 4 roles: Administrador Central, Administrador/Ventas CDU, Ventas Sociales y Ventas Matemáticas. | 5 | 5 | 5.0 | Alta | Validado. Jerarquía de permisos confirmada. |
| RF-1.2 | EPIC-01 | Registro de productos por CDU con código, nombre, tipo, color, talla, costo neto, precio de venta, proveedor y almacén. | 5 | 5 | 5.0 | Alta | Validado con el proceso de inventario del cliente. |
| RF-1.3 | EPIC-01 | Generación y asignación automática de un código de barras único después de crear un producto. | 5 | 5 | 5.0 | Alta | Validado con el proceso de inventario del cliente. |
| RF-1.4 | EPIC-01 | Control de existencias separado por bodega, con actualización del stock global y local. Incluye traspaso de mercancía desde CDU. | 5 | 4 | 4.6 | Alta | Validado. CDU opera como almacén central unificado. |
| RF-2.1 | EPIC-02 | Búsqueda predictiva o lectura de código de barras para precargar imagen, código, nombre y precio del producto. | 5 | 4 | 4.6 | Alta | Validado; el cliente requiere el uso de lector. |
| RF-2.2 | EPIC-02 | Descuento automático del stock del almacén/boutique donde se realiza la venta. | 5 | 4 | 4.6 | Alta | Validado con el proceso de ventas. |
| RF-2.3 | EPIC-02 | Procesamiento de pagos en mostrador: efectivo, tarjeta e Interuady. | 5 | 4 | 4.6 | Alta | Validado. (Pagos web descartados del alcance). |
| RF-2.4 | EPIC-02 | Formulario obligatorio para Interuady con Departamento/Dependencia, CP responsable del Pago y Solicitante/Autoriza. | 5 | 5 | 5.0 | Alta | Validado con el proceso de ventas. |
| RF-2.5 | EPIC-02 | Recibo para impresora térmica de 58mm u 80mm con cálculo y desglose matemático de subtotal e IVA. | 5 | 4 | 4.6 | Alta | Validado. Se confirma exigencia de desglose contable. |
| RF-4.2 | EPIC-04 | Envío automático de las transacciones pagadas con Interuady al módulo central de Cuentas por Cobrar. | 5 | 4 | 4.6 | Alta | Validado con el proceso de ventas. |
| RF-4.3 | EPIC-04 | Consulta general de cuentas por cobrar. Permiso exclusivo y restringido a "Administrador Central" para marcar deuda como liquidada. | 5 | 5 | 5.0 | Alta | Validado. CDU no tendrá autorización para liquidar. |
| RNF-1 | TRANSVERSAL | Las actualizaciones de inventario en POS deben reflejarse en la base de datos en máximo 1.5 segundos. | 5 | 4 | 4.6 | Alta | Requisito de rendimiento confirmado. |
| RNF-4 | TRANSVERSAL | La base de datos centralizada debe operar en nube o infraestructura UADY con uptime mínimo de 99%. | 5 | 3 | 4.2 | Alta | Requisito crítico de infraestructura. |
| RNF-5 | EPIC-02 | El POS web debe interpretar Keyboard Wedge generado por lectores de códigos de barras USB genéricos. | 5 | 5 | 5.0 | Alta | Validado con la necesidad de hardware. |

### Épicas y ámbitos

Las épicas agrupan los requisitos funcionales y no funcionales de acuerdo con el área
del sistema a la que pertenecen. Toda la gestión E-commerce (antigua EPIC-03) ha sido declarada fuera de alcance por el cliente para esta fase.

| Código | Épica / ámbito | Descripción |
|---|---|---|
| EPIC-01 | Gestión de Inventario y Catálogo | Agrupa los requisitos relacionados con la gestión de productos, existencias inter-campus y códigos de barras. |
| EPIC-02 | Punto de Venta Físico (POS) | Agrupa los requisitos relacionados con las operaciones de venta presencial, pagos físicos, lectores de códigos y tickets contables. |
| EPIC-04 | Cuentas por Cobrar (Interuady) | Agrupa los módulos de consulta y liquidación de adeudos institucionales con permisos restringidos. |
| TRANSVERSAL | Rendimiento y Arquitectura | Agrupa los requisitos técnicos generales que garantizan el funcionamiento y la disponibilidad del sistema. |

## 2. Método de priorización

Se utiliza una **Matriz de Priorización Ponderada** con dos criterios:

| Criterio | Escala | Peso | Interpretación |
|---|---|---:|---|
| Importancia | 1–5 | 60% | Necesidad y valor del requisito para el proceso del cliente. |
| Factibilidad | 1–5 | 40% | Viabilidad de implementación con el alcance y conocimiento actuales. |

**Puntaje:**  
`Puntaje = (Importancia × 0.60) + (Factibilidad × 0.40)`

| Rango | Prioridad |
|---|---|
| 4.0–5.0 | Alta |
| 3.0–3.9 | Media |
| 1.0–2.9 | Baja |

*Nota: Tras la reunión de levantamiento de requisitos con el cliente, todos los elementos de prioridad Media y Baja (asociados a E-commerce y BOPIS) han sido retirados del Product Backlog definitivo.* 
