## ❌ Mal ejemplo: Asistente conversacional para consultas oficiales sin control de uso

<img width="782" height="345" alt="image" src="https://github.com/user-attachments/assets/d23a97ba-e5ea-4c24-a96b-08463cacd3f5" />


### Contexto
Se implementa un **asistente conversacional** que permite consultar información oficial de empresas brasileñas (CNPJ), responder automáticamente al usuario y registrar cada consulta como evidencia.

El flujo opera a través de:
- un webhook de entrada (WhatsApp u otro canal),
- un agente de IA con modelo GPT,
- una API externa (OpenCNPJ),
- y un registro de resultados en Google Sheets.

### Qué hace el flujo

- Recibe un mensaje externo desde un canal de mensajería.
- Activa un agente de IA que:
  - interpreta la intención del usuario,
  - consulta datos oficiales de una empresa vía OpenCNPJ,
  - procesa e interpreta la respuesta (no solo la devuelve cruda).
- Registra la consulta y la respuesta en Google Sheets para trazabilidad básica.
- Devuelve la información al usuario final.

### Qué NO hace (y debería hacer)

Este flujo **omite controles críticos**, entre ellos:

- ❌ No valida permisos ni perfil del usuario.
- ❌ No controla abuso ni frecuencia de consultas (rate limiting).
- ❌ No clasifica errores de la API (CNPJ inválido, empresa inexistente, caída del servicio).
- ❌ No distingue entre uso informativo y uso legal/comercial.
- ❌ No define una política explícita de cumplimiento, retención o auditoría real.
- ❌ Usa Google Sheets como log, lo cual **no es un mecanismo de auditoría formal**.

### Por qué esto es un problema
Aunque el flujo parezca simple, **ya se comporta como un sistema de consulta oficial**, no como un demo.
Esto introduce riesgos relevantes:

- **Uso indebido de datos oficiales**  
  La información puede interpretarse como validación legal sin serlo.

- **Expectativa de exactitud y responsabilidad**  
  El usuario puede asumir que la respuesta es confiable para decisiones operativas.

- **Dependencia crítica de una API externa**  
  No hay manejo explícito de fallos ni degradación del servicio.

- **Trazabilidad insuficiente**  
  Google Sheets no cubre quién consultó, con qué intención, ni bajo qué contexto.

### Pregunta de control que este flujo no puede responder
> **“¿Podemos usar este sistema para validar proveedores o clientes?”**

Hoy, la respuesta honesta sería:
> **“Técnicamente sí.  
> Legalmente, operativamente y desde gobernanza: todavía no.”**

### Por qué este flujo requiere intervención humana

Este flujo **no debería operar sin supervisión humana** porque:
- Puede inducir a decisiones con impacto legal o reputacional.
- No distingue intención informativa vs operativa.
- No ofrece disclaimers claros al usuario.
- No cumple con principios de accountability ni explicabilidad.

### Qué se necesitaría para llevarlo al siguiente nivel
Prioridades mínimas para un uso responsable:
1. Manejo explícito de errores de OpenCNPJ.
2. Clasificación de intención del usuario (informativo vs operativo).
3. Logging estructurado (quién, cuándo, desde dónde, para qué).
4. Mensajes de disclaimer claros al usuario.
5. Definición de cuándo se requiere aprobación humana.

---

# Governance Considerations & Anti-Patterns
## External Official Data Assistant (CNPJ / Datos Oficiales)

Este documento describe las **consideraciones de gobernanza** y los **anti-patterns** asociados a un asistente conversacional que consulta datos oficiales externos, los interpreta mediante IA y devuelve respuestas a usuarios finales.

El objetivo no es detallar la implementación técnica, sino **establecer límites claros, riesgos asumidos y controles necesarios**.
---

## 1. Consideraciones de Gobernanza

### 1.1 Naturaleza real del sistema

Aunque el flujo pueda parecer simple, este asistente **ya se comporta como un sistema de consulta oficial**, no como un demo o chatbot informativo genérico.

