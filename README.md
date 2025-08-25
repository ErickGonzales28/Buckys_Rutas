# Proyecto de Clusterización y Visualización Geoespacial 

Este proyecto tiene por objetivo segmentar puntos de interés (puntos de venta minoristas) y generar rutas óptimas de distribución.  
La información se presenta en un mapa interactivo accesible mediante **GitHub Pages**, permitiendo explorar resultados de forma visual y práctica.  

---

## Exploración de los datos
El dataset inicial contiene **puntos georreferenciados** (latitud y longitud).  
Antes de la clusterización se realizó un análisis exploratorio para:
- Identificar la densidad de puntos por distrito, provincia y región.  
- Detectar **puntos aislados** que podrían distorsionar las rutas.  
- Visualizar la distribución de datos en mapas base para validar la cobertura.

Se usaron librerías como **GeoPandas y Matplotlib** para la visualización inicial.

---

## Preprocesamiento 
Para garantizar una buena calidad en la clusterización se aplicaron transformaciones clave:
- Conversión de coordenadas geográficas (WGS84) a métricas (proyección en metros).  
- Definición de **islas geográficas**: agrupaciones previas de puntos según polígonos de referencia.  
- Filtrado de clusters demasiado pequeños para evitar rutas poco prácticas.  
- Separación de **puntos alejados** y asignación a clusters propios o subgrupos.  

---

## Clusterización
Se utilizaron diferentes técnicas de clusterización en función del número de registros:

- **DBSCAN**:  
  - Agrupa puntos cercanos por densidad.  
  - Ideal para datasets con más de 120 puntos.  
  - Respeta barreras naturales como calles o avenidas principales.  

- **KMeans**:  
  - Se aplica en escenarios con pocos registros (< 120 puntos).  
  - También se aplica para **subdividir clusters demasiado grandes**, garantizando un tamaño balanceado.  

- Reglas de negocio:  
  - Cada cluster debe contener entre **40 y 80 puntos** (ajustable vía `config.json`).  
  - Clusters menores al umbral se eliminan o fusionan con otros.  
  - Los puntos aislados se convierten en clusters individuales.  

---

## Visualización de resultados
Los resultados se proyectan en un **mapa interactivo** con Leaflet.js y OpenStreetMap.  
Cada cluster se representa como una ruta con un color diferente, mostrando:
- Distribución espacial de rutas.  
- Tamaño de cada cluster.  
- Prioridad según distancia al punto de referencia (ej. la sede principal).  

Esto permite validar fácilmente si la segmentación se ajusta a la logística real.

---

## Beneficios 
- **Rutas balanceadas**: entre 40 y 80 puntos por cluster.  
- **Reducción de costos logísticos**: rutas más eficientes reducen tiempos y distancias.  
- **Flexibilidad**: parámetros configurables en `config.json` (prefijo, fecha, eps, tamaños, rutas máximas, etc.).  
- **Escalabilidad**: capaz de procesar desde unos pocos registros hasta miles de puntos.  
- **Accesibilidad**: resultados exportados en **CSV y GeoJSON**, además de visualización web.  

---

## Herramientas utilizadas 
- **Python**: procesamiento y clusterización.  
- **Scikit-learn**: DBSCAN, KMeans.  
- **GeoPandas & Shapely**: análisis geoespacial.  
- **Matplotlib**: gráficos exploratorios.  
- **Leaflet.js**: mapa interactivo.  
- **OpenStreetMap**: base cartográfica.  

Esto permite una **segmentación eficiente y práctica**, con rutas priorizadas según proximidad al punto de referencia (sede/distribuidor).  

---

## Descripción de los clusters
- **Clusters grandes** → se dividen con KMeans en subclusters balanceados.  
- **Clusters pequeños** → se fusionan o eliminan.  
- **Puntos aislados** → se convierten en clusters individuales, los cuales seran eliminados si no cumplen con el mínimo requerido.  

De esta forma se asegura una distribución de rutas práctica y cercana a la realidad logística

