# 03. Diseño y matriz de trazabilidad de requisitos

## 1. Propósito

Este documento establece la correspondencia entre los requisitos vigentes en el proyecto **Jaguar Retail System** y los elementos que los representan en el diseño orientado a objetos y en la arquitectura del sistema. Su objetivo es permitir una revisión verificable: para cada requisito se identifica la clase, relación o componente arquitectónico responsable, la evidencia disponible en el diseño y las brechas que deben resolverse.

## 2. Artefactos de referencia

- [Requisitos funcionales y no funcionales](./docs/requisitos.md).
- [Diseño de arquitectura y estructura de clases](./docs/diseno.md).
- [Especificación del diagrama de clases UML](./docs/diagramas/diagrama-clases.md).
- [Bitácora y acuerdos del equipo](./docs/bitacora.md).

Se consideran únicamente los requisitos vigentes de las épicas EPIC-01, EPIC-02, EPIC-04 y los requisitos transversales. E-commerce, pagos web y BOPIS se encuentran fuera del alcance actual.

## 3. Elementos de diseño identificados

### 3.1. Clases del modelo

| Área | Clases existentes en el diseño | Responsabilidad identificada |
|---|---|---|
| Usuarios y permisos | `Usuario`, `CDU`, `AdmonCentral`, `UsuarioSucursal` | Datos comunes del usuario y especialización de los perfiles operativos. |
| Catálogo e inventario | `Producto`, `Inventario`, `Almacén` | Datos del producto y control de existencias por bodega. |
| Ventas y pagos | `Venta`, `detalleVenta`, `Pago` | Registro de la venta, sus partidas, importes y forma de pago. |

### 3.2. Componentes arquitectónicos

El documento `diseno.md` también identifica tres componentes del patrón MVC:

- **Modelo:** entidades del dominio, como `Producto`, `Inventario`, `Venta`, `Pago` y la jerarquía de `Usuario`.
- **Vista:** interfaz del POS, recibos térmicos y entrada de datos desde hardware.
- **Controlador:** coordinación de solicitudes de sucursales, procesamiento de pagos Interuady y sincronización de existencias.

Estos componentes arquitectónicos no deben confundirse con clases UML ya especificadas. Cuando la matriz menciona una función que todavía no tiene una clase o método explícito, se registra como brecha y no como una implementación existente.

## 4. Criterios de evaluación de trazabilidad

| Estado | Significado |
|---|---|
| **Modelado** | El diseño identifica explícitamente la clase, atributo, método o relación necesaria para el requisito. Este estado solo acredita cobertura a nivel de diseño. |
| **Parcial** | Hay clases o componentes relacionados, pero faltan atributos, métodos, relaciones, reglas o detalles necesarios para verificar el requisito completo. |
| **No representado** | El diseño consultado no identifica un elemento suficiente para representar el requisito. |

Una relación indirecta no se considera cobertura completa. Por ejemplo, que `Pago` contemple Interuady no demuestra por sí solo que exista un módulo de cuentas por cobrar ni que se transmitan transacciones a ese módulo.

## 5. Matriz de trazabilidad de requisitos funcionales

