[← modelo de grafos](./modelo-de-grafos.md#nodos-de-catálogo)

### Institucion - InstitutionEntity

El nodo **`Institucion`** representa una organización académica o profesional relacionada con la formación o acreditación de los prestadores de servicios legales dentro del grafo. Permite identificar la institución asociada a los antecedentes académicos o profesionales registrados y contextualizarla dentro de la estructura territorial.

* **Identificación Institucional:**
  Su función es representar de manera normalizada a las instituciones mediante un identificador y la información correspondiente a su registro. Esto permite que una misma institución pueda ser referenciada desde diferentes entidades del grafo sin duplicar su información.

* **Clasificación y Contexto Institucional:**
  Permite caracterizar la institución mediante atributos propios de su registro, como su naturaleza y la información necesaria para identificarla dentro del modelo. Esta estructura proporciona un contexto común para las relaciones académicas y profesionales que se establecen con otras entidades del grafo.

* **Contextualización Geográfica:**
  La institución puede asociarse con diferentes niveles de la estructura territorial, permitiendo representar la ubicación de sus instalaciones o sedes dentro del modelo geográfico. Esta información permite relacionar la institución con el Estado, Municipio y Asentamiento correspondientes.

## Lógica de Conectividad

En la arquitectura de catálogos, el nodo **`Institucion`** se relaciona con la estructura geográfica mediante las relaciones **`INSTITUCION_UBICADA_EN_ESTADO`**, **`INSTITUCION_UBICADA_EN_MUNICIPIO`** e **`INSTITUCION_UBICADA_EN_ASENTAMIENTO`**.

Estas relaciones permiten contextualizar territorialmente a la institución y establecer diferentes niveles de ubicación dentro del grafo. A partir de esta estructura, la institución puede participar posteriormente en relaciones con las entidades académicas y profesionales que la referencian.
