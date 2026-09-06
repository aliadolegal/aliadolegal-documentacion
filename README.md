![](./images/logo_aliadolegal.png)

# AliadoLegal

**AliadoLegal** es una plataforma digital de tecnología jurídica (*LegalTech*) desarrollada en México para simplificar y transparentar la manera en que las personas encuentran, evalúan y conectan con abogados especialistas.

---

## Justificación de la creación

En la actualidad, encontrar un abogado que realmente ayude a enfrentar problemas legales es un proceso complicado por diversas razones:

* **La recomendación sigue siendo principalmente de boca en boca:** La mayoría de las personas elige a su abogado por referencias personales —familiares, amigos o conocidos—, un método limitado, subjetivo y poco verificable. Esto deja fuera a muchos profesionales competentes y no garantiza que el abogado recomendado sea el más adecuado para el tipo de caso o necesidad específica.
* **Falta de información confiable y verificable:** Los usuarios carecen de fuentes objetivas para conocer la experiencia real, los resultados o la reputación de los abogados. Las opiniones en línea suelen ser escasas o poco fiables, lo que genera incertidumbre y riesgo al tomar una decisión.
* **Complejidad de la especialización legal:** El derecho es un área amplia y diversa: civil, penal, laboral, fiscal, mercantil, administrativo, entre otros. Elegir un abogado que maneje la especialización exacta requerida para un caso específico es confuso y puede llevar a errores costosos.
* **Dificultad para evaluar ética y desempeño profesional:** Aunque un abogado tenga buenas credenciales, no siempre es posible conocer su comportamiento ético, su compromiso con el cliente o la calidad real de sus resultados. Esto aumenta el riesgo de contratar a alguien que no priorice los intereses del cliente.
* **Barreras de acceso y comunicación:** Muchos clientes no saben cómo acercarse a un abogado de manera efectiva, ni cómo expresar sus necesidades. Además, en ciertas regiones puede ser difícil encontrar profesionales con experiencia en casos específicos o que sean accesibles económicamente.
* **Falta de transparencia en procesos y resultados:** El seguimiento de casos legales puede ser opaco. Los clientes a menudo no tienen claridad sobre el avance de su caso, los costos asociados o las mejores estrategias a seguir, lo que genera desconfianza y frustración.

---

## ¿Cómo AliadoLegal aborda estas dificultades?

La plataforma utiliza tecnología y comunidad para superar estas barreras:

* Opiniones verificadas y transparentes que permiten evaluar la reputación y desempeño de cada abogado.
* Clasificación por especialización y resultados, para que el usuario encuentre al profesional adecuado según su necesidad específica.
* OCR y validación documental para garantizar la veracidad de la información proporcionada por los abogados.
* Herramientas de búsqueda avanzada y análisis de relaciones que facilitan descubrir conexiones útiles entre casos, abogados y especializaciones.
* Enfoque en ética y resultados, premiando a los abogados que se destacan por su calidad profesional y compromiso con el cliente.

---

## Propósito, Misión, Visión y Valores

> **AliadoLegal** democratiza el acceso a la justicia usando inteligencia artificial y automatización para conectar a las personas con abogados confiables, validados por la comunidad.

### Propósito
AliadoLegal democratiza el acceso a la justicia mediante inteligencia artificial, automatización y asistencia legal digital, conectando a las personas con abogados confiables, éticos y con reputación comprobada. Nuestra tecnología transforma la forma en que las personas buscan, comparan y eligen servicios legales, reduciendo las barreras de información y promoviendo decisiones informadas, justas y basadas en la confianza.

### Misión
Empoderar a las personas para que elijan abogados confiables mediante opiniones reales, verificadas y transparentes, apoyadas en herramientas de IA y análisis automatizado que hacen accesible la justicia para todos. Construimos una comunidad donde la ética, la calidad profesional y los resultados se reconocen por mérito, no por publicidad.

### Visión
Transformar la manera en que la sociedad elige abogados, digitalizando la confianza legal y fomentando un entorno donde la reputación se gane con resultados y el reconocimiento provenga de la voz de los usuarios. Ser la plataforma de referencia en México para encontrar abogados destacados por mérito y reputación comprobada, promoviendo una cultura de confianza, transparencia y colaboración entre profesionales del derecho y quienes requieren sus servicios.

### Valores
* **Transparencia**: Opiniones auténticas, sin intereses ocultos.
* **Ética**: Reconocimiento basado en méritos reales, no en publicidad.
* **Confianza**: Validación y verificación de experiencias compartidas. Cada recomendación refleja una experiencia auténtica.
* **Comunidad**: Usuarios y abogados creciendo juntos mediante la retroalimentación.
* **Respeto**: Cuidamos tanto la voz de los usuarios como la reputación de los profesionales.
* **Justicia**: Promovemos la equidad y la transparencia en la elección de servicios legales.
* **Honestidad**: Sin promociones ni favoritismos, solo valoraciones reales.
* **Excelencia profesional**: Impulsamos estándares más altos en el ejercicio legal.
* **Innovación tecnológica**: Usamos IA y automatización para mejorar la experiencia legal y hacerla accesible, clara y humana.

---

## Estructura de la documentación ejecutiva

- **Documentos ejecutivos**
  - **Flujos de datos**
---

## Estructura de la documentación técnica

- Documentos técnicos
  - **Arquitectura**
    - Arquitectura general
    - [`Arquitectura del Modelo de Datos y Grafo (Neo4j)`](docs/tecnico/arquitectura/comunicacion)
    - comunicacion
    - persistencia
    - eventos
  - **Microservicios**
    - aliadolegal-gateway
    - aliadolegal-eureka
    - aliadolegal-security
    - aliadolegal-users
    - [`aliadolegal-lawyers`](docs/tecnico/microservicios/aliadolegal-lawyers/README.md)
    - aliadolegal-clients
    - aliadolegal-catalogs
    - aliadolegal-specialty-intelligence
    - aliadolegal-hfmoderator
  - **Datos**
    - neo4j
    - elasticsearch
    - modelo-de-informacion
  - **Inteligencia Artificial**
    - ocr
    - bert
    - busqueda-semantica
    - moderacion
  - **Eventos**
    - modelo-de-eventos
    - stages
    - kafka