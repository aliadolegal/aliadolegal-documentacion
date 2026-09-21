[← Volver a página principal](../README.md)

---
# Elasticsearch

**Elasticsearch** es un motor distribuido de búsqueda y análisis de datos, diseñado para almacenar, indexar y consultar grandes volúmenes de información de manera rápida y flexible.

Está construido sobre **Apache Lucene** y utiliza estructuras de índices invertidos y otros mecanismos de indexación para realizar búsquedas eficientes sobre información estructurada y no estructurada.

A diferencia de una base de datos relacional tradicional, cuyo objetivo principal es gestionar datos transaccionales y mantener la consistencia de las operaciones, Elasticsearch está especializado en **búsqueda, recuperación y análisis de información**.

## Modelo de información

Elasticsearch organiza la información mediante:

* **Índices (*Indexes*)**: contienen una colección de documentos relacionados.
* **Documentos (*Documents*)**: representan unidades individuales de información y se almacenan en formato JSON.
* **Campos (*Fields*)**: representan los atributos contenidos dentro de cada documento.
* **Mappings**: definen cómo se interpretan e indexan los campos de los documentos.

Una representación sencilla puede ser:

```json
{
  "id": 123,
  "nombre": "Ejemplo",
  "categoria": "Tecnología",
  "descripcion": "Información de ejemplo"
}
```

Este documento puede almacenarse dentro de un índice y sus campos pueden ser configurados para diferentes tipos de búsqueda y análisis.

## Indexación

La **indexación** es uno de los elementos fundamentales de Elasticsearch.

Cuando un documento es incorporado a un índice, Elasticsearch procesa sus campos de acuerdo con el *mapping* correspondiente y genera las estructuras necesarias para realizar búsquedas eficientes.

Dependiendo del tipo de campo, puede utilizar diferentes mecanismos de indexación.

Por ejemplo, un campo de texto puede analizarse para permitir búsquedas por términos, mientras que un campo numérico puede utilizarse para filtros y operaciones de comparación.

## Búsqueda

Elasticsearch permite realizar diferentes tipos de consultas sobre los documentos indexados.

Entre ellas se encuentran:

* Búsqueda por términos.
* Búsqueda de texto completo (*full-text search*).
* Búsqueda mediante coincidencias parciales.
* Consultas booleanas.
* Filtros por campos.
* Consultas por rangos.
* Ordenamiento y relevancia.
* Agregaciones para análisis de información.

Una consulta sencilla puede expresarse mediante su API REST:

```json
{
  "query": {
    "match": {
      "descripcion": "contrato laboral"
    }
  }
}
```

El motor analiza la consulta y determina los documentos que presentan coincidencias de acuerdo con la configuración de sus campos y los criterios de relevancia.

## Búsqueda semántica y vectores

Elasticsearch también puede utilizarse para implementar mecanismos de **búsqueda semántica** mediante representaciones vectoriales (*embeddings*).

Un documento puede contener un vector que representa semánticamente su contenido:

```json
{
  "texto": "El trabajador fue despedido después de reclamar el pago de horas extra",
  "embedding": [0.021, -0.184, 0.073, "..."]
}
```

Estos vectores permiten realizar búsquedas basadas en **similitud semántica**, en lugar de depender exclusivamente de la coincidencia literal de palabras.

Este mecanismo permite combinar diferentes estrategias de búsqueda, por ejemplo:

* coincidencia textual;
* filtros estructurados;
* relevancia;
* similitud vectorial;
* y reglas específicas de consulta.

## Mappings y tipos de datos

El *mapping* determina cómo Elasticsearch interpreta los campos de un documento.

Entre los tipos utilizados se encuentran:

* `text`: contenido destinado principalmente a búsquedas de texto completo.
* `keyword`: valores que deben conservarse como términos exactos.
* `integer`, `long`, `float`, `double`: valores numéricos.
* `boolean`: valores verdadero/falso.
* `date`: fechas y horas.
* `geo_point`: coordenadas geográficas.
* `dense_vector`: representaciones vectoriales utilizadas, entre otros escenarios, para búsqueda por similitud.

Por ejemplo:

```json
{
  "mappings": {
    "properties": {
      "nombre": {
        "type": "text"
      },
      "categoria": {
        "type": "keyword"
      },
      "fecha": {
        "type": "date"
      }
    }
  }
}
```

## Agregaciones

Además de buscar documentos, Elasticsearch permite realizar **agregaciones**, que permiten obtener información resumida sobre los datos indexados.

Por ejemplo, pueden utilizarse para:

* contar documentos;
* agrupar información por categorías;
* calcular valores estadísticos;
* analizar distribuciones;
* obtener métricas sobre los resultados de búsqueda.

Esto permite utilizar Elasticsearch no solamente como motor de búsqueda, sino también como herramienta para análisis de información.

## Arquitectura distribuida

Elasticsearch está diseñado para trabajar de manera distribuida.

La información puede distribuirse entre diferentes **nodos** y **shards**, permitiendo manejar grandes volúmenes de documentos y distribuir las operaciones de almacenamiento y consulta.

Los principales elementos de esta arquitectura incluyen:

* **Cluster**: conjunto de nodos que trabajan como una unidad.
* **Node**: instancia de Elasticsearch que participa en el cluster.
* **Index**: conjunto lógico de documentos.
* **Shard**: partición de un índice.
* **Replica**: copia de un shard utilizada principalmente para disponibilidad y capacidad de consulta.

Esta arquitectura permite escalar horizontalmente el almacenamiento y procesamiento de información.

## Elasticsearch como tecnología de búsqueda

Elasticsearch no debe considerarse únicamente como una base de datos utilizada para almacenar información.

Su función principal dentro de una arquitectura es proporcionar **indexación y recuperación eficiente de información**, especialmente cuando se requieren búsquedas complejas, análisis de texto, filtros, relevancia o similitud semántica.

Por esta razón, puede utilizarse como una capa especializada de búsqueda sobre información que también puede existir en otros sistemas de almacenamiento.

## Resumen

| Característica        | Elasticsearch                                    |
| --------------------- | ------------------------------------------------ |
| Tipo de tecnología    | Motor distribuido de búsqueda y análisis         |
| Motor subyacente      | Apache Lucene                                    |
| Modelo de información | Documentos JSON                                  |
| Organización          | Índices y documentos                             |
| Consulta              | Query DSL / API REST                             |
| Búsqueda textual      | Sí                                               |
| Búsqueda semántica    | Sí, mediante vectores                            |
| Análisis              | Agregaciones                                     |
| Arquitectura          | Distribuida                                      |
| Escalabilidad         | Horizontal mediante nodos y shards               |
| Uso característico    | Búsqueda, recuperación y análisis de información |

Elasticsearch proporciona una plataforma especializada para **indexar, buscar, recuperar y analizar información**, permitiendo combinar datos estructurados, texto y representaciones vectoriales dentro de una misma infraestructura de búsqueda.


---

[← Volver a página principal](../README.md)
