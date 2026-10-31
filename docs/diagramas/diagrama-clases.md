# Diagrama de Clases del Sistema - Jaguar Retail System

## 1. Introducción y Contexto del Modelo
El presente documento describe de manera exhaustiva el **Diagrama de Clases** correspondiente al sistema **Jaguar Retail System**. Este modelo UML traduce los requerimientos funcionales y de negocio de la Boutique UADY a un diseño orientado a objetos estructurado, asegurando un bajo acoplamiento, alta cohesión y la correcta aplicación de los principios de la Programación Orientada a Objetos (POO).

---

## 2. Especificación Técnica de Clases, Atributos y Métodos

El dominio del sistema se compone de las siguientes clases principales, detallando su visibilidad (`+` público, `-` privado), tipos de datos y responsabilidades operativas:

### 2.1. Jerarquía de Usuarios y Control de Acceso (Herencia)
* **`Usuario` (Superclase / Clase Base Abstracta):**
  * *Atributos:* 
    * `-id: String` (Identificador único del usuario en el sistema).
    * `-nombre: String` (Nombre completo del personal o administrador).
    * `-correo: String` (Correo institucional UADY de contacto y autenticación).
  * *Descripción:* Define la estructura genérica y los atributos comunes de autenticación para cualquier rol dentro del sistema.

* **`CDU` (Clase Derivada):**
  * *Relación:* Hereda directamente de `Usuario`.
  * *Métodos:* 
    * `+ModificarInventario(): void` (Permite actualizar existencias y bodegas).
    * `+ConsultarInventario(): void` (Visualiza el stock centralizado).
  * *Descripción:* Representa al personal del Centro de Desarrollo Universitario que administra el almacén principal.

* **`AdmonCentral` (Clase Derivada):**
  * *Relación:* Hereda directamente de `Usuario`.
  * *Métodos:* 
    * `+ConsultarInventario(): void` (Consulta global de existencias).
  * *Descripción:* Rol con privilegios administrativos superiores, autorizado exclusivamente para la gestión y liquidación de cuentas por cobrar.

* **`UsuarioSucursal` (Clase Derivada):**
  * *Relación:* Hereda directamente de `Usuario`.
  * *Atributos:* 
    * `-idSucursal: String` (Identificador de la tienda o campus asignado, ej. Matemáticas o Sociales).
  * *Descripción:* Personal operativo enfocado en la atención de los puntos de venta físicos locales.

### 2.2. Núcleo Transaccional y de Cobranza
* **`Venta`:**
  * *Atributos:* 
    * `-id: String` (Folio único del ticket).
    * `-fecha: LocalDate` (Fecha exacta de la transacción).
    * `-subtotal: double` (Monto acumulado antes de impuestos).
    * `-iva: double` (Desglose matemático del impuesto correspondiente).
    * `-total: double` (Monto final a pagar por el cliente).
  * *Métodos:* 
    * `+AgregarDetalle(): void`
    * `+EliminarDetalle(): void`
    * `+CalcularSubtotal(): double`
    * `+CalcularIVA(): double`
    * `+CalcularTotal(): double`
    * `+ProcesarPago(): boolean`
    * `+Cobrar(): void`
  * *Descripción:* Clase central que administra la cabecera del ticket de compra y sus cálculos contables y fiscales.

* **`detalleVenta` (Composición Estricta):**
  * *Atributos:* 
    * `-cantidad: int` (Unidades compradas del producto).
    * `-precioUnitario: double` (Costo por unidad al momento de la venta).
    * `-subtotal: double` (Multiplicación de cantidad por precio unitario).
  * *Métodos:* 
    * `+CalcularSubtotal(): double`
  * *Descripción:* Representa cada línea individual dentro de un ticket. Mantiene una relación de composición con `Venta`, lo que significa que no puede existir de forma independiente.

* **`Pago`:**
  * *Atributos:* 
    * `-id: String` (Identificador del comprobante de pago).
    * `-monto: double` (Cantidad monetaria recibida).
    * `-formaDePago: String` (Efectivo, tarjeta o el esquema institucional Interuady).
  * *Métodos:* 
    * `+ProcesarPago(): boolean`
    * `+ValidarPago(): boolean`
  * *Descripción:* Modela la forma en que se liquida la transacción comercial, validando los datos requeridos por la administración.

### 2.3. Control de Inventarios y Almacenes
* **`Producto`:**
  * *Atributos:* 
    * `-código: int` (Código de barras o identificador único del artículo).
    * `-nombre: String` (Denominación comercial del producto).
    * `-tipo: String` (Categoría de la prenda o artículo).
    * `-color: String` (Variante de color).
    * `-talla: String` (Medida o dimensión).
    * `-precioVenta: double` (Precio al público).
  * *Descripción:* Catálogo base de artículos disponibles en la tienda.

* **`Inventario`:**
  * *Atributos:* 
    * `-existencia: int` (Cantidad de unidades disponibles).
    * `-costo: double` (Costo de adquisición para la tienda).
  * *Métodos:* 
    * `+ConsultarExistencia(): int`
    * `+AumentarExistencia(): void`
    * `+ReducirExistencia(): void`
    * `+ActualizarExistencia(): void`
  * *Descripción:* Entidad intermedia que gestiona el stock físico y los costos asociados por cada bodega.

* **`Almacén`:**
  * *Atributos:* 
    * `-id: String` (Identificador de la bodega).
    * `-nombre: String` (Nombre de la sucursal o CDU).
    * `-tipo: String` (Clasificación de almacenamiento).
  * *Descripción:* Representa físicamente los espacios de almacenamiento y distribución de la mercancía en la universidad.

---

## 3. Especificación de Relaciones UML

1. **Herencia (Generalización):** 
   - Se aplica desde la superclase `Usuario` hacia las subclases `CDU`, `AdmonCentral` y `UsuarioSucursal`, permitiendo heredar atributos y centralizar la lógica de autenticación.
2. **Composición (Todo-Parte):** 
   - Se establece entre la clase `Venta` y la clase `detalleVenta`, garantizando que el ciclo de vida de los detalles de la compra dependa estrictamente de la existencia de la venta principal.
3. **Asociación y Multiplicidad:** 
   - Relación bidireccional entre `Venta` y `Pago` para registrar la liquidación de los tickets.
   - Conexión estructurada entre `Producto`, `Inventario` y `Almacén` mediante multiplicidades (como `1` a `*`), modelando con precisión la distribución de existencias por bodega.