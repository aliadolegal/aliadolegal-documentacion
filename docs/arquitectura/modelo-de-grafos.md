[← Volver a página principal](./../../README.md%23estructura-de-la-documentaci%C3%B3n-t%C3%A9cnica)

# Modelo de Grafos de Neo4j: Arquitectura de Datos de AliadoLegal

Este documento detalla el modelo de datos basado en grafos implementado en **Neo4j** para la plataforma **AliadoLegal**, estructurando las relaciones centrales entre los perfiles profesionales, la geolocalización territorial, las especialidades jurídicas, los órganos jurisdiccionales y los expedientes de control.

---

## 1. Visión General del Modelo

El modelo de grafos está diseñado para reflejar con precisión el dominio legal en México, permitiendo un rendimiento óptimo en consultas de correlación (como el *matching* entre ciudadanos y abogados). El nodo central del ecosistema es **`Abogado`**, a partir del cual se ramifican las relaciones hacia dimensiones clave:

* **Ámbito Geográfico:** Entidades, Municipios, Ciudades, Asentamientos y Tipos de Asentamiento.
* **Ámbito Profesional y Académico:** Especialidades de Derecho, Experiencia, Cédulas, Formación, Reputación y Validación.
* **Ámbito Jurisdiccional:** Sedes de Justicia, Órganos Jurisdiccionales, Fueros y Casos Tipo.
* **Ámbito de Control y Progreso:** Perfiles de avance, métricas de excelencia y años de experiencia.

---

## 2. Estructura de Nodos y Relaciones Principales

### A. Nodo Central y Perfil

* **`Abogado` (Nodo Rosa / Central):** Representa al profesional del derecho registrado en la plataforma.
* *Relaciones salientes principales:*
* `TIENE_PERFIL` $\rightarrow$ `AbogadoPerfilProfesional`
* `TIENE_VALIDACION` $\rightarrow$ `AbogadoValida`
* `TIENE_REPUTACION` $\rightarrow$ `AbogadoReputacion`
* `TIENE_FORMACION` $\rightarrow$ `AbogadoFormacion`
* `TIENE_EXCELENCIA` $\rightarrow$ `AbogadoExcelencia`
* `TIENE_RANGO_EXPERIENCIA` $\rightarrow$ `AniosEjerciendo`
* `TRABAJO_EN` $\rightarrow$ `AbogadoExperiencia`





### B. Cobertura Geográfica y Asentamientos

* **`Abogado`** se conecta con la estructura territorial mediante relaciones de servicio y pertenencia:
* `OFRECE_SERVICIO_EN_ENTIDAD` $\rightarrow$ `Entidad`
* `OFRECE_SERVICIO_EN_MUNICIPIO` $\rightarrow$ `Municipio`
* `OFRECE_SERVICIO_EN_CIUDAD` $\rightarrow$ `Ciudad`
* `OFRECE_SERVICIO_EN_ASENTAMIENTO` $\rightarrow$ `Asentamiento`


* **Jerarquía Territorial:**
* `Municipio` $\leftarrow$ `SEDE_PERTENECE_A_MUNICIPIO` $\leftarrow$ `SedeJusticia`
* `Asentamiento` $\rightarrow$ `ASENTAMIENTO_CONTIENE_TIPO` $\rightarrow$ `TipoAsentamiento`
* `Asentamiento` $\leftrightarrow$ `CERCANO_A` (Relación reflexiva para proximidad geográfica).



### C. Especialidades y Materias Jurídicas

* **`AbogadoExperiencia`** / **`EspecialidadDerecho`**:
* `ESPECIALIDAD_PERTENECE_A_MATERIA` $\rightarrow$ `MateriaDerecho`
* `ES_ESPECIALIDAD` $\rightarrow$ `SedeJusticia`
* `ORGANO_UBICATED_EN_SEDE` $\rightarrow$ `OrganoJurisdiccional`



### D. Órganos Jurisdiccionales y Fueros

* **`OrganoJurisdiccional`**:
* `RADICADO_EN_ORGANO` $\rightarrow$ `Fuero`
* `TIENE_MATERIA` $\rightarrow$ `MateriaDerecho`



---

## 3. Representación Gráfica del Grafo

---

## 4. Resumen Operativo de Entidades del Grafo

| Entidad / Nodo | Color Base | Rol Principal en el Negocio |
| --- | --- | --- |
| **Abogado** | Rosa | Nodo raíz que centraliza toda la actividad, credenciales y relaciones del profesional. |
| **Entidad / Municipio / Ciudad / Asentamiento** | Beige / Canela | Jerarquía geoespacial que define la cobertura exacta donde el abogado puede ofrecer sus servicios. |
| **EspecialidadDerecho / MateriaDerecho** | Naranja / Verde | Clasificación taxonómica de las ramas del derecho que domina el profesional. |
| **SedeJusticia / OrganoJurisdiccional** | Naranja / Amarillo | Mapeo de tribunales, juzgados y fueros vinculados a la práctica legal. |
| **AbogadoFormacion / AbogadoValida / AbogadoReputacion** | Marrón | Módulos satélite que resguardan el historial académico, estatus de validación y métricas de confianza. |

---

![esquema de grafos](./images/esquema-de-grafos.png)

---

## 5. Nodos de catálogo
* [StateEntity-StateEntity:](Entidad-StateEntity.md)
El nodo Entidad constituye el ancla regional y el nivel superior de la jerarquía geográfica en el grafo. Es el contenedor principal que organiza la distribución territorial de los datos.
* [Municipio - MunicipalityEntity:](Municipio-MunicipalityEntity.md)
El nodo Municipio representa la división política y administrativa intermedia dentro del grafo de AliadoLegal. Su función es servir de puente organizativo entre la Entidad Federativa y las unidades territoriales específicas.

