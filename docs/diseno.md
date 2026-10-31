# Diseño de Arquitectura y Estructura de Clases

## 1. Patrón Arquitectónico (MVC y Capas)

Para el desarrollo del **Jaguar Retail System**, optamos por estructurar el proyecto bajo una adaptación del patrón **Modelo-Vista-Controlador (MVC)**, complementado con una separación clara por capas para mantener orden en la lógica y cumplir con los fundamentos de la **Programación Orientada a Objetos (POO)**.

La arquitectura se divide en tres componentes principales:
* **Modelo:** Contiene las clases del dominio del negocio (`Producto`, `Inventario`, `Venta`, `Pago` y la jerarquía de `Usuario`). Se encarga de manejar el estado y la persistencia hacia la base de datos de la UADY.
* **Vista:** Representada por la interfaz del Punto de Venta (POS) en la web, los recibos impresos en formato térmico y la lectura de datos mediante hardware.
* **Controlador:** Gestiona la interacción entre las solicitudes de las sucursales (CDU, Matemáticas, Sociales), el procesamiento de los pagos institucionales (vales "Interuady") y la sincronización del stock.

---

## 2. Estructura y Responsabilidades de las Clases

El diseño de clases preliminar modela los elementos clave de la boutique de la siguiente manera:

### Jerarquía de Usuarios (Herencia)
* **`Usuario` (Clase base):** Define los atributos generales de identificación y acceso (`id`, `nombre`, `correo`).
* **`CDU`, `AdmonCentral`, `UsuarioSucursal` (Clases derivadas):** Heredan de `Usuario` para delimitar permisos. Por ejemplo, `AdmonCentral` es la única entidad autorizada en el sistema para marcar las cuentas por cobrar como liquidadas, mientras que `CDU` administra el stock central.

### Núcleo de Transacciones
* **`Venta`:** Controla la cabecera del ticket (`id`, `fecha`, `subtotal`, `iva`, `total`) e integra los métodos necesarios para calcular montos y procesar el pago.
* **`detalleVenta` (Composición):** Mantiene una relación directa con `Venta`, registrando la cantidad, precio unitario y subtotal de cada `Producto` agregado.
* **`Pago`:** Maneja las formas de pago disponibles (efectivo, tarjeta o el esquema institucional `Interuady`), validando los datos requeridos por administración.

### Gestión de Inventario
* **`Producto`:** Almacena los datos del artículo (`código`, `nombre`, `tipo`, `color`, `talla`, `precioVenta`).
* **`Inventario`:** Administra las existencias y costos asociados a cada bodega o sucursal, permitiendo actualizar el stock de forma automática al concretar una venta.
* **`Almacén`:** Representa los espacios físicos (CDU y puntos de venta) que centralizan la mercancía.