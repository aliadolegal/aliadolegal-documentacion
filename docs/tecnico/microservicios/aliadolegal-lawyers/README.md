# AliadoLegal - Lawyers Microservice (`aliadolegal-lawyers`)

Microservicio backend especializado en la gestión, indexación y consulta de información de abogados dentro de la plataforma legal tecnológica **AliadoLegal**. Desarrollado bajo una arquitectura orientada a microservicios con comunicación reactiva/asíncrona y persistencia políglota.

## 🗂️ Estructura del Proyecto
``` Plaintext
aliadolegal-lawyers/
├── src/
│   ├── main/
│   │   ├── java/com/jmc/aliadolegal/lawyers/   # Controladores, servicios y repositorios
│   │   └── resources/                          # Configuración de Spring Boot y perfiles
│   └── test/                                   # Pruebas unitarias e integración
├── pom.xml                                     # Configuración de Maven, dependencias y plugins de calidad
└── README.md                                   # Documentación del microservicio
```

---

## 🚀 Stack Tecnológico

* **Core:** Java 17, Spring Boot 3.5.7
* **Ecosistema Spring Cloud:** Eureka Client (Descubrimiento), Spring Cloud Vault (Configuración centralizada), OpenFeign (Comunicación declarativa).
* **Persistencia y Búsqueda:** 
  * Spring Data Neo4j (Grafos de relaciones jurídicas).
  * Spring Data Elasticsearch (Búsqueda textual avanzada de perfiles).
* **Mensajería:** Apache Kafka (Event-driven architecture).
* **Seguridad:** Spring Security (OAuth2 Resource Server & JWT).
* **Calidad y Pruebas:** JUnit 5, Mockito, JaCoCo, Checkstyle, PMD, SpotBugs, Maven Site.

---

## 🛠️ Requisitos Previos

* **Java JDK:** Versión 17 o superior.
* **Apache Maven:** Versión 3.8+ (o el wrapper integrado).
* **Infraestructura Local (Opcional para ejecución completa):** Servidor Eureka, Kafka, Elasticsearch y Neo4j activos según los perfiles de entorno (`dev`/`prod`).

---

## ⚙️ Compilación y Ejecución Básica

Para compilar el proyecto sin ejecutar pruebas pesadas:
```bash
mvn clean install -DskipTests
```
Para compilar el proyecto sin ejecutar pruebas pesadas:
```bash
mvn spring-boot:run
```
---

## 📊 Calidad de Código y Reportes (Perfil reporting)
El proyecto cuenta con un perfil Maven dedicado (reporting) que integra herramientas de análisis estático, cobertura de pruebas y generación de un portal web consolidado (Maven Site).

### Herramientas Integradas
1. **Checkstyle**: Valida el cumplimiento de estándares de codificación (Google Checks).

2. **PMD & CPD**: Detección de malas prácticas, código muerto y duplicidad de código.

3. **SpotBugs**: Análisis estático de bytecode para identificar bugs potenciales y vulnerabilidades de seguridad.

4. **JaCoCo**: Medición y reporte de cobertura de código por pruebas unitarias e de integración.

### Comandos de Ejecución de Reportes
1. Verificación completa (Compilación, Pruebas y Análisis)   
Ejecuta la limpieza, pruebas unitarias, de integración y prepara los datos para las métricas de calidad:
```bash
mvn clean verify -P reporting
```
2. Generación del Portal Web Estático (Maven Site)   
Una vez finalizada la verificación, genera el sitio HTML consolidado con todas las métricas:
```bash
mvn clean test site:site -P reporting
```
### 📁 Ubicación de los Reportes Generados   
Al finalizar la ejecución de `mvn site -P reporting`, los reportes se estructuran localmente dentro de la carpeta `target/site/` :
* **Portal Principal (Índice general)**: `target/site/index.html` (Abre este archivo en tu navegador web para ver el menú consolidado).
* **Portal Principal (Índice general)**: `target/site/index.html` (Abre este archivo en tu navegador web para ver el menú consolidado).
* **Reporte de Calidad Estática (PMD)**: `target/site/pmd.html`
* **Reporte de Bugs Potenciales (SpotBugs)**: `target/site/spotbugs.html`

## 🔌 Catálogo de Endpoints REST
A continuación se detallan las rutas y operaciones expuestas por los controladores del microservicio:  
1. **Gestión de Abogados** (/api/lawyer) - **LawyerController.java**   
    * **[POST `/api/lawyer/add` - Registra un nuevo perfil de abogado en el sistema (`201 Created`).](./flujos/flujo-creacion-abogado.md)**
    Creación de Abogado: Detalle del proceso de inicialización de entidades y eventos de creación.
    
    * POST `/api/lawyer/update/basic_data` - Actualiza los datos básicos de un abogado existente (`201 Created`).
    * GET `/api/lawyer/getAll` - Obtiene la lista completa de abogados registrados (`201 Created`).
    * DELETE `/api/lawyer/delete/{lawyerId}` - Elimina el registro de un abogado por su identificador (204 No Content).
    * PUT `/api/lawyer/activate/{lawyerId}` - Activa el estatus de un abogado (`204 No Content`).
    * PUT `/api/lawyer/deactivate/{lawyerId}` - Desactiva el estatus de un abogado (`204 No Content`).

