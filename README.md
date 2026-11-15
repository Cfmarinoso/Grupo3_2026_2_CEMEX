# Business Intelligence para diagnosticar la caída del market share del cemento Cemex en la autoconstrucción del norte del Perú (2023–2025)

## Integrantes:
* Carlos Mariños
* Andrea Nepo
* Paula Ramos
* Daniela Yaulli

## 1. Introducción a la empresa
CEMEX nació en 1906 en Nuevo León, México, como una pequeña planta regional llamada Cementos Hidalgo. En 1931, se fusionó con Cementos Portland Monterrey, dando origen oficial a Cementos Mexicanos (CEMEX).
Las operaciones de CEMEX en Peru se dan principalmente bajo un modelo de importación y distribución, consolidándose como un fuerte competidor en el mercado nacional de materiales desde su ingreso en 2007.
CEMEX ofrece una cartera diversificada de materiales para la construcción y centra su estrategia global en la sostenibilidad, la digitalización y la economía circular.

# Negocio analizado: Autoconstrucción
La autoconstrucción es la construcción o ampliación de viviendas por parte de las propias familias, por etapas, con un maestro de obra y comprando los materiales poco a poco en ferreterías y depósitos. En este segmento el cemento se vende en bolsas, y la marca suele ser influida por el maestro de obra y el ferretero, además del dueño de la obra.

Para monitorear este segmento, la empresa realiza un censo de obras, es decir: censadores visitan obras en construcción y registran qué marca de cemento se está usando, en qué etapa de obra, con qué volumen y a qué precio. Con estos registros se calcula el share de Cemex frente a todas las demás marcas.

En los datos, la marca propia es "Cemex Cemento". Las competidoras principales son Mochica, Inka, Holcim, Quisqueya, Qhuna y La Nacional.

## 2. Problemática
La gerencia comercial ha observado una caída sostenida del market share del cemento Cemex en el segmento de autoconstrucción en los últimos 6 meses del año 2025.

El share de volumen pasó de 83.1% (ene–jun 2025) a 76.2% (jul–dic 2025): −7.0 pp. Contra el mismo periodo del año anterior (83.6%) la caída es de −7.5 pp, por lo que no se explica por estacionalidad.
El volumen censado del mercado creció +5.0%, el de Cemex cayó −3.8% y el de la competencia creció +48.4%: Por lo que, las ventas de Cemex no siguieron el crecimiento del mercado.

Mantener promociones y beneficios para los clientes tiene un costo operativo y financiero, por lo que la empresa necesita saber en qué zonas, segmentos y periodos se concentra la caída, quién captura el share perdido y si el precio influye, para focalizar sus acciones.


### Objetivo
Diseñar e implementar una solución de Business Intelligence que permita diagnosticar dónde, cuándo y por qué cae el market share del cemento Cemex en la autoconstrucción en el norte del Perú, y proponer una estrategia de mejora para recuperarlo.

## 3. Marco teórico
### Business Intelligence
El término Business Intelligence [BI] fue usado por primera por vez Hans Peter Luhn en 1958, pero pasó a tener reconocimiento recién en los años 90s gracias a Gartner, quien lo define como un proceso que usa tecnología para analizar datos y presentar información útil, con el fin de que ejecutivos y gerentes tomen mejores decisiones de negocio.
Para el caso de este proyecto, dirigido a CEMEX, BI sirve para convertir los datos de los despachos de cemento (pedidos, entregas, facturación) en información que ayude a tomar decisiones comerciales. Por ejemplo, saber si la promoción de "triple puntos" del producto Antisalitre HS realmente está funcionando o si solo está generando gasto sin mejorar la participación de mercado.

### Data Warehouse y Datamart
El Data Warehouse [DW] es el repositorio central que reúne datos extraídos de los sistemas operacionales, los cuales son transformados para hacerlos consistentes y cargados para su análisis (Inmon, 1992). Por su parte, el Datamart, es un subconjunto del DW enfocado en un área específica del negocio, como por ejemplo, ventas, marketing o recursos humanos, es más rápido de construir, más económico y más fácil de mantener que un DW completo.