| ID | Requisito (resumen) | Clases / componentes de diseño relacionados | Evidencia actual en el diseño | Estado | Brecha o ajuste requerido |
|---|---|---|---|---|---|
| **RF-1.1** | Autenticación y autorización con cuatro roles. | `Usuario`, `CDU`, `AdmonCentral`, `UsuarioSucursal`; Controlador. | `Usuario` define los datos comunes; las tres clases derivadas representan perfiles. `AdmonCentral` se identifica como el perfil con privilegios exclusivos. | **Parcial** | No se especifican operaciones de autenticación ni reglas de autorización. `UsuarioSucursal` agrupa los perfiles de Ventas Sociales y Ventas Matemáticas, pero el diseño no muestra cómo los distingue. Definir el rol y la sucursal asignada, así como la validación de permisos para cada operación. |
| **RF-1.2** | Registrar productos con código, nombre, tipo, color, talla, costo neto, precio, proveedor y almacén. | `Producto`, `Inventario`, `Almacén`. | `Producto` contiene código, nombre, tipo, color, talla y precio de venta. `Inventario` incluye costo y `Almacén` representa la bodega. | **Parcial** | El modelo de `Producto` no muestra proveedor ni costo neto como atributos propios, ni especifica las asociaciones necesarias entre producto, proveedor, inventario y almacén. Definir también si el costo pertenece al producto o a cada registro de inventario. |
| **RF-1.3** | Generar y asignar automáticamente un código de barras único al crear un producto. | `Producto`; Controlador. | `Producto.código` se describe como código de barras o identificador único. | **Parcial** | No se especifica el método o servicio que genera el código, ni la regla de unicidad o el momento en que se asigna. Incorporar la operación de generación y la validación de duplicados al flujo de alta de producto. |
| **RF-1.4** | Controlar existencias por bodega, actualizar stock local y global y permitir traspasos desde CDU. | `Producto`, `Inventario`, `Almacén`, `CDU`; Controlador. | `Inventario` contempla consultar, aumentar, reducir y actualizar existencias. El diseño relaciona producto, inventario y almacén. | **Parcial** | No existe una clase o proceso explícito para registrar un traspaso con almacén de origen, destino, productos y cantidades. Tampoco se detalla cómo se concilian las existencias globales y locales. Modelar el traspaso y sus efectos sobre ambos inventarios. |
| **RF-2.1** | Buscar productos o leer su código de barras para precargar imagen, código, nombre y precio. | `Producto`; Vista; Controlador. | `Producto` contiene código, nombre y precio; la arquitectura contempla la entrada de datos mediante hardware. | **Parcial** | El atributo de imagen y la operación de búsqueda no aparecen en el modelo. Tampoco se especifica el componente que recibe el código leído y consulta el catálogo. Definir la búsqueda por código/nombre y el dato de imagen del producto. |
| **RF-2.2** | Descontar automáticamente el inventario de la sucursal donde se realiza la venta. | `Venta`, `detalleVenta`, `Producto`, `Inventario`, `Almacén`, `UsuarioSucursal`; Controlador. | `detalleVenta` registra cantidad y producto; `Inventario` contiene `ReducirExistencia()`. La arquitectura atribuye al Controlador la sincronización del stock. | **Parcial** | No se documenta explícitamente la relación de una venta con el almacén/sucursal que la origina ni el flujo transaccional que descuenta las cantidades al confirmar el pago. Vincular la venta con la sucursal y especificar la actualización correspondiente de inventario. |
| **RF-2.3** | Procesar pagos en mostrador: efectivo, tarjeta e Interuady. | `Pago`, `Venta`; Controlador. | `Pago.formaDePago` contempla efectivo, tarjeta e Interuady, y define `ProcesarPago()` y `ValidarPago()`. `Venta` también contempla el procesamiento del pago. | **Modelado** | Las tres modalidades y las operaciones principales están identificadas. Para mayor precisión, conviene restringir `formaDePago` a un conjunto de valores definido y aclarar la responsabilidad: `Venta` coordina el cobro y `Pago` valida/procesa el medio de pago. |
| **RF-2.4** | Solicitar obligatoriamente datos de Interuady: departamento/dependencia, CP responsable del pago y solicitante/autoriza. | `Pago`; Vista; Controlador. | `Pago` contempla Interuady y su descripción indica que valida datos requeridos por administración. | **Parcial** | No se identifican los campos obligatorios ni una entidad que los agrupe. Especificar los datos del formulario y asociarlos al pago Interuady; definir la validación de obligatoriedad antes de aceptar la transacción. |
| **RF-2.5** | Emitir recibo térmico de 58 mm u 80 mm con subtotal e IVA desglosados. | `Venta`, `detalleVenta`, `Pago`; Vista. | `Venta` contiene subtotal, IVA y total, con métodos de cálculo. `detalleVenta` calcula el subtotal de cada partida. La Vista contempla recibos térmicos. | **Parcial** | No se especifica una clase o servicio de generación/impresión del recibo, la selección del ancho de papel ni el contenido completo del comprobante. Vincular la salida impresa con los datos calculados en `Venta` y definir el formato de 58/80 mm. |
| **RF-4.2** | Enviar automáticamente transacciones Interuady pagadas al módulo central de Cuentas por Cobrar. | `Venta`, `Pago`, `AdmonCentral`; Controlador. | El modelo reconoce los pagos Interuady y la arquitectura menciona su procesamiento por el Controlador. | **Parcial** | No se identifica la entidad `CuentaPorCobrar`, el registro de la deuda ni el componente/interfaz que transmite la transacción al módulo central. Incorporar esos elementos y definir cuándo se genera y envía el registro después de confirmar el pago. |
| **RF-4.3** | Consultar cuentas por cobrar y permitir que solo Administración Central marque una deuda como liquidada. | `AdmonCentral`, `Usuario`; Controlador. | La descripción de `AdmonCentral` establece que es el único perfil autorizado para liquidar cuentas por cobrar. | **Parcial** | No existe una clase `CuentaPorCobrar` ni se especifican operaciones de consulta y liquidación. Incorporar el estado de la cuenta y la operación de liquidación, protegiéndola mediante una validación explícita del rol `AdmonCentral`. |

