[← Volver a página Documentación Técnica y ejecutiva](../documentacion-tecnica-ejecutiva.md)

---

# AliadoLegal - Files Upload OCR (`aliadolegal-filesuploadOCR`)
Microservicio backend de **AliadoLegal** especializado en la carga de archivos, el reconocimiento óptico de caracteres (OCR), la limpieza de texto y la extracción de información de cédulas profesionales. Procesa documentos PDF e imágenes y publica eventos de verificación de educación mediante Apache Kafka.

---

## 🚀 Stack Tecnológico

* **Core:** Java 17, Spring Boot 3.5.7.
* **Ecosistema Spring Cloud:** Eureka Client (registro y descubrimiento), OpenFeign (comunicación declarativa con otros microservicios).
* **Procesamiento documental:** Tess4J/Tesseract para OCR de imágenes, Apache PDFBox para convertir y procesar documentos PDF.
* **Procesamiento de lenguaje:** LanguageTool para limpieza y normalización de texto.
* **Inteligencia artificial:** Cliente oficial de OpenAI para estructurar la información extraída.
* **Mensajería:** Apache Kafka para la publicación de eventos de verificación.
* **Observabilidad:** Spring Boot Actuator.
* **Utilidades:** Lombok y ModelMapper.
* **Pruebas:** Spring Boot Starter Test.

---

## 🛠️ Requisitos Previos

* **Java JDK:** Versión 17 o superior.
* **Apache Maven:** Versión 3.8 o superior.
* **Apache Kafka:** Disponible en `localhost:9092` para el flujo de eventos.
* **Servidor Eureka:** Disponible en `http://localhost:8761/eureka/` para el registro del servicio.
* **OpenAI:** Una clave válida configurada de forma segura para el procesamiento de información OCR.
* **Servicio de educación:** El microservicio consultado por OpenFeign debe estar disponible para los endpoints de licencia profesional.

La aplicación utiliza el puerto `10020` por defecto y acepta archivos de hasta `10 MB`.

---

## ⚙️ Configuración

La configuración principal se encuentra en `src/main/resources/application.yml`. Antes de ejecutar la aplicación, revisa especialmente:

* `files.repository`: directorio donde se almacenan los archivos cargados.
* `spring.kafka.bootstrap-servers`: dirección del broker Kafka.
* `eureka.client.service-url.defaultZone`: URL del servidor Eureka.
* `openai.api.key`: clave de OpenAI. No debe almacenarse directamente en el repositorio; utiliza variables de entorno o un mecanismo de configuración segura.
* `app.kafka.lawyer-topic`: tópico utilizado para eventos de abogados.

Los valores de referencia para licenciatura, maestría y doctorado se encuentran en `src/main/resources/ocr.properties`.

---

## ▶️ Compilación y Ejecución

Para compilar el proyecto sin ejecutar las pruebas:

```bash
mvn clean install -DskipTests
```

Para ejecutar las pruebas y validar el proyecto:

```bash
mvn clean verify
```

Para levantar el microservicio:

```bash
mvn spring-boot:run
```

También puede ejecutarse el JAR generado por Maven:

```bash
java -jar target/aliadolegal-filesuploadOCR-1.0-SNAPSHOT.jar
```

---

## 🔌 Catálogo de Endpoints REST

Los endpoints de carga reciben archivos mediante `multipart/form-data` y aceptan documentos PDF o imágenes JPG, JPEG y PNG.

### Carga y procesamiento general (`/api/files`)

* **POST `/api/files/upload`**: almacena un archivo en el repositorio configurado, organizado por `idAbogado`.
  * Parámetros: `idAbogado`, `file`.
  * Respuesta exitosa: `200 OK`.
* **POST `/api/files/upload-ocr`**: procesa un PDF o imagen, ejecuta OCR, limpia el texto y devuelve el resultado.
  * Parámetros: `file`, `palabrasExtra` opcional.
  * Respuesta exitosa: `200 OK` con el texto extraído.
* **POST `/api/files/limpiaTxt`**: limpia y normaliza texto existente.
  * Parámetros: `texto`, `palabrasExtra` opcional.
  * Respuesta exitosa: `200 OK` con el texto procesado.

### OCR de licencia profesional (`/api/ocr`)

Estos endpoints reciben `file`, `lawyerId` y `educationId`. Consultan los datos de educación, procesan el documento y publican un evento de verificación en Kafka.

* **POST `/api/ocr/professional_license`**: ejecuta OCR sobre el documento y estructura la información mediante OpenAI.
* **POST `/api/ocr/professional_licenseL`**: devuelve la información configurada para licenciatura.
* **POST `/api/ocr/professional_licenseM`**: devuelve la información configurada para maestría.
* **POST `/api/ocr/professional_licenseD`**: devuelve la información configurada para doctorado.

Las respuestas exitosas de estos endpoints tienen estado `200 OK`. Un archivo vacío devuelve `400 Bad Request`, un formato no permitido devuelve `415 Unsupported Media Type` y los errores de procesamiento devuelven `500 Internal Server Error`.

---

## 📨 Eventos Kafka

Después de procesar una licencia profesional, el servicio publica un evento de verificación para la etapa `EDUCATION` del abogado. Los tópicos configurados por defecto son:

* `abogado-events`: eventos relacionados con abogados.
* `cliente-events`: eventos relacionados con clientes.

El grupo consumidor configurado es `aliadolegal-lawyers-group`.

---

## 🩺 Actuator

Los endpoints de Actuator están expuestos para facilitar la monitorización local. Entre ellos se encuentran los endpoints de salud, información, métricas y estados de disponibilidad.

* **Health:** `GET /actuator/health`
* **Info:** `GET /actuator/info`

---

## 🗂️ Estructura del Proyecto

```text
aliadolegal-filesuploadOCR/
├── src/
│   ├── main/
│   │   ├── java/com/jmc/aliadolegal/ocr/
│   │   │   ├── client/       # Clientes OpenFeign y DTOs externos
│   │   │   ├── config/       # Configuración de Kafka, OCR y diccionarios
│   │   │   ├── controllers/  # Endpoints de carga y OCR
│   │   │   ├── kafka/        # Productores, dispatchers y eventos
│   │   │   ├── services/     # Servicios de OCR, limpieza y OpenAI
│   │   │   └── model/        # Modelos y respuestas del servicio
│   │   └── resources/        # application.yml, ocr.properties y tessdata
│   └── test/                 # Pruebas unitarias e integración
├── pom.xml                   # Dependencias y configuración de Maven
└── README.md                 # Documentación del microservicio
```

## 📄 Licencia y Autoría

Desarrollado como parte del ecosistema AliadoLegal. Todos los derechos reservados.

---

[← Volver a página Documentación Técnica y ejecutiva](./../../README.md#estructura-de-la-documentación-técnica)