### Modelamiento Dimensional (Data Dimensional)
Este modelo fue desarrollado por Ralph Kimball, es el estándar para diseñar la capa analítica de un data warehouse. Se basa en organizar los datos en dos tipos de tablas: tablas de hechos (que guardan las medidas) y tablas de dimensiones (que dan el contexto descriptivo). (Kimball y Ross, 2013)
Tabla de hechos. Guarda las medidas o métricas de un evento del negocio (normalmente números) además de las llaves foráneas que apuntan a las dimensiones.
Tabla de dimensiones. Guarda los atributos descriptivos (algunos ejemplos pueden ser: cliente, producto, ruta, transporte, tiempo) que sirven para filtrar y agrupar las medidas de la tabla de hechos.
Esquema estrella vs. copo de nieve. En un esquema estrella, la tabla de hechos se conecta directo con tablas de dimensión que no están normalizadas. De acuerdo a Kimball (2013) se recomienda evitar normalizar las dimensiones (el "copo de nieve") salvo que sea realmente necesario, porque hace las consultas más lentas y complica el modelo para el usuario final.

### Cubos OLAP
El término OLAP (Online Analytical Processing) fue desarrollado por Codd, Codd y Salley en 1993, quienes plantean un tipo de procesamiento de datos distinto al transaccional, pensado específicamente para el análisis. Luego, Chaudhuri y Dayal (1997), explican que un cubo OLAP se construye típicamente sobre un esquema estrella o copo de nieve dentro de un data warehouse: las medidas salen de la tabla de hechos y las dimensiones de las tablas de dimensión. Para CEMEX, esto significará poder ver, por ejemplo, la venta despachada de Antisalitre HS por ruta, por vendedor y por mes, para saber si la promoción de puntos realmente está dando resultado.



## 4. Diccionario de datos

El Data Warehouse de operaciones logísticas y comerciales está diseñado bajo un modelo dimensional de tipo estrella. Este esquema permite analizar los despachos de materiales desde múltiples perspectivas geográficas, comerciales y temporales para evaluar estrategias comerciales. 

El modelo está compuesto por una tabla de hechos central, FACT_CONSUMOS la cual se relaciona con once dimensiones para contextualizar el análisis operativo. A continuación, se detalla el diccionario de datos consolidado, ajustado a los datos reales de la operación.

### 3.3 Diccionario de datos

