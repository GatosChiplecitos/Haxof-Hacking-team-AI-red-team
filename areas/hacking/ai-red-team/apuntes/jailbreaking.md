# Jailbreaking

- **Autor:** Atribución individual pendiente
- **Fecha:** 2026-11-12
- **Última actualización:** 2026-11-12
- **Estado:** Por explorar
- **Etiquetas:** ai-security, llm-security, jailbreaking, ai-safety
- **Fuentes:** [Referencias](#referencias)

**Categoría:** AI Security / LLM Security  
**Área relacionada:** AI Safety / Safety Alignment

## 1. ¿Qué es Jailbreaking?

Jailbreaking es una técnica de ataque adversarial que busca eludir los mecanismos de seguridad y las restricciones de comportamiento de un modelo de lenguaje de gran tamaño (LLM).

Su objetivo es inducir al modelo a generar respuestas o realizar acciones que normalmente rechazaría debido a su entrenamiento de seguridad, políticas de uso o mecanismos de protección.

A diferencia de una vulnerabilidad tradicional de software, un jailbreak no requiere necesariamente explotar un error de programación. Puede aprovechar las limitaciones del modelo para interpretar instrucciones, evaluar solicitudes y mantener un comportamiento alineado.

## 2. Diferencias entre Prompt Injection y Jailbreaking

Aunque ambos conceptos están relacionados, es útil distinguirlos por su objetivo principal.

|Característica|Prompt Injection|Jailbreaking|
|---|---|---|
|Objetivo|Alterar el comportamiento previsto de una aplicación LLM|Eludir restricciones de seguridad del modelo|
|Superficie principal|Entradas o contenido no confiable procesado por la aplicación|Interacción adversarial con el modelo|
|Problema central|Cruce indebido de límites de confianza|Fallo de las salvaguardas de seguridad|
|Impacto|Manipulación de respuestas, datos o acciones|Generación de contenido o comportamiento restringido|
|Protección|Controles de aplicación, aislamiento y autorización|Alineación, evaluaciones y salvaguardas del modelo|

Esta distinción no es absoluta. Existen ataques que combinan ambos mecanismos y clasificaciones, como la de OWASP, que utilizan una definición más amplia de Prompt Injection.

**La diferencia fundamental está en qué propiedad de seguridad se intenta vulnerar: la integridad de las instrucciones de una aplicación o las restricciones de seguridad del modelo.**

## 3. ¿Qué es Safety Alignment?

_Safety Alignment_ es el conjunto de procesos y técnicas utilizados para orientar el comportamiento de un modelo hacia determinados objetivos, restricciones y principios de seguridad.

Durante su desarrollo, los modelos pueden pasar por distintas etapas de entrenamiento destinadas a mejorar su utilidad, reducir respuestas perjudiciales y fomentar el cumplimiento de políticas.

Entre las técnicas utilizadas se encuentran:

- **Supervised Fine-Tuning (SFT):** ajuste supervisado mediante ejemplos de comportamiento esperado.
    
- **Reinforcement Learning from Human Feedback (RLHF):** entrenamiento mediante señales de preferencia derivadas de evaluaciones humanas.
    
- **Reinforcement Learning from AI Feedback (RLAIF):** utilización de retroalimentación generada por sistemas de IA para orientar el entrenamiento.
    
- **Constitutional AI:** metodología que emplea principios explícitos para orientar la evaluación y revisión de respuestas durante el entrenamiento.
    

Estas técnicas permiten mejorar el comportamiento del modelo, pero no garantizan que todas sus respuestas futuras respeten las restricciones establecidas.

## 4. ¿Por qué puede fallar la alineación de seguridad?

Los modelos de lenguaje aprenden patrones y comportamientos a partir de sus procesos de entrenamiento.

Las respuestas de rechazo pueden formar parte de esos comportamientos aprendidos. Sin embargo, el modelo debe generalizar lo aprendido a situaciones, formulaciones y contextos que no necesariamente estuvieron presentes durante su entrenamiento.

Por esta razón, las salvaguardas pueden presentar limitaciones frente a entradas adversariales.

Entre los factores relevantes se encuentran:

- La sensibilidad del comportamiento a la formulación y al contexto.
    
- Los conflictos entre objetivos de utilidad y restricciones de seguridad.
    
- La dificultad de generalizar a situaciones adversariales no observadas.
    
- La influencia del historial conversacional sobre las respuestas.
    
- Las limitaciones de los mecanismos automáticos para identificar solicitudes problemáticas.
    

Es importante distinguir entre las salvaguardas aprendidas durante el entrenamiento y los controles externos implementados por una aplicación.

Los sistemas de producción pueden incorporar filtros, validaciones, políticas de acceso y mecanismos independientes de autorización.

## 5. Helpfulness y Harmlessness

Uno de los desafíos del diseño de modelos alineados consiste en equilibrar dos objetivos:

- **Helpfulness:** proporcionar respuestas útiles y apropiadas para las necesidades legítimas del usuario.
    
- **Harmlessness:** evitar respuestas que puedan facilitar daños o incumplir restricciones de seguridad.
    

Estos objetivos pueden entrar en tensión.

Un sistema excesivamente restrictivo podría rechazar solicitudes legítimas. Por otro lado, uno que priorice indiscriminadamente el cumplimiento de instrucciones puede facilitar resultados perjudiciales.

La investigación en alineación busca mejorar este equilibrio, reduciendo tanto las respuestas inseguras como los rechazos innecesarios.

El objetivo no consiste simplemente en aumentar la frecuencia de los rechazos, sino en conseguir que el modelo distinga de manera más fiable entre solicitudes legítimas y solicitudes que requieren restricciones.

## 6. Jailbreaking en conversaciones de múltiples turnos

Los ataques de _Multi-Turn Jailbreaking_ utilizan el desarrollo de una conversación como parte de la interacción adversarial.

A diferencia de los intentos realizados mediante una sola entrada, estos ataques aprovechan el contexto acumulado durante varios intercambios.

El modelo debe interpretar cada mensaje considerando tanto la solicitud actual como el historial de conversación.

Esto introduce desafíos relacionados con:

- La consistencia entre respuestas anteriores y posteriores.
    
- La interpretación de intenciones que cambian durante la conversación.
    
- La acumulación de contexto y sus posibles efectos sobre las decisiones.
    
- La aplicación sostenida de políticas de seguridad durante interacciones prolongadas.
    

Un ejemplo investigado de esta categoría es **Crescendo**, documentado por investigadores de Microsoft y publicado en USENIX Security 2025.

Esta investigación demuestra que determinadas estrategias adversariales desarrolladas a lo largo de una conversación pueden comprometer salvaguardas que resultan eficaces ante solicitudes individuales.

No obstante, la efectividad depende del modelo, los controles implementados y las condiciones de evaluación.

## 7. Riesgos asociados

Los ataques de jailbreaking pueden afectar la seguridad y la confiabilidad de sistemas basados en LLM.

Sus posibles consecuencias incluyen:

- Generación de contenido contrario a las políticas de seguridad.
    
- Incumplimiento de restricciones de comportamiento.
    
- Exposición de usuarios a respuestas perjudiciales.
    
- Degradación de la confiabilidad del modelo.
    
- Incremento del riesgo cuando el modelo dispone de herramientas o capacidades externas.
    

Un jailbreak exitoso no implica necesariamente que un atacante haya obtenido acceso al sistema operativo, vulnerado una base de datos o comprometido la infraestructura.

El alcance del impacto debe evaluarse según las capacidades reales del sistema y la naturaleza del comportamiento conseguido.

## 8. Evaluación y mitigación

La reducción del riesgo requiere combinar mejoras en la alineación del modelo con controles técnicos adicionales.

Entre las medidas generales se encuentran:

- Realizar evaluaciones adversariales mediante AI Red Teaming.
    
- Evaluar el comportamiento ante conversaciones prolongadas.
    
- Verificar la consistencia de las respuestas con las políticas de seguridad.
    
- Incorporar mecanismos de detección y supervisión.
    
- Aplicar controles de autorización independientes del modelo.
    
- Limitar los privilegios de agentes y herramientas conectadas.
    
- Reevaluar las salvaguardas después de cambios de modelo o procesos de ajuste fino.
    

La evaluación debe considerar no solo si el modelo rechaza determinadas solicitudes, sino también su capacidad para mantener las restricciones de seguridad en distintos contextos.

## 9. Conclusión

Jailbreaking evidencia las limitaciones de los mecanismos de seguridad de los modelos de lenguaje frente a interacciones adversariales.

La alineación permite orientar el comportamiento de un modelo, pero no constituye una garantía absoluta de cumplimiento.

Comprender estas limitaciones resulta fundamental para diseñar evaluaciones de AI Safety, identificar riesgos y desarrollar aplicaciones que no dependan exclusivamente del comportamiento aprendido del modelo para garantizar su seguridad.

---

## Referencias

- [OWASP — Jailbreak Prompt](https://genai.owasp.org/glossary/jailbreak-prompt/)
    
- [OWASP — LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
    
- [Simon Willison — Prompt Injection and Jailbreaking Are Not the Same Thing](https://simonwillison.net/2024/mar/5/prompt-injection-jailbreaking/)
    
- [Anthropic — Constitutional AI: Harmlessness from AI Feedback](https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback)
    
- [USENIX Security 2025 — The Crescendo Multi-Turn LLM Jailbreak Attack](https://www.usenix.org/conference/usenixsecurity25/presentation/russinovich)
    
- TryHackMe — Material formativo sobre Jailbreaking y Prompt Injection, utilizado como referencia de estudio.