2. **Colegios de Abogados** (/api/lawyer/barassociation)
    * POST `/api/lawyer/barassociation/add` - Agrega un nuevo registro de colegiación (`201 Created`).  
    * POST `/api/lawyer/barassociation/update` - Actualiza un registro de colegiación existente (`200 OK`).  
    * POST `/api/lawyer/barassociation/delete/{idColegiado}` - Elimina un registro de colegiación por ID (`204 No Content`).    
3. Tipos de Casos (`/api/lawyer/casetype`)   
    * GET `/api/lawyer/casetype/getAll/{lawyerId}` - Obtiene todos los tipos de casos de un abogado (`200 OK`).  
    * GET `/api/lawyer/casetype/get/{caseTypeId}` - Consulta un registro de tipo de caso por su ID (`200 OK`).  
    * POST `/api/lawyer/casetype/add` - Añade un nuevo caso legal vinculado al abogado (`201 Created`).  
    * POST `/api/lawyer/casetype/update` - Actualiza un caso legal existente (`200 OK`).  
    * POST `/api/lawyer/casetype/delete/{caseTypeId}` - Elimina un caso por su identificador (`204 No Content`).  
4. Cursos y Capacitaciones (/api/lawyer/curse)   
    * POST `/api/lawyer/curse/add` - Registra un curso o capacitación (`201 Created`).  
    * POST `/api/lawyer/curse/update` - Actualiza los datos de un curso registrado (`200 OK`).  
    * POST `/api/lawyer/curse/delete/{curseId}` - Elimina un registro de curso mediante su ID (`204 No Content`).  
5. Educación Académica (/api/lawyer/education)   
    * POST `/api/lawyer/education/add` - Crea un nuevo registro de antecedentes académicos (`201 Created`).  
    * GET `/api/lawyer/education/get/{lawyerId} / getAll/{lawyerId}` - Obtiene el historial educativo de un abogado (`200 OK`).  
    * POST `/api/lawyer/education/update` - Actualiza información académica existente (`200 OK`).  
    * DELETE `/api/lawyer/education/delete/{lawyerId}/{educationId}` - Elimina un registro educativo específico (`204 No Content`).  
6. Experiencia Profesional (`/api/lawyer/experience`)   
    * POST `/api/lawyer/experience/add` - Añade un historial de experiencia laboral (`201 Created`).  
    * POST `/api/lawyer/experience/update` - Actualiza un registro de experiencia profesional (200 OK).  
    * POST `/api/lawyer/experience/delete/{experienceId}` - Elimina la experiencia laboral por ID (204 No Content).  
    * GET `/api/lawyer/experience/getAll/{lawyerId}` - Recupera todos los puestos de experiencia del abogado (`201 Created`).  
7. Perfil Profesional (`/api/lawyer/profile`)
    * POST `/api/lawyer/profile/add` - Registra el perfil profesional principal (`201 Created`).  
    * GET `/api/lawyer/profile/get/{lawyerId}` - Consulta el perfil profesional por ID del abogado (`201 Created`).
    * POST `/api/lawyer/profile/update` - Modifica los datos del perfil profesional (`200 OK`).
8. Servicios del Abogado (`/api/lawyer/service`)
    * POST `/api/lawyer/service/add` - Agrega una oferta de servicio profesional (`201 Created`).
    * GET `/api/lawyer/service/get/{idAbogado}` - Consulta los servicios configurados por ID (`201 Created`).
    * `POST /api/lawyer/service/update` - Actualiza los servicios ofrecidos (`200 OK`).  
9. Especialidades (`/api/lawyer/speciality`)
    * POST `/api/lawyer/speciality/add` - Asocia una nueva especialidad legal (`201 Created`).
    * POST `/api/lawyer/speciality/update` - Actualiza una especialidad registrada (`200 OK`).
    * DELETE `/api/lawyer/speciality/delete` - Elimina una especialidad enviando el request body (`204 No Content`).
    * GET `/api/lawyer/speciality/getAll/{idAbogado}` - Lista especialidades de un abogado específico (`201 Created`).
    * GET `/api/lawyer/speciality/getAll` - Obtiene el catálogo global de especialidades (`201 Created`).    
10. Soporte y Validación Asíncrona (Dev / Pruebas) (`/api/lawyer/update-status`)   
    * POST `/api/lawyer/update-status/phone-validated/{lawyerId}/{isValidated}` - Simula validación telefónica vía Kafka (`202 Accepted`).  
    * POST `/api/lawyer/update-status/casetype/validate` - Simula evento de validación de tipo de caso (`200 OK`).  
    * POST `/api/lawyer/update-status/validate-ocr` - Simula el flujo de validación OCR para licencias (`200 OK`).  
    * POST `/api/lawyer/update-status/validate-document` - Valida documentos educativos contra el perfil (`200 OK` / `400 Bad Request`).
