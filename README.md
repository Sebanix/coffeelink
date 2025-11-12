# ☕ CoffeeLink: Plataforma de Café Artesanal

¡Bienvenido a CoffeeLink! Esta es una aplicación de comercio electrónico especializada en café premium. El sistema está compuesto por tres servicios (API-Core, BFF y Frontend) que deben correr simultáneamente.

---

## 🚀 1. Requisitos de Instalación

Para ejecutar la aplicación localmente, asegúrate de tener instalado:

| Requisito | Propósito | Versión Requerida (Mínima) |
| :--- | :--- | :--- |
| **Java Development Kit (JDK)** | Ejecutar los servicios de backend (API-Core y BFF). | JDK 17 o superior |
| **Node.js y npm** | Ejecutar la interfaz web (Frontend). | Node.js (Recomendado v14 o superior) |
| **PostgreSQL** | Base de datos para almacenar usuarios, productos y pedidos. | PostgreSQL (v10+) |

---

## 🛠️ 2. Guía de Configuración y Arranque

Debes iniciar tres componentes en el siguiente orden.

### Paso A: Base de Datos (PostgreSQL)

1.  **Crea la Base de Datos:** Crea una base de datos llamada **`coffeelink_db`**.
2.  **Activa la Extensión (Búsqueda de Tildes):** Ejecuta el siguiente comando SQL para que la búsqueda en el catálogo funcione correctamente:
    ```sql
    CREATE EXTENSION IF NOT EXISTS unaccent;
    ```
3.  **Carga Inicial de Datos:** Ejecuta el script `coffeelink-api-core/database-scripts/V1__init.sql` para crear las tablas e insertar los usuarios de prueba.

### Paso B: Iniciar Servicios de Backend

Abre **dos terminales** separadas para iniciar los servicios Java (API-Core y BFF).

| Servicio | Ubicación de la Carpeta | Comando de Inicio | Puerto |
| :--- | :--- | :--- | :--- |
| **API-Core** (Cerebro) | `coffeelink-api-core` | `./mvnw spring-boot:run` | 8080 |
| **BFF** (Seguridad) | `coffeelink-bff` | `./mvnw spring-boot:run` | 8081 |

### Paso C: Iniciar el Frontend (Interfaz Web)

Abre una **tercera terminal**, ve a la carpeta `coffeelink-frontend` y ejecuta:

1.  **Instalar dependencias:** `npm install`
2.  **Iniciar la App:** `npm start`

La aplicación se abrirá en tu navegador en **`http://localhost:3000`**.

---

## 👤 3. Cuentas de Prueba y Funcionalidad

El sistema utiliza dos roles principales, cada uno con permisos diferentes:

| Rol | Email | Contraseña | Permisos |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin@coffeelink.com` | `123456` | Gestión de productos (CRUD) y acceso a `/admin`. |
| **Cliente** | `cliente@coffeelink.com` | `123456` | Navegación, carrito de compras y realizar pedidos. |

### Flujo A: Cliente (Comprar)

1.  **Registro e Ingreso:** Crea tu cuenta o usa la de prueba (`cliente@coffeelink.com`).
2.  **Catálogo:** La búsqueda es **insensible a tildes y mayúsculas/minúsculas**.
3.  **Carrito de Compras:**
    * **Añadir:** El botón **"Agregar al Carrito"** añade el producto a tu sesión.
    * **Resumen y Total:** El total de artículos y el precio acumulado se muestran en el Navbar.
    * **Gestionar (en `/carrito`):** Puedes usar los botones `+` / `-` para cambiar la cantidad (respetando el stock) y el botón de la papelera para eliminar productos.
    * **Seguridad:** El carrito se vacía automáticamente al cerrar la sesión.

### Flujo B: Administrador (Gestionar Inventario)

1.  **Ingresa:** Usa la cuenta de prueba (`admin@coffeelink.com`).
2.  **Acceso Admin:** El enlace **"Admin"** aparecerá en el menú.
3.  **Gestión (CRUD):** Usa el formulario para crear/editar productos, o el botón **"Eliminar"** de la tabla.