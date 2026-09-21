[← modelo de grafos](./modelo-de-grafos.md#nodos-de-catálogo)

# Años de Ejercicio - YearsOfExperienceRangeEntity
El nodo `AniosEjercicio` representa el catálogo que categoriza la antigüedad profesional del abogado en rangos estructurados, permitiendo agrupar su trayectoria para fines de filtrado y ponderación algorítmica.

* **Categorización de Experiencia:**
A diferencia de calcular los años de forma estricta o individual, este nodo agrupa al prestador en bloques estandarizados (como "1-5" o "11-15" años), facilitando la gestión visual y las consultas de rangos operativos dentro de la plataforma.

* **Ponderación para el Mérito Global:**
Almacena un peso estadístico (`peso`) con valores normalizados de 0.0 a 1.0. Su función principal es alimentar el motor de evaluación de excelencia (*merit optimization algorithm academic*), determinando qué tanto influye la antigüedad acumulada en el puntaje global del abogado.

* **Ordenamiento de Interfaz:**
Incluye un parámetro de ordenamiento (`orden`) que garantiza que los selectores y listas desplegables en el frontend presenten los rangos de experiencia de manera cronológica y coherente para el usuario.

## Lógica de Conectividad
En la arquitectura de catálogos y perfiles, el nodo `AniosEjercicio` funciona como un referente de valor paramétrico para los registros de trayectoria del profesional, estableciendo los límites numéricos (`min` y `max`) que validan a qué rango pertenece la experiencia declarada por el abogado.

![](images/AniosEjercicio-YearsOfExperienceRangeEntity-tb.png)
![](./images/AniosEjercicio-YearsOfExperienceRangeEntity-node.png)
![](./images/AniosEjercicio-YearsOfExperienceRangeEntity-node1.png)