#### Tabla de hechos: `fact_consumo`
Grano: una encuesta de censador.

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Código de la encuesta de censador (`codencuestacensadores`). |
| visitaID | INT | Código de la visita. **Dimensión degenerada.** |
| tiempoID | INT (FK) | Fecha de la encuesta → `Tiempo`. |
| oficinaID | INT (FK) | Oficina responsable → `Oficina`. |
| ubigeoID | INT (FK) | Ubicación de la obra → `Ubigeo`. |
| obraID | INT (FK) | Obra censada → `Obra`. |
| tipoEstructuraID | INT (FK) | Etapa de la obra → `TipoEstructura`. |
| marcaID | INT (FK) | Marca de cemento encontrada → `Marca`. |
| unidadMedidaID | INT (FK) | Unidad del volumen → `UnidadMedida`. |
| motivoID | INT (FK) | Motivo de compra de la marca → `Motivo`. |
| ejecutivoID | INT (FK) | Ejecutivo que registró la encuesta → `Ejecutivo`. |
| proveedorID | INT (FK) | Proveedor registrado → `Proveedor`. |
| competenciaID | INT (FK) | Tipo de competencia y marca competidora → `Competencia`. |
| idSociedad | SMALLINT | Sociedad comercial (6012 / 6052). |
| sociedadCli6052 | BIT | Indica si el cliente pertenece a la sociedad 6052. |
| sociedadCli6012 | BIT | Indica si el cliente pertenece a la sociedad 6012. |
| tipoVisita | TINYINT | Tipo de visita (1 / 2; significado por confirmar con negocio). |
| estado | BIT | 1 = encuesta activa, 0 = dada de baja. El dashboard usa solo las activas. |
| posibilidadReversion | BIT | Indicador (sí/no) de posibilidad de reversión registrado en la encuesta. |
| flagMotivoCpsaa | BIT | Indicador (sí/no) del flag de motivo CPSAA registrado en la encuesta. |
| **volumen** | DECIMAL(18,2) | **Medida.** Cantidad de cemento de la marca en la obra (bolsas en el análisis). Aditiva. |
| **volumenRestante** | DECIMAL(18,2) | **Medida.** Volumen restante registrado para la obra. |
| **precio** | DECIMAL(18,2) | **Medida.** Precio registrado por unidad (S/ por bolsa). No aditiva: se promedia ponderado por volumen. |
| fechaIngreso | DATE | Fecha de ingreso registrada en la encuesta. |
| fechaCreacion | DATETIME2 | Fecha y hora de creación del registro. |
| fechaModificacion | DATETIME2 | Fecha y hora de la última modificación. |
| checkin | DATETIME2 | Fecha y hora de llegada del censador a la obra. |
| checkout | DATETIME2 | Fecha y hora de salida del censador de la obra. |
| duracionMin | calculada | Minutos entre check-in y check-out. |
| checkinLatitud / checkinLongitud | DECIMAL(10,7) | Coordenadas del check-in (se usan para ubicar distritos sin polígono en el mapa). |
| checkoutLatitud / checkoutLongitud | DECIMAL(10,7) | Coordenadas del check-out. |
| acciones | NVARCHAR(500) | Acciones registradas por el censador. |
| observacion | NVARCHAR(500) | Observaciones del censador. |

#### Dimensión `Tiempo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Fecha en formato numérico YYYYMMDD (ej. 20250315). |
| fecha | DATE | Fecha calendario. |
| anio | SMALLINT | Año. |
| trimestre | TINYINT | Trimestre del año (1–4). |
| numMes | TINYINT | Número del mes (1–12). |
| mes | NVARCHAR(12) | Nombre del mes. |
| anioMes | INT | Año y mes en formato YYYYMM. |
| dia | TINYINT | Día del mes. |
| semanaISO | TINYINT | Número de semana ISO. |
| diaSemana | TINYINT | Día de la semana (1 = lunes … 7 = domingo). |
| nombreDia | NVARCHAR(12) | Nombre del día de la semana. |
| esFinDeSemana | BIT | 1 si es sábado o domingo. |

#### Dimensión `Oficina`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de origen de la oficina (`idoficina`). |
| nombre | NVARCHAR(100) | Nombre de la oficina (ej. Trujillo, Piura, Chimbote). |
| idSociedad | INT | Sociedad a la que pertenece la oficina. |

#### Dimensión `Ubigeo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | CHAR(6) | Ubigeo INEI: departamento (2) + provincia (2) + distrito (2). |
| codDepartamento / codProvincia / codDistrito | INT | Códigos de cada nivel geográfico. |
| departamento | NVARCHAR(100) | Nombre del departamento. |
| provincia | NVARCHAR(100) | Nombre de la provincia. |
| distrito | NVARCHAR(100) | Nombre del distrito. |

#### Dimensión `Obra`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | BIGINT | Código de la obra (`codigoobra`). |
| idMaestroObra | INT | Maestro de obra de la obra (si hay varios, se toma el de la encuesta más reciente). |
| tipoEjecucion | NVARCHAR(50) | Tipo de ejecución de la obra (ej. Remodelación). |

#### Dimensión `TipoEstructura`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de origen del tipo de estructura. |
| nombre | NVARCHAR(100) | Etapa de la obra (ej. muros). |

