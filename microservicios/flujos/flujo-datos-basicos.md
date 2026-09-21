# Flujo de Negocio: Actualización de Datos Básicos

Este documento detalla el proceso técnico para la actualización de la información demográfica y de contacto de un abogado dentro de **AliadoLegal**.

## 1. Visión General
El flujo permite la actualización de datos personales y de contacto (nombre, teléfono, correo electrónico) y resuelve de manera dinámica la validación geográfica mediante el código postal suministrado, manteniendo la integridad y coherencia con la base de datos de ubicaciones.

## 2. Flujo de Operación

1. **Recepción y Validación de Entrada:** El endpoint `POST /update/basic_data` recibe el objeto `LawyerRequest` junto con cabeceras de seguridad.
2. **Evaluación de Cambios e Impacto en Validaciones:** El servicio `updateLawyerBasicData` busca la entidad en el repositorio. Evalúa si el teléfono o correo electrónico sufrieron modificaciones:
    - Si un dato cambia, su flag de validación previa se invalida en el payload del evento (`isPhoneValidated`, `isMailValidated`).
    - Si no cambia, mantiene el estado de validación previo.
3. **Resolución Geográfica (Código Postal):** Con base en el `postalCode` proporcionado, se consultan los repositorios de Estado, Municipio, Ciudad y Asentamientos:
    - Si se encuentran coincidencias geográficas, se vinculan las entidades a la entidad del abogado y se marca `isCPValidated: true`.
    - Si el código postal no arroja resultados, se limpian las referencias y se marca `isCPValidated: false`.
4. **Persistencia y Emisión de Evento:** Se persisten los cambios en la base de datos y se dispara un `LawyerEvent` (etapa `BASIC_DATA`, tipo `UPDATED`) conteniendo el payload con el balance de validaciones.
5. **Cierre de Ciclo en Progreso:** El componente `LawyerBasicDataUpdatedHandler` escucha el evento de Kafka y delega a `eventBasicDataUpdated`, ordenando la actualización del avance en `lawyerProgressValidationService`.

## 3. Diagrama de Secuencia Lógico

| Paso | Componente | Acción |
| :--- | :--- | :--- |
| 1 | Controller | Recibe `LawyerRequest` en `POST /update/basic_data`. |
| 2 | LawyerService | Valida existencia, compara cambios de contacto y resuelve entidades geográficas por CP. |
| 3 | Producer | Persiste entidad y emite evento Kafka (`BASIC_DATA` / `UPDATED`). |
| 4 | Handler | `LawyerBasicDataUpdatedHandler` captura el evento. |
| 5 | ProgressService | Delega a `updateBasicDataCompleted` para actualizar el avance. |