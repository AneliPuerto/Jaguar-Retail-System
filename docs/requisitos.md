## 1. Lista Requisitos Funcionales y No Funcionales

| ID | Épica / ámbito | Descripción del PBI / requisito | Imp. | Fact. | Puntaje | Prioridad | Estado / validación |
|---|---|---|---:|---:|---:|---|---|
| RF-1.1 | EPIC-01 | Autenticación y autorización basada en 4 roles: Administrador Central (CDU), Ventas CDU, Ventas Sociales y Ventas Matemáticas. | 5 | 5 | 5.0 | Alta | Validado con el documento de requisitos |
| RF-1.2 | EPIC-01 | Registro de productos por CDU con código, nombre, tipo, color, talla, costo neto, precio de venta, proveedor y almacén. | 5 | 5 | 5.0 | Alta | Validado con el proceso de inventario del cliente |
| RF-1.3 | EPIC-01 | Generación y asignación automática de un código de barras único después de crear un producto. | 5 | 5 | 5.0 | Alta | Validado con el proceso de inventario del cliente |
| RF-2.4 | EPIC-02 | Formulario obligatorio para Interuady con Departamento/Dependencia, CP responsable del Pago y Solicitante/Autoriza. | 5 | 5 | 5.0 | Alta | Validado con el proceso de ventas |
| RNF-5 | EPIC-02 | El POS web debe interpretar Keyboard Wedge generado por lectores de códigos de barras USB genéricos. | 5 | 5 | 5.0 | Alta | Validado con la necesidad del lector en POS |
| RF-1.4 | EPIC-01 | Control de existencias separado por bodega, con actualización del stock global y local. | 5 | 4 | 4.6 | Alta | Validado con el proceso de inventario y venta |
| RF-2.1 | EPIC-02 | Búsqueda predictiva o lectura de código de barras para precargar imagen, código, nombre y precio del producto. | 5 | 4 | 4.6 | Alta | Validado; el cliente menciona explícitamente el lector |
| RF-2.2 | EPIC-02 | Descuento automático del stock del almacén/boutique donde se realiza la venta. | 5 | 4 | 4.6 | Alta | Validado con el proceso de ventas |
| RF-2.3 | EPIC-02 | Procesamiento de pagos en efectivo, tarjeta e Interuady. | 5 | 4 | 4.6 | Alta | Validado con el proceso de ventas |
| RF-2.5 | EPIC-02 | Recibo para impresora térmica de 58mm u 80mm con subtotal, IVA y proceso de facturación. | 5 | 4 | 4.6 | Alta | Pendiente de validación contable |
| RF-4.2 | EPIC-04 | Envío de las transacciones pagadas con Interuady al módulo central de Cuentas por Cobrar. | 5 | 4 | 4.6 | Alta | Validado con el proceso de ventas del cliente |
| RF-4.3 | EPIC-04 | Consulta general de cuentas por cobrar y permiso restringido para marcar como liquidado. | 5 | 4 | 4.6 | Alta | Validado parcialmente; falta confirmar permisos definitivos |
| RNF-1 | TRANSVERSAL | Las actualizaciones de inventario en web y POS deben reflejarse en la base de datos en máximo 1.5 segundos. | 5 | 4 | 4.6 | Alta | Requisito cuantificable definido en el documento |
| RNF-3 | EPIC-03 | No almacenar PAN, CVV ni fecha de expiración; el pago debe delegarse al proveedor mediante API/token. | 5 | 3 | 4.2 | Alta | Requisito de seguridad; depende de proveedor de pago |
| RNF-4 | TRANSVERSAL | La base de datos centralizada debe operar en nube o infraestructura UADY con uptime mínimo de 99%. | 5 | 3 | 4.2 | Alta | Requisito crítico de infraestructura; arquitectura por definir |
| RF-3.1 | EPIC-03 | Catálogo público web con disponibilidad en tiempo real y filtrado de productos sin existencia global. | 2 | 5 | 3.2 | Media | Requiere validación de alcance |
| RF-3.2 | EPIC-03 | Carrito de compras virtual para usuarios clientes. | 2 | 4 | 2.8 | Baja | Requiere validación de alcance |
| RF-4.1 | EPIC-04 | Validación de recolección mediante folio web y cambio del pedido a estado Entregado. | 2 | 4 | 2.8 | Baja | Depende de confirmar BOPIS como alcance |
| RNF-2 | EPIC-03 | El catálogo web debe ser 100% responsivo en smartphones y tablets, sin deformación de imágenes ni botones inaccesibles. | 2 | 4 | 2.8 | Baja | Ligado al alcance de E-commerce; validar alcance |
| RF-3.3 | EPIC-03 | Integración de una pasarela de pago virtual para tarjetas desde la web. | 2 | 3 | 2.4 | Baja | Requiere validación de alcance y proveedor |
| RF-3.4 | EPIC-03 | Comprobante digital con folio/QR e indicación de la sucursal de recolección. | 2 | 3 | 2.4 | Baja | Requiere validación de alcance |

### Épicas y ámbitos

Las épicas agrupan los requisitos funcionales y no funcionales de acuerdo con el área
del sistema a la que pertenecen. Los requisitos identificados como **TRANSVERSAL**
corresponden a aspectos que afectan al sistema de manera general y no se limitan a
una única épica.

| Código | Épica / ámbito | Descripción |
|---|---|---|
| EPIC-01 | Gestión de Inventario y Catálogo | Agrupa los requisitos relacionados con la gestión de productos, existencias, códigos de barras y catálogo interno. |
| EPIC-02 | Punto de Venta Físico (POS) | Agrupa los requisitos relacionados con las operaciones de venta presencial, pagos, lectores de códigos y comprobantes. |
| EPIC-03 | E-commerce y Pasarela Virtual | Agrupa los requisitos relacionados con el catálogo público, carrito de compras y pagos realizados desde la plataforma web. |
| EPIC-04 | BOPIS y Cuentas por Cobrar | Agrupa los requisitos relacionados con la recolección de pedidos y la gestión de cuentas por cobrar. |
| TRANSVERSAL | Rendimiento, Disponibilidad y Arquitectura | Agrupa los requisitos que aplican de manera general al sistema, como rendimiento, disponibilidad e infraestructura. |

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

Los RNF también reciben importancia y factibilidad para permitir una lectura integral del backlog. Su prioridad es provisional y no implica que deban convertirse en funcionalidades independientes.