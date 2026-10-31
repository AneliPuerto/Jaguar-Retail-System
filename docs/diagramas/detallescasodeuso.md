# Documento de Especificación de Historias de Usuario y Casos de Uso

## 1. Control de Cambios y Evidencias de Actualización

Se presentan los cambios significativos realizados en los artefactos del sistema con respecto a la entrega previa, derivados del refinamiento de requerimientos con el cliente y los diagramas de casos de uso actualizados.

### 1.1 Matriz de Cambios Respecto a la Entrega Pasada

| Artefacto / Ámbito | Estado Previo | Estado Actual (Entrega Presente) | Justificación / Evidencia | 
| ----- | ----- | ----- | ----- | 
| **Alcance E-commerce (EPIC-03)** | Incluido (Ventas web, carrito, entrega a domicilio, BOPIS). | **Fuera de Alcance**. Eliminado del backlog definitivo. | Decisión estratégica del cliente para priorizar el control de inventarios y el POS presencial. | 
| **Integración POS - Interuady** | Pago genérico institucional. | **Formulario obligatorio** con campos: Departamento/Dependencia, CP responsable y Solicitante/Autoriza. Integración con Cuentas por Cobrar. | RF-2.4 y RF-4.2 validados con la normativa contable de la institución. | 
| **Impresión de Tickets** | Comprobante simple. | Ticket en impresora térmica (58mm/80mm) con desglose explícito de Subtotal e IVA. | Exigencia del departamento contable (RF-2.5). | 
| **Jerarquía de Permisos** | Roles genéricos de usuario. | 4 roles definidos: Administrador Central, Administrador/Ventas CDU, Ventas Sociales, Ventas Matemáticas. | Restricción de liquidación de cuentas por cobrar exclusivamente a "Administrador Central" (RF-4.3). | 
| **Lector de Código de Barras** | Opcional / Teclado manual. | Soporte obligatorio *Keyboard Wedge* mediante lector USB genérico (RF-2.1, RNF-5). | Optimización del tiempo de atención en caja. | 

### 1.2 Evidencias de Cambios Significativos en Artefactos

1. **Rediseño del Diagrama de Casos de Uso:**

   * **Inclusión de Relaciones `<<include>>` y `<<extend>>`:** Se agregaron dependencias explícitas desde el flujo de venta POS hacia el módulo de *Keyboard Wedge* (`<<include>>`) y la captura de datos Interuady (`<<extend>>`).

   * **Reestructuración de Actores y Roles:** Modificación de los límites del sistema para reflejar la jerarquía de 4 roles (Administrador Central, Administrador/Ventas CDU, Ventas Sociales, Ventas Matemáticas), delimitando el acceso exclusivo del Administrador Central al caso de uso *Liquidar Deuda Interuady*.

   * **Depuración de Casos de Uso E-commerce:** Se removieron los nodos correspondientes a *Carrito Web*, *Pasarela de Pago Online* y *Gestión de Envíos*.

2. **Actualización de la Documentación de Historias de Usuario:**

   * **Campos Específicos para Interuady:** Adición explícita de los criterios de aceptación CA-3.1 y CA-3.2 para asegurar la validación de Dependencia, C.P. Responsable y Solicitante.

   * **Tratamiento de Excepciones Técnicas:** Incorporación de flujos alternativos para fallos de hardware (lectores USB e impresoras térmicas de 58mm/80mm) y desincronización de stock en tiempo real ($\le 1.5$s).

## 2. Especificación Detallada de Historias de Usuario y Casos de Uso

### HU-01 / CU-01: Gestión de Catálogo de Productos y Existencias (EPIC-01)

* **Requisitos Vinculados:** RF-1.1, RF-1.2, RF-1.3, RF-1.4.

* **Actor Principal:** Administrador Central / Administrador CDU.

* **Descripción:** Permite registrar un nuevo producto en el catálogo centralizado de la CDU, generando automáticamente un código de barras único y asignando stock inicial por almacén/boutique, así como gestionar traspasos entre bodegas.

#### Flujo Principal (Camino Feliz)

1. El Administrador inicia sesión y accede al módulo de **Gestión de Inventario**.

2. Selecciona la opción **Registrar Nuevo Producto**.

3. Ingresa los datos requeridos: Nombre, Tipo, Color, Talla, Costo Neto, Precio de Venta, Proveedor y Almacén inicial.

4. El sistema valida la integridad de los datos y calcula automáticamente el código de barras único.

