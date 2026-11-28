# Modelo de datos de ubicación
El siguiente formato de texto representa los datos de ubicación almacenados de manera no estructurada (NoSQL), correspondientes a las capturas de ubicación GPS del dispositivo móvil del repartidor.

## Enfoques disponibles
Las bases de datos NoSQL manejan diferentes enfoques para organizar los datos:
### Documental
Es el más similar al enfoque estructurado, se manejan documentos JSON, XML o de otros tipos como los registros o filas de una tabla. Los documentos se agrupan en colecciones, que serían las tablas de la base de datos.

### Par clave-valor
Como indica su nombre, guardan datos importantes y puntuales en un par clave-valor que permite recuperar la información rápidamente.

### Basadas en columnas
Almacenan y leen datos en columnas en lugar de por filas, siendo usados ampliamente para el análisis de datos ya que se pueden aplicar varias operaciones de cálculo y procesamiento por columna.

### Basadas en grafos
Estas bases de datos almacenan información en forma de nodos, donde cada registro es representado por un nodo. Los nodos pueden conectarse entre sí para crear asociaciones de datos y ser consultados.

## Enfoque escogido
El enfoque escogido fue el de una base de datos **documental** con documentos de tipo JSON, ya que se aproxima al manejo de las bases de datos estructuradas, lo que facilita trabajar con un modelo de datos con atributos definidos, agrupar datos por colecciones y procesar en un formato ligero (JSON).

Las demás bases de datos se rechazaron por los siguientes motivos:
|**Enfoque**|**Motivo de rechazo**|
|:-:|-|
|**Par clave-valor**| Este enfoque no es viable para almacenar un gran volumen de datos de ubicación para construir las trazas, ya que solo maneja un atributo y un valor a la vez, hacer esto por cada punto de ubicación es imposible sin evitar repetir el nombre de un atributo.|
|**Basado en columnas**|El guardado y lectura en columnas hacen imposible el correcto procesamiento de la información geográfica que requiere procesar a la vez longitud, latitud y el momento de captura para crear el recorrido en el mapa.|
|**Basado en grafos**| Al igual que con el par-clave valor, este enfoque no es viable para almacenar un gran volumen de datos similares, ya que se centra más en relacionar nodos de información para crear nuevos datos y ejecutar consultas compuestas|

## Colecciones 
Se usarán 2 colecciones en base a lo definido en el modelo de datos general. Las entidades `Traza` y `Ubicación` pasan a ser colecciones de documentos.

### Ubicación
Los documentos en ubicación representan un punto de ubicación GPS obtenida del dispositivo móvil. Tendrán los siguientes atributos y formato:

**Ejemplo**
```JSON
{
      "_id": "b4471987-0f1c-4c3a-87b7-bd1b02b2c73a", UUID del documento
      "punto": { Punto de ubicación
        "type": "Point", Tipo de punto
        "coordinates": [-78.4678, -0.1807] Longitud y latitud
      },
      "capturada": "2026-11-27T00:00:00Z", Timestamp de captura
      "precision_metros": 100 Precisión de la ubciación obtenida en metros
}
```

### Trazas
Los documentos en trazas contienen la información agrupada de los puntos de ese recorrido (entrega) para reconstruir la trayectoria y mostrarla en el mapa. Tendrán los siguientes atributos y formato:

**Ejemplo**
```JSON
{
  "_id": "328r90r92302039", UUID del documento
  "recorrido_ref": "jkvokopefkwepofkwopefkwf", Id del recorrido asociado
  "estado": "activa", Estado de la traza [activa(en vivo)|inactiva]
  "geometria_calculada": { Calculo de la geometria del recorrido
    "type": "LineString",
    "coordinates": [ Lista de puntos
      [-78.4678, -0.1807],
      [-78.4669, -0.1798]
    ]
  },
  "ultima_ubicacion": Punto de la última ubicacion
  {"_id": "b4471987-0f1c-4c3a-87b7-bd1b02b2c73a",
      "punto": { 
        "type": "Point", 
        "coordinates": [-78.4678, -0.1807] 
      },
      "capturada": "2026-11-27T00:00:00Z",
      "precision_metros": 100},
  "actualizada_en": "2026-11-27T22:30:01Z" Última actializacion de la traza
}
```