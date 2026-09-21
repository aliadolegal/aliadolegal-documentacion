[← modelo de grafos](https://www.google.com/search?q=./modelo-de-grafos.md%23nodos-de-cat%C3%A1logo)

# Nivel de Mérito - MeritLevelEntity

El nodo `NivelMerito` representa el catálogo que define la posición y el estatus de excelencia profesional (**Junior, Avanzado, Especialista y Experto**) que un abogado posee de forma específica por cada especialidad que ejerce.

* **Ponderación por Especialidad:**
A diferencia de una evaluación general, este nodo permite que el profesional obtenga un nivel de mérito granular y diferenciado para cada rama del derecho que domina, reflejando con precisión su grado de especialización real.
* **Factor Multiplicador de Búsqueda:**
Almacena un factor numérico de ponderación (`multiplicador`) con valores que van de 0.0 a 1.0. Su función técnica es incidir directamente en el posicionamiento y la jerarquización del abogado dentro de los resultados de búsqueda de la plataforma.
* **Identidad Visual y Visibilidad:**
Integra propiedades de presentación y control como el código de color hexadecimal (`colorHex`) para el renderizado de insignias (*badges*) en el frontend y una bandera de visibilidad (`isSearchable`) que determina si los profesionales de este nivel aparecen activos en consultas públicas.

## Lógica de Conectividad

En la arquitectura del sistema, el nodo `NivelMerito` actúa como un parámetro de clasificación cualitativa y cuantitativa vinculado a la práctica especializada del prestador, proveyendo los coeficientes de mérito necesarios para los algoritmos de recomendación y despliegue visual.