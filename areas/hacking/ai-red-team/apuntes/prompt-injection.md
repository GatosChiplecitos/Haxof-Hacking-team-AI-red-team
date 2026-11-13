# Prompt Injection

- **Autor:** Atribución individual pendiente
- **Fecha:** 2026-11-12
- **Última actualización:** 2026-11-12
- **Estado:** Por explorar
- **Etiquetas:** ai-security, llm-security, prompt-injection
- **Fuentes:** [Referencias](#referencias)

**Categoría:** AI Security / LLM Security  
**Clasificación OWASP:** LLM01:2025 — Prompt Injection

## 1. ¿Qué es Prompt Injection?

Prompt Injection es una técnica de ataque contra sistemas basados en modelos de lenguaje de gran tamaño (LLM), mediante la cual un atacante introduce instrucciones diseñadas para alterar el comportamiento previsto del modelo o de la aplicación que lo utiliza.

Su objetivo es conseguir que el sistema procese instrucciones no autorizadas, modificando sus respuestas, eludiendo restricciones o provocando acciones ajenas a su función original.

Este riesgo es especialmente relevante en aplicaciones que integran modelos de lenguaje con información externa, herramientas, bases de datos o servicios automatizados.

## 2. ¿Cómo funciona?

Los modelos de lenguaje procesan información contextual que puede incluir instrucciones del sistema, mensajes del desarrollador, solicitudes del usuario y contenido procedente de fuentes externas.

Aunque las aplicaciones modernas establecen jerarquías de instrucciones y mecanismos para distinguir estos elementos, el modelo puede interpretar contenido no confiable como si fueran instrucciones que debe seguir.

La vulnerabilidad aparece cuando se cruza una **frontera de confianza (**_**trust boundary**_**)**: información que debería tratarse únicamente como datos termina influyendo indebidamente en el comportamiento del sistema.

Por tanto, una distinción fundamental en la seguridad de aplicaciones LLM es identificar qué contenido tiene autoridad para establecer instrucciones y qué contenido debe procesarse exclusivamente como información.

## 3. Tipos de Prompt Injection

### 3.1. Direct Prompt Injection

Ocurre cuando un atacante introduce instrucciones directamente a través de una interfaz de entrada accesible, con el propósito de modificar el comportamiento previsto del modelo.

El atacante controla el contenido de su entrada, pero no debería poder utilizarla para sustituir instrucciones de mayor prioridad ni obtener capacidades que la aplicación no le ha autorizado.

### 3.2. Indirect Prompt Injection

Ocurre cuando las instrucciones maliciosas se encuentran incorporadas en fuentes externas que el sistema consulta o procesa durante una tarea legítima.

Estas instrucciones no necesitan llegar mediante una conversación directa entre el atacante y el modelo.

Entre las posibles superficies de entrada se encuentran:

- Páginas web y contenido HTML.
    
- Correos electrónicos y archivos adjuntos.
    
- Documentos PDF y archivos de texto.
    
- Repositorios de código y documentación técnica.
    
- Respuestas de herramientas, servicios y API.
    
- Información recuperada mediante sistemas RAG (_Retrieval-Augmented Generation_).
    

El riesgo surge cuando la aplicación incorpora ese contenido a su contexto y el modelo responde a las instrucciones incrustadas como si fueran legítimas.

## 4. Causa fundamental: límites de confianza

Una de las principales dificultades de seguridad en los LLM es mantener una separación fiable entre instrucciones autorizadas y contenido no confiable.

Los modelos pueden haber sido entrenados para respetar prioridades entre instrucciones, pero su capacidad para reconocerlas no equivale a un mecanismo de autorización independiente.

Los formatos de mensajes, roles y delimitadores ayudan a organizar el contexto y reducir la probabilidad de confusión. Sin embargo, no sustituyen los controles de seguridad implementados por la aplicación.

**El problema fundamental no es únicamente el contenido de un prompt malicioso, sino la posibilidad de que datos sin autoridad sean tratados como instrucciones autorizadas.**

## 5. Riesgos e impacto

Las consecuencias de un ataque exitoso dependen de la arquitectura, los permisos y las capacidades del sistema afectado.

Los impactos potenciales incluyen:

- **Manipulación de respuestas:** alteración de la información generada por el modelo.
    
- **Divulgación de información sensible:** exposición de datos confidenciales accesibles para la aplicación.
    
- **Acciones no autorizadas:** utilización indebida de funciones, herramientas o servicios conectados.
    
- **Alteración de procesos:** modificación del comportamiento esperado en tareas automatizadas.
    
- **Compromiso de decisiones:** influencia indebida sobre procesos que dependen de las respuestas del modelo.
    

La superficie de riesgo aumenta cuando una aplicación puede acceder a información sensible o realizar operaciones sobre sistemas externos.

No obstante, incluso una aplicación sin herramientas ni permisos elevados puede sufrir problemas de integridad en sus respuestas.

## 6. Prompt Injection en sistemas con herramientas y agentes

En aplicaciones tradicionales, un LLM puede limitarse a generar texto.

En sistemas que incorporan agentes o capacidades de ejecución, el modelo también puede participar en decisiones operativas, consultar información, utilizar herramientas o solicitar acciones.

Esto amplía las posibles consecuencias de una inyección de instrucciones.

Por ello, el modelo no debe actuar como único mecanismo de autorización. Las operaciones sensibles requieren controles independientes que validen los permisos, el contexto y la legitimidad de las acciones solicitadas.

## 7. Principios generales de mitigación

Aunque no existe una defensa universal que garantice la eliminación completa del riesgo, es posible reducir su probabilidad e impacto mediante una estrategia de seguridad por capas.

Entre los principios fundamentales se encuentran:

- Establecer y mantener límites de confianza entre instrucciones y datos.
    
- Aplicar el principio de mínimo privilegio.
    
- Restringir las herramientas y recursos disponibles para el modelo.
    
- Validar entradas, salidas y operaciones críticas.
    
- Incorporar controles de autorización fuera del LLM.
    
- Requerir aprobación humana para acciones de alto impacto cuando corresponda.
    
- Realizar evaluaciones de seguridad y pruebas adversariales periódicas.
    

Las instrucciones defensivas y los filtros pueden fortalecer el sistema, pero no deben considerarse suficientes de manera aislada.

## 8. Conclusión

Prompt Injection representa un riesgo importante para las aplicaciones basadas en modelos de lenguaje porque aprovecha las dificultades para mantener separados los datos no confiables y las instrucciones autorizadas.

Su impacto no depende exclusivamente del modelo, sino también de la arquitectura de la aplicación, sus integraciones y los privilegios disponibles.

La seguridad requiere combinar mecanismos de protección del modelo con controles verificables en los componentes externos que gestionan datos, permisos y acciones.

---

## Referencias

- [OWASP — LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
    
- [Microsoft Learn — Prompt Injection (Direct / Indirect)](https://learn.microsoft.com/en-us/security/zero-trust/catalog-ai-attack-techniques/prompt-injection)
    
- TryHackMe — Material formativo sobre Prompt Injection, consultado como base de estudio.