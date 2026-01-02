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
| `confidence_sco_
