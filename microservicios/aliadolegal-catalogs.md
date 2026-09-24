[← Volver a página Documentación Técnica y ejecutiva](../documentacion-tecnica-ejecutiva.md)

---
# AliadoLegal Catalogs (`aliadolegal-catalogs`)

Microservicio backend perteneciente al ecosistema **AliadoLegal**, encargado de la gestión, consulta y administración de catálogos especializados de información legal (materias de derecho, especialidades de abogados, niveles de estudio e instituciones académicas/profesionales).

---

## 🛠️ Stack Tecnológico

* **Core:** Java 17, Spring Boot 3.5.7


* **Arquitectura Microservicios / Cloud:** Spring Cloud (Eureka Client, OpenFeign)


* **Bases de Datos y Persistencia:**
* **Neo4j** (Spring Data Neo4j) para grafos y relaciones jurídicas/académicas.


* **Elasticsearch** (Spring Data Elasticsearch) para búsquedas indexadas.




* **Utilidades & Mapeo:** Lombok, ModelMapper (v3.2.0), Commons Library (`aliadolegal-commons`).


* **Monitoreo y Salud:** Spring Boot Actuator.



---

## ⚙️ Configuración del Entorno (application.yml)

El servicio corre por defecto en el puerto **`10030`** y se registra automáticamente en el servidor de descubrimiento Eureka.

Principales propiedades configuradas:

* **Puerto:** `10030`

* **Base de datos Grafos (Neo4j):** Conexión local en `bolt://localhost:7687` (Base de datos: `aliadolegal`).


* **Eureka Discovery:** Apuntando a `http://jorgeAdmin:eurekaJMC@localhost:8761/eureka/`.



---

## 🚀 Endpoints Principales (`/api/catalog`)

El microservicio expone los siguientes endpoints REST para la consulta de catálogos:

| Método | Endpoint | Descripción |
| --- | --- | --- |
| `GET` | `/api/catalog/subjectlaw/get/{subjectId}` | Obtiene una materia de derecho por su identificador único. |
| `GET` | `/api/catalog/subjectlaw/getAll/{subjectName}` *(o sin sufijo)* | Lista o filtra las materias de derecho según el texto de búsqueda. |
| `GET` | `/api/catalog/lawspecialty/getAll/{subjectId}/{specialtyName}` | Obtiene las especialidades legales asociadas a una materia y filtradas por nombre. |
| `GET` | `/api/catalog/lawspecialty/getById/{specialtyId}` | Busca una especialidad legal por su ID. |
| `GET` | `/api/catalog/lawspecialty/getByName/{specialtyName}` | Busca una especialidad legal por su nombre exacto. |
| `GET` | `/api/catalog/nivelestudio/getAll` | Lista todos los niveles de estudio disponibles en el catálogo. |
| `GET` | `/api/catalog/institution/{institutionId}` | Consulta el detalle de una institución por su ID. |

---

## 💻 Guía de Ejecución Local

1. **Requisitos previos:**
* Tener instalado **Java 17**.


* Tener corriendo los servicios de soporte locales (Servidor Eureka en el puerto `8761` y Neo4j en el puerto `7687`).




2. **Compilar el proyecto:**
```bash
mvn clean install

```


3. **Ejecutar la aplicación:**
   Puedes levantar el servicio directamente usando Maven o el JAR generado:
```bash
mvn spring-boot:run

```


O bien mediante el empaquetado ejecutable:
```bash
java -jar target/aliadolegal-catalogs-1.0-SNAPSHOT.jar

```



---

## 🔍 Monitoreo y Actuator

El microservicio incluye endpoints de salud y métricas mediante Spring Boot Actuator en la ruta `/actuator`, con detalles completos de *liveness* y *readiness* habilitados para entornos de desarrollo.
---
[← Volver a página Documentación Técnica y ejecutiva](../documentacion-tecnica-ejecutiva.md)

