# Logging Standard
## Estándar mínimo de trazabilidad y evidencia para automatizaciones

Este documento define el **estándar transversal de logging** para automatizaciones (con o sin IA).  
Su objetivo es garantizar **trazabilidad, auditabilidad y rendición de cuentas**, no solo debugging técnico.

Este estándar aplica a:
- workflows de n8n,
- asistentes conversacionales,
- integraciones con APIs externas,
- automatizaciones con intervención humana (HITL).

---

## 1. Principio rector

> Si no puedes reconstruir qué ocurrió, quién lo pidió, qué decidió el sistema y qué entregó,  
> **la automatización no es defendible**.

El logging es un **mecanismo de control**.  
No es opcional cuando hay impacto en personas, reputación, dinero, cumplimiento o datos oficiales.

---

## 2. Alcance: qué debe registrarse

El sistema debe registrar evidencia suficiente para responder, como mínimo:

- **Quién** activó la automatización (usuario / sistema)
- **Cuándo** ocurrió (timestamp)
- **Desde dónde** (canal / origen)
- **Qué se solicitó** (intención + parámetros mínimos)
- **Qué decisión tomó** (ruta del flujo / outcome)
- **Qué se entregó** (tipo de respuesta + resumen)
- **Si hubo intervención humana** (y en qué paso)

---

## 3. Campos mínimos obligatorios

| Campo | Descripción | Obligatorio | Riesgo que controla |
|------|-------------|-------------|---------------------|
| `timestamp` | Fecha/hora exacta de la ejecución (ISO 8601) | ✅ | Imposibilidad de auditoría temporal |
| `request_id` | ID único por ejecución/consulta | ✅ | No correlación entre eventos |
| `workflow_id` | Identificador del workflow | ✅ | Ambigüedad sobre qué flujo actuó |
| `workflow_version` | Versión o hash del flujo | ✅ | Cambios no trazables (“antes funcionaba”) |
| `trigger_type` | Tipo de disparo (webhook, schedule, manual, event) | ✅ | Ejecuciones fuera de contexto |
| `source_channel` | Canal/origen (WhatsApp, Web, API, Slack, etc.) | ✅ | Falta de contexto de entrada |
| `actor_type` | `human` / `system` | ✅ | Responsabilidad difusa |
| `actor_id` | ID del usuario/sistema que activó (si aplica) | ✅* | Activaciones no atribuibles |
| `intent` | Clasificación de intención (informativo / operativo / ambigua / otro) | ✅ | Uso indebido del sistema |
| `decision_path` | Ruta de decisión tomada (ej. `auto`, `escalate`, `block`) | ✅ | Decisiones opacas |
| `outcome` | Resultado final (`success`, `error`, `blocked`, `escalated`) | ✅ | Falta de control del resultado |
| `response_type` | Tipo de respuesta entregada (info, warning, error, handoff) | ✅ | Mensajes no gobernados |
| `disclaimer_shown` | `true/false` | ✅ | Riesgo reputacional por opacidad |
| `human_intervention` | `none`, `required`, `performed` | ✅ | Falta de accountability humana |

\* `actor_id` puede ser obligatorio o anonimizado según el canal y políticas de privacidad.

---

## 4. Campos recomendados (según riesgo)

Estos campos se registran cuando el caso de uso lo justifique:

| Campo | Uso recomendado | Riesgo que controla |
|------|------------------|---------------------|
| `confidence_score` | Cuando hay IA / clasificación | Decisiones automáticas con baja certeza |
| `error_code` / `error_source` | Cuando se consumen APIs externas | Fallos silenciosos / diagnósticos incompletos |
| `latency_ms` | Producción / SLAs | Degradación no detectada |
| `retry_count` | Cuando hay reintentos | Abuso de proveedor / loops |
| `rate_limit_status` | Sistemas expuestos públicamente | Abuso / scraping |
| `pii_detected` | Cuando hay entrada libre de usuario | Exposición de datos sensibles |
| `data_source` | Cuando se consulta información externa | Dependencia invisible del proveedor |
| `query_fingerprint` | Hash de parámetros (sin exponer PII) | Reconstrucción sin retener datos sensibles |

---

## 5. Retención y minimización (no negociable)

- Registrar **solo lo necesario** para auditoría y control.
- Preferir **hash/enmascaramiento** sobre almacenar valores completos.
- Definir una política de retención (ej. 30/90/180 días) por nivel de riesgo.
- Implementar borrado o expiración automática cuando aplique.

Pregunta obligatoria para cualquier implementación:
> “¿Qué datos guardamos y por cuánto tiempo?”

Si no hay respuesta documentada, **no hay gobernanza**.

---

## 6. Qué NO debe registrarse (por defecto)

A menos que exista justificación y control explícito, evitar:

- textos completos de conversación del usuario
- datos sensibles (salud, biometría, financieros, credenciales)
- tokens, API keys, secretos, headers de autenticación
- contenido completo de respuestas oficiales si no es necesario (preferir resumen + referencia)

Cuando se requiera evidencia detallada, usar:
- enmascaramiento,
- segmentación,
- acceso restringido,
- y retención limitada.

---

## 7. Evidencia de mensajes (vinculación con comunicación)

Si el sistema se comunica con usuarios, el log debe capturar:

- `message_type`
- `disclaimer_shown`
- `response_type`

Esto habilita auditoría de transparencia:
> “¿Qué se le dijo exactamente al usuario y bajo qué riesgo?”

---

## 8. Intervención humana (HITL)

Cuando exista Human-in-the-Loop, el log debe registrar:

- `human_intervention = required/performed`
- `approver_id` (si aplica, anonimizado si se requiere)
- `approval_timestamp`
- `approval_outcome` (approved / rejected / modified)

---

## 9. Implementación (nota práctica)

El estándar no depende del destino del log. Puede implementarse con:
- base de datos,
- sistema de logs,
- herramientas de observabilidad,
- o un datastore simple (solo para prototipos controlados).

Lo importante es:
- consistencia,
- trazabilidad,
- y acceso controlado.

---

## 10. Cumplimiento y auditoría

Un sistema se considera **log-compliant** si:

- todos los campos obligatorios están presentes,
- existe política de retención,
- existe control de acceso,
- se puede reconstruir una ejecución completa con `request_id`,
- y se registran disclaimers y resultados.

---


Dónde lo pones (esto es lo importante)

Hay 3 lugares posibles y solo 1 es el correcto según tu objetivo:

✅ 1) Logging después de generar respuesta (éxito)

Colócalo justo antes del “Send message”.

Registra: input + output + status SUCCESS.

✅ 2) Logging en errores (imprescindible)

En n8n, usa un Error Trigger workflow separado o ramas con IF / Try-Catch (depende tu versión/nodos).

Registra: input + error + status ERROR.

✅ 3) Logging al inicio (solo si quieres auditoría de entrada)

Útil si quieres evidencia aunque se caiga la IA.

Registra: input + status RECEIVED.

Lo ideal: 1 + 2.
Si haces solo 1, vas a tener logs “bonitos” pero te vas a quedar ciega cuando falle.

## Referencias internas

- `governance/n8n-governance-controls-mapping.md` (controles y objetos de n8n)
- `governance/automated-messaging-governance.md` (mensajes mínimos por riesgo)
- `governance/when-human-approval-is-required.md` (cuándo escalar a humano)
