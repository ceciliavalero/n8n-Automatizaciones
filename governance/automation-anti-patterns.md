## ❌ Mal ejemplo: Asistente conversacional para consultas oficiales sin control de uso

<img width="782" height="345" alt="image" src="https://github.com/user-attachments/assets/d23a97ba-e5ea-4c24-a96b-08463cacd3f5" />


### Contexto
Se implementa un **asistente conversacional** que permite consultar información oficial de empresas brasileñas (CNPJ), responder automáticamente al usuario y registrar cada consulta como evidencia.

Este flujo **omite controles críticos**, entre ellos:

- ❌ No valida permisos ni perfil del usuario.
- ❌ No controla abuso ni frecuencia de consultas (rate limiting).
- ❌ No clasifica errores de la API (CNPJ inválido, empresa inexistente, caída del servicio).
- ❌ No distingue entre uso informativo y uso legal/comercial.
- ❌ No define una política explícita de cumplimiento, retención o auditoría real.
- ❌ Usa Google Sheets como log, lo cual **no es un mecanismo de auditoría formal**.

**costos si no se aplica gobernanza y control:**.
- reputacional.
- operativo.
- y potencialmente legal.


Caso completo en Use Cases