#### Dimensión `Marca`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de la marca local (`codigotipomarcalocal`). |
| tipoMarca | NVARCHAR(100) | Descripción del tipo de marca. No se usa para analizar marcas. |
| codMarca | INT | Código de la marca. |
| marca | NVARCHAR(100) | Nombre de la marca tal como llega del origen. |
| marcaNormalizada | NVARCHAR(100) | Nombre en mayúsculas, para unir variantes (ej. "Holcim" y "HOLCIM"). |
| esMarcaPropia | BIT | 1 si es marca propia de Cemex (código de marca 1, "Cemex Cemento"). |
| codLinea / linea | INT / NVARCHAR(100) | Código y nombre de la línea. |
| codSublinea / sublinea | INT / NVARCHAR(100) | Código y nombre de la sublínea. |
| codProducto / producto | INT / NVARCHAR(100) | Código y nombre del producto (el análisis usa *Cemento*). |

#### Dimensión `UnidadMedida`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de origen de la unidad. |
| abreviatura | NVARCHAR(20) | Abreviatura de la unidad (BLS = bolsas). |

#### Dimensión `Motivo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de origen del motivo. |
| nombre | NVARCHAR(100) | Motivo de compra (ej. Precio, Afinidad, Crédito y precio, Experiencia del servicio, Fuera de cobertura, Gestión, Otro). El código 12 llega sin descripción. |

#### Dimensión `Ejecutivo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código del ejecutivo que creó la encuesta (`codcreadorom`). |
| ejecutivoSAP | INT | Código del ejecutivo en SAP. |
| nombre | NVARCHAR(150) | Nombre del ejecutivo. |

#### Dimensión `Proveedor`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | BIGINT | Código del proveedor (12 dígitos). |
| nombre | NVARCHAR(200) | Nombre del proveedor. No viene en el dataset: se completaría desde el sistema comercial. |

#### Dimensión `Competencia`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. El valor −1 significa "no aplica" (~89% de las filas). |
| codTipoCompetencia | INT | Código del tipo de competencia (0 = no aplica). |
| tipoCompetencia | NVARCHAR(100) | Tipo de competencia. |
| codMarcaCemento | INT | Código de la marca de cemento competidora (0 = no aplica). |
| marcaCemento | NVARCHAR(100) | Marca de cemento competidora. |

## Modelo multidimensional:
<img width="754" height="626" alt="Modelo_multidimensional" src="https://github.com/user-attachments/assets/dd3b670a-3681-4abb-b46c-75da40783b43" />

## 4. Diseño y análisis de los resultados de Business Intelligence

A continuación, se creó un dashboard interactivo con el fin de que la gerencia comercial pueda responder qué está pasando con la participación de mercado de Cemex: **cuándo empezó a caer, en qué zonas, frente a qué marcas y si el precio influye**. Además de diagnosticar, permite simular el efecto de las acciones de mejora y monitorear cada mes los indicadores clave con semáforos. Este dashboard está publicado en Railway, por lo que cualquier usuario puede consultarlo desde un link público.

## Enlace al dashboard:
https://cemex-dash.up.railway.app/

### 4.1 Arquitectura y herramientas

Flujo de datos: **Excel del censo de obras → ETL en Python → datamart relacional (esquema estrella) → dashboard web → publicado en Railway.**

| Herramienta | Para qué se usa |
| :--- | :--- |
| Python (pandas, numpy, openpyxl) | ETL: limpieza, carga de dimensiones y hechos, y cálculo de indicadores. |
| pyodbc | Conexión entre Python y la base de datos. |
| SQL | Creación del datamart y consultas analíticas. |
| Dash + Plotly (y sqlglot) | Dashboard interactivo con filtros, mapas y gráficos. sqlglot valida las consultas SQL de la pestaña 5. Se eligió por estar basado en la plantilla vista en clase y por trabajar todo en Python. |
| Railway | Despliegue de la aplicación. Se eligió porque es una herramienta gratuita que nos permite tener un **link público**, como pide la rúbrica. |

### 4.2 Definición de indicadores

A modo de contexto, un censador visita una **obra** (una vivienda en construcción) y registra qué marca de cemento usa y cuántas **bolsas**.

