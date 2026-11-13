# Prompt Defense

- **Autor:** Atribución individual pendiente
- **Fecha:** 2026-11-12
- **Última actualización:** 2026-11-12
- **Estado:** Por explorar
- **Etiquetas:** ai-security, llm-security, prompt-defense, guardrails
- **Fuentes:** [Referencias](#referencias)

**Categoría:** AI Security / LLM Security  
**Área relacionada:** Prompt Injection / Jailbreaking / Defensive AI Security

## 1. ¿Qué es Prompt Defense?

Prompt Defense comprende el conjunto de estrategias, controles y mecanismos diseñados para reducir los riesgos asociados con ataques que manipulan las instrucciones y el comportamiento de los modelos de lenguaje de gran tamaño (LLM).

Estas defensas buscan prevenir, detectar o limitar ataques como Prompt Injection y Jailbreaking, protegiendo tanto el comportamiento del modelo como los recursos de las aplicaciones que lo integran.

A diferencia de muchas vulnerabilidades tradicionales de software, los ataques basados en prompts pueden aprovechar las limitaciones de interpretación y generalización de los modelos, sin necesidad de explotar un defecto convencional de programación.

Por ello, la protección no debe depender exclusivamente de las instrucciones proporcionadas al modelo.

## 2. Limitaciones de las defensas basadas en prompts

Los modelos de lenguaje utilizan mecanismos de entrenamiento y jerarquías de instrucciones para orientar sus respuestas.

Sin embargo, estos mecanismos no garantizan que un modelo interprete correctamente todas las entradas adversariales.

Modificar las instrucciones de sistema puede fortalecer el comportamiento esperado, pero no garantiza la eliminación del riesgo de Prompt Injection o Jailbreaking.

La seguridad debe considerar que una instrucción maliciosa podría influir en el modelo a pesar de las medidas implementadas.

**El objetivo de Prompt Defense no es asumir que el modelo nunca fallará, sino construir un sistema capaz de prevenir, detectar y contener las consecuencias de posibles fallos.**

## 3. Defensa en profundidad (Defense in Depth)

La defensa en profundidad consiste en implementar múltiples capas de protección para evitar que la seguridad dependa de un único mecanismo.

En aplicaciones basadas en LLM, estas capas actúan en distintas etapas del procesamiento de información.

|Capa de defensa|Función principal|
|---|---|
|System Prompt Hardening|Fortalecer las instrucciones y delimitar el comportamiento esperado|
|Input Guardrails|Detectar entradas potencialmente maliciosas|
|Separación de confianza|Distinguir instrucciones autorizadas de información externa|
|Deployment Controls|Restringir permisos, herramientas y recursos disponibles|
|Output Validation|Validar respuestas y acciones antes de utilizarlas|
|Monitoring & Evaluation|Identificar comportamientos anómalos y evaluar la efectividad de las defensas|

La combinación de estos controles permite reducir tanto la probabilidad de un ataque exitoso como su impacto potencial.

## 4. System Prompt Hardening

El endurecimiento del _system prompt_ consiste en diseñar instrucciones de sistema claras, delimitadas y coherentes con la función de la aplicación.

Entre sus principios fundamentales se encuentran:

- **Tight Scoping:** definir explícitamente el propósito, las funciones y los límites operativos del asistente.
    
- **Explicit Refusal Instructions:** establecer cómo debe responder el modelo ante solicitudes que contradigan sus restricciones.
    
- **Persona Restriction:** limitar la adopción de roles o identidades incompatibles con el comportamiento autorizado.
    

Estas medidas ayudan al modelo a identificar las instrucciones que debe priorizar.

Sin embargo, el system prompt no debe considerarse una barrera de seguridad independiente.

### Información sensible en instrucciones de sistema

Las instrucciones de sistema no deben utilizarse para almacenar secretos.

Esto incluye credenciales, claves API, tokens de autenticación y cadenas de conexión.

La posibilidad de que un modelo revele parcial o totalmente sus instrucciones representa un riesgo adicional cuando contienen información confidencial.

OWASP clasifica este problema como **LLM07:2025 — System Prompt Leakage**.

Los datos sensibles y los controles de autorización deben mantenerse fuera del contexto accesible al modelo siempre que sea posible.

## 5. Separación estructural de instrucciones

Las API modernas de LLM permiten organizar los mensajes mediante roles, como _system_, _developer_, _user_ y _tool_, según el proveedor.

Esta organización facilita la identificación de las distintas fuentes de información.

Las buenas prácticas incluyen:

- Mantener las instrucciones confiables separadas de las entradas de usuarios.
    
- No incorporar contenido externo no confiable dentro de instrucciones privilegiadas.
    
- Identificar claramente los documentos y resultados recuperados como datos.
    
- Utilizar interfaces estructuradas y delimitadores cuando corresponda.
    

Formatos como ChatML y Harmony permiten representar estructuras conversacionales y prioridades de instrucciones.

Sin embargo, **la separación estructural no sustituye los mecanismos de autorización y aislamiento implementados fuera del modelo**.

## 6. Input Guardrails

Los _Input Guardrails_ son mecanismos que analizan información antes de incorporarla al procesamiento del modelo.

Su objetivo es identificar entradas potencialmente adversariales, contenido restringido o patrones incompatibles con las políticas de la aplicación.

Pueden utilizar distintas técnicas:

### 6.1. Blocklists y filtros de patrones

Las listas de bloqueo permiten identificar expresiones o secuencias previamente definidas.

Son útiles para detectar patrones conocidos mediante comprobaciones relativamente sencillas.

Su principal limitación es que los ataques pueden expresarse de múltiples maneras, incluyendo variaciones semánticas, cambios de formato y representaciones codificadas.

Por ello, las blocklists no deben constituir el mecanismo principal de protección frente a instrucciones adversariales.

### 6.2. Clasificadores basados en IA

Los clasificadores utilizan modelos especializados para identificar características asociadas con entradas maliciosas.

A diferencia de los filtros basados exclusivamente en coincidencias de texto, pueden reconocer determinados patrones semánticos y generalizar a variantes no idénticas a las observadas durante su entrenamiento.

Un ejemplo es **Llama Prompt Guard 2**, desarrollado por Meta, que dispone de versiones de 86 y 22 millones de parámetros.

Estos modelos clasifican entradas como benignas o maliciosas y están orientados a la detección de Prompt Injection y Jailbreaking.

No obstante, los clasificadores también pueden presentar errores y ser vulnerables a entradas adversariales.

Su rendimiento depende de las características del conjunto de entrenamiento, los idiomas, los umbrales de clasificación y el contexto de aplicación.

## 7. Output Guardrails

Los _Output Guardrails_ permiten inspeccionar y validar el contenido generado por un modelo antes de que sea entregado al usuario o utilizado por otros componentes.

Entre sus funciones se encuentran:

- Detectar posibles filtraciones de información sensible.
    
- Identificar respuestas incompatibles con las políticas de seguridad.
    
- Verificar la estructura y el formato de los datos generados.
    
- Validar parámetros y solicitudes de acciones dirigidas a herramientas externas.
    
- Reducir el riesgo de que contenido no confiable afecte sistemas posteriores.
    

La validación de acciones debe realizarse antes de su ejecución.

Es importante diferenciar la revisión de contenido generado de los controles deterministas de seguridad. Un filtro puede detectar respuestas sospechosas, pero las comprobaciones de permisos y autorización deben implementarse independientemente.

## 8. Protección frente a Indirect Prompt Injection

Los ataques indirectos representan un desafío particular porque las instrucciones adversariales pueden encontrarse en contenido externo que la aplicación procesa durante una tarea legítima.

Entre las superficies relevantes se encuentran:

- Documentos y archivos adjuntos.
    
- Correos electrónicos.
    
- Páginas web.
    
- Resultados de herramientas.
    
- Información recuperada mediante RAG.
    

Una aplicación que únicamente inspecciona los mensajes directos del usuario puede pasar por alto instrucciones maliciosas incorporadas posteriormente mediante fuentes externas.

Por ello, los mecanismos de protección deben considerar todos los puntos de incorporación de información no confiable.

Es importante establecer controles de procedencia, límites de confianza y validación sobre los datos recuperados.

El contenido externo puede proporcionar información útil para una tarea, pero no debe adquirir automáticamente autoridad para modificar instrucciones o solicitar acciones.

## 9. Deployment Controls

Los controles de despliegue establecen restricciones sobre las capacidades efectivas de las aplicaciones que utilizan LLM.

Su importancia aumenta cuando el modelo puede interactuar con herramientas, archivos, servicios o bases de datos.

Entre las medidas recomendadas se encuentran:

- Aplicar el principio de mínimo privilegio.
    
- Implementar controles de acceso y autorización independientes.
    
- Restringir las operaciones disponibles para agentes y herramientas.
    
- Validar los parámetros de las acciones solicitadas.
    
- Utilizar entornos aislados para operaciones de riesgo.
    
- Solicitar aprobación humana para acciones sensibles cuando corresponda.
    

Estas medidas permiten limitar el impacto de un ataque incluso cuando las defensas implementadas en el modelo resultan insuficientes.

## 10. Limitaciones y evaluación de los Guardrails

La incorporación de controles adicionales también introduce desafíos operativos.

Entre ellos destacan:

**False Positives:** solicitudes legítimas clasificadas incorrectamente como maliciosas.

**False Negatives:** entradas adversariales que no son detectadas.

**Latency:** tiempo adicional requerido para ejecutar mecanismos de validación.

**Generalization:** capacidad de los controles para detectar ataques distintos a los utilizados durante su entrenamiento.

La evaluación de estas defensas requiere considerar métricas de precisión, cobertura, latencia y efectividad frente a diferentes escenarios adversariales.

No existe un único mecanismo de clasificación que garantice una protección completa frente a todas las variantes de ataques basados en prompts.

Por ello, es necesario realizar pruebas periódicas y revisar los resultados cuando cambian los modelos, las integraciones o las condiciones de despliegue.

## 11. Conclusión

Prompt Defense requiere una estrategia de seguridad integral que combine mecanismos de protección del modelo con controles independientes dentro de la aplicación.

El endurecimiento de instrucciones, los guardrails y la separación estructural contribuyen a reducir la exposición a ataques adversariales.

Sin embargo, las restricciones de permisos, la validación de operaciones y el aislamiento de recursos son fundamentales para contener las consecuencias de un posible compromiso del comportamiento del modelo.

**La seguridad de una aplicación LLM no debe depender únicamente de que el modelo interprete correctamente las instrucciones, sino de que el sistema mantenga sus controles incluso cuando el modelo comete errores.**

---

## Referencias

- [OWASP — LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
    
- [OWASP — LLM07:2025 System Prompt Leakage](https://genai.owasp.org/llmrisk/llm072025-system-prompt-leakage/)
    
- [OWASP — LLM Prompt Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html)
    
- [Meta — Llama Prompt Guard 2](https://dev.meta.ai/llama/docs/model-cards-and-prompt-formats/prompt-guard)
    
- [OpenAI — Designing AI Agents to Resist Prompt Injection](https://openai.com/index/designing-agents-to-resist-prompt-injection/)
    
- TryHackMe — Material formativo sobre Prompt Defense, utilizado como referencia de estudio.