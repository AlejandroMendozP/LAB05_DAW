# 🛒 Backend API E-Commerce - Supabase & Postman

Este proyecto consiste en el diseño, estructuración e implementación del backend para una plataforma de comercio electrónico utilizando **Supabase** como Backend-as-a-Service (BaaS) impulsado por **PostgreSQL**. La API RESTful expuesta permite gestionar de manera relacional el flujo completo de una tienda virtual: administración de usuarios, segmentación por categorías, control de inventario, gestión del carrito de compras en tiempo real e historial de pedidos.

El entorno está diseñado para realizar pruebas de rendimiento, consultas relacionales avanzadas (Joins anidados) y filtros complejos mediante **Postman**.

---

## 🛠️ Arquitectura y Tecnologías
* **Motor de Base de Datos:** PostgreSQL (Alojado en Supabase)
* **Capa de API:** PostgREST (Generación automática de endpoints RESTful)
* **Herramienta de Testing:** Postman
* **Formato de Datos:** JSON

---

## 🗄️ Modelo del Diagrama Entidad-Relación (DER)

El sistema cuenta con una arquitectura relacional sólida con restricciones de integridad y claves foráneas (`Foreign Keys`).

### Mapeo de Entidades

| Entidad | Clave Primaria (PK) | Claves Foráneas (FK) | Atributos adicionales |
| :--- | :--- | :--- | :--- |
| **`usuarios`** | `id` | *Ninguna* | `nombre`, `correo`, `created_at` |
| **`categorias`** | `id` | *Ninguna* | `nombre`, `descripcion` |
| **`productos`** | `id` | `usuario_id`<br>`categoria_id` | `nombre`, `descripcion`, `precio`, `stock` |
| **`carrito`** | `id` | `usuario_id`<br>`producto_id` | `cantidad` |
| **`pedidos`** | `id` | `comprador_id` | `total`, `estado`, `created_at` |
| **`detalles_pedido`**| `id` | `pedido_id`<br>`producto_id` | `cantidad`, `precio_unitario` |

### Reglas de Cardinalidad
1. **Usuarios y Productos (1:N):** Un usuario puede registrar múltiples productos.
2. **Categorías y Productos (1:N):** Una categoría agrupa múltiples artículos.
3. **Usuarios y Pedidos (1:N):** Un cliente puede generar múltiples órdenes de compra.
4. **Relaciones Muchos a Muchos (N:M):** Las tablas **`carrito`** y **`detalles_pedido`** actúan como entidades puente.

---

## 🚀 Guía de Despliegue (Datos de Prueba)

Ejecuta el siguiente script en el **SQL Editor** de Supabase para poblar la base de datos:

```sql
INSERT INTO usuarios (nombre, correo) VALUES 
  ('Lucia Fernandez', 'lucia.f@gmail.com'),
  ('Mateo Quispe', 'mateo.q.unsa@gmail.com');

INSERT INTO categorias (nombre, descripcion) VALUES 
  ('Periféricos de Alta Gama', 'Teclados magnéticos y hardware especializado.'),
  ('Componentes de PC', 'Procesadores y tarjetas gráficas.');

INSERT INTO productos (nombre, descripcion, precio, stock, categoria_id, usuario_id) VALUES 
  ('Teclado Magnético Pro', 'Ideal para ritmo y gaming competitivo.', 650.00, 15, (SELECT id FROM categorias WHERE nombre = 'Periféricos de Alta Gama' LIMIT 1), (SELECT id FROM usuarios ORDER BY random() LIMIT 1)),
  ('Procesador Ryzen 7 7700X', '8 núcleos y 16 hilos.', 1200.00, 8, (SELECT id FROM categorias WHERE nombre = 'Componentes de PC' LIMIT 1), (SELECT id FROM usuarios ORDER BY random() LIMIT 1));
URL del video: https://youtu.be/qwTrNnZkZH8?feature=shared
