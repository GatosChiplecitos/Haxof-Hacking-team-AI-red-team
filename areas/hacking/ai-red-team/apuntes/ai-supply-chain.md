# Cadena de suministro de IA

- **Autor:** Atribución individual pendiente
- **Fecha:** 2026-11-17
- **Última actualización:** 2026-11-17
- **Estado:** Por explorar
- **Etiquetas:** ai-security, supply-chain, model-security, dependencies
- **Fuentes:** [Referencias](#referencias)

**Categoría:** AI Security / Software Supply Chain  
**Área relacionada:** Seguridad de modelos / Integridad de datos / Gestión de dependencias

## 1. ¿Qué es una cadena de suministro de IA?

La cadena de suministro de software reúne los componentes, proveedores y procesos que una aplicación necesita para construirse, distribuirse y funcionar. Incluye las **dependencias directas**, que el equipo selecciona, y las **dependencias transitivas**, que llegan a través de los paquetes que esas dependencias utilizan.

La IA añade modelos preentrenados, conjuntos de datos, adaptadores y herramientas de entrenamiento e inferencia. Cada elemento introduce una relación de confianza con quienes lo producen, transforman o distribuyen. El alcance de un problema depende de qué sistemas incorporaron el componente y en qué condiciones lo utilizan. [OWASP Gen AI Security Project](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)

Dos incidentes de software ilustran riesgos distintos:

|Caso|Qué ocurrió|Qué muestra|
|---|---|---|
|**SolarWinds Orion (2020)**|El compromiso del proceso de compilación permitió distribuir una actualización alterada por el canal habitual del proveedor.|Un canal legítimo puede distribuir un artefacto comprometido antes de su publicación.|
|**Log4Shell (2021)**|Una vulnerabilidad en Log4j afectó a aplicaciones que utilizaban versiones vulnerables de la biblioteca, incluso como dependencia transitiva.|Una falla en una biblioteca compartida puede extender la exposición a muchos sistemas.|

## 2. Modelos preentrenados y riesgos heredados

La **arquitectura** define cómo se organizan las operaciones del modelo y los **pesos** son los valores que se aprenden durante el entrenamiento. El **aprendizaje por transferencia** aprovecha un modelo entrenado previamente para otra tarea. Una forma de hacerlo es el **ajuste fino** (_fine-tuning_), que adapta sus parámetros a nuevos datos u objetivos.

Métodos como **LoRA** permiten distribuir un adaptador más pequeño que el modelo completo. Cuando el adaptador viene de otro proveedor, el equipo debe evaluar la procedencia y el comportamiento tanto del modelo base como del adaptador y de la combinación final.

Un modelo puede funcionar bien con entradas habituales y presentar un comportamiento no deseado en ciertas condiciones. En experimentos controlados se ha observado que algunas puertas traseras pueden persistir después de etapas posteriores de ajuste. Por eso, conviene complementar las métricas generales con evaluaciones centradas en los comportamientos que importan para el despliegue. [OWASP Gen AI Security Project](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)

## 3. Modelos descargados y modelos por API

|Aspecto|Modelo descargado|Modelo consumido por API|
|---|---|---|
|**Dónde se ejecuta**|En infraestructura bajo control del equipo.|En infraestructura del proveedor o de un intermediario.|
|**Qué controla el equipo**|El artefacto, el entorno de carga y la versión que se despliega.|Los datos enviados, la integración y la validación de las respuestas.|
|**Qué debe verificar**|La procedencia, la integridad, el cargador y las transformaciones de los archivos.|La política de datos, las opciones de versionado y cómo se anuncian o aplican los cambios del servicio.|

En un despliegue propio, una conversión o cuantización de terceros añade un paso a la procedencia del modelo. La **cuantización** utiliza representaciones de menor precisión para reducir el tamaño o el consumo de memoria. En una API, parte de la evidencia sobre la cadena queda en manos del proveedor. Las evaluaciones de regresión ayudan a detectar cambios de comportamiento que afecten a la aplicación. [Hugging Face](https://huggingface.co/docs/hub/gguf)

## 4. Superficies de riesgo

|Capa|Qué se revisa|Riesgo principal|
|---|---|---|
|**Modelo**|Pesos, adaptadores, configuración y código de carga.|Carga insegura de archivos o cambios en el comportamiento del modelo.|
|**Dependencias**|Frameworks, bibliotecas y paquetes directos y transitivos.|Incorporación de componentes vulnerables, alterados o de procedencia dudosa.|
|**Datos**|Fuentes de entrenamiento y evaluación, versiones y transformaciones.|Datos alterados que influyen en lo que aprende el modelo o distorsionan cómo se mide su desempeño.|
|**Infraestructura**|Repositorios, registros, cuentas de publicación y procesos de CI/CD.|Modificación o distribución de artefactos desde un canal que otros equipos consideran confiable.|

Dentro de la capa de modelo conviene distinguir tres puntos de revisión:

- **Serialización.** Importa qué puede hacer el cargador al leer el archivo. Algunos _checkpoints_ usan `pickle`, cuya deserialización sin restricciones puede ejecutar código. El riesgo depende del contenido y del cargador, por lo que la extensión sirve solo como pista. [PyTorch 2.15 documentation](https://docs.pytorch.org/docs/stable/generated/torch.load)
- **Arquitectura.** Importa qué operaciones y componentes se ejecutan al construir el modelo o realizar inferencia. Las capas personalizadas y el código asociado requieren una revisión propia. [OWASP Gen AI Security Project](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)
- **Pesos.** Importa el comportamiento que producen los parámetros al usarse en una arquitectura concreta. Formatos como SafeTensors y GGUF evitan la deserialización mediante `pickle`, mientras que la evaluación del comportamiento y la revisión del software de carga siguen siendo necesarias. [Hugging Face](https://huggingface.co/docs/safetensors/index)

## 5. Implicaciones para AI Security

- **Registrar procedencia y versiones.** Mantener un inventario de modelos, adaptadores, conjuntos de datos y dependencias, con la fuente exacta y las transformaciones conocidas.
- **Verificar la integridad.** Comparar el hash con un valor obtenido de una fuente confiable o verificar la firma cuando esté disponible. Un hash permite comprobar que el archivo coincide con una referencia conocida. La confianza en esa referencia se evalúa por separado.
- **Aislar la incorporación de artefactos.** Revisar y cargar componentes externos en entornos con permisos limitados antes de incorporarlos al despliegue.
- **Evaluar el sistema final.** Comprobar el comportamiento de la combinación que llegará a producción y repetir las evaluaciones relevantes cuando cambien el modelo, los adaptadores, el cargador o el proveedor.

El análisis debe seguir el recorrido del componente hasta el despliegue. Un mismo modelo puede pasar por entrenamiento, ajuste, conversión y distribución antes de que una aplicación lo use. Registrar esas etapas permite identificar qué versión se evaluó y qué parte de la cadena revisar si aparecen cambios de comportamiento. [OWASP Gen AI Security Project](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)

---

## Referencias

- [TryHackMe — Understanding AI Supply Chains](https://tryhackme.com/room/understanding-ai-supplychains)
- [OWASP — LLM03:2025 Supply Chain](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)
- [PyTorch — Serialization semantics](https://docs.pytorch.org/docs/stable/notes/serialization.html)
- [Hugging Face — SafeTensors](https://huggingface.co/docs/safetensors/index) · [GGUF](https://huggingface.co/docs/hub/gguf)
- [Anthropic — Sleeper Agents](https://www.anthropic.com/research/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training)
- [NCSC — SolarWinds Orion Software Supply Chain Attack](https://www.dni.gov/files/NCSC/documents/SafeguardingOurFuture/SolarWinds%20Orion%20Software%20Supply%20Chain%20Attack.pdf) · [Apache — Avisos de seguridad de Log4j](https://logging.apache.org/security.html)
