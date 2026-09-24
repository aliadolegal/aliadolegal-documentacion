[← Volver a página principal](./README.md)

---
# Documentación Técnica y ejecutiva

## Estructura de la documentación ejecutiva AliadoLegal

- **Documentos ejecutivos**
  - **Flujos de datos**
    - **[01 Crear abogado](microservicios/flujos/flujo-creacion-abogado.md)**: Registra un nuevo perfil de abogado en el sistema: Detalle del proceso de inicialización de entidades y eventos de creación del abogado.
---

## Estructura de la documentación técnica

- Documentos técnicos
  - **Arquitectura**
    - Arquitectura general
    - [Arquitectura del Modelo de Datos y Grafo AliadoLegal (Neo4j)](arquitectura/arquitectura%20de%20datos/modelo-de-grafos.md)
    - Tecnologías
        - [neo4j](arquitectura/neo4j.md)
        - [elasticsearch](arquitectura/elasticsearch.md)
  - **Microservicios**
    - [aliadolegal-catalogs](microservicios/aliadolegal-catalogs.md)
    Microservicio backend perteneciente al ecosistema **AliadoLegal**, encargado de la gestión, consulta y administración de catálogos especializados de información legal (materias de derecho, especialidades de abogados, niveles de estudio e instituciones académicas/profesionales).
    - aliadolegal-clients
    - [aliadolegal-eureka_server](microservicios/aliadolegal-eureka_server.md)
    Servidor de registro y descubrimiento de servicios para la arquitectura de microservicios de **AliadoLegal**. Basado en Spring Cloud Netflix Eureka Server, este componente permite la localización dinámica e interconexión resiliente de todos los microservicios del ecosistema.
    - [aliadolegal-filesuploadOCR](microservicios/aliadolegal-filesuploadOCR.md)
    Microservicio backend de **AliadoLegal** especializado en la carga de archivos, el reconocimiento óptico de caracteres (OCR), la limpieza de texto y la extracción de información de cédulas profesionales. Procesa documentos PDF e imágenes y publica eventos de verificación de educación mediante Apache Kafka.
    - aliadolegal-gateway
    - aliadolegal-hfmoderator
    - [aliadolegal-lawyers](microservicios/aliadolegal-lawyers.md)
    Microservicio backend especializado en la gestión, indexación y consulta de información de abogados dentro de la plataforma legal tecnológica **AliadoLegal**. Desarrollado bajo una arquitectura orientada a microservicios con comunicación reactiva/asíncrona y persistencia políglota.
    - aliadolegal-security
    - aliadolegal-specialty-intelligence
    - aliadolegal-users
---
[← Volver a página principal](./README.md)
