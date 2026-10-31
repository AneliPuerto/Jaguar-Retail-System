# Producto

## 1. Identificación del producto

**Nombre:** Jaguar Retail System

**Tipo:** Sistema de gestión de inventario y ventas (Punto de Venta Físico Multi-sucursal).

## 2. Propuesta de Valor

**"Un Sistema Integral POS diseñado específicamente para la estructura interna de la UADY, que centraliza el control de inventarios de múltiples campus y automatiza el flujo de cobranza institucional."**

**Diferenciador principal frente a software comercial:**
La integración nativa del método de pago universitario. A diferencia de un POS genérico, este sistema procesa transacciones con Interuady, despliega un formulario obligatorio para capturar la dependencia y responsable del pago, y envía automáticamente estas operaciones a un módulo central de Cuentas por Cobrar exclusivo para la administración.

**Necesidad que resuelve:**
Elimina discrepancias de mercancía mediante un control de existencias separado por sucursal y un módulo de traspasos centralizados desde el CDU. Esto garantiza inventarios exactos al actualizar de forma simultánea el stock global y local, agilizando además la operación física con lectura predictiva de códigos de barras y emisión de recibos con desglose fiscal en impresora térmica.

## 3. Objetivo del producto

Desarrollar un sistema de escritorio/web interno que permita centralizar y gestionar de forma estricta los procesos de inventario, traspasos y ventas en mostrador, proporcionando información actualizada y fidedigna sobre productos, existencias y deudas institucionales.

## 4. Usuarios del sistema

El sistema contempla una jerarquía de acceso estricta basada en 4 roles definitivos:

- Administrador Central (Único con permisos para liquidar Cuentas por Cobrar)
- Ventas CDU (Gestión de catálogo global, almacén principal y ventas locales)
- Ventas Sociales (Operaciones exclusivas de cobro e inventario local)
- Ventas Matemáticas (Operaciones exclusivas de cobro e inventario local)

## 5. Alcance del producto

Tras el refinamiento operativo con el cliente, el producto se centra exclusivamente en los siguientes ámbitos:

- Gestión de inventario, catálogo local y traspasos de mercancía.
- Punto de venta físico (POS).
- Cuentas por cobrar (Gestión de deuda institucional Interuady).
- Requisitos transversales de rendimiento, disponibilidad, seguridad y arquitectura.

## 6. Funcionalidades principales

Entre las funcionalidades fundamentales identificadas se encuentran:

- Registro y gestión centralizada de productos desde el CDU.
- Control de existencias separado por sucursal.
- Traspaso de mercancía del almacén principal a sucursales.
- Generación de códigos de barras únicos.
- Procesamiento de ventas en mostrador (Efectivo y Tarjeta).
- Procesamiento de cobro institucional (Interuady) con captura de datos de la dependencia.
- Integración con lectores de códigos de barras USB genéricos.
- Emisión de comprobantes térmicos con desglose de Subtotal e IVA.
- Gestión y liquidación centralizada de cuentas por cobrar (Interuady).

## 7. Fuera de alcance

A partir de la confirmación directa de la administración de la boutique, las siguientes áreas quedan **totalmente excluidas** del alcance del proyecto:

- E-commerce y catálogos públicos en línea para estudiantes.
- Integración con pasarelas de pago virtuales bancarias.
- Carrito de compras virtual.
- Flujo de recolección en tienda (BOPIS).

## 8. Estado actual del producto

El proyecto se encuentra en la fase de diseño arquitectónico y modelado orientado a objetos (Segunda Entrega). El Product Backlog ha sido completamente depurado y validado basándose en las reglas de negocio reales proporcionadas por el cliente. Actualmente se trabaja en la abstracción del Diagrama de Clases (UML) y la documentación técnica definitiva.
