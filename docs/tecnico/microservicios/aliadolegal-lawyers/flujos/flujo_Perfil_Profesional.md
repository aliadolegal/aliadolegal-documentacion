# Documentación del Flujo de Negocio: Creación de Perfil Profesional (AliadoLegal)

Este documento detalla el proceso técnico y de negocio para la creación de la descripción narrativa (perfil profesional) de un abogado dentro de la plataforma **AliadoLegal**.

## 1. Visión General
El proceso de registro de perfil profesional permite incorporar la descripción narrativa o resumen biográfico del abogado ("Acerca de mí") dentro de la plataforma **AliadoLegal**. Esta sección está diseñada para destacar la especialización y trayectoria del usuario, almacenándose de forma estructurada en la base de datos de grafos (Neo4j) e integrándose directamente con su avance de integración en el sistema.

## 2. Flujo de Operación

1.  **Recepción de la Solicitud:** El flujo se activa a través del endpoint `POST /api/lawyer/profile/add`, el cual recibe un objeto `LawyerProfessionalProfileRequest` validado previamente para asegurar la calidad de la información narrativa proporcionada.
2.  **Vinculación en la Base de Datos de Grafos:** El servicio localiza la entidad principal del abogado y procesa el mapeo hacia una instancia de `LawyerProfessionalProfileEntity`. A este nuevo perfil se le asigna un identificador único universal (`professionalProfileId`) y se establece una relación de entrada de tipo `TIENE_PERFIL` (`@Relationship`) que conecta directamente el nodo del perfil con el abogado en la estructura de Neo4j.
3.  **Propagación Asíncrona (Event-Driven):** Una vez persistido el cambio, el sistema emite un evento de dominio (`LawyerEvent`) con la etapa `PROFILE` y el tipo `CREATED` a través de Kafka, desacoplando la operación de escritura de los procesos secundarios del sistema.
4.  **Actualización de Progreso:** El componente manejador intercepta el evento y coordina con el servicio de validación para actualizar de forma transaccional el estado del perfil en la entidad de progreso del abogado (`LawyerProgressEntity`), asegurando que los indicadores de avance reflejen el cumplimiento de este hito.

## 3. Diagrama de Secuencia Lógico

| Paso | Componente | Acción |
| :--- | :--- | :--- |
| 1 | Controller | Recibe `LawyerProfessionalProfileRequest` en `/api/lawyer/profile/add`. |
| 2 | Service | Obtiene abogado, mapea el perfil, asigna UUID, persiste en Neo4j y emite evento (PROFILE / CREATED). |
| 3 | Handler | `LawyerProfileCreatedHandler` intercepta el evento de Kafka. |
| 4 | EventResponseService | Identifica la etapa `PROFILE` y delega la actualización de progreso. |
| 5 | ProgressRepository | Actualiza `profileCompleted = true` de forma transaccional. |

## 4. Estructura de la Entidad (Neo4j)

El perfil se almacena como un nodo `AbogadoPerfilProfesional`:
- **ID:** `id_perfil_profesional` (Identificador único).
- **Propiedad:** `perfil_profesional` (Descripción narrativa).
- **Relación:** `TIENE_PERFIL` (Relación entrante desde el abogado).