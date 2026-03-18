# SRS Template — Sistema de Gestión de Citas Médicas
**Software Requirements Specification (SRS)**

---

**Proyecto:** Sistema de Gestión de Citas Médicas  
**Versión:** 1.0  
**Fecha:** Marzo 2026  
**Grupo:** Grupo 3 — Ingeniería del Software I  
**Institución:** INVENIO University  

---

## 1. Introducción

### 1.1 Propósito
Este documento describe los requisitos funcionales y no funcionales del Sistema de Gestión de Citas Médicas, incluyendo la API REST y el modelo de datos subyacente.

### 1.2 Alcance
El sistema permite registrar pacientes, gestionar citas médicas y consultar información mediante endpoints REST documentados con OpenAPI 3.0.

### 1.3 Definiciones, Acrónimos y Abreviaturas

| Término | Definición |
|---------|-----------|
| API | Application Programming Interface |
| REST | Representational State Transfer |
| ERD | Entity-Relationship Diagram |
| PK | Primary Key (Llave Primaria) |
| FK | Foreign Key (Llave Foránea) |
| SRS | Software Requirements Specification |

### 1.4 Referencias
- OpenAPI Specification 3.0.3: https://spec.openapis.org/oas/v3.0.3
- RFC 7231 — HTTP/1.1 Semantics
- Documento del proyecto: `APIs_y_Datos_.docx`

---

## 2. Descripción General del Sistema

### 2.1 Perspectiva del producto
El sistema actúa como backend REST API que provee servicios de gestión de datos médicos para ser consumidos por aplicaciones cliente (web/móvil).

### 2.2 Funciones principales
- Consultar listado de pacientes registrados
- Consultar citas médicas (con filtros opcionales)
- Crear nuevas citas médicas
- Cancelar citas existentes

### 2.3 Características de usuarios

| Tipo de usuario | Descripción |
|----------------|-------------|
| Administrador  | Gestión completa del sistema |
| Recepcionista  | Crear y consultar citas |
| Paciente       | Consultar sus propias citas |

### 2.4 Restricciones
- El formato de intercambio de datos es JSON
- La API sigue el estilo arquitectónico REST
- Versión actual: v1

---

## 3. Requisitos Funcionales

### RF-001: Consultar lista de pacientes
- **Endpoint:** `GET /patients`
- **Descripción:** El sistema debe retornar la lista completa de pacientes registrados.
- **Entrada:** Ninguna
- **Salida:** Array JSON con objetos `Patient`
- **Código éxito:** HTTP 200

### RF-002: Consultar paciente por ID
- **Endpoint:** `GET /patients/{patientId}`
- **Descripción:** El sistema debe retornar la información de un paciente específico.
- **Entrada:** `patientId` (path param, entero)
- **Salida:** Objeto JSON `Patient`
- **Errores:** HTTP 404 si no existe

### RF-003: Consultar lista de citas
- **Endpoint:** `GET /appointments`
- **Descripción:** El sistema debe retornar el listado de citas, con filtros opcionales.
- **Entrada (query params opcionales):** `patientId`, `date`
- **Salida:** Array JSON con objetos `Appointment`
- **Código éxito:** HTTP 200

### RF-004: Crear nueva cita
- **Endpoint:** `POST /appointments`
- **Descripción:** El sistema debe permitir registrar una nueva cita médica.
- **Entrada (body JSON):** `patientId`, `date`, `time`
- **Salida:** Mensaje de confirmación y `appointmentId` generado
- **Código éxito:** HTTP 201
- **Errores:** HTTP 400 (datos inválidos), HTTP 404 (paciente no existe), HTTP 409 (horario ocupado)

### RF-005: Cancelar cita
- **Endpoint:** `DELETE /appointments/{appointmentId}`
- **Descripción:** El sistema debe permitir cancelar una cita registrada.
- **Entrada:** `appointmentId` (path param, entero)
- **Salida:** Mensaje de confirmación
- **Errores:** HTTP 404 si no existe

---

## 4. Requisitos No Funcionales

### RNF-001: Rendimiento
El sistema debe responder a las solicitudes en menos de 500ms bajo carga normal.

### RNF-002: Disponibilidad
El sistema debe estar disponible el 99.5% del tiempo (uptime mensual).

### RNF-003: Seguridad
- Las comunicaciones deben usar HTTPS/TLS
- Los endpoints deben requerir autenticación (token Bearer)

### RNF-004: Mantenibilidad
El código debe seguir estándares de documentación OpenAPI 3.0 y estar versionado en Git.

### RNF-005: Escalabilidad
La arquitectura debe soportar al menos 100 usuarios concurrentes.

---

## 5. Modelo de Datos (Resumen)

### Entidades

**PACIENTE**
| Campo | Tipo | Descripción |
|-------|------|-------------|
| patientId | INT (PK) | Identificador único |
| name | VARCHAR(100) | Nombre completo |
| email | VARCHAR(150) | Correo electrónico |
| phone | VARCHAR(20) | Teléfono |
| createdAt | TIMESTAMP | Fecha de registro |

**CITA**
| Campo | Tipo | Descripción |
|-------|------|-------------|
| appointmentId | INT (PK) | Identificador único |
| patientId | INT (FK) | Referencia al paciente |
| date | DATE | Fecha de la cita |
| time | TIME | Hora de la cita |
| status | VARCHAR(20) | Estado (activa/cancelada) |
| notes | TEXT | Observaciones |

### Relación
`PACIENTE (1) —— tiene —— (N) CITA`

---

## 6. Restricciones y Suposiciones

- Se asume que la base de datos es relacional (MySQL/PostgreSQL)
- El sistema no gestiona autenticación de pacientes en esta versión (v1)
- Los datos de prueba son ficticios

---

## 7. Historial de Cambios

| Versión | Fecha | Autor | Descripción |
|---------|-------|-------|-------------|
| 1.0 | Marzo 2026 | Grupo 3 | Versión inicial |

---

*Documento generado como plantilla editable para el proyecto de Ingeniería del Software I — INVENIO University*
