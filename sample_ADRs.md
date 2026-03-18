# ADR-001: Uso de REST como estilo arquitectónico de la API

**Fecha:** Marzo 2026  
**Estado:** Aceptado  
**Autores:** Grupo 3 — INVENIO University  

---

## Contexto

El sistema de gestión de citas médicas necesita exponer sus funcionalidades a clientes web y móviles. Se debe decidir qué estilo arquitectónico usar para la API de comunicación.

## Decisión

Se adopta **REST (Representational State Transfer)** como estilo arquitectónico de la API, utilizando **JSON** como formato de intercambio de datos y el protocolo **HTTP/HTTPS**.

## Opciones consideradas

| Opción | Ventajas | Desventajas |
|--------|----------|-------------|
| REST | Ampliamente conocido, sin estado, cacheable, fácil de documentar con OpenAPI | Puede ser verbose para operaciones complejas |
| GraphQL | Flexible en consultas, un solo endpoint | Mayor complejidad de implementación, curva de aprendizaje |
| gRPC | Alta performance, tipado estricto | Menor soporte en browsers, requiere herramientas específicas |

## Consecuencias

✅ **Positivas:**
- Fácil integración con cualquier cliente HTTP
- Documentación estándar con OpenAPI 3.0
- Compatible con herramientas como Swagger UI, Postman
- Stateless: escalabilidad horizontal simplificada

⚠️ **Negativas:**
- Puede requerir múltiples llamadas para operaciones complejas
- Versioning manual necesario (`/v1/`, `/v2/`)

---

---

# ADR-002: Uso de OpenAPI 3.0 para documentar la API

**Fecha:** Marzo 2026  
**Estado:** Aceptado  
**Autores:** Grupo 3 — INVENIO University  

---

## Contexto

La API necesita documentación clara, estándar y mantenible que permita a otros equipos integrar sus sistemas sin depender de comunicación informal.

## Decisión

Se utiliza **OpenAPI Specification 3.0.3** (anteriormente Swagger) para documentar todos los endpoints, esquemas de datos, códigos de respuesta y ejemplos de la API.

## Opciones consideradas

| Opción | Descripción |
|--------|-------------|
| OpenAPI 3.0 | Estándar de la industria, amplio soporte de herramientas |
| RAML 1.0 | Alternativa YAML, menos adoptada |
| API Blueprint | Markdown-based, menos herramientas disponibles |
| Solo README | Simple pero no estructurado ni validable |

## Consecuencias

✅ **Positivas:**
- Generación automática de clientes con herramientas como openapi-generator
- Validación de contratos (contract testing)
- Interfaz interactiva con Swagger UI
- Formato estándar reconocido mundialmente

⚠️ **Negativas:**
- Mantenimiento manual del archivo YAML si no se usa code-first
- Requiere disciplina del equipo para mantenerlo actualizado

---

---

# ADR-003: Modelo de base de datos relacional

**Fecha:** Marzo 2026  
**Estado:** Aceptado  
**Autores:** Grupo 3 — INVENIO University  

---

## Contexto

El sistema maneja relaciones claras entre entidades (Paciente → Citas). Se debe decidir el tipo de base de datos a utilizar.

## Decisión

Se usa una **base de datos relacional** (MySQL o PostgreSQL) con las entidades `PACIENTE` y `CITA`, relacionadas mediante llave foránea (`patientId`).

## Opciones consideradas

| Opción | Ventajas | Desventajas |
|--------|----------|-------------|
| Relacional (SQL) | ACID, relaciones fuertes, JOINs, amplio soporte | Esquema rígido |
| NoSQL (MongoDB) | Flexible, escalable horizontalmente | Sin JOINs nativos, consistencia eventual |
| Híbrido | Lo mejor de ambos | Mayor complejidad operacional |

## Consecuencias

✅ **Positivas:**
- Integridad referencial garantizada
- Consultas complejas con JOIN nativas
- Amplio soporte de ORMs (Sequelize, Prisma, Hibernate)
- Transacciones ACID para operaciones críticas

⚠️ **Negativas:**
- Migraciones de esquema requieren planeación
- Escalado vertical inicial (aunque puede replicarse)

---

---

# ADR-004: Versionado de la API mediante prefijo de ruta

**Fecha:** Marzo 2026  
**Estado:** Aceptado  
**Autores:** Grupo 3 — INVENIO University  

---

## Contexto

La API puede evolucionar en el tiempo. Se deben prever cambios en endpoints sin romper clientes existentes.

## Decisión

Se adopta el **versionado mediante prefijo de URL**: todas las rutas incluyen `/v1/` (por ejemplo, `/v1/patients`, `/v1/appointments`).

## Opciones consideradas

| Estrategia | Ejemplo | Notas |
|-----------|---------|-------|
| Prefijo en URL | `/v1/resource` | Simple, visible, ampliamente usado |
| Header de versión | `Accept: application/vnd.api+json;version=1` | Más RESTful pero menos obvio |
| Query param | `/resource?version=1` | Fácil de probar pero no recomendado |

## Consecuencias

✅ **Positivas:**
- Cambios breaking no afectan clientes en versiones anteriores
- Fácil de probar en navegador
- Compatible con proxies y caches

⚠️ **Negativas:**
- Duplicación de código entre versiones si no se estructura bien
- URLs más largas

---

*Archivo: `sample_ADRs.md` — Plantilla de Architecture Decision Records para el Sistema de Citas Médicas*  
*Grupo 3 | Ingeniería del Software I | INVENIO University | 2026*
