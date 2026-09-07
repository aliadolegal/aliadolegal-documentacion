[← modelo de grafos](./modelo-de-grafos.md#nodos-de-catálogo)

# NivelEstudio - StudyLevelEntity

El nodo **`NivelEstudio`** representa los niveles académicos que forman parte del historial educativo de un profesional. Es un **catálogo inmutable** que establece la clasificación normalizada de los grados de estudio y proporciona los valores utilizados para determinar su peso y posición dentro de la jerarquía académica.

* **Normalización de Grados Académicos:**
  Estandariza las categorías de formación académica mediante un identificador y un nombre de nivel de estudio, evitando la fragmentación de información por diferentes representaciones de un mismo grado. El catálogo contempla niveles como `Licenciatura`, `Maestría` y `Doctorado`, entre otros.

* **Ponderación del Mérito Académico:**
  El atributo `peso` establece el valor asociado a cada nivel de estudio para los procesos de evaluación y cálculo del mérito académico. Al estar definido en un nodo de catálogo, el peso correspondiente a cada grado constituye un criterio común para la evaluación de los diferentes niveles de formación.

* **Control de Jerarquía Académica:**
  La propiedad `orden` establece la posición relativa de cada nivel dentro de la estructura académica. Este valor permite representar la progresión entre los diferentes niveles de formación y establecer la secuencia jerárquica utilizada por los procesos que evalúan la trayectoria académica.

## Lógica de Conectividad

El nodo **`NivelEstudio`** funciona como un nodo de catálogo que proporciona la clasificación normalizada de los niveles académicos utilizados por las entidades de formación profesional. Su identificador permite establecer una referencia común al nivel de estudio correspondiente, mientras que los atributos `peso` y `orden` proporcionan los criterios necesarios para la evaluación y organización de la trayectoria académica.

### Catálogo: NivelEstudio (Configuración de Pesos)

| Nivel de Estudio | Weight (Peso) | Order (Orden) | Impacto en Algoritmo                               |
| ---------------- | ------------: | ------------: | -------------------------------------------------- |
| **Licenciatura** |      **0.80** |             1 | Valor base de acreditación profesional.            |
| **Maestría**     |     **0.90** |             2 | Incremento del 10% por especialización avanzada. |
| **Doctorado**    |       **1.0** |             3 | Valor máximo de excelencia y autoridad técnica.    |


![](images/NivelEstudio-StudyLevelEntity-tb1.png)
![](./images/NivelEstudio-StudyLevelEntity-node.png)
![](./images/NivelEstudio-StudyLevelEntity-node1.png)
