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

**Sin governanza el costo puede ser:
#reputacional
#operativo.
#potencialmente legal.



## Anti-Patterns (Malas Prácticas)

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

La gobernanza no limita el valor del asistente... **Lo hace sostenible.**
