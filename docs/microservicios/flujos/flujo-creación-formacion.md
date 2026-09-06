# Flujo de Negocio: Creación de Formación Educativa

Este documento detalla el proceso técnico y de negocio para el registro, validación y persistencia de un nuevo antecedente académico en el perfil de un abogado dentro de **AliadoLegal**.

## 1. Visión General
El flujo gestiona la incorporación de una nueva formación educativa (`education`). Incluye la resolución inteligente de instituciones (catálogo existente o creación de una nueva institución provisional), la verificación estricta del nivel académico mediante catálogo, la validación de reglas de integridad académica, la persistencia del agregado y un ciclo completo de comunicación asíncrona mediante Kafka (publicación y consumo del evento de creación).

## 2. Flujo de Operación

1. **Recepción de la Solicitud:** El cliente invoca el endpoint `POST /add` enviando un objeto `LawyerEducationRequest` validado mediante anotaciones de bean validation.
2. **Obtención del Abogado:** Se recupera la entidad principal `LawyerEntity` a partir del `lawyerId` provisto en el request y se extrae su lista actual de educaciones.
3. **Resolución de la Institución Académica:**
    - Si el request incluye un `institutionId`, se busca en el repositorio de instituciones (lanza `InstitutionNotFoundException` si no existe).
    - Si no se provee un `institutionId`, se registra una nueva institución como probable, asignándole un UUID aleatorio, nombre suministrado, un ranking académico por defecto (`0.50`), `verified = false` y `active = true`, procediendo a guardarla.
4. **Resolución del Nivel Académico:** Se valida y obtiene el nivel académico estrictamente desde el catálogo de `StudyLevel` usando el `studyLevelId` (lanza `StudyLevelNotFoundException` si no se encuentra).
5. **Evaluación de Reglas Académicas:** Se crea el objeto de educación y se evalúa si puede ser incorporado al historial del abogado mediante las reglas de integridad académica (`addEducationIfAllowed`).
6. **Persistencia del Agregado:** Se guarda la entidad actualizada del abogado con su nueva colección de educaciones en la base de datos.
7. **Validación de Origen y Publicación de Evento (Kafka - Productor):** El despachador evalúa las reglas de negocio de origen permitiendo exclusivamente llamadas originadas en `LawyerEducationServiceImpl.addLawyerEducation`. Si es válido, construye el objeto `LawyerEvent` con metadatos de trazabilidad y lo despacha a través de `lawyerEventProducer`.
8. **Consumo y Procesamiento Asíncrono (Kafka - Listener):** El componente oyente intercepta el evento `LawyerEvent`, valida mediante el `OriginValidator` que provenga del origen autorizado y delega la ejecución al servicio de respuesta para procesar el evento de creación (`eventEducationCreated`).
9. **Respuesta HTTP:** Finalmente, se mapea la lista de entidades educativas a objetos DTO (`LawyerEducation`) y se retorna un código HTTP `201 Created`.

## 3. Diagrama de Secuencia Lógico

| Paso | Componente | Acción |
| :--- | :--- | :--- |
| 1 | Controller | Recibe `LawyerEducationRequest` en `POST /add`. |
| 2 | LawyerEducationService | Obtiene `LawyerEntity` y evalúa reglas de institución (catálogo o nueva). |
| 3 | Repositories | Resuelve o persiste la institución y valida el `StudyLevel`. |
| 4 | Service / Business Rules | Crea la educación y valida reglas de integridad académica (`addEducationIfAllowed`). |
| 5 | Database | Persiste los cambios en `LawyerEntity`. |
| 6 | Event Dispatcher / Publisher | Valida origen autorizado, construye `LawyerEvent` y publica vía `lawyerEventProducer`. |
| 7 | Kafka Listener | Intercepta el evento, valida el origen esperado y delega en el servicio (`eventEducationCreated`). |
| 8 | Mapper / Controller | Mapea la lista a DTO y retorna HTTP `201 Created`. |

## Diagrama de Secuencia

```plantuml
@startuml
autonumber
skinparam BoxPadding 10
skinparam ParticipantPadding 10

actor Client
participant "LawyerEducationController" as C
participant "LawyerEducationServiceImpl" as S
participant "InstitutionRepository" as IR
participant "StudyLevelRepository" as SLR
database "Database / Repositories" as DB
participant "EventProducerDispatcher" as EPD
participant "OriginValidator" as OV
participant "Kafka Producer" as KP
participant "LawyerKafkaListener" as LK
participant "EventResponseServiceImpl" as ERS

Client -> C: POST /add (LawyerEducationRequest)
C -> S: addLawyerEducation(request)
S -> DB: getLawyerEntityById(lawyerId)
alt institutionId provided
    S -> IR: findByInstitutionId(institutionId)
else new institution name
    S -> IR: save(New InstitutionEntity)
end
S -> SLR: findByStudyLevelId(studyLevelId)
S -> S: createEducation() & addEducationIfAllowed()
S -> DB: lawyerRepository.save(lawyerEntity)
S -> EPD: dispatchAndPublish(EDUCATION, CREATED, lawyerId, payload)
EPD -> OV: validateOriginOrWarn(allowedOrigins)
alt Origin Authorized
    OV --> EPD: Valid
    EPD -> KP: lawyerEventProducer.send(LawyerEvent)
else Origin Rejected
    OV --> EPD: Ignore & Log Warning
end
S --> C: List<LawyerEducation> (DTOs)
C --> Client: ResponseEntity.status(CREATED)

== Procesamiento Asíncrono (Kafka) ==
KP -> LK: Mensaje broker (abogado-events)
LK -> OV: Valida origen autorizado en Listener
alt Origin Authorized
    OV --> LK: Valid
    LK -> ERS: eventEducationCreated(event)
    ERS -> ERS: Log Info de Creación de Educación
end
@enduml