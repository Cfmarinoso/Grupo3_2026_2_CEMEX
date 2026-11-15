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

### Negocio analizado: Autoconstrucción
La autoconstrucción es la construcción o ampliación de viviendas por parte de las propias familias, por etapas, con un maestro de obra y comprando los materiales poco a poco en ferreterías y depósitos. En este segmento el cemento se vende en bolsas, y la marca suele ser influida por el maestro de obra y el ferretero, además del dueño de la obra.

Para monitorear este segmento, la empresa realiza un censo de obras, es decir: censadores visitan obras en construcción y registran qué marca de cemento se está usando, en qué etapa de obra, con qué volumen y a qué precio. Con estos registros se calcula el share de Cemex frente a todas las demás marcas.

En los datos, la marca propia es "Cemex Cemento". Mientras que las competidoras principales son Mochica, Inka, Holcim, Quisqueya, Qhuna y La Nacional.

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
| id | INT (PK) | Código de la encuesta. |
| visitaID | INT | Código de la visita (sin dimensión propia, usado para agrupar encuestas por visita). |
| tiempoID | INT (FK) | Fecha de la encuesta. |
| oficinaID | INT (FK) | Oficina responsable. |
| ubigeoID | INT (FK) | Ubicación de la obra. |
| obraID | INT (FK) | Obra censada. |
| tipoEstructuraID | INT (FK) | Etapa de la obra. |
| marcaID | INT (FK) | Marca de cemento encontrada. |
| unidadMedidaID | INT (FK) | Unidad del volumen. |
| motivoID | INT (FK) | Motivo de compra. |
| ejecutivoID | INT (FK) | Ejecutivo que registró la encuesta. |
| proveedorID | INT (FK) | Proveedor registrado. |
| competenciaID | INT (FK) | Tipo y marca de competencia. |
| idSociedad | SMALLINT | Sociedad comercial (6012 / 6052). |
| sociedadCli6052 | BIT | Cliente de la sociedad 6052 (sí/no). |
| sociedadCli6012 | BIT | Cliente de la sociedad 6012 (sí/no). |
| tipoVisita | TINYINT | Tipo de visita (1 / 2, por confirmar con negocio). |
| estado | BIT | 1 = activa, 0 = dada de baja. |
| posibilidadReversion | BIT | Posibilidad de reversión registrada (sí/no). |
| flagMotivoCpsaa | BIT | Flag de motivo CPSAA (sí/no). |
| **volumen** | DECIMAL(18,2) | **Medida.** Bolsas de la marca en la obra. |
| **volumenRestante** | DECIMAL(18,2) | **Medida.** Volumen restante de la obra. |
| **precio** | DECIMAL(18,2) | **Medida.** Precio por bolsa (S/). |
| fechaIngreso | DATE | Fecha de ingreso de la encuesta. |
| fechaCreacion | DATETIME2 | Fecha y hora de creación. |
| fechaModificacion | DATETIME2 | Fecha y hora de la última modificación. |
| checkin | DATETIME2 | Hora de llegada del censador. |
| checkout | DATETIME2 | Hora de salida del censador. |
| duracionMin | calculada | Minutos entre check-in y check-out. |
| checkinLatitud / checkinLongitud | DECIMAL(10,7) | Coordenadas del check-in. |
| checkoutLatitud / checkoutLongitud | DECIMAL(10,7) | Coordenadas del check-out. |
| acciones | NVARCHAR(500) | Acciones registradas. |
| observacion | NVARCHAR(500) | Observaciones del censador. |

#### Dimensión `Tiempo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Fecha en formato YYYYMMDD. |
| fecha | DATE | Fecha calendario. |
| anio | SMALLINT | Año. |
| trimestre | TINYINT | Trimestre (1–4). |
| numMes | TINYINT | Número del mes (1–12). |
| mes | NVARCHAR(12) | Nombre del mes. |
| anioMes | INT | Año y mes (YYYYMM). |
| dia | TINYINT | Día del mes. |
| semanaISO | TINYINT | Semana ISO del año. |
| diaSemana | TINYINT | Día de la semana (1 = lunes … 7 = domingo). |
| nombreDia | NVARCHAR(12) | Nombre del día. |
| esFinDeSemana | BIT | 1 si es sábado o domingo. |