## 6. Matriz de trazabilidad de requisitos no funcionales

Los requisitos no funcionales se verifican principalmente contra decisiones arquitectónicas, infraestructura y mecanismos de validación. No todos se representan como clases del dominio.

| ID | Requisito (resumen) | Elementos de diseño relacionados | Evidencia actual en el diseño | Estado | Brecha o mecanismo de verificación requerido |
|---|---|---|---|---|---|
| **RNF-1** | Reflejar las actualizaciones del inventario en la base de datos en un máximo de 1.5 segundos. | `Inventario`; Controlador; persistencia/base de datos centralizada. | La arquitectura indica que el Modelo gestiona la persistencia hacia la base de datos de UADY y que el Controlador sincroniza las existencias. | **Parcial** | No se documenta cómo se garantiza ni se mide el tiempo máximo de actualización. Se requiere verificar el tiempo transcurrido entre la actualización del inventario en el POS y su persistencia en la base de datos, comprobando el cumplimiento del límite de 1.5 segundos. |
| **RNF-4** | Operar con base de datos centralizada en nube o infraestructura UADY y disponibilidad mínima del 99 %. | Arquitectura cliente-servidor; base de datos centralizada; Modelo/persistencia. | `diseno.md` establece la persistencia hacia la base de datos de UADY. `proceso.md` contempla una arquitectura cliente-servidor con base de datos centralizada. | **Parcial** | No se documentan las características de la infraestructura de despliegue, los mecanismos de recuperación ante fallos ni el monitoreo de disponibilidad. Se requiere verificar la disponibilidad de la base de datos en la infraestructura correspondiente mediante mediciones que permitan comprobar el cumplimiento del 99 %. |
| **RNF-5** | Interpretar la entrada Keyboard Wedge de lectores USB genéricos en el POS web. | Vista POS; entrada por hardware; Controlador; `Producto`. | `diseno.md` contempla la lectura de datos mediante hardware desde la Vista y `Producto` dispone de un código identificador. | **Parcial** | No se describe el mecanismo de captura de la entrada Keyboard Wedge ni cómo el código recibido se utiliza para consultar el catálogo. Se requiere verificar que el POS web reciba correctamente la entrada de un lector USB genérico y recupere el producto correspondiente. |
## 7. Brechas de diseño que requieren atención

La matriz muestra que el diseño actual identifica las entidades principales del dominio, pero aún no representa de forma explícita varios procesos necesarios para satisfacer todos los requisitos. Las siguientes propuestas son **elementos por modelar**, no clases que ya existan en el diagrama:

