# When Human Approval Is Required
## Criterios para intervención humana en automatizaciones

Este documento define **cuándo una automatización debe requerir aprobación humana** antes de ejecutar una acción, con el objetivo de **reducir riesgos, mantener responsabilidad y asegurar decisiones coherentes con el contexto del negocio**.

La automatización sin intervención humana **no es el default**.  
La intervención humana se elimina solo cuando el riesgo es bajo, el contexto es estable y el impacto es reversible.

---

## Principio rector

> **Si una automatización puede causar daño irreversible, impacto legal, pérdida de confianza o afectación a personas, debe incluir aprobación humana.**


--


<img width="1100" height="605" alt="image" src="https://github.com/user-attachments/assets/18499925-5fb7-44f1-bf7f-c8170936363a" />

### Contexto
Se utiliza **Weaviate (vector store persistente)** para almacenar información relacionada con **recordatorios de cumpleaños**, un caso de uso **eventual y sensible** que involucra **datos personales**.

### ¿Por qué es un problema?
Este enfoque introduce varios riesgos innecesarios:

- **Retención injustificada de datos personales**  
  Un vector store persistente no es adecuado para información temporal como recordatorios.

- **Ausencia de una política clara de borrado o expiración**  
  No se define cuánto tiempo se conservan los datos ni cuándo deben eliminarse, lo que entra en conflicto con principios de privacidad (GDPR / protección de datos).

- **Mezcla peligrosa de propósitos**  
  Se combinan funciones de:
  - recordatorios operativos (datos efímeros)
  - base de conocimiento persistente  
  lo que rompe el principio de minimización y separación de responsabilidades.

### Pregunta de control (que este diseño no puede responder)

> **“¿Qué datos personales estamos almacenando, con qué propósito y por cuánto tiempo?”**

Si el sistema no puede responder esta pregunta de forma inmediata y documentada,  
**la automatización no debería ejecutarse sin revisión humana**.

__

<img width="782" height="345" alt="image" src="https://github.com/user-attachments/assets/d56ab1b1-a971-481e-8185-82b8b92e944b" />




## 1. Decisiones que afectan a personas

Se **requiere aprobación humana** cuando la automatización:

- Impacta directamente a clientes, usuarios o empleados
- Puede generar exclusión, rechazo, bloqueo o penalización
- Modifica condiciones contractuales, acceso o beneficios

**Ejemplos:**
- Bloqueo de cuentas
- Rechazo de solicitudes
- Cambios en elegibilidad
- Comunicaciones sensibles (rechazos, sanciones, advertencias)

---

## 2. Decisiones con impacto legal, ético o reputacional

Debe existir validación humana cuando:

- Se involucran datos personales, sensibles o regulados
- La acción puede generar incumplimiento normativo
- La decisión no es completamente explicable de forma automática

**Ejemplos:**
- Envío de información sensible
- Uso de datos personales en modelos o prompts
- Acciones sujetas a regulación (finanzas, salud, educación)

---

## 3. Automatizaciones con IA o modelos no determinísticos

Cuando la automatización incluye:
- modelos de IA generativa,
- clasificación probabilística,
- scoring o recomendaciones,

se requiere aprobación humana si:

- La confianza del modelo no supera un umbral definido
- No existe trazabilidad clara de la decisión
- El modelo puede amplificar sesgos o errores

**Regla práctica:**  
> *Si no puedes explicar la decisión a un auditor o usuario, no debe ejecutarse sola.*

---

## 4. Acciones irreversibles o de alto costo

Se requiere intervención humana cuando la automatización:

- Ejecuta acciones no reversibles
- Genera costos económicos relevantes
- Afecta sistemas productivos críticos

**Ejemplos:**
- Eliminación de datos
- Transferencias financieras
- Activación/desactivación de servicios
- Cambios en producción

---

## 5. Casos fuera de patrón (excepciones)

Aunque una automatización sea normalmente segura, debe escalarse a humano cuando:

- Los datos de entrada son incompletos o inconsistentes
- El contexto difiere del escenario esperado
- Se detectan valores atípicos (outliers)

Esto evita que la automatización **tome decisiones sin contexto**.

---

## 6. Señales de alerta que activan aprobación humana

Una automatización debe detenerse y solicitar aprobación si:

- Se supera un umbral de riesgo definido
- El score de confianza cae por debajo del mínimo aceptable
- Se detectan conflictos entre reglas
- El sistema entra en un estado no previsto

---

## 7. Tipos de aprobación humana

No toda aprobación humana es igual. Puede ser:

- **Validación previa**: antes de ejecutar la acción
- **Revisión posterior**: ejecución automática + revisión humana
- **Escalamiento**: el humano decide si continúa o no
- **Override manual**: el humano corrige o revierte

La elección depende del nivel de riesgo.

---

## 8. Lo que NO debe automatizarse sin humanos

Nunca debe ejecutarse de forma completamente automática:

- Decisiones disciplinarias
- Evaluaciones éticas
- Juicios complejos sin reglas claras
- Situaciones con alto impacto humano o social

---

## 9. Relación con gobernanza y marcos de referencia

Este enfoque está alineado con:

- **ISO/IEC 42001** – Supervisión humana significativa
- **NIST AI RMF** – Govern & Manage
- **Principios de IA Responsable** – Accountability y explicabilidad

---

## Nota final

La pregunta correcta no es:
> “¿Se puede automatizar?”

La pregunta correcta es:
> **“¿Debe automatizarse sin una persona validando?”**

La automatización responsable **incluye al humano como parte del sistema**, no como excepción.

