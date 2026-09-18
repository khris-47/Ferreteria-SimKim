# 📦 Sistema de Gestión de Inventarios — Ferretería Simkin

> **Proyecto universitario** — Curso de **Métodos Cuantitativos para la Toma de Decisiones**  
> **Universidad de Costa Rica (UCR)**

Sistema de escritorio para la gestión y evaluación de inventarios de la **Ferretería Simkin**, que aplica **modelos cuantitativos clásicos de inventarios** (EOQ, punto de reorden, inventario de seguridad y clasificación ABC) para apoyar la toma de decisiones basada en datos.

---

## 🧠 ¿De qué trata el proyecto?

El proyecto modela el problema real de una ferretería que necesita decidir **cuánto pedir** y **cuándo pedir** cada producto. Para ello se aplican modelos matemáticos de teoría de inventarios, los cuales se ejecutan con los datos del negocio almacenados en una base de datos:

| Modelo / Técnica | Fórmula utilizada | Propósito |
|---|---|---|
| **EOQ** (Cantidad Económica de Pedido) | `Q* = √(2·D·S / H)` | Determina el tamaño óptimo de pedido que minimiza el costo total anual |
| **Punto de Reorden** (PRO / ROP) | `R = d·L + SS` | Indica el nivel de stock en el que se debe lanzar una nueva orden |
| **Inventario de Seguridad** | `SS = Z·σd·√L` | Cubre la variabilidad de la demanda durante el tiempo de entrega (Z = 1.65 ≈ 95%) |
| **Clasificación ABC** | Por porcentaje acumulado de ventas | Prioriza productos: A (≤80%), B (≤95%), C (resto) |
| **Costos de inventario** | `CT = (D/Q)·S + (Q/2)·H` | Costo anual de ordenar + costo anual de conservación |

Todo esto se presenta en una **aplicación de escritorio** con interfaz gráfica moderna, paneles de indicadores (KPIs), gráficos y exportación de reportes.

---

## ✨ Funcionalidades

La aplicación cuenta con **4 módulos** accesibles desde una barra de navegación lateral:

### 1. 📦 Gestionar Inventarios
- **CRUD completo** de productos (agregar, editar, eliminar).
- Formulario con validación de datos (enteros, decimales).
- Campos de producto: nombre, categoría, stock actual, costo unitario (en colones `₡`) y proveedor.
- Campos de parámetros: demanda anual, costo de pedido (S), costo de mantenimiento (H) y tiempo de entrega (L).
- Códigos de producto autogenerados (`FER001`, `FER002`, …).
- Búsqueda en tiempo real por código, nombre, categoría o proveedor.

### 2. 📊 Evaluar Inventarios
- Cálculo automático de **ventas anuales**, porcentaje y porcentaje acumulado por producto.
- Cálculo de **EOQ**, **punto de reorden**, costo anual de ordenar, costo anual de conservación y **costo total**.
- **Clasificación ABC** automática según el porcentaje acumulado de ventas.
- KPI globales: costo anual de ordenar, costo anual de conservación, costo total y ventas anuales totales.
- Guarda los resultados en la tabla `Resultados_Modelos` para trazabilidad histórica (con fecha de cálculo).

### 3. 🔗 Inventario de Seguridad
- Cálculo del **inventario de seguridad** usando `Z = 1.65` (nivel de servicio ~95%).
- Comparación del stock actual contra el inventario de seguridad.
- Estado por producto: ✅ **Seguro** u ❌ **Riesgo** (stock insuficiente).

### 4. 📈 Dashboard / Reportes
- **KPIs**: producto estrella (mayores ventas), mayor EOQ, promedio de tiempo de entrega y productos en riesgo.
- **Gráficos** (matplotlib): Top 10 de ventas anuales (barras), clasificación ABC (dona), seguridad de inventario (dona Riesgo/Seguro).
- Selector de **fecha de cálculo** para ver resultados históricos de los modelos.
- **Exportar a Excel** (.xlsx) el reporte de la fecha seleccionada.

---

## 🛠️ Tecnologías y herramientas

### Lenguaje y frontend
| Tecnología | Uso |
|---|---|
| **Python 3.11+** | Lenguaje principal del proyecto |
| **Tkinter + ttk** | Interfaz base y tablas (`Treeview`) |
| **CustomTkinter** | Widgets modernos (botones, tarjetas, barras, formularios) en la interfaz |

### Datos y persistencia
| Tecnología | Uso |
|---|---|
| **MySQL** | Motor de base de datos relacional |
| **mysql-connector-python** | Conector oficial de MySQL para Python |
| **SQL** | Creación de tablas e inserción de datos semilla |
| **python-dotenv** | Carga de credenciales desde archivo `.env` |

