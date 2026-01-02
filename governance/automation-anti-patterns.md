## ❌ Mal ejemplo: Asistente conversacional para consultas oficiales sin control de uso

<img width="782" height="345" alt="image" src="https://github.com/user-attachments/assets/d23a97ba-e5ea-4c24-a96b-08463cacd3f5" />
Caso completo en Use Cases: Use Cases /external official data assistant.md

### Contexto
Se implementa un **asistente conversacional** que permite consultar información oficial de empresas brasileñas (CNPJ), responder automáticamente al usuario y registrar cada consulta como evidencia.
Este flujo **omite controles críticos**:
- ❌ No valida permisos ni perfil del usuario.
- ❌ No controla abuso ni frecuencia de consultas (rate limiting).
- ❌ No clasifica errores de la API (CNPJ inválido, empresa inexistente, caída del servicio).
- ❌ No distingue entre uso informativo y uso legal/comercial.
- ❌ No define una política explícita de cumplimiento, retención o auditoría real.
- ❌ Usa Google Sheets como log, lo cual **no es un mecanismo de auditoría formal**.

**En este caso Sin governanza el costo puede ser:**
**#reputacional**
**#operativo.**
**#potencialmente legal.**


## Anti-Patterns (Malas Prácticas)
Los siguientes patrones **deben evitarse explícitamente** en este caso de uso.
---
### ❌ Anti-Pattern 1: Tratar el flujo como un “demo”
Riesgo: El sistema se usa en contextos reales sin salvaguardas.

---
### ❌ Anti-Pattern 2: Presentar respuestas como validación oficial
Riesgo: Uso indebido de la información en decisiones legales o comerciales.
---
### ❌ Anti-Pattern 3: No distinguir intención del usuario
Riesgo: El mismo flujo responde escenarios con impactos radicalmente distintos.
---
### ❌ Anti-Pattern 4: Logging superficial
Usar registros básicos (ej. Google Sheets) como si fueran auditoría formal.
Riesgo: No poder reconstruir decisiones ante incidentes o cuestionamientos.
---
### ❌ Anti-Pattern 5: Dependencia silenciosa de APIs externas
Riesgo: El sistema responde información incorrecta sin advertencia.
---
### ❌ Anti-Pattern 6: Uso de IA sin disclaimers
Riesgo: Falsa sensación de certeza y confianza excesiva en el sistema.
---
### ❌ Anti-Pattern 7: Automatización sin punto de fricción
Riesgo: Escalamiento automático de errores y malas decisiones.
---