| Indicador | Qué mide | Cálculo |
| :--- | :--- | :--- |
| **Share de volumen** | De cada 100 bolsas de cemento vistas en las obras, cuántas son de Cemex. Es el indicador principal. | `bolsas Cemex / bolsas de todas las marcas × 100` |
| **Media móvil 3M** | El mismo share, suavizado para que un mes atípico no distorsione la tendencia. | Share calculado sobre la suma de los últimos 3 meses |
| **Meta** | Valor de referencia para comparar. Por defecto es el share del mismo periodo del año anterior. | Dato de entrada |
| **Brecha** | Qué tan lejos está el share de la meta. | `share actual − meta` (en pp) |
| **Penetración en obras** | En qué porcentaje de las obras aparece Cemex, sin importar cuántas bolsas. | `obras con Cemex / obras censadas × 100` |
| **Índice de precio** | Cuánto más caro (o barato) vende Cemex que el resto. 100 = mismo precio. | `precio promedio Cemex / precio promedio competencia × 100`, ponderado por bolsas |
| **Bolsas por recuperar** | Cuántas bolsas habría que volver a ganar para llegar a la meta (total) o al share anterior (por oficina o distrito). | Se calcula por segmento a partir del share perdido |
| **Efecto tasa y efecto mezcla** | Separa el cambio total del share en dos partes. **Tasa:** el share cayó dentro de una oficina (caída real). **Mezcla:** cambió el peso de cada oficina en los datos (efecto de cobertura del censo). | Tasa + mezcla = cambio total |
| **Semáforo** | Estado del indicador. | **En meta:** brecha ≥ −1 pp. **Alerta:** entre −1 y −3 pp. **Crítico:** peor que −3 pp |
| **Oficina en alerta** | Oficina cuyo share cayó 3 pp o más. | Conteo sobre las oficinas con datos |

### 4.3 Estructura del dashboard

**Filtros globales:** periodo (3, 6, 9 o 12 meses), comparar contra (periodo anterior o mismo periodo del año anterior), meta, departamento, oficina, etapa de obra y tipo de ejecución.

El dashboard se divide en 5 pestañas, cada una con un propósito en específico:
| Pestaña | Para qué sirve | Gráficos principales |
| :--- | :--- | :--- |
| **1. Resumen y KPIs** | Ver el estado actual y la evolución del share. | 8 tarjetas con semáforo, insights automáticos, líneas de share y volumen, cascada del cambio |
| **2. ¿Dónde y cuándo cae?** | Ubicar la caída por oficina, distrito y periodo. | Barras por segmento, mapa de distritos, mapa de calor oficina × mes, tabla de distritos |
| **3. Competencia y precio** | Ver quién gana lo que Cemex pierde y si el precio influye. | Barras por marca, líneas de competidoras y de precio, dispersión precio vs caída, motivos de compra |
| **4. Propuesta** | Priorizar acciones y simular su efecto. | Simulador, tabla de prioridad por oficina, acciones y tablero de indicadores |
| **5. Consultas SQL** | Hacer consultas libres sobre el datamart. | Consola SQL con validación, tabla de resultados, descarga CSV/Excel |

### 4.4 Resultados: qué está pasando

**1. Cuándo cae.** El share de Cemex pasó de **83.1% a 76.2%** (−7.0 pp) en el último semestre, y también está −7.5 pp por debajo del mismo periodo del año anterior, así que no es estacionalidad. La media móvil tuvo su máximo en **marzo 2025 (85.2%)** y cruzó por debajo de la meta en **junio 2025**: es una caída reciente y rápida. Los 8 indicadores de seguimiento muestran 6 en estado Crítico, 1 en Alerta y 1 en seguimiento.

**2. Quién crece.** En el mismo periodo el volumen censado del mercado creció **5.0%**, el de Cemex cayó **3.8%** y el de la competencia creció **48.4%**. Cemex no acompañó el crecimiento del mercado.