### Cálculo, gráficos y exportación
| Tecnología | Uso |
|---|---|
| **`decimal` (estándar)** | Cálculos de alta precisión (compatibles con campos `DECIMAL(10,2)` de MySQL) |
| **Matplotlib** | Gráficos del dashboard (barras y donas) |
| **pandas** | Manipulación de datos y generación de reportes |
| **openpyxl** | Exportación de reportes a Excel |
| **reportlab** | Generación de documentos PDF (dependencia del proyecto) |

### Estructura del proyecto
```
Proyecto Inventarios/
├── inventario_app.py          # Punto de entrada: ventana principal + navegación
├── gestion_inventarios.py     # Módulo: CRUD de productos y parámetros
├── evaluar_inventarios.py     # Módulo: EOQ, punto de reorden, ABC, costos
├── inventario_seguridad.py    # Módulo: inventario de seguridad y riesgo de stock
├── reportes.py                # Módulo: dashboard, KPIs, gráficos y exportación
├── db_config.py               # Conexión a MySQL usando variables de entorno
├── requirements.txt           # Dependencias del proyecto
├── .env                       # Credenciales de la base de datos (no versionar)
├── Datos/                     # Dump de la BD, script de tablas e inserts
└── Misc/                      # Recursos (logo de la ferretería)
```

---

## 🗄️ Base de datos

Base de datos: `ferreteria_simkin`

| Tabla | Descripción |
|---|---|
| `Productos` | Catálogo de productos: código, nombre, categoría, stock, costo unitario, proveedor |
| `Parametros` | Parámetros de los modelos: demanda anual, costo de pedido (S), costo de mantenimiento (H), tiempo de entrega (L), variabilidad de demanda (σd) |
| `Movimientos` | Historial de entradas y salidas de inventario |
| `Resultados_Modelos` | Resultados de los modelos EOQ/PRO/ABC por fecha de cálculo (trazabilidad histórica) |

En la carpeta `Datos/` se encuentran:
- `Tablas.sql` — creación de la base de datos y tablas.
- `inserts.sql` — datos semilla (productos de la ferretería con parámetros).
- `Base de datos.sql` / `Datos.sql` / `bakup.sql` — variantes para importación.
- `datos proyecto.ods` — hoja de cálculo con los datos del proyecto.

---

## 🚀 Instalación y ejecución

### Requisitos previos
- **Python 3.11+**
- **MySQL** instalado y accesible desde el equipo

### 1. Configurar la base de datos
Dirígete a la carpeta `Datos/`, donde encontrarás el dump de la base de datos y el Excel.
En MySQL Workbench usa `Server` → `Data Import` → **Import from Self-Contained File** y sube el archivo de la base de datos.

### 2. Activar el entorno virtual
```powershell
.venv\Scripts\Activate.ps1
```

### 3. Instalar dependencias
```powershell
pip install -r requirements.txt
```

### 4. Crear el archivo `.env`
Crea un archivo `.env` en la raíz del proyecto:
```
DB_HOST=localhost
DB_PORT=3306
DB_USER=tu_user
DB_PASSWORD=tu_password
DB_NAME=ferreteria_simkin
```

### 5. Ejecutar la aplicación
```powershell
python inventario_app.py
```

---

## 🧮 Marco teórico (resumen)

1. **EOQ — Cantidad Económica de Pedido.** Modelo de pedido de cantidad fija que minimiza la suma del costo de ordenar y el costo de conservación:
   - `Q* = √(2DS / H)` donde `D` = demanda anual, `S` = costo de pedido, `H` = costo de mantenimiento por unidad/año.

2. **Punto de Reorden.** Nivel de inventario que dispara una nueva orden para que el stock no se agote antes de que llegue el pedido:
   - `R = d̄·L + SS`, con `d̄ = D / 365`.

3. **Inventario de Seguridad.** Respaldo para absorber la variabilidad de la demanda durante el tiempo de entrega:
   - `SS = Z · σd · √L`, con `Z = 1.65` (~95% de nivel de servicio).

4. **Clasificación ABC.** Prioriza artículos según su impacto en las ventas (regla 80/20):
   - **A**: acumulan hasta el 80% de las ventas (control estricto).
   - **B**: hasta el 95% (control moderado).
   - **C**: resto (control simple).

---

## 📚 Documentación adicional

- `COMO USAR.md` — guía rápida de instalación y configuración.

---

## ⚠️ Notas
- Los costos se muestran en **colones costarricenses** (₡).
- No compartir el archivo `.env`, ya que contiene credenciales de la base de datos.
- Este proyecto es con fines **académicos**.