# spec.md — Especificación de la API de Citas Médicas

**Proyecto:** Sistema de Gestión de Citas Médicas  
**Versión de API:** v1  
**Formato:** REST / JSON  
**Especificación completa:** `openapi.yaml`  
**Grupo:** Grupo 3 — Ingeniería del Software I, INVENIO University  

---

## Resumen de Endpoints

| Método | Ruta | Descripción | Auth |
|--------|------|-------------|------|
| GET | `/v1/patients` | Listar todos los pacientes | Sí |
| GET | `/v1/patients/{patientId}` | Obtener paciente por ID | Sí |
| GET | `/v1/appointments` | Listar citas (filtros opcionales) | Sí |
| POST | `/v1/appointments` | Crear nueva cita | Sí |
| DELETE | `/v1/appointments/{appointmentId}` | Cancelar cita | Sí |

---

## Esquemas de datos

### Patient
```json
{
  "patientId": 1,
  "name": "María López",
  "email": "maria.lopez@email.com",
  "phone": "+506 8888-9999"
}
```

### Appointment
```json
{
  "appointmentId": 10,
  "patientId": 1,
  "date": "2026-03-20",
  "time": "09:00",
  "status": "activa",
  "notes": "Primera consulta"
}
```

### AppointmentRequest (POST body)
```json
{
  "patientId": 1,
  "date": "2026-03-20",
  "time": "10:00"
}
```

---

## Códigos de respuesta

| Código | Significado |
|--------|------------|
| 200 | OK — Solicitud exitosa |
| 201 | Created — Recurso creado exitosamente |
| 400 | Bad Request — Datos de entrada inválidos |
| 404 | Not Found — Recurso no encontrado |
| 409 | Conflict — Horario ya ocupado |
| 500 | Internal Server Error — Error del servidor |

---

## Reglas de negocio

1. Un paciente puede tener múltiples citas (**1:N**)
2. No se permiten dos citas en el mismo horario
3. Para crear una cita, el `patientId` debe existir en el sistema
4. Las citas canceladas no se eliminan físicamente (se marca `status = "cancelada"`)
5. El formato de fecha es **ISO 8601**: `YYYY-MM-DD`
6. El formato de hora es **HH:MM** (24 horas)

---

## Ambiente y URLs base

| Ambiente | URL |
|----------|-----|
| Producción | `https://api.medicalapp.invenio.ac.cr/v1` |
| Desarrollo | `http://localhost:3000/v1` |

---

## Cómo usar la especificación OpenAPI

Para visualizar el archivo `openapi.yaml` de forma interactiva:

1. Ir a [https://editor.swagger.io](https://editor.swagger.io)
2. Seleccionar **File → Import file**
3. Cargar el archivo `openapi.yaml`
4. La interfaz mostrará todos los endpoints con documentación interactiva

---

*Ver archivo `openapi.yaml` para la especificación técnica completa.*  
*Grupo 3 | INVENIO University | Ingeniería del Software I — 2026*
