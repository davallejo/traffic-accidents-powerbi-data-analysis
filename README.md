# 🚗 Dashboard de Seguimiento de Accidentes de Tránsito — Período ANT 2018–2020
**Power BI** 📈 + **Power Query** 🔄 + **DAX** 🧮 + **Mapas Geoespaciales** 🗺️

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge&logo=microsoft&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Nivel](https://img.shields.io/badge/Nivel-Principiante-00C49A?style=for-the-badge)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)

Panel de control operativo para el análisis de accidentes viales en carreteras federales de la región sudeste de Brasil, desarrollado como solución analítica para apoyar la expansión logística de **MasterLogistics** hacia dicha región.

**Datos crudos → ETL → Modelado → Visualización geoespacial + temporal → Decisión estratégica**

Elaborado por: Diego Vallejo

---

## 🎯 Contexto del Negocio (Business Case)

**MasterLogistics**, especializada en soluciones logísticas, busca expandir su área de operación a la **región sudeste de Brasil**. La investigación inicial del equipo de seguridad determinó que las zonas con mayor concentración de accidentes son las **carreteras federales**, lo que representa un riesgo operativo directo para la flota y el personal.

La alta dirección solicitó al área de Analytics desarrollar un dashboard para profundizar el análisis, con los siguientes requerimientos estratégicos:

- 📊 Cuantificar el **total de eventos** con vistas por hora, día de la semana y estación del año
- 🏥 Recopilar la **cantidad de fallecidos y heridos** por período
- 🗺️ Crear un **mapa con geolocalización** de los eventos
- ⚠️ Evaluar las **Top 7 causas** de accidentes
- 🚨 Cuantificar los accidentes con **más de 3 víctimas** en un solo evento
- 🔍 Permitir **filtros** por mes, año y clasificación del incidente
- 📋 Todo en **una única pestaña** del Dashboard

---

## 🏗️ Arquitectura del Sistema

```
┌────────────────────────────────────────────────────────────────────┐
│                                                                    │
│  📋 FUENTE DE DATOS      🔄 POWER QUERY        📐 MODELO          │
│  ─────────────────       ───────────────        ────────           │
│  Registros de       →    ETL: limpieza,    →   Tabla única        │
│  accidentes viales       normalización         de accidentes      │
│  2018, 2019, 2020        y tipificación        con 19 campos      │
│                                                                    │
│         ↓                      ↓                    ↓             │
│                                                                    │
│  🧮 MEDIDAS DAX          📊 DASHBOARD         🔍 SEGMENTACIÓN     │
│  ──────────────          ─────────────         ───────────────    │
│  Accidentes,             9 visualizaciones     Por año,           │
│  Heridos,                + mapa                mes y              │
│  Muertos, +3 víctimas    geoespacial           clasificación      │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

---

## 🔍 KPIs Principales del Dashboard

```
┌────────────────────────────────────────────────────────────────────┐
│                                                                    │
│  K1 🚑 ACCIDENTES TOTALES    Conteo total de eventos registrados  │
│                               en carreteras federales 2018–2020   │
│                                                                    │
│  K2 🤕 TOTAL DE HERIDOS      Sumatoria de heridos leves y graves  │
│                               por período analizado               │
│                                                                    │
│  K3 ⚰️  TOTAL DE MUERTOS     Fallecidos registrados en el        │
│                               período completo                    │
│                                                                    │
│  K4 🚨 ACCIDENTES +3 VÍCTIMAS  Conteo y porcentaje de eventos    │
│                                 de alta gravedad                  │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

---

## ✨ Características Principales

- **Análisis Temporal Completo:** Distribución de accidentes por hora del día, día de la semana y mes, permitiendo identificar ventanas de mayor riesgo operativo.
- **Geolocalización de Eventos:** Mapa interactivo con coordenadas geodésicas de cada accidente, habilitando decisiones de rutas más seguras para la flota.
- **Top 7 Causas de Accidentes:** Ranking de las principales causas sistémicas para priorizar programas de prevención y capacitación de conductores.
- **Indicador de Alta Gravedad:** Métrica exclusiva que cuantifica los accidentes con más de 3 víctimas en un solo evento, clasificando los incidentes más críticos.
- **Filtros Cruzados:** Segmentadores por año (2018/2019/2020) y clasificación del accidente (con víctimas fatales, heridas o sin víctimas) que actualizan todos los visuales en tiempo real.
- **Vista Consolidada:** Toda la información en una única pestaña del dashboard, cumpliendo el requerimiento de la dirección.

---

## 🗄️ Diccionario de Datos

### Tabla Principal: `Accidentes`

```
┌──────────────────────────────────────────────────────────────────────┐
│                           Accidentes                                 │
├──────────────────────────┬───────────┬───────────────────────────────┤
│ Campo                    │ Tipo      │ Descripción                   │
├──────────────────────────┼───────────┼───────────────────────────────┤
│ id                       │ Entero    │ Identificador único del evento │
│ fecha_hora               │ DateTime  │ Fecha y hora de la ocurrencia  │
│ uf                       │ Texto     │ Estado (Unidad de la Federación│
│ municipio                │ Texto     │ Municipio del accidente        │
│ causa_accidente          │ Texto     │ Causa principal identificada   │
│ tipo_accidente           │ Texto     │ Tipo de colisión o evento      │
│ clasificacion_accidente  │ Texto     │ Gravedad del accidente         │
│ sentido_via              │ Texto     │ Sentido de la vía en el punto  │
│ tipo_pista               │ Texto     │ Tipo de pista (carriles)       │
│ personas                 │ Entero    │ Total de personas involucradas │
│ muertos                  │ Entero    │ Total de fallecidos            │
│ heridos_leves            │ Entero    │ Total con heridas leves        │
│ heridos_graves           │ Entero    │ Total con heridas graves       │
│ ilesos                   │ Entero    │ Total de personas ilesas       │
│ ignorados                │ Entero    │ Estado físico desconocido      │
│ heridos                  │ Entero    │ Suma heridos leves + graves    │
│ vehiculos                │ Entero    │ Total de vehículos involucrados│
│ latitud                  │ Decimal   │ Coordenada geodésica latitud   │
│ longitud                 │ Decimal   │ Coordenada geodésica longitud  │
└──────────────────────────┴───────────┴───────────────────────────────┘
```

### Ejemplo de Registro

```json
{
  "id":                      78432,
  "fecha_hora":              "2019-07-14 16:45:00",
  "uf":                      "MG",
  "municipio":               "Belo Horizonte",
  "causa_accidente":         "Falta de atención a la conducción",
  "tipo_accidente":          "Colisión trasera",
  "clasificacion_accidente": "Con víctimas heridas",
  "tipo_pista":              "Dupla",
  "personas":                4,
  "muertos":                 0,
  "heridos_leves":           2,
  "heridos_graves":          1,
  "heridos":                 3,
  "vehiculos":               2,
  "latitud":                 -19.9167,
  "longitud":                -43.9345
}
```

---

## 📊 Visualizaciones Implementadas

| # | Tipo de Visual | Nombre en Dashboard | Descripción |
|---|---------------|---------------------|-------------|
| 1 | **Card (KPI)** | Accidentes 🚑 | Conteo total de eventos registrados |
| 2 | **Card (KPI)** | Heridos 🤕 | Total de heridos en el período |
| 3 | **Card (KPI)** | Muertos ⚰️ | Total de fallecidos en el período |
| 4 | **Card (KPI)** | Accidentes +3 víctimas | Conteo y porcentaje de alta gravedad |
| 5 | **Tabla** | Accidentes por Estado | Distribución por UF con % del total |
| 6 | **Gráfico de Líneas** | Total de eventos y heridos por mes | Evolución mensual dual (accidentes vs heridos) |
| 7 | **Gráfico de Área** | Total de eventos por hora | Distribución horaria de 0:00 a 23:59 |
| 8 | **Gráfico de Columnas** | Total de eventos por día de la semana | Comparativa lunes a domingo |
| 9 | **Gráfico de Barras** | Top 7 causas de accidentes | Ranking de causas más frecuentes |
| 10 | **Gráfico de Barras** | Total de eventos por tipo de carril | Distribución por tipo de pista |
| 11 | **Mapa Geoespacial** | Mapa de geolocalización | Coordenadas de cada accidente en el territorio |
| 12 | **Slicer** | Filtro por clasificación | Con víctimas fatales / heridas / sin víctimas |
| 13 | **Slicer** | Filtro por año | 2018 / 2019 / 2020 |

---

## ⚡ Flujo ETL con Power Query

```
Datos crudos (registros de accidentes 2018–2020)
       ↓
  Power Query Editor
       ↓
  ┌───────────────────────────────────────────────────────────────┐
  │  Paso 1: Carga de archivos por año (2018, 2019, 2020)         │
  │  Paso 2: Promoción de encabezados y detección de tipos        │
  │  Paso 3: División de columna fecha_hora → Fecha + Hora        │
  │  Paso 4: Extracción de Hora, Día de semana y Mes              │
  │  Paso 5: Normalización de categorías (causa, clasificación)   │
  │  Paso 6: Gestión de valores nulos en coordenadas              │
  │  Paso 7: Columna calculada: heridos = leves + graves          │
  │  Paso 8: Append de las 3 tablas anuales en una sola           │
  └───────────────────────────────────────────────────────────────┘
       ↓
  Tabla Accidentes consolidada → Modelo Power BI
       ↓
  Medidas DAX + Filtros cruzados → Dashboard interactivo
```

---

## 📂 Estructura del Proyecto

```
dashboard-accidentes-transito/
│
├── 📊 Dashboard_Seguimiento_de_accidentes_de_Tránsito.pbix
│                                  ← Archivo principal Power BI
│                                    (modelo + ETL + DAX + visuales)
│
├── 📁 data/
│   ├── acidentes_2018.csv         ← Registros año 2018
│   ├── acidentes_2019.csv         ← Registros año 2019
│   └── acidentes_2020.csv         ← Registros año 2020
│
└── 📄 README.md                   ← Este archivo
```

---

## 📊 Análisis de Datos

### 🔎 1. Visión General de KPIs

| Métrica | Valor |
|---------|-------|
| 🚑 Total de accidentes | 60.531 |
| 🤕 Total de heridos | 71.260 |
| ⚰️ Total de muertos | 4.062 |
| 🚨 Accidentes con +3 víctimas | 8.525 (14,08%) |

El período 2018–2020 registró más de 60 mil accidentes en las carreteras federales de la región sudeste, con un promedio de ~20.177 eventos anuales. La relación heridos/accidentes de 1.18 indica que prácticamente cada evento genera al menos una víctima, mientras que 1 de cada 15 accidentes resulta fatal. La tasa de mortalidad sobre el total de heridos alcanza el 5.7%, lo que refleja la severidad característica de los siniestros en carreteras de alta velocidad.

> 🚨 **Alerta crítica:** 1 de cada 7 accidentes (14.08%) supera las 3 víctimas en un solo evento, lo que representa una carga hospitalaria y de emergencias significativa para la región y un impacto operativo severo para cualquier empresa con flota activa en estas rutas.

---

### 🗺️ 2. Distribución por Estado (UF)

| Estado | Accidentes | % del Total |
|--------|-----------|-------------|
| **MG** (Minas Gerais) | 26.160 | **43,22%** |
| RJ (Rio de Janeiro) | 13.417 | 22,17% |
| SP (São Paulo) | 12.936 | 21,37% |
| ES (Espírito Santo) | 8.018 | 13,25% |
| **Total** | **60.531** | **100%** |

#### 🧠 Interpretación

**Minas Gerais concentra casi la mitad de todos los accidentes de la región (43.22%)**, siendo con creces el estado de mayor riesgo vial del sudeste. Su extensa red de carreteras federales —incluyendo tramos montañosos de la BR-381 (Rodovia Fernão Dias) y la BR-040— y el alto volumen de transporte de carga pesada hacia el interior del país explican parcialmente esta concentración.

Rio de Janeiro y São Paulo, con porcentajes similares (~22% y ~21%), conforman el segundo nivel de riesgo. A pesar de tener menor cantidad de eventos que MG, su alta densidad de tráfico interurbano y la concentración de rutas de distribución metropolitanas elevan la probabilidad de accidentes de alto impacto. Espírito Santo, con el 13.25%, presenta la menor exposición relativa del grupo.

> 💡 **Insight para MasterLogistics:** Las rutas que atraviesan Minas Gerais deben ser objeto de protocolos de seguridad reforzados, sistemas de telemetría vehicular y posiblemente restricciones de horario nocturno para la flota propia. El 43% de los accidentes en un solo estado no es una anomalía estadística; es una señal estructural de la geografía y el volumen de tráfico de esa región.

---

### 📅 3. Tendencia Mensual de Eventos y Heridos

El gráfico de líneas duales (accidentes en azul, heridos en naranja) revela los siguientes patrones:

Ambas líneas siguen una trayectoria paralela a lo largo del año, confirmando una correlación directa y estable entre el volumen de accidentes y el número de heridos. Se observa una **caída notable en los meses de marzo y abril**, posiblemente asociada a menor circulación vehicular en períodos festivos (Semana Santa) o a condiciones climáticas de menor exposición.

Los meses de **julio y agosto** muestran una recuperación sostenida, vinculada al incremento de viajes durante las vacaciones de invierno en Brasil. El **último trimestre (octubre–diciembre)** registra los valores más elevados del año en ambas métricas —superando los 6.000 accidentes mensuales—, constituyendo el período de mayor riesgo operativo del ciclo anual.

> 💡 **Insight clave:** La alta dirección de MasterLogistics debería planificar con anticipación los movimientos de flota en el Q4. Este trimestre concentra el pico anual de siniestralidad y debería coincidir con el mayor nivel de alerta operativa, no con la relajación de fin de año.

---

### ⏰ 4. Distribución de Eventos por Hora

El gráfico de área horaria (0:00 a 23:59) revela un patrón bimodal característico del tráfico vial:

- **Valle nocturno (0:00–5:00):** Volumen mínimo de accidentes, cercano a cero. Baja circulación vehicular general, aunque los accidentes que ocurren en este tramo suelen ser más graves por la fatiga del conductor.
- **Primera escalada (6:00–8:00):** Incremento abrupto coincidente con el inicio de la jornada laboral y el tráfico de ingreso a centros urbanos e industriales.
- **Pico diurno sostenido (10:00–18:00):** El período de mayor concentración de accidentes, con valores que alcanzan los 1.000 eventos en los picos más altos. La hora del mediodía muestra un leve descenso seguido de recuperación en la tarde.
- **Descenso nocturno (19:00–23:59):** Caída progresiva, aunque con valores aún elevados entre las 19:00 y 21:00 por el tráfico de retorno laboral.

> 💡 **Insight operativo:** Las ventanas horarias de 6:00–8:00 y 16:00–18:00 son críticas. Para la flota de MasterLogistics, programar salidas antes de las 5:30 AM o después de las 20:00 podría reducir significativamente la exposición al riesgo de colisión.

---

### 📆 5. Distribución por Día de la Semana

| Día | Accidentes (aprox.) |
|-----|---------------------|
| Lunes | 8.000 |
| Martes | 7.000 |
| Miércoles | 8.000 |
| Jueves | 8.000 |
| Viernes | 9.000 |
| **Sábado** | **10.000** |
| **Domingo** | **10.000** |

#### 🧠 Interpretación

El patrón semanal muestra una **escalada progresiva de lunes a domingo**, con los fines de semana concentrando el mayor volumen de accidentes (~33% del total semanal). Este comportamiento es consistente con el fenómeno ampliamente documentado en seguridad vial: los desplazamientos de esparcimiento del fin de semana combinan mayor velocidad, mayor consumo de alcohol y menor prudencia al volante.

El viernes muestra ya un incremento notable respecto a los días hábiles de mitad de semana, funcionando como día de transición hacia el patrón de mayor riesgo. El martes registra el valor más bajo de la semana —el día de mayor rutina laboral y menor circulación recreativa.

> 💡 **Insight para la gestión de flota:** Los trayectos en viernes por la tarde y durante el fin de semana presentan el mayor riesgo de la semana. Implementar políticas de descanso obligatorio en sábado y domingo para rutas de larga distancia, o al menos reforzar el monitoreo en esos días, puede tener impacto directo en la siniestralidad de la flota.

---

### ⚠️ 6. Top 7 Causas de Accidentes

| Posición | Causa | Eventos (aprox.) |
|----------|-------|-----------------|
| 🥇 1° | Falta de atención a la conducción | 22.000 |
| 🥈 2° | Velocidad incompatible | 8.000 |
| 🥉 3° | Desobediencia a las normas | 6.000 |
| 4° | Defecto mecánico | 4.000 |
| 5° | No mantenimiento | 4.000 |
| 6° | Consumo de alcohol | 3.000 |
| 7° | Pista resbaladiza | 3.000 |

#### 🧠 Interpretación

**La falta de atención a la conducción es, con diferencia, la causa dominante con 22.000 eventos**, triplicando a la segunda causa más frecuente y representando el 36% de todos los accidentes registrados. Esta categoría engloba el uso del teléfono al volante, distracción por pasajeros, fatiga y microsueño —todos factores prevenibles con tecnología y protocolos de conducción.

La velocidad incompatible (8.000) y la desobediencia a las normas (6.000) completan el podio de causas evitables por comportamiento humano. En conjunto, las **tres primeras causas representan aproximadamente el 60% de todos los accidentes** y comparten una característica fundamental: son 100% prevenibles.

El defecto mecánico (4.000) y la falta de mantenimiento (4.000) suman 8.000 accidentes directamente vinculados al estado del vehículo, una variable totalmente controlable por cualquier empresa con flota propia. El consumo de alcohol (3.000) y la pista resbaladiza (3.000) cierran el ranking con menor frecuencia relativa, aunque con impacto severo en la mortalidad cuando ocurren.

> 💡 **Recomendación directa:** MasterLogistics debería priorizar la implementación de sistemas de **monitoreo de fatiga y distracción** (DMS — Driver Monitoring Systems) en su flota de expansión, dado que la causa número 1 —con 22.000 eventos— es directamente prevenible con tecnología disponible en el mercado actual.

---

### 🛣️ 7. Distribución por Tipo de Carril

| Tipo de Pista | Accidentes (aprox.) | % del Total |
|---------------|---------------------|-------------|
| **Dupla (doble vía)** | **28.000** | **46%** |
| Simple (vía única) | 25.000 | 41% |
| Múltiple | 7.000 | 12% |

#### 🧠 Interpretación

Las carreteras de **doble vía (Dupla) concentran el mayor volumen de accidentes con 28.000 eventos (46% del total)**, seguidas de cerca por las vías simples (25.000, 41%). Las pistas múltiples, con mayor cantidad de carriles y generalmente mejor señalizadas e iluminadas, registran el menor número de eventos (7.000, 12%).

Contraintuitivamente, las carreteras de doble vía —que deberían ser más seguras al separar el tráfico contrario— lideran la siniestralidad. Esto se explica porque las rutas de doble vía son precisamente las de mayor tráfico, mayores velocidades permitidas y mayor concentración de vehículos pesados, lo que incrementa tanto la frecuencia como la severidad de los eventos.

> 💡 **Insight para el ruteo:** Las rutas de doble vía de alta velocidad combinan alto volumen con alta velocidad —una ecuación de riesgo máximo. La gestión de velocidad y los tiempos de descanso obligatorios deben ser más estrictos en este tipo de carreteras, especialmente en los tramos más transitados de Minas Gerais.

---

### 🔥 Conclusión General

```
┌────────────────────────────────────────────────────────────────────┐
│                                                                    │
│  🔴 FACTORES DE RIESGO CRÍTICOS    🟡 PATRONES IDENTIFICADOS      │
│  ──────────────────────────────    ──────────────────────────      │
│  • MG concentra el 43% de los      • Fines de semana son los      │
│    accidentes de toda la región      días de mayor siniestralidad  │
│  • 14.08% de accidentes supera     • Q4 (oct–dic) es el           │
│    3 víctimas en un solo evento      trimestre más peligroso       │
│  • Falta de atención causa el      • Picos horarios a las 6-8 AM  │
│    36% de todos los accidentes       y 16-18 PM son las ventanas   │
│  • 8.000 accidentes por defecto      de máximo riesgo diario       │
│    mecánico son 100% prevenibles   • Pistas dupla lideran          │
│    con mantenimiento adecuado        accidentes por tipo de vía    │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

---

## 💡 Recomendaciones Estratégicas para MasterLogistics

### 1. 🛡️ Protocolo de Rutas Diferenciado por Estado
Establecer niveles de alerta diferenciados: **Riesgo Crítico** para Minas Gerais (43% de accidentes), con monitoreo en tiempo real, velocidades máximas internas por debajo del límite legal y puntos de descanso obligatorio cada 2 horas en los tramos identificados de mayor concentración.

### 2. 📱 Implementación de DMS (Driver Monitoring System)
Con la falta de atención como causa del 36% de los accidentes, instalar sistemas de detección de fatiga y distracción en la flota es la inversión con mayor retorno en seguridad para el proyecto de expansión. La tecnología DMS puede prevenir directamente hasta 22.000 de los 60.000 eventos registrados.

### 3. 📅 Política de Operación Diferenciada en Fin de Semana
Sábados y domingos concentran ~33% del total semanal de accidentes. Para rutas de alta exposición en carreteras federales de MG y RJ, implementar política de no-conducción nocturna en fin de semana reduciría significativamente la exposición de la flota.

### 4. ⏰ Optimización de Ventanas Horarias de Salida
Programar salidas de flota antes de las 5:30 AM o después de las 20:00 para evitar los picos horarios de 6:00–8:00 y 16:00–18:00, que concentran el mayor volumen diario de accidentes.

### 5. 🔧 Programa de Mantenimiento Preventivo Riguroso
Defecto mecánico (4.000) y falta de mantenimiento (4.000) suman 8.000 accidentes directamente prevenibles. Implementar un sistema de gestión de mantenimiento predictivo para toda la flota de expansión elimina una causa de riesgo completamente controlable.

### 6. 🎓 Capacitación Reforzada Antes del Q4
El último trimestre registra los picos anuales más altos. Programar capacitaciones de manejo defensivo y actualización de protocolos antes de octubre prepara a la flota para navegar el período de mayor siniestralidad del año con menor exposición.

---

## 🚀 Cómo usar el Dashboard

**Paso 1 — Prerrequisitos**

Descargar e instalar **Power BI Desktop** (gratuito):
👉 https://www.microsoft.com/es-es/power-platform/products/power-bi/desktop

**Paso 2 — Abrir el proyecto**

```
1. Descargar Dashboard_Seguimiento_de_accidentes_de_Tránsito.pbix
2. Abrir el archivo con Power BI Desktop
3. Si solicita credenciales, seleccionar "Anónimo" para fuentes locales
```

**Paso 3 — Actualizar la fuente de datos (opcional)**

```
Inicio → Transformar datos → Configuración del origen de datos
→ Apuntar a los archivos CSV en tu ruta local
→ Cerrar y aplicar
```

**Paso 4 — Interactuar con el Dashboard**

```
✅ Usar los filtros de clasificación (Con víctimas fatales /
   Con víctimas heridas / Sin víctimas) para segmentar por gravedad
✅ Seleccionar el año (2018 / 2019 / 2020) para análisis temporales
✅ Clic en cualquier barra del Top 7 para filtrar toda la vista
   por esa causa específica de accidente
✅ Clic en cualquier estado de la tabla para ver solo los accidentes
   de ese estado en todos los demás visuales
✅ Usar el mapa para identificar zonas de alta concentración
   y planificar rutas alternativas
✅ Publicar en Power BI Service para compartir con el equipo de
   seguridad y operaciones
```
<img width="883" height="495" alt="image" src="https://github.com/user-attachments/assets/a953e805-8d40-4a66-8e85-324c57ea1890" />

---

## 🛠️ Stack Tecnológico

| Herramienta | Rol en el proyecto |
|-------------|-------------------|
| 📈 **Power BI Desktop** | Modelado de datos y creación de visualizaciones interactivas |
| 🔄 **Power Query (M)** | ETL: limpieza, transformación y consolidación de datos multi-año |
| 🧮 **DAX** | Medidas calculadas para KPIs y métricas de alta gravedad (+3 víctimas) |
| 🗺️ **Mapa de Power BI** | Geolocalización de accidentes con coordenadas geodésicas decimales |
| 📋 **CSV / Datos abiertos** | Fuente de datos de accidentes viales federales (2018–2020) |
| 🪟 **Windows** | Sistema operativo compatible (10 / 11) |

---

## 📈 Posibles Extensiones

- 🤖 **Modelo Predictivo de Siniestralidad:** Usar datos históricos para predecir tramos y horarios de mayor riesgo con modelos de machine learning integrados vía Python en Power BI.
- 🌦️ **Correlación con Datos Meteorológicos:** Cruzar los registros con datos climáticos (lluvia, niebla, temperatura) para identificar el impacto del clima en la frecuencia de eventos.
- 📊 **Análisis Interanual (YoY):** Comparar la evolución de cada KPI entre 2018, 2019 y 2020 con medidas de variación porcentual año contra año.
- 🚛 **Segmentación por Tipo de Vehículo:** Identificar si los camiones de carga (relevantes para MasterLogistics) presentan patrones de accidentabilidad distintos al resto del parque vehicular.
- 📡 **Telemetría en Tiempo Real:** Conectar con APIs de telemetría vehicular para alertar cuando vehículos de la flota ingresan a zonas de alta concentración de siniestros.
- 🔐 **Row-Level Security (RLS):** Seguridad por región para que cada jefe de zona visualice solo los datos de su área de responsabilidad operativa.

---

## 🧰 Solución de Problemas Comunes

**❌ Error: No se puede abrir el archivo `.pbix`**
```
→ Asegúrate de tener Power BI Desktop instalado (no Power BI Report Builder)
→ Descarga la versión más reciente desde Microsoft Store o el sitio oficial
```

**❌ El mapa no muestra datos / aparece mensaje de error**
```
→ El visual de mapa requiere habilitación por el administrador del tenant
→ En entornos personales: Archivo → Opciones → Seguridad →
  Habilitar objetos visuales de mapa
→ Verificar que las columnas latitud y longitud no contengan valores nulos
```

**❌ Error: No se encuentran los archivos de datos**
```
→ Inicio → Transformar datos → Configuración del origen de datos
→ Cambiar origen → Navegar hasta la ubicación de los CSV en tu equipo
→ Verificar que los 3 archivos (2018, 2019, 2020) estén en la misma carpeta
```

**❌ Los filtros por año no actualizan todos los visuales**
```
→ Verificar que los slicers de año estén conectados a la columna
  de año extraída de fecha_hora durante el ETL en Power Query
→ Formato → Editar interacciones → Activar filtro cruzado en cada visual
```

**❌ El mapa no muestra los puntos en las coordenadas correctas**
```
→ Verificar que la columna latitud esté categorizada como "Latitud"
  y longitud como "Longitud" en las propiedades del modelo
→ Columna → Herramientas de columna → Categoría de datos
```

---

> Construido con 📈 Power BI + 🔄 Power Query + 🧮 DAX + 🗺️ Mapas Geoespaciales
>
> Caso de negocio desarrollado en el marco del programa **Master Power BI — Nivel Principiante** de **Daxus Latam**
>
> Para uso educativo y demostración de soluciones analíticas aplicadas a seguridad vial y logística de transporte
>
> Elaborado por: **Diego Vallejo**
