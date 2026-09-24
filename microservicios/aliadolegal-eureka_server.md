[← Volver a página Documentación Técnica y ejecutiva](../documentacion-tecnica-ejecutiva.md)

---

# Service Discovery Server (`aliadolegal-eureka_server`)

Servidor de registro y descubrimiento de servicios para la arquitectura de microservicios de **AliadoLegal**. Basado en Spring Cloud Netflix Eureka Server, este componente permite la localización dinámica e interconexión resiliente de todos los microservicios del ecosistema.

---

## 🛠️ Stack Tecnológico

* **Framework:** Spring Boot 3.5.7, Spring Cloud Netflix Eureka Server


* **Seguridad:** Spring Security (HTTP Basic Auth para `ROLE_ADMIN`, `ROLE_ACTUATOR` y `ROLE_MANAGER`)


* **Monitoreo & Métricas:** Spring Boot Actuator, Prometheus


* **Java:** JDK 17+



---

## ⚙️ Configuración del Servidor (`application.yml`)

```yaml
server:
  port: 8761
  shutdown: graceful

spring:
  application:
    name: eureka-server-service
  main:
    allow-circular-references: true
  lifecycle:
    timeout-per-shutdown-phase: 30s

eureka:
  instance:
    lease-renewal-interval-in-seconds: 10
    lease-expiration-duration-in-seconds: 2
    prefer-ip-address: true
  server:
    enable-self-preservation: false    # Desactiva la autodefensa (solo en dev)
    wait-time-in-ms-when-sync-empty: 0
    eviction-interval-timer-in-ms: 5000
  client:
    register-with-eureka: false
    fetch-registry: false
    healthcheck:
      enabled: true

management:
  endpoints:
    web:
      exposure:
        include: "*"   # expone todos los endpoints Actuator (dev)
  endpoint:
    health:
      show-details: always  # ver detalles de health en dev

logging:
  file:
    name: logs/eureka-server.log
  level:
    root: INFO
    com.netflix.eureka: DEBUG
    com.netflix.discovery: DEBUG
    org.springframework.cloud.netflix.eureka: DEBUG
    web: DEBUG

```

---

## 📋 Aspectos Clave de Configuración

| Parámetro | Configuración | Descripción de Negocio / Técnica |
| --- | --- | --- |
| **Puerto Standalone** | `8761` | Puerto estándar para el panel y registro del servidor Eureka. |
| **Graceful Shutdown** | `30s` | Apagado ordenado asegurando la evacuación/desregistro de clientes antes de finalizar el proceso. |
| **Self-Preservation** | `false` | Desactivado para entornos de desarrollo/pruebas. Permite desregistrar rápidamente instancias inactivas. |
| **Eviction Timer** | `5000 ms` | Intervalo de barrido acelerado (5s) para limpiar servicios caídos en entornos de prueba local. |
| **Auto-Registro** | `false` | Impide que el servidor Eureka intente registrarse a sí mismo como un microservicio cliente. |

---

## 🔗 URLs y Endpoints Disponibles

### 🌐 Dashboard y Registro de Eureka

* **Dashboard Principal:** `http://localhost:8761/`
* **Consulta de Aplicaciones (XML/JSON):** `http://localhost:8761/eureka/apps`

### 📊 Endpoints de Actuator (`/actuator`)

* **Índice General de Actuator:** `http://localhost:8761/actuator`
* **Métricas para Prometheus:** `http://localhost:8761/actuator/prometheus`
* **Estado de Salud (Health Check):** `http://localhost:8761/actuator/health` *(requiere autenticación con `ROLE_ACTUATOR` o `ROLE_ADMIN`)*
* **Información de la Aplicación:** `http://localhost:8761/actuator/info`
* **Métricas del Sistema:** `http://localhost:8761/actuator/metrics`
* **Mapeo de Endpoints HTTP:** `http://localhost:8761/actuator/mappings`
* **Entorno y Propiedades:** `http://localhost:8761/actuator/env`
* **Beans de Spring:** `http://localhost:8761/actuator/beans`
* **Configuración de Logs (Loggers):** `http://localhost:8761/actuator/loggers`

---

## 📈 Integración con Prometheus (`prometheus.yml`)

Para recolectar métricas del servidor Eureka mediante Prometheus, utiliza la siguiente configuración en tu archivo `prometheus.yml`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'spring-actuator'
    metrics_path: '/actuator/prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:8761']

```

---

## 🚀 Inicio y Acceso Local

### 1. Compilación y Ejecución

```bash
# Compilar el proyecto
mvn clean package -DskipTests

# Ejecutar el servidor Eureka
mvn spring-boot:run

```

### 2. Autenticación

El acceso al panel web y endpoints protegidos requiere autenticación mediante **HTTP Basic Popup**:

* **Admin:** `jorgeAdmin` / `eurekaJMC` (Acceso total)
* **Actuator:** `jorgeAdmin` (Tiene `ROLE_ACTUATOR`)
* **Manager:** `jorgeManager` / `eurekaJMC` (`ROLE_MANAGER`)

---

## 📊 Monitoreo y Traza de Eventos

* **Endpoints de Actuator:** Exposición total habilitada (`/actuator/health` con detalle extendido).


* **Logs del Servidor:** Almacenamiento local en `logs/eureka-server.log` con trazas de depuración (`DEBUG`) sobre descubrimientos y registros de la red de microservicios.

---

[← Volver a página Documentación Técnica y ejecutiva](../documentacion-tecnica-ejecutiva.md)