| Elemento propuesto | Responsabilidad | Requisitos que ayudaría a cubrir |
|---|---|---|
| `TraspasoInventario` (o `TransferenciaInventario`) | Registrar origen, destino, partidas, cantidades, fecha y confirmación de un movimiento entre almacenes. | RF-1.4 |
| `Proveedor` y asociación con `Producto` | Representar el proveedor asociado al producto. | RF-1.2 |
| `CuentaPorCobrar` | Registrar el adeudo asociado a una transacción Interuady, su estado y su liquidación. | RF-4.2, RF-4.3 |
| `DatosInteruady` (o clase equivalente) | Agrupar y validar departamento/dependencia, CP responsable y solicitante/autoriza. | RF-2.4 |
| `ServicioCodigoBarras` / servicio de catálogo | Generar códigos únicos, buscar productos y resolver la lectura capturada por el POS. | RF-1.3, RF-2.1, RNF-5 |
| `ServicioTicket` (o componente de impresión) | Construir e imprimir el recibo con el ancho y desglose exigidos. | RF-2.5 |
| `ServicioInventario` / `RepositorioInventario` | Coordinar operaciones atómicas y persistencia de existencias, con medición de rendimiento. | RF-2.2, RNF-1 |
| `ServicioIntegracionCuentasPorCobrar` | Enviar transacciones Interuady confirmadas al módulo central y gestionar errores de comunicación. | RF-4.2 |

La solución definitiva debe mantener la coherencia con el diagrama UML aprobado por el equipo. No es obligatorio crear una clase por cada fila: una responsabilidad puede resolverse mediante métodos o servicios existentes si el equipo justifica esa decisión y actualiza el diagrama.

## 8. Mecanismo de verificación y mantenimiento

Para conservar la trazabilidad durante los siguientes incrementos del proyecto, se seguirá este mecanismo:

1. **Identificación:** todo requisito nuevo o modificado conserva su ID en `requisitos.md`.
2. **Asignación:** antes de considerar el requisito diseñado, se registra en esta matriz al menos una clase, relación o componente arquitectónico responsable.
3. **Comprobación:** se revisa que la evidencia citada exista en `diagrama-clases.md` o `diseno.md`. Si faltan atributos, métodos, relaciones o decisiones arquitectónicas, el estado permanece como **Parcial** o **No representado**.
4. **Actualización:** cuando cambie el diagrama o la arquitectura, se actualizan en la misma tarea la matriz y los documentos afectados. No se debe dejar una clase nueva sin explicar qué requisito respalda, ni un requisito sin componente asociado o brecha registrada.
5. **Revisión Scrum:** el responsable del diseño actualiza el artefacto en su rama; el equipo revisa la correspondencia durante la revisión del incremento antes de integrar los cambios en `main`.
6. **Validación posterior:** la cobertura de diseño no equivale a implementación. Cuando exista código, cada requisito deberá vincularse además con las clases implementadas y con pruebas de aceptación; los requisitos no funcionales deberán contar con mediciones específicas.

### Criterios de aceptación de esta matriz

- Todos los requisitos vigentes RF y RNF de `requisitos.md` aparecen identificados por su ID.
- Cada requisito indica las clases o componentes de diseño relacionados.
- La evidencia citada corresponde al diseño actual, no a clases hipotéticas.
- Toda cobertura incompleta tiene una brecha o acción de diseño explícita.
- Los elementos propuestos se distinguen claramente de las clases ya documentadas.
- Las modificaciones posteriores de requisitos, clases o arquitectura se reflejan en esta matriz.

## 9. Conclusión de la revisión

El diagrama actual ofrece una base para la gestión de usuarios, productos, inventario, ventas y pagos. La trazabilidad detecta brechas especialmente en transferencias de inventario, generación y captura de códigos de barras, datos obligatorios de Interuady, emisión de recibos y módulo de cuentas por cobrar. Los requisitos no funcionales también necesitan decisiones arquitectónicas y pruebas medibles.

Por tanto, la matriz permite verificar qué requisitos ya tienen una representación reconocible en el diseño y cuáles requieren refinamiento antes de considerarse completamente cubiertos.