5. El sistema guarda el producto y actualiza el stock local del almacén seleccionado y el stock global de la institución.

6. El sistema muestra la confirmación de registro y genera el código de barras en pantalla listo para impresión/asignación.

#### Criterios de Aceptación

* **CA-1.1 (Campos Obligatorios):** Todos los campos (Nombre, Tipo, Color, Talla, Costo Neto, Precio Venta, Proveedor, Almacén) deben ser obligatorios. Si falta alguno, el sistema no permite guardar.

* **CA-1.2 (Generación de Código):** El código de barras generado debe ser único en la base de datos y cumplir con el estándar asignado.

* **CA-1.3 (Sincronización de Stock):** El incremento de existencias debe verse reflejado en el inventario del almacén destino y en el stock consolidado en menos de 1.5 segundos (RNF-1).

* **CA-1.4 (Traspasos entre Bodegas):** Un Administrador CDU puede solicitar/registrar traspaso de mercancía a otra boutique; el stock de origen disminuye y el de destino aumenta únicamente tras la confirmación de recepción.

#### Excepciones y Flujos Alternativos

* **EX-1.1 (Código Duplicado o Conflicto):** Si por alguna anomalía el código generado colisiona con uno existente, el sistema aborta la transacción, genera un nuevo código mediante un algoritmo de resguardo y notifica al usuario.

* **EX-1.2 (Precio de Venta Menor al Costo):** Si el usuario ingresa un `Precio de Venta` menor o igual al `Costo Neto`, el sistema muestra una advertencia bloqueante: *"El precio de venta debe ser mayor al costo neto del producto."*

* **EX-1.3 (Sin Conexión con BD Central):** Si la base de datos no responde durante el guardado, el sistema cancela la operación y despliega el mensaje *"Error de conexión con el servidor central. Intente nuevamente."*

### HU-02 / CU-02: Procesamiento de Ventas en Punto de Venta POS (EPIC-02)

* **Requisitos Vinculados:** RF-2.1, RF-2.2, RF-2.3, RF-2.5, RNF-1, RNF-5.

* **Actor Principal:** Vendedor (Ventas CDU, Ventas Sociales, Ventas Matemáticas).

* **Descripción:** Registrar la venta de productos en mostrador mediante lectura de código de barras o búsqueda predictiva, procesar el pago en efectivo o tarjeta y generar un ticket térmico con desglose contable de IVA.

#### Flujo Principal (Camino Feliz)

1. El Vendedor escanea el código de barras del producto con el lector USB (*Keyboard Wedge*) o escribe el término en la búsqueda predictiva.

2. El sistema precarga inmediatamente la imagen, código, nombre, precio unitario y valida la disponibilidad en el stock local.

3. El Vendedor agrega los ítems deseados al carrito de venta.

4. El Vendedor selecciona la forma de pago: **Efectivo** o **Tarjeta de Débito/Crédito**.

5. El sistema calcula el Subtotal, IVA (16%) y Total.

6. Se procesa la transacción, se descuenta automáticamente el stock de la boutique actual y se envía la orden de impresión del ticket a la impresora térmica (58mm u 80mm).

#### Criterios de Aceptación

* **CA-2.1 (Lectura Barcode):** El sistema debe capturar el evento *Keyboard Wedge* del lector USB genérico en el campo activo del POS sin requerir clics adicionales (RNF-5).

* **CA-2.2 (Descuento Instantáneo):** El stock de la boutique donde se efectúa la venta debe actualizarse de forma inmediata ($\le 1.5$ segundos).

* **CA-2.3 (Desglose Térmico):** El ticket emitido debe incluir obligatoriamente: Nombre de boutique, Fecha/Hora, Lista de artículos con IVA desglosado, Subtotal, IVA total, Monto Total, Método de pago y Folio de venta (RF-2.5).

#### Excepciones y Flujos Alternativos

* **EX-2.1 (Stock Insuficiente Local):** Si el producto escaneado no cuenta con stock en la boutique actual, el sistema muestra una alerta: *"Sin stock disponible en esta sucursal. Existencias en otros campus: \[Lista de Campus\]"*. No se permite agregar la prenda al carrito de venta actual.

* **EX-2.2 (Fallo de Impresora Térmica):** Si la impresora no está conectada o se queda sin papel, la venta se registra en base de datos de manera exitosa y el sistema ofrece la opción de *"Reimprimir comprobante"* o *"Descargar comprobante PDF"*.

