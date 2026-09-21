[← Volver a página principal](../README.md)

---
# Neo4j

**Neo4j** es un sistema de gestión de bases de datos orientado a grafos (*Graph Database Management System, GDBMS*), diseñado para almacenar, consultar y recorrer información en la que las **relaciones entre los datos constituyen una parte fundamental del modelo de información**.

A diferencia de una base de datos relacional, donde la información se organiza principalmente mediante tablas, filas y relaciones entre tablas, Neo4j utiliza un **modelo de grafo de propiedades (*Property Graph*)**, en el que las entidades y sus relaciones se representan directamente como elementos del grafo.

## Modelo de datos

El modelo de Neo4j está compuesto principalmente por:

* **Nodos (*Nodes*)**: representan entidades, objetos o elementos del dominio.
* **Relaciones (*Relationships*)**: representan conexiones dirigidas entre nodos y pueden expresar el tipo de relación existente.
* **Propiedades (*Properties*)**: contienen atributos asociados tanto a los nodos como a las relaciones.
* **Etiquetas (*Labels*)**: permiten clasificar los nodos según su tipo o categoría.

Por ejemplo, una estructura sencilla puede representarse como:

```text
(Persona)-[:TRABAJA_EN]->(Empresa)
```

En esta representación:

* `Persona` es un nodo.
* `Empresa` es un nodo.
* `TRABAJA_EN` es una relación.
* Tanto los nodos como la relación pueden contener propiedades.

Por ejemplo:

```text
(Persona {
    nombre: "Juan"
})-[:TRABAJA_EN {
    desde: 2020
}]->(Empresa {
    nombre: "Empresa A"
})
```

Esto permite representar directamente tanto los elementos de información como las relaciones que existen entre ellos.

## Características principales

### Modelo orientado a relaciones

Las relaciones forman parte explícita del modelo de datos. Esto permite representar estructuras en las que un elemento puede estar conectado con múltiples elementos mediante diferentes tipos de relaciones.

### Navegación sobre el grafo

Las consultas pueden recorrer las relaciones existentes entre los nodos para encontrar patrones y estructuras conectadas.

Esto resulta especialmente útil cuando las consultas dependen de **cómo están relacionados los datos**, además de los atributos individuales de cada entidad.

### Propiedades en nodos y relaciones

Neo4j permite almacenar atributos tanto en los nodos como en las relaciones.

Esto permite que una relación no sea únicamente una conexión, sino que también pueda contener información propia.

### Etiquetas y tipos de relación

Las etiquetas permiten clasificar nodos, mientras que los tipos de relación permiten expresar semánticamente la conexión entre ellos.

Por ejemplo:

```text
(Persona)-[:PERTENECE_A]->(Organizacion)
(Persona)-[:TRABAJA_EN]->(Empresa)
(Persona)-[:ESTUDIO_EN]->(Universidad)
```

Cada relación representa una conexión diferente dentro del modelo.

## Lenguaje de consulta

Neo4j utiliza **Cypher** como lenguaje declarativo para consultar y modificar el grafo.

Cypher permite expresar patrones de nodos y relaciones de una forma cercana a la representación visual del grafo.

Por ejemplo:

```cypher
MATCH (p:Persona)-[:TRABAJA_EN]->(e:Empresa)
RETURN p, e;
```

Esta consulta busca personas relacionadas con empresas mediante una relación `TRABAJA_EN`.

También es posible utilizar patrones más complejos que involucren múltiples niveles de relaciones.

## Grafo de propiedades

El modelo utilizado por Neo4j puede representarse conceptualmente como:

```text
        ┌──────────────┐
        │    Nodo      │
        │  propiedades │
        └──────┬───────┘
               │
        RELACIÓN
        propiedades
               │
               ▼
        ┌──────────────┐
        │    Nodo      │
        │  propiedades │
        └──────────────┘
```

La característica importante es que **la relación es un elemento explícito del modelo**, y no únicamente una asociación implícita entre registros.

## Neo4j dentro de las tecnologías de datos

Neo4j pertenece a la categoría de tecnologías **NoSQL orientadas a grafos**.

Su principal característica no consiste únicamente en almacenar grandes cantidades de información, sino en proporcionar un modelo especializado para representar y consultar **estructuras altamente conectadas**.

Por esta razón, resulta apropiado para dominios en los que las relaciones entre entidades tienen relevancia estructural y deben poder recorrerse, consultarse y analizarse como parte de la información.

## Resumen

| Característica        | Neo4j                                                    |
| --------------------- | -------------------------------------------------------- |
| Tipo de tecnología    | Sistema de gestión de bases de datos                     |
| Modelo                | Grafo de propiedades (*Property Graph*)                  |
| Categoría             | NoSQL orientada a grafos                                 |
| Elementos principales | Nodos, relaciones, propiedades y etiquetas               |
| Lenguaje de consulta  | Cypher                                                   |
| Representación        | Entidades y relaciones explícitas                        |
| Uso característico    | Información altamente relacionada y navegación de grafos |

Neo4j proporciona, por tanto, una tecnología especializada para **modelar, almacenar y consultar información estructurada como un grafo**, haciendo que las relaciones entre las entidades formen parte directa del modelo de datos.

---

[← Volver a página principal](../README.md)
