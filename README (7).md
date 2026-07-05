# 📊 Dashboard Comercial Inmobiliario — Grupo Andes

Dashboard ejecutivo en **Power BI** para analizar el desempeño comercial de una empresa inmobiliaria ficticia, cubriendo ventas, clientes y propiedades a través de un modelo de datos en esquema estrella.

Proyecto desarrollado como parte del bootcamp de **Data Science de TripleTen**.

---

## 🎯 Objetivo del proyecto

El sector inmobiliario necesita evaluar su desempeño comercial para comprender crecimiento, rentabilidad y comportamiento de clientes. Este dashboard responde preguntas de negocio en cuatro frentes:

- **Desempeño general**: ingreso total, ventas, precio promedio, comisión total.
- **Análisis comercial**: qué tipo de propiedad, segmento de cliente y canal de venta generan más ingresos.
- **Análisis temporal**: evolución de ventas, crecimiento YoY, acumulado YTD.
- **Cohortes de clientes**: si los clientes vuelven a comprar después de su primera compra.

---

## 📁 Estructura del repositorio

```
├── README.md
├── LICENSE
├── data/
│   ├── hecho_ventas_propiedades.csv     # Tabla de hechos: transacciones de venta
│   ├── dim_clientes.csv                  # Dimensión: clientes y segmentación
│   └── dim_propiedades.csv               # Dimensión: características de propiedades
├── dashboard/
│   ├── hecho_venta_propiedad_dashboard.pbix   # Archivo de Power BI (modelo + reporte)
│   └── hecho_venta_propiedad_dashboard.pdf    # Exportación en PDF del dashboard
└── notebook/
    └── Revision_Estudiante_Proyecto_InmobiliarioGrupoAndes.ipynb   # Documentación del proceso
```

---

## 🗂️ Datasets

| Tabla | Descripción | Filas |
|---|---|---|
| `hecho_ventas_propiedades` | Tabla de hechos: transacciones de venta (precio, cliente, propiedad, canal, fecha) | 8,500 |
| `dim_clientes` | Segmentación de clientes (segmento comprador, país, ciudad) | 3,500 |
| `dim_propiedades` | Características de propiedades (tipo, tamaño, ubicación, precio publicado) | 8,000 |
| `dim_fecha` | Tabla calendario creada con DAX (`CALENDAR` + `ADDCOLUMNS`) dentro del `.pbix` | — |

---

## 🛠️ Proceso

### 1. Limpieza de datos
Validación de tipos de datos (`fecha_venta` → Date, `porcentaje_comision` → porcentaje), revisión de nulos y duplicados en claves primarias. Resultado: sin nulos ni duplicados en ninguna de las 3 tablas.

### 2. Tabla calendario (`dim_fecha`)
Construida dinámicamente desde la fecha mínima hasta la máxima de `hecho_ventas_propiedades`, con columnas de Año, Mes, Número de mes y Año-Mes. Marcada como tabla de fechas para habilitar funciones de inteligencia de tiempo.

### 3. Modelado en esquema estrella
`hecho_ventas_propiedades` como tabla central, con relaciones **uno a muchos (1:*)**, dirección de filtro única y activas hacia las 3 dimensiones.

### 4. Medidas DAX
- **Medidas base**: Ingreso Total, Cantidad de Ventas, Ticket Promedio, Comisión Total.
- **Medidas de contexto de filtro**: participación % de ingresos por tipo de propiedad, canal de venta y segmento de cliente (usando `CALCULATE` + `ALL`).
- **Inteligencia de tiempo**: Ingresos YTD, MTD, año anterior y % crecimiento YoY (`TOTALYTD`, `TOTALMTD`, `SAMEPERIODLASTYEAR`).
- **Columnas calculadas de cohortes**: Primera Compra por Cliente, Mes Cohorte, Mes Venta (base para la matriz de recurrencia).

### 5. Estructura del reporte (3 páginas)
1. **Overview Ejecutivo** — KPIs principales, tendencia de ventas, ingresos por ciudad, crecimiento YoY con formato condicional.
2. **Análisis Comercial** — ingreso por tipo de propiedad, canal de venta y segmento de cliente, tabla con formato condicional tipo semáforo.
3. **Análisis de Cohortes** — matriz de cohortes (mes de adquisición vs. mes de venta) para medir recurrencia de clientes.

---

## 📈 Hallazgos clave

- **Ingreso total (2023–2024)**: $6,012,502,170, generado por 8,500 ventas.
- El tipo de propiedad con mayor ingreso es **Casa** ($2,240,535,304; 2,324 ventas).
- **Ciudad de México** genera ligeramente más ingresos que Bogotá.
- El canal **Corredor** concentra el 72.85% del ingreso total.
- El segmento **Primera vez** genera la mayor parte del ingreso (~$3.8 mil M).
- Crecimiento de **11.14% YoY** en 2024 vs. 2023.
- Las cohortes de inicios de 2023 (enero-marzo) muestran las tasas de recompra más altas (>2.2x su compra inicial).

## 💡 Recomendaciones estratégicas

- Priorizar la comercialización de propiedades tipo Casa.
- Fortalecer el canal Corredor, dominante en ingresos.
- Diseñar campañas de retención dirigidas a clientes de Primera vez.
- Investigar los factores detrás de la alta recompra en las cohortes iniciales de 2023 para replicarlos.

---

## 🧰 Herramientas utilizadas

- **Power BI Desktop** (Power Query, modelado de datos, DAX)
- **Python / Jupyter Notebook** (exploración y validación de datos previa)

---

## 📄 Licencia

Este proyecto está bajo licencia MIT — ver el archivo [LICENSE](LICENSE) para más detalles.

---

*Proyecto realizado en el marco del bootcamp de Data Science de [TripleTen](https://tripleten.com).*
