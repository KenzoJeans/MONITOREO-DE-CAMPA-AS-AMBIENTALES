# Dashboard de Campañas Ambientales – Kenzo Jeans

Dashboard para el seguimiento de las campañas ambientales de recolección de Kenzo Jeans:

| Campaña | Qué se recolecta |
|---|---|
| ♻️ **Botellas con Amor** | Botellas plásticas (kg) |
| 🔵 **Tapas para Sanar** | Tapas plásticas (kg), a beneficio de la Fundación Sanar |
| 🛢️ **Aceite Green Fuel** | Aceite usado (kg) |

## Propósito

Centralizar en un solo lugar los kilos recolectados por cada participante, tienda y área, para:

- Conocer el total recolectado por campaña y su evolución en el tiempo.
- Reconocer a los operadores, tiendas y áreas administrativas más participativos (rankings y competencia interna).
- Comunicar el impacto de las campañas (equivalencias ambientales y sociales, reporte en PDF e infografía).

## Cómo fluye la información

```
Formulario de Google  →  Hoja de cálculo (Google Sheets)  →  Dashboard (Streamlit)
```

1. Cada participación se registra en el formulario de la campaña.
2. Las respuestas llegan a la hoja de cálculo oficial.
3. El dashboard lee la hoja como CSV cada vez que se carga o se pulsa **"Refrescar datos ahora"**.

La hoja oficial está definida en la constante `URL_OFICIAL` del código. Debe tener acceso de lectura por enlace o estar publicada en la web, de lo contrario el dashboard no puede leerla.

### Columnas que espera la hoja

El dashboard identifica las columnas por **palabras clave en el encabezado**, no por posición. Si se renombran preguntas en el formulario, hay que verificar que sigan conteniendo estas palabras:

| Dato | Palabra(s) clave en el encabezado |
|---|---|
| Marca temporal de Google Forms | `marca temporal` o `timestamp` |
| Fecha (si existe como columna manual) | `fecha` |
| Grupo de participación | `grupo` y `participac` |
| Área administrativa | `área`, `area` o `administrativo` |
| Nombre de la persona | `nombre`, o `persona` + `operac` |
| Tienda | `tienda` y `pertenece` |
| Botellas (kg) | `botellas` |
| Tapas (kg) | `tapas` |
| Aceite (kg) | `aceite` |

Los grupos de participación reconocidos son **Operación**, **Administrativo** y **Tienda**.

### Limpieza automática de datos

- **Pesos (kg):** se toleran comas decimales (`2,4`), unidades pegadas (`2.4kg`), espacios, puntos sobrantes (`2.4.`) y el texto "No participa". Lo que no se pueda interpretar se cuenta como `0` para no romper el dashboard.
- **Fechas:** la marca temporal de Google Forms se interpreta como mes/día/año; la columna `fecha` manual, como día/mes/año. Si más del 30 % de las fechas fallan, se prueba la orientación contraria.

## Qué muestra el dashboard

- **Resumen general:** participantes y kg por campaña.
- **Participación por grupos** y distribución de kg por campaña.
- **Evolución temporal** (solo si hay más de una fecha).
- **Rankings** (Top N configurable de 5 a 20): operadores, tiendas, administrativos y aceite en todos los grupos.
- **Competencia interna entre grupos administrativos.**
- **Reporte PDF** descargable con KPIs y rankings.
- **Infografía animada** (HTML) descargable, también como imagen PNG.
- **Mapa de calor** de área administrativa vs. campaña.
- **Tabla de datos** descargable en CSV.

Barra lateral: selección de fuente de datos (hoja oficial, otra URL de Google Sheets o archivo CSV/Excel subido), botón de refresco y filtro por rango de fechas. El PDF, la infografía y la tabla respetan el filtro activo.

## Reglas de negocio que viven en el código

Estas reglas se editan directamente en el archivo principal. Conviene revisarlas si cambia la estructura de la empresa o de las campañas.

**Grupos administrativos de la competencia interna** (`ADMIN_GRUPOS`)

| Grupo | Áreas |
|---|---|
| Grupo 1 | SGA, SST, Sistemas, Inventarios, Comercial |
| Grupo 2 | Finanzas |
| Grupo 3 | RRHH |
| Grupo 4 | Diseño |
| Grupo 5 | Mercadeo |
| Grupo 6 | Tintorería |

- Un área que **no esté en esta lista no cuenta** en la competencia interna.
- La competencia suma **solo Botellas + Tapas**; el aceite no entra.

**Equivalencias de impacto** (estimaciones referenciales, ajustables)

| Constante | Valor | Uso |
|---|---|---|
| `PESO_PROMEDIO_BOTELLA_G` | 25 g | Número de botellas salvadas |
| `PESO_PROMEDIO_TAPA_G` | 2 g | Número de tapas recicladas |
| `CO2_EVITADO_KG_POR_KG_PET` | 1,5 kg | CO₂ evitado por kg de PET |
| `LITROS_AGUA_PROTEGIDOS_POR_KG_ACEITE` | 1.100 L | Agua protegida por kg de aceite |
| `KG_TAPAS_POR_TRATAMIENTO` | 7 kg | Tratamientos financiados (Fundación Sanar; rango de referencia 6–8 kg) |

Si el equipo ambiental cuenta con cifras oficiales propias, deben reemplazar estos valores.

## Tecnologías y dependencias

Python con Streamlit, Pandas, Plotly, NumPy, ReportLab (PDF) y openpyxl (lectura de Excel). La infografía usa `html2canvas` desde CDN para exportar la imagen.

> Completar: confirmar que `requirements.txt` incluya `streamlit`, `pandas`, `plotly`, `numpy`, `reportlab` y `openpyxl`.

## Ejecución y despliegue

> Completar:

- **Archivo principal:** `[nombre del archivo, p. ej. app.py]`
- **Instalar dependencias:** `pip install -r requirements.txt`
- **Ejecutar en local:** `streamlit run [archivo].py`
- **Despliegue:** Streamlit Cloud, cuenta `[completar]`, URL `[completar]`
- **Formulario(s) de la campaña:** `[completar enlaces de respuesta]`

## Responsables

- **Dueño del proceso (campañas):** `[área / cargo]`
- **Mantenimiento técnico del dashboard:** `[nombre / cargo]`
- **Cuentas (GitHub, Streamlit, Google):** `[a nombre de quién]`