#### Dimensión `Oficina`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de la oficina en origen. |
| nombre | NVARCHAR(100) | Nombre de la oficina (ej. Trujillo). |
| idSociedad | INT | Sociedad de la oficina. |

#### Dimensión `Ubigeo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | CHAR(6) | Ubigeo INEI (departamento + provincia + distrito). |
| codDepartamento / codProvincia / codDistrito | INT | Código de cada nivel geográfico. |
| departamento | NVARCHAR(100) | Departamento. |
| provincia | NVARCHAR(100) | Provincia. |
| distrito | NVARCHAR(100) | Distrito. |

#### Dimensión `Obra`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | BIGINT | Código de la obra. |
| idMaestroObra | INT | Maestro de obra (el de la encuesta más reciente). |
| tipoEjecucion | NVARCHAR(50) | Tipo de ejecución (ej. Remodelación). |

#### Dimensión `TipoEstructura`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de la etapa en origen. |
| nombre | NVARCHAR(100) | Etapa de la obra (ej. Muros). |

#### Dimensión `Marca`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de la marca local. |
| tipoMarca | NVARCHAR(100) | Tipo de marca (no se usa en el análisis). |
| codMarca | INT | Código de la marca. |
| marca | NVARCHAR(100) | Nombre de la marca. |
| marcaNormalizada | NVARCHAR(100) | Nombre en mayúsculas (une variantes como "Holcim" y "HOLCIM"). |
| esMarcaPropia | BIT | 1 si es marca de Cemex. |
| codLinea / linea | INT / NVARCHAR(100) | Código y nombre de la línea. |
| codSublinea / sublinea | INT / NVARCHAR(100) | Código y nombre de la sublínea. |
| codProducto / producto | INT / NVARCHAR(100) | Código y nombre del producto (ej. Cemento). |

#### Dimensión `UnidadMedida`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código de la unidad en origen. |
| abreviatura | NVARCHAR(20) | Abreviatura (BLS = bolsas). |

#### Dimensión `Motivo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código del motivo en origen. |
| nombre | NVARCHAR(100) | Motivo de compra (ej. Precio, Afinidad, Gestión). |

#### Dimensión `Ejecutivo`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | INT | Código del ejecutivo que creó la encuesta. |
| ejecutivoSAP | INT | Código del ejecutivo en SAP. |
| nombre | NVARCHAR(150) | Nombre del ejecutivo. |

#### Dimensión `Proveedor`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta. |
| codigo | BIGINT | Código del proveedor (12 dígitos). |
| nombre | NVARCHAR(200) | Nombre del proveedor (no viene en el dataset). |

#### Dimensión `Competencia`

| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| id | INT (PK) | Llave sustituta (−1 = no aplica). |
| codTipoCompetencia | INT | Código del tipo de competencia (0 = no aplica). |
| tipoCompetencia | NVARCHAR(100) | Tipo de competencia. |
| codMarcaCemento | INT | Código de la marca competidora (0 = no aplica). |
| marcaCemento | NVARCHAR(100) | Marca competidora. |


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
| Dash + Plotly (y sqlglot) | Dashboard interactivo con filtros, mapas y gráficos.
| Railway | Despliegue de la aplicación. Se eligió porque es una herramienta gratuita que nos permite tener un **link público**. |

### 4.2 Definición de indicadores

**Contexto:** un censador visita una obra (vivienda en construcción) y registra la marca de cemento y las bolsas que usa. Marca propia = Cemex; competencia = las demás marcas. **pp** = puntos porcentuales.