**3. Dónde cae.** Está muy concentrada: **Trujillo, Piura y Chimbote explican ≈78% de la caída real**. Trujillo bajó de 66.7% a 54.2% (−12.5 pp) y Chimbote de 61.8% a 53.3% (−8.5 pp); ambas son zonas de bajo share. Piura bajó de 91.9% a 86.7% (−5.2 pp), pero es la oficina de mayor volumen (912,735 bolsas), así que pesa mucho. A nivel distrito, La Esperanza (Trujillo) cayó −21.8 pp. Por segmento, la mayor caída está en la etapa de obra *Muros* (−9.0 pp) y en el tipo de ejecución *Remodelación* (−15.5 pp). Algunas oficinas sí subieron: Yurimaguas +7.2 pp, Huamachuco +2.4 pp y Tumbes +1.9 pp.

**4. Quién gana.** Tres marcas capturan el share perdido: **Mochica +3.5 pp**, **Holcim +3.0 pp** (de 0.4% a 3.3%) e **Inka +2.5 pp**. Cada una gana en zonas distintas: Inka en Trujillo y Piura, Mochica en Chimbote y Holcim en Cajamarca y Chiclayo.

**5. El precio.** Cemex vende **8.4% más caro** (S/ 32.63 vs S/ 30.10 por bolsa) y "Precio" es el motivo de compra de la competencia en 61% de los casos con motivo registrado. Sin embargo, el índice de precio casi no cambió (107 → 108) mientras el share cayó 7 pp, y la correlación entre precio y caída por oficina es moderada (r = −0.46, solo 13 oficinas). El precio es lo que dicen las obras, pero **no basta para explicar la caída reciente**. Correlación no es causalidad.

**6. Calidad del dato.** De los −7.0 pp, **−4.9 pp son caída real (efecto tasa) y −2.0 pp (≈29%) vienen de cambios en la cobertura del censo (efecto mezcla)**. Además, el motivo de compra solo está registrado en 45% del volumen de competencia, por lo que las conclusiones sobre motivos son indicativas.

### 4.5 Propuesta de mejora y factibilidad

Objetivo: recuperar el share hacia la meta (83.6%) priorizando donde más bolsas se pueden recuperar. Plazos, recursos y costos son **referenciales** y deben validarse con las áreas comercial y de finanzas.

| # | Acción | Dónde | Plazo | Recursos | Costo |
| :-- | :--- | :--- | :--- | :--- | :--- |
| **A1** | Plan de choque con metas mensuales de share por oficina | Trujillo, Piura, Chimbote | 0–3 meses | Gerencia comercial, jefes de oficina, fuerza de ventas | Medio |
| **A2** | Defensa por competidor: seguimiento de obras y fidelización de maestros de obra y ferreterías | Inka en Trujillo y Piura; Mochica en Chimbote; Holcim en Cajamarca y Chiclayo | 1–3 meses | Ejecutivos y visitas a obras y ferreterías | Medio |
| **A3** | Promociones de precio focalizadas (no descuento general), con grupo de control para medir el retorno | Oficinas con precio relativo alto y share en caída | Piloto de 3 meses | Marketing, finanzas, sistema de puntos | Variable (presupuesto acotado) |
| **A4** | Documentar y replicar las prácticas de las oficinas que suben | Yurimaguas, Huamachuco, Tumbes | 1 mes | Jefes de oficina y analista | Bajo |
| **A5** | Mejorar el dato: registrar canal de venta y promoción aplicada, subir el motivo informado de 45% a ≥80% y estabilizar la cobertura del censo | Todas las oficinas | 1–2 meses | TI y analista de datos | Bajo |
| **A6** | Tablero mensual con semáforos y meta definida por la gerencia | Gerencia comercial | 1 mes (ya existe) | Analista BI y hosting en Railway | Bajo |

