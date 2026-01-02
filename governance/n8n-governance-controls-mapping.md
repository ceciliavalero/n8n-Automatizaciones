# n8n Governance Controls Mapping
## Principios de gobernanza materializados en controles de automatización

Este documento mapea **principios de gobernanza de automatización** con **objetos y funcionalidades concretas de n8n**, mostrando cómo los riesgos se controlan a través del diseño del flujo.

No describe flujos específicos.  
Describe **mecanismos de control reutilizables**.

## Tabla resumen – Principios de gobernanza aplicados a n8n

Esta tabla conecta **principios de gobernanza** con **objetos concretos de n8n** y los **riesgos que controlan**, mostrando cómo el diseño se materializa en la práctica.

| Principio | Objeto / Funcionalidad de n8n | Riesgo que controla |
|---------|-------------------------------|---------------------|
| La automatización no es neutral | IF / Switch Node | Decisiones implícitas y opacas que escalan impacto sin control |
| El riesgo define el nivel de control | IF / Switch Node + Human in the Loop | Tratamiento igual de escenarios con riesgos distintos |
| Human-in-the-Loop es un diseño | Human in the Loop / Wait for Approval | Ejecución automática de decisiones con impacto humano, legal o reputacional |
| Separación clara de propósitos | Workflows separados + condiciones explícitas | Mezcla de casos informativos con usos operativos o legales |
| Minimización y temporalidad de datos | Configuración de nodos + decisiones de persistencia | Retención innecesaria de datos personales o sensibles |
| Trazabilidad antes que conveniencia | Logging (Sheets / DB / Logs estructurados) | Imposibilidad de auditoría o rendición de cuentas |
| Fallar de forma segura | Error Workflow / Error Branch | Propagación de errores y acciones incorrectas ante fallos externos |
| La automatización no sustituye procesos | Manual Trigger / Approval Steps | Automatizar procesos no definidos o no aprobados |
| Transparencia hacia el usuario | Mensajes explícitos en nodos de salida | Falsas expectativas sobre exactitud o responsabilidad del sistema |
| Evolución controlada, no improvisación | Versionado de workflows + revisiones | Flujos experimentales convertidos en productivos sin control |
| La herramienta no define la gobernanza | Gestión de credenciales y permisos | Acceso excesivo, uso indebido de cuentas o privilegios |
| Automatizar también implica decir “no” | Pausas, bloqueos y aprobaciones explícitas | Escalar automatización donde el riesgo supera el beneficio |