| Indicador | Qué mide | Cálculo |
| :--- | :--- | :--- |
| **Share de volumen** | De cada 100 bolsas vistas, cuántas son de Cemex. Es el indicador principal. | `bolsas Cemex / bolsas totales × 100` |
| **Media móvil 3M** | El share suavizado, para que un mes atípico no distorsione la tendencia. | Share sobre los últimos 3 meses |
| **Meta y brecha** | La meta es el valor de referencia (por defecto, el share del mismo periodo del año anterior). La brecha es la distancia a esa meta. | `brecha = share − meta` (pp) |
| **Penetración en obras** | En qué % de las obras aparece Cemex, sin importar el volumen. | `obras con Cemex / obras censadas × 100` |
| **Índice de precio** | Cuánto más caro o barato vende Cemex que el resto (100 = igual). | `precio Cemex / precio competencia × 100` |
| **Bolsas por recuperar** | Bolsas que habría que volver a ganar para llegar a la meta o al share anterior. | Se calcula por segmento |
| **Efecto tasa y mezcla** | Divide el cambio total en dos: *tasa* (el share cayó dentro de una oficina) y *mezcla* (cambió el peso de cada oficina en los datos). | `tasa + mezcla = cambio total` |
| **Semáforo** | Estado del indicador según su brecha. Una oficina está "en alerta" si su share cae 3 pp o más. | En meta: ≥ −1 pp · Alerta: −1 a −3 pp · Crítico: < −3 pp |

### 4.3 Estructura del dashboard
**Filtros globales:** periodo, comparación (periodo anterior o año anterior), meta, departamento, oficina, etapa de obra y tipo de ejecución.

| Pestaña | Qué representa |
| :--- | :--- |
| **1. Resumen y KPIs** | Estado actual y evolución del share, con semáforos. |
| **2. ¿Dónde y cuándo cae?** | La caída por oficina, distrito y mes (barras, mapa y mapa de calor). |
| **3. Competencia y precio** | Qué marcas ganan share y si el precio influye. |
| **4. Propuesta** | Priorización por oficina y simulador de acciones. |
| **5. Consultas SQL** | Consola para consultar el datamart y descargar resultados. |

### 4.4 Resultados
La participación de mercado (market share) de Cemex en la autoconstrucción del norte del Perú cayó en 7 puntos porcentuales (pp), de 83.1% a 76.2% en un semestre y se ubica 7.5 pp por debajo del mismo periodo del año anterior, por lo que no se trata de estacionalidad: tras su máximo en marzo de 2025 (85.2%), cruzó la meta en junio de 2025, así que la caída es reciente y rápida. Mientras el mercado creció 5.0%, Cemex cayó 3.8% y la competencia creció 48.4%. Trujillo, Piura y Chimbote explican ≈78% de la caída real: Trujillo bajó de 66.7% a 54.2% (−12.5 pp), Chimbote de 61.8% a 53.3% (−8.5 pp) y Piura, aunque solo cayó 5.2 pp, pesa mucho por ser la zona de mayor volumen (912,735 bolsas). Quienes ganan terreno son Mochica (+3.5 pp), Holcim (+3.0 pp) e Inka (+2.5 pp), cada uno en zonas distintas: Inka en Trujillo y Piura, Mochica en Chimbote y Holcim en Cajamarca y Chiclayo. El precio influye, pero no explica por sí solo la caída: Cemex vende 8.4% más caro (S/ 32.63 vs S/ 30.10 por bolsa) y aunque el "precio” es el motivo de compra de la competencia en 61% de los casos registrados, el índice de precio casi no cambió (107 → 108) mientras el share cayó 7 pp, y la correlación por oficina es moderada (r = −0.46, 13 oficinas). Finalmente, estas cifras deben leerse con cautela: de la caída de 7 pp , 4.9  pp corresponden a una caída real y −2.0 pp (29% aproximadamente) provienen de cambios en la cobertura del censo, y el motivo de compra solo está registrado en 45% de los casos.

