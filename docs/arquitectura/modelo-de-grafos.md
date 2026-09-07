[← Volver a página principal](../../README.md%23estructura-de-la-documentaci%C3%B3n-t%C3%A9cnica)

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
El nodo `Entidad` constituye el ancla regional y el nivel superior de la jerarquía geográfica en el grafo. Es el contenedor principal que organiza la distribución territorial de los datos.

* [Municipio - MunicipalityEntity:](Municipio-MunicipalityEntity.md)
El nodo `Municipio` representa la división política y administrativa intermedia dentro del grafo de AliadoLegal. Su función es servir de puente organizativo entre la Entidad Federativa y las unidades territoriales específicas.

* [Ciudad - CityEntity:](Ciudad-CityEntity.md)
El nodo `Ciudad` representa una unidad de agregación urbana o metropolitana dentro del grafo. Su función es agrupar diversos sectores geográficos bajo una identidad de mancha urbana común.

* [Asentamiento - SettlementEntity:](Asentamiento-SettlementEntity.md)
El nodo `Asentamiento` constituye la unidad de información más granular en la jerarquía geográfica del sistema. Representa el punto de contacto final y específico donde se sitúa la necesidad del usuario o la ubicación del despacho profesional.

* [TipoAsentamiento - SettlementTypeEntity:](TipoAsentamiento-SettlementTypeEntity.md)
El nodo `TipoAsentamiento` añade una capa de metadatos a la ubicación física. Mientras que el nodo Asentamiento nos dice 'dónde' está el abogado, el `TipoAsentamiento` describe el 'contexto socio-urbano' de su zona de influencia. Esta distinción es clave para que el `merit optimization algorithm academic` pueda ponderar la relevancia de un perfil según el entorno donde se solicita el servicio legal.

* [Institucion - InstitutionEntity:](Institucion-InstitutionEntity.md)
El nodo `Institucion` representa a las organizaciones académicas o profesionales encargadas de la formación y certificación de los prestadores de servicios legales en el grafo.

* [NivelEstudio - StudyLevelEntity:](NivelEstudio-StudyLevelEntity.md)
El nodo `NivelEstudio` representa los niveles académicos que forman parte del historial educativo de un profesional.

* [MateriaDerecho - SubjectLawEntity:](MateriaDerecho-SubjectLawEntity.md)
El nodo `MateriaDerecho` representa una categoría de nivel superior dentro de la taxonomía jurídica del sistema. Agrupa las especialidades jurídicas que pertenecen a un mismo ámbito del derecho y proporciona el contexto necesario para organizar, clasificar y relacionar el conocimiento jurídico dentro del grafo.

* [EspecialidadDerecho - LawSpecialtyEntity:](EspecialidadDerecho-LawSpecialtyEntity.md)
El nodo `EspecialidadDerecho` representa un área específica de práctica jurídica dentro de la taxonomía legal del sistema. Define las características que permiten contextualizar una especialidad, relacionarla con una materia de derecho y establecer parámetros para la evaluación de experiencia y mérito profesional.

* [Fuero - JurisdictionEntity:](Fuero-JurisdictionEntity.md)
El nodo `Fuero` representa el ámbito de jurisdicción legal bajo el cual se organiza la actividad de las entidades jurídicas y judiciales dentro del grafo.

* [SedeJusticia - CourtBuildingEntity:](SedeJusticia-CourtBuildingEntity.md)
