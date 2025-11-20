# Reporte_Ventas_Anuales
Reporte y análisis de ventas anuales por sector, con dashboard interactivo

📊 Reporte de Ventas Anuales

Dataset anual generado a partir de los reportes del ERP, procesado en Google Sheets y utilizado como fuente para dashboards en Looker Studio.
Incluye la limpieza de datos, normalización de fechas, cálculo de kilos por producto mediante tablas propias y armado del dataset final anual.

🧱 1. Fuente del Dataset

Cada mes se descarga desde el ERP (Neuralsoft / Presupuesto – Ventas) un archivo XLSX con más de 35.000 registros anuales.

Columnas originales relevantes:

FECHA

FORMULARIO

NNUMERO

CODIGOCOMP

P_ANUAL

MES

CPTE

CLIEN

ZONA_C

CATEGORIA

VEND

U_NEG

COD_ART

ARTICULO

RUBRO_A

SUBRUBRO_A

T_VENTA

CANT_VTA

KG_VTA

N_VENTA

IVA_ART

T_COSTO

UT_BRUTA

PORC_UT_B

🧹 2. Limpieza y Selección de Campos en Google Sheets

El archivo XLSX se abre en Google Sheets, donde se realiza:

Normalización de fechas (FECHA y MES)

Eliminación de espacios en blanco

Armonización de códigos y categorías

Selección de solo las columnas necesarias para el análisis anual

🔍 Selección de columnas con QUERY

Se seleccionan las columnas:

FECHA

MES

COD_ART

T_VENTA

CANT_VTA

KG_VTA

Ejemplo de la fórmula QUERY utilizada:

=QUERY(Data!A:Z;
"SELECT A, F, M, Q, R, S
 WHERE A IS NOT NULL"; 
1)


(Se ajusta según la posición de cada columna del archivo mensual.)

⚖️ 3. Cálculo Real de KG por Producto (con BUSCARX)

Muchos productos no vienen con el peso cargado o vienen con valores incompletos.
Por eso se utiliza una tabla propia de kilos por producto.

Tablas reales utilizadas:

KG ROTO → productos rotomoldeados

KG SOPLADO → productos soplados (tanques)

Ambas contienen:

COD_ART

KG_REAL

🧮 Fórmula BUSCARX integrada:

Para calcular el peso correcto por producto se utiliza un esquema de búsqueda doble:

=SI.ERROR(
    BUSCARX([@COD_ART]; 'KG ROTO'!A:B; 'KG ROTO'!B:B);
    BUSCARX([@COD_ART]; 'KG SOPLADO'!A:B; 'KG SOPLADO'!B:B)
)


Esto genera la columna:

KG_CALCULADO → usado para métricas reales de producción y ventas.

📈 4. Métricas Derivadas

A partir del dataset limpio se generan:

✔ Ventas por unidad (CANT_VTA)
✔ Ventas por kilogramo (KG_CALCULADO)
✔ Ventas totales por mes
✔ Tendencia anual por producto
✔ Comparación de kilos vs unidades por categoría
✔ Distribución por rubro y subrubro

Este dataset sirve como insumo para todos los dashboards operativos.

📊 5. Dashboards vinculados (Looker Studio)

El dataset anual se utiliza para alimentar los dashboards:

-Ventas Anuales por Categoría

-Ventas por Zona / Vendedor

-Unidades vs Kilos: Comparativo Anual

-Dashboard General de Performance Comercial

Enlace: https://lookerstudio.google.com/u/0/reporting/59214710-4f63-4bd2-9316-9a64847b39c9/page/bv6dF

🗂 6. Estructura sugerida del repositorio

```
Reporte_Ventas_Anuales/
│
├── data/                             # Datos crudos y procesados
│   ├── ventas_raw_2025.xlsx          # Exportación mensual del ERP (archivo original)
│   ├── ventas_clean_2025.csv         # Dataset limpio generado en Google Sheets
│   │
│   └── tablas_pesos/                 # Tablas reales de pesos por producto
│       ├── kg_roto.xlsx              # Pesos de productos rotomoldeados
│       └── kg_soplado.xlsx           # Pesos de productos soplados (tanques)
│
├── sheets/                           # Documentación técnica del procesamiento
│   └── formulas.md                   # QUERY, BUSCARX y pasos de limpieza
│
├── dashboards/                       # Accesos y documentación visual
│   └── links.txt                     # Enlaces a Looker Studio vinculados a este dataset
│
└── README.md                         # Documentación principal del proyecto
```


🧑‍💻 7. Tecnologías utilizadas

-ERP Neuralsoft / Presupuesto (fuente)

-Google Sheets

-QUERY

-BUSCARX

-Normalización de datos

-Looker Studio

-GitHub (control de versiones y portafolio profesional)

🎯 8. Objetivo del proyecto

*Centralizar y documentar el proceso real de generación del dataset anual de ventas, asegurando:

-Limpieza consistente

-Trazabilidad

-Preparación para dashboards

-Estandarización del cálculo de KG por producto