Esto implica:
- expectativa de exactitud,
- posible uso en decisiones operativas,
- riesgo reputacional y legal si la información es incorrecta o mal interpretada.

Por lo tanto, **requiere gobernanza explícita**.

---

### 1.2 Uso de datos oficiales externos

El sistema depende de una API externa que provee información oficial.

Riesgos asociados:
- dependencia de disponibilidad del proveedor,
- cambios no controlados en el esquema de datos,
- errores o inconsistencias en la fuente.

Controles esperados:
- manejo explícito de errores,
- mensajes claros cuando la fuente no es confiable,
- prohibición de uso como “validación legal”.

---

### 1.3 Uso de IA para interpretación de datos

La IA:
- interpreta,
- resume,
- y contextualiza información oficial.

Esto introduce riesgos de:
- alucinación,
- simplificación excesiva,
- pérdida de matices legales o administrativos.

La IA **no debe presentar conclusiones como certezas**, sino como apoyo informativo.

---

### 1.4 Human-in-the-Loop (HITL)

Este caso de uso **no debe operar sin supervisión humana** en los siguientes escenarios:

- ambigüedad en la intención del usuario,
- uso potencialmente operativo o legal,
- errores o respuestas incompletas de la API,
- solicitudes fuera del patrón esperado.

Debe existir:
- escalamiento,
- validación,
- o bloqueo explícito cuando aplique.
- <img width="1792" height="978" alt="image" src="https://github.com/user-attachments/assets/ecf293c4-5dd9-47f2-89fb-e98e4b9e8656" />


---

### 1.5 Trazabilidad y evidencia

El sistema debe poder responder, como mínimo:

- quién realizó la consulta,
- cuándo y desde qué canal,
- qué datos se consultaron,
- qué respuesta se entregó,
- bajo qué contexto.

La trazabilidad **no es opcional** cuando se trabaja con datos oficiales.

---

### 1.6 Comunicación y transparencia al usuario

<<en donde el texto explícito que aclara límites>>

El usuario debe saber claramente que:

- interactúa con un sistema automatizado,
- la información es informativa, no certificatoria,
- pueden existir errores o limitaciones,
- en ciertos casos intervendrá una persona.

La opacidad genera falsas expectativas y riesgo reputacional.

Tan simple como integrar un texto informativo "Esto es un sistema automatizado. No es una validación oficial."
con esto:  no se induce a interpretaciones legales u operativas

La transparencia **no se asume**, se diseña

---

## 2. Anti-Patterns (Malas Prácticas)

Los siguientes patrones **deben evitarse explícitamente** en este caso de uso.

---

### ❌ Anti-Pattern 1: Tratar el flujo como un “demo”

Asumir que, por ser técnicamente simple, no requiere control.

**Riesgo:**  
El sistema se usa en contextos reales sin salvaguardas.

---

### ❌ Anti-Pattern 2: Presentar respuestas como validación oficial

Responder con lenguaje que sugiera:
- certificación,
- verificación legal,
- o garantía de exactitud.

**Riesgo:**  
Uso indebido de la información en decisiones legales o comerciales.

---

### ❌ Anti-Pattern 3: No distinguir intención del usuario

No diferenciar entre:
- consulta informativa,
- uso operativo,
- uso legal o comercial.

**Riesgo:**  
El mismo flujo responde escenarios con impactos radicalmente distintos.

---

### ❌ Anti-Pattern 4: Logging superficial

Usar registros básicos (ej. Google Sheets) como si fueran auditoría formal.

**Riesgo:**  
No poder reconstruir decisiones ante incidentes o cuestionamientos.

---

### ❌ Anti-Pattern 5: Dependencia silenciosa de APIs externas

No manejar explícitamente:
- caídas del servicio,
- respuestas incompletas,
- errores de formato.

**Riesgo:**  
El sistema responde información incorrecta sin advertencia.

---

### ❌ Anti-Pattern 6: Uso de IA sin disclaimers