**Simulador (pestaña 4).** Recuperar el 50% de la caída en las 5 oficinas prioritarias (Trujillo, Piura, Chimbote, Chiclayo y Cajamarca) suma **+2.30 pp** de share (≈82,911 bolsas), es decir, **31% de la brecha** a la meta. Incluso recuperando el 100% de la caída en todas las oficinas (182,111 bolsas) no se llega a la meta (269,043 bolsas), porque parte de la diferencia viene de la cobertura del censo; por eso la meta debe revisarse con la gerencia.

**Valor de referencia.** Esas 82,911 bolsas equivalen a unos **S/ 2.7 millones** por semestre y cerrar toda la brecha a unos **S/ 8.8 millones** (bolsas × S/ 32.63). Son montos de venta, no de margen, y sirven como tope para comparar el costo de las acciones.


## 5. Conclusiones

1. **El share cae rápido y es reciente:** de 83.1% a 76.2% en un semestre, con quiebre en junio 2025, mientras el mercado crece 5.0%.
2. **La competencia absorbe el crecimiento:** Mochica, Inka y Holcim crecen en zonas distintas, por lo que una estrategia única sería poco eficaz.
3. **El problema está concentrado:** tres oficinas (Trujillo, Piura y Chimbote) explican ≈78% de la caída real.
4. **El precio influye, pero no lo explica todo:** Cemex es 8% más caro, pero esa diferencia casi no cambió (107 → 108) mientras el share cayó 7 pp.
5. **Parte de la caída es de medición:** ≈29% del cambio (−2.0 pp) viene de la cobertura del censo, y el motivo de compra solo está registrado en 45% de los casos.
6. **La solución de BI cumple su objetivo:** el datamart (1 tabla de hechos y 11 dimensiones) y el dashboard de 5 pestañas permiten ubicar dónde, cuándo y frente a quién cae el share, simular acciones y monitorear 8 indicadores con semáforo. Sus límites: solo cubre cemento en bolsa en autoconstrucción y no tiene campo de canal de venta.


## 6. Recomendaciones

1. **Priorizar Trujillo, Piura y Chimbote** con metas mensuales y seguimiento semanal (A1).
2. **Responder por competidor y por zona**, no con una campaña nacional (A2).
3. **No usar descuento general:** probar promociones focalizadas con grupo de control (A3).
4. **Aprender de las oficinas que suben** (Yurimaguas, Huamachuco, Tumbes) (A4).
5. **Mejorar la captura de datos:** agregar canal de venta y promoción aplicada, y completar el motivo de compra (A5). Sin esto no se puede medir el retorno de la promoción de puntos.
6. **Definir una meta de share con la gerencia** y revisar el tablero cada mes (A6).
7. **Validar las acciones con el equipo comercial** antes de ejecutarlas: son hipótesis construidas a partir de los datos.
8. **Trabajo futuro:** incluir otras líneas (concreto, bloques y adoquines), agregar datos de despachos facturados y ampliar el rango de años.


## 7. Dataset
https://drive.google.com/drive/folders/1pt_AFsa8XqcQDsAzD5DGkNqI7F3tiFYS?usp=sharing 

## 8. Presentación
https://www.canva.com/design/DAHWKtakACw/rGvoX6XlY_uhJObBKPuAjg/edit

## 9. Referencias
Chaudhuri, S., & Dayal, U. (1997). An overview of data warehousing and OLAP technology. ACM SIGMOD Record, 26(1), 65–74. https://doi.org/10.1145/248603.248616

Codd, E. F., Codd, S. B., & Salley, C. T. (1993). Providing OLAP (On-line analytical processing) to user-analysts: An IT mandate. E.F. Codd & Associates. https://staff.icar.cnr.it/manco/Teaching/2006/datamining/articoli/olapcoddwp.pdf

Gartner. (s.f.). Business intelligence (BI) [Entrada de glosario]. Gartner Glossary. https://www.gartner.com/en/information-technology/glossary/business-intelligence-bi

Inmon, W. H. (1992). Building the data warehouse. Wiley.

Kimball, R., & Ross, M. (2013). The data warehouse toolkit: The definitive guide to dimensional modeling (3.ª ed.). Wiley.