### 4.5 Propuesta de mejora y factibilidad

A continuación se presentan las acciones propuestas a Cemex para recuperar su participación de mercado.

| # | Qué hacer | Dónde | Plazo | Quién participa | Costo |
| :-- | :--- | :--- | :--- | :--- | :--- |
| **Propuesta 1** | Concentrar el esfuerzo en las oficinas donde más se pierde, con una meta de share para cada mes. | Trujillo, Piura, Chimbote | 0–3 meses | Gerencia comercial, jefes de oficina y vendedores | Medio |
| **Propuesta 2** | Contrarrestar a cada competidor en su zona: visitar obras y ferreterías, y dar beneficios a maestros de obra y ferreteros para que sigan eligiendo Cemex. | Inka en Trujillo y Piura; Mochica en Chimbote; Holcim en Cajamarca y Chiclayo | 1–3 meses | Ejecutivos comerciales | Medio |
| **Propuesta 3** | Dar descuentos o puntos solo donde Cemex es más caro y pierde clientes, y comparar con oficinas sin promoción para saber si funcionó. | Oficinas con precio alto y share en caída | Piloto de 3 meses | Marketing y finanzas | Variable (presupuesto acotado) |
| **Propuesta 4** | Revisar qué hacen distinto las oficinas que sí ganan share y aplicarlo en las demás. | Yurimaguas, Huamachuco, Tumbes | 1 mes | Jefes de oficina y un analista | Bajo |
| **Propuesta 5** | Registrar mejores datos en el censo: por qué punto de venta se compra, qué promoción recibió cada obra y por qué se eligió otra marca (hoy solo se sabe en 45% de los casos; la meta es ≥80%). | Todas las oficinas | 1–2 meses | Equipo de TI y analista de datos | Bajo |
| **Propuesta 6** | Revisar cada mes el dashboard con semáforos y que la gerencia fije la meta de share. | Gerencia comercial | 1 mes (ya existe) | Analista BI | Bajo |

**¿Cuánto se podría recuperar?** El dashboard incluye un simulador. Si se recupera la mitad de lo perdido en las 5 oficinas prioritarias (Trujillo, Piura, Chimbote, Chiclayo y Cajamarca), el share sube **2.3 puntos** (≈82,911 bolsas por semestre). 



## 5. Conclusiones
Con base en lo descubierto en los resultados con las herramientas de BI, presentamos las siguientes conclusiones: 

1. **El share cae rápido y es reciente:** de 83.1% a 76.2% en un semestre, con quiebre en junio 2025, mientras el mercado crece 5.0%.
2. **La competencia absorbe el crecimiento:** Mochica, Inka y Holcim crecen en zonas distintas, por lo que una estrategia única sería poco eficaz.
3. **El problema está concentrado:** tres oficinas (Trujillo, Piura y Chimbote) explican ≈78% de la caída real.
4. **Parte de la caída es de medición:** 29% del cambio (−2.0 pp) viene de la cobertura del censo, y el motivo de compra solo está registrado en 45% de los casos.
5. **La solución de BI cumple su objetivo:** el datamart propuesto (1 tabla de hechos y 11 dimensiones) y el dashboard de 5 pestañas permiten ubicar dónde, cuándo y frente a quién cae el share, simular acciones y monitorear 8 indicadores con semáforo.


## 6. Recomendaciones
1. **Priorizar Trujillo, Piura y Chimbote** con metas mensuales y seguimiento semanal.
2. **Responder con campañas por competidor y por zona**, no con una campaña nacional.
3. **No usar un descuento general:** probar promociones focalizadas con grupo de control.
4. **Aprender de las oficinas que suben** tales como Yurimaguas, Huamachuco y Tumbes.
5. **Definir una meta de share con la gerencia** y revisar el tablero cada mes.
6. **Trabajo futuro:** incluir otras líneas (concreto, bloques y adoquines), agregar datos de despachos facturados y ampliar el rango de años.

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