Permitir que la IA:
- interprete datos oficiales,
- sin advertir limitaciones o posibilidad de error.

**Riesgo:**  
Falsa sensación de certeza y confianza excesiva en el sistema.

---

### ❌ Anti-Pattern 7: Automatización sin punto de fricción

Permitir ejecución automática continua sin:
- pausas,
- validación humana,
- ni control de excepciones.

**Riesgo:**  
Escalamiento automático de errores y malas decisiones.

---

## 3. Criterio de Aceptación de Gobernanza

Este caso de uso **solo debe operar sin supervisión constante** si:

- la intención del usuario es claramente informativa,
- los datos son no sensibles,
- la fuente responde correctamente,
- y la respuesta se presenta con disclaimers adecuados.

En cualquier otro escenario, **debe activarse intervención humana o bloqueo**.

---

## Nota final

El riesgo principal de este caso de uso **no es técnico**.

Es:
- reputacional,
- operativo,
- y de uso indebido.

La gobernanza no limita el valor del asistente.  
**Lo hace sostenible.**
---

# Checklist – External Official Data Assistant

Este checklist valida que el caso de uso esté **listo para operar de forma responsable**, con controles adecuados de riesgo, trazabilidad y decisión humana.
---
## 1. Definición del Caso de Uso
- [ ] ¿El propósito del asistente está claramente definido (informativo vs operativo)?
- [ ] ¿Está documentado qué NO hace el asistente?
- [ ] ¿Se evita presentar la respuesta como validación legal o certificación?
---
## 2. Origen y Uso de Datos
- [ ] ¿La fuente de datos oficial está claramente identificada (API, proveedor)?
- [ ] ¿Se documentan las limitaciones de la fuente externa?
- [ ] ¿Se evita almacenar datos personales innecesarios?
- [ ] ¿Existe política de retención / expiración de datos?
---
## 3. Uso de IA y Riesgos Asociados
- [ ] ¿Se documenta cómo la IA interpreta los datos (no solo los devuelve)?
- [ ] ¿Se reconoce explícitamente que la IA puede cometer errores?
- [ ] ¿Existen disclaimers visibles para el usuario?
- [ ] ¿Se define un umbral de confianza o condiciones de escalamiento?
---
## 4. Human-in-the-Loop
- [ ] ¿Se distingue entre consultas informativas y uso con impacto operativo?
- [ ] ¿Existen puntos explícitos de aprobación humana?
- [ ] ¿El flujo se detiene ante ambigüedad o excepción?
- [ ] ¿Está documentado quién asume responsabilidad en decisiones finales?
---
## 5. Manejo de Errores y Excepciones
- [ ] ¿Se clasifican errores de la API (inexistente, inválido, caído)?
- [ ] ¿El sistema falla de forma segura?
- [ ] ¿Se evita responder con información incompleta o engañosa?
- [ ] ¿Los errores se registran con contexto?
---
## 6. Trazabilidad y Evidencia
- [ ] ¿Se registra quién hizo la consulta?
- [ ] ¿Se registra cuándo y desde qué canal?
- [ ] ¿Se guarda la respuesta entregada?
- [ ] ¿El log es suficiente para auditoría (no solo debugging)?
---
## 7. Comunicación al Usuario
- [ ] ¿El usuario sabe que interactúa con un sistema automatizado?
- [ ] ¿Se explica el alcance y limitaciones del asistente?
- [ ] ¿Se indica cuándo interviene un humano?
- [ ] ¿Se evita lenguaje que induzca falsa certeza?
---
## 8. Preparación para Escalamiento
- [ ] ¿El caso de uso puede escalar sin aumentar riesgo?
- [ ] ¿Se ha evaluado dependencia de proveedores externos?
- [ ] ¿Existe plan de mejora antes de uso productivo?
- [ ] ¿Está claro cuándo este flujo NO debe usarse?
---
## Criterio final de aprobación
Si alguna sección crítica no puede marcarse como completa,  
**el flujo no debe operar sin supervisión humana explícita**.