* **EX-2.3 (Monto Recibido En Efectivo Insuficiente):** Si el pago es en efectivo y el monto ingresado es menor al Total de la venta, el sistema desactiva el botón de finalizar compra e indica la diferencia pendiente.

### HU-03 / CU-03: Procesamiento de Ventas Institucionales vía Interuady (EPIC-02)

* **Requisitos Vinculados:** RF-2.3, RF-2.4, RF-4.2.

* **Actor Principal:** Vendedor (Ventas CDU, Ventas Sociales, Ventas Matemáticas).

* **Descripción:** Registrar una venta institucional cuyo pago se realiza mediante el esquema "Interuady", capturando los datos administrativos requeridos para su posterior cobro y conciliación.

#### Flujo Principal (Camino Feliz)

1. El Vendedor ingresa los productos a la venta en la interfaz del POS.

2. Al momento de cobrar, selecciona como método de pago **Interuady**.

3. El sistema despliega el **Formulario Obligatorio Interuady**.

4. El Vendedor captura los datos:

   * Departamento / Dependencia solicitante.

   * C.P. (Contador Público / Enlace) responsable del Pago.

   * Nombre del Solicitante / Persona que autoriza.

5. El sistema valida los datos y registra la transacción como "Pendiente de Cobro Institutional".

6. El sistema envía automáticamente la transacción al módulo central de **Cuentas por Cobrar** (RF-4.2), descuenta el stock e imprime el recibo térmico con la leyenda "PAGO INTERUADY - PENDIENTE DE LIQUIDACIÓN".

#### Criterios de Aceptación

* **CA-3.1 (Formulario Obligatorio):** Ninguna venta Interuady puede finalizarse sin que los tres campos del formulario (Dependencia, C.P. Responsable, Solicitante) contengan información válida.

* **CA-3.2 (Integración Cuentas por Cobrar):** La transacción debe registrarse en tiempo real en la tabla central de Cuentas por Cobrar con estatus `Por Cobrar` y los datos del folio asociado.

#### Excepciones y Flujos Alternativos

* **EX-3.1 (Datos Incompletos en Formulario Interuady):** Si alguno de los campos obligatorios del formulario se deja en blanco, el sistema resalta el campo en rojo y bloquea el botón "Completar Venta Interuady".

* **EX-3.2 (Dependencia No Registrada/Inválida):** Si el usuario ingresa una dependencia o centro de costo que no coincide con el catálogo oficial, el sistema muestra una sugerencia predictiva para corregir la nomenclatura.

### HU-04 / CU-04: Consulta y Liquidación de Cuentas por Cobrar Interuady (EPIC-04)

* **Requisitos Vinculados:** RF-1.1, RF-4.3.

* **Actor Principal:** Administrador Central.

* **Descripción:** Módulo administrativo para consultar el estado de las deudas por concepto de ventas Interuady y registrar la liquidación de las mismas.

#### Flujo Principal (Camino Feliz)

1. El Administrador Central inicia sesión y navega al módulo **Cuentas por Cobrar**.

2. Filtra los registros por fecha, dependencia solicitante o estatus (`Pendiente` / `Liquidada`).

3. Selecciona una o más cuentas con estatus `Pendiente`.

4. Tras verificar el pago/transferencia institucional, hace clic en el botón **Marcar como Liquidada**.

5. El sistema solicita confirmación de la operación.

6. Tras confirmar, el sistema actualiza el estatus a `Liquidada`, registra la fecha de liquidación y el ID del Administrador que realizó la operación.

#### Criterios de Aceptación

* **CA-4.1 (Restricción de Acceso Strict):** Únicamente los usuarios autenticados con el rol **Administrador Central** tienen visibilidad del botón y la función para marcar deudas como liquidadas (RF-4.3).

* **CA-4.2 (Auditoría de Liquidación):** Cada cambio de estado de deudas debe registrar de forma inmutable el usuario, fecha, hora y folio de la operación.

#### Excepciones y Flujos Alternativos

* **EX-4.1 (Acceso No Autorizado):** Si un usuario con rol *Administrador/Ventas CDU*, *Ventas Sociales* o *Ventas Matemáticas* intenta acceder mediante URL directa al panel de liquidación, el sistema deniega el acceso, muestra un error `403 Prohibido` y registra el intento de acceso en la bitácora de seguridad.

* **EX-4.2 (Intento de Re-liquidación):** Si se intenta modificar el estado de una cuenta que ya figura como `Liquidada`, el sistema inhabilita la acción y despliega el mensaje: *"Esta cuenta ya ha sido liquidada anteriormente."*