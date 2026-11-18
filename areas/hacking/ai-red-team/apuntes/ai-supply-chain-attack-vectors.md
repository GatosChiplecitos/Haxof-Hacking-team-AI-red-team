# Vectores de ataque en la cadena de suministro de IA

- **Autor:** Atribución individual pendiente
- **Fecha:** 2026-11-17
- **Última actualización:** 2026-11-17
- **Estado:** Por explorar
- **Etiquetas:** ai-security, supply-chain, model-security, dependencies, api-security
- **Fuentes:** [Referencias](#referencias)

**Categoría:** AI Security / Software Supply Chain  
**Área relacionada:** Seguridad de modelos / Gestión de dependencias / Seguridad de APIs

## 1. Carga de modelos y deserialización insegura

La **serialización** convierte datos u objetos en una representación que se puede guardar o transmitir. La **deserialización** reconstruye esos datos u objetos. En aprendizaje automático, esto permite guardar parámetros y recuperar un modelo entrenado para volver a utilizarlo.

`pickle` puede reconstruir objetos complejos de Python. Durante esa reconstrucción, un archivo manipulado puede provocar que el proceso ejecute código con los permisos del cargador. Algunos _checkpoints_ de PyTorch utilizan `pickle`, y la opción `weights_only` restringe qué puede reconstruir el cargador. La extensión del archivo sirve como pista. Para evaluar el riesgo hay que identificar el formato real, el cargador y su configuración. [Python 3.15.2 documentation](https://docs.python.org/3/library/pickle.html)

|Superficie|Qué se debe revisar|
|---|---|
|**Serialización**|Qué operaciones permite el cargador al leer el archivo.|
|**Arquitectura**|Qué capas y componentes personalizados se incorporan al modelo y qué puede ejecutar el entorno al reconstruirlos o usarlos.|
|**Pesos**|Cómo se comporta el modelo con entradas relevantes para el despliegue, incluidas condiciones poco frecuentes que una métrica global puede ocultar.|

Keras advierte que ciertas capas `Lambda` serializadas tienen riesgos al cargarse. **SafeTensors** guarda tensores y metadatos, y **GGUF** utiliza su propio formato. Ambos evitan la deserialización mediante `pickle` para esos archivos. Si el despliegue sigue usando código externo para reconstruir la arquitectura, ese código conserva su propio riesgo. Cambiar el formato tampoco valida el comportamiento de los pesos. [keras.io](https://keras.io/api/layers/core_layers/lambda/)

## 2. Dependencias y resolución de paquetes

Cuando se configura `pip` para consultar varios índices, el instalador considera candidatos de todos ellos. El índice privado carece de prioridad automática. La elección depende de los candidatos disponibles y de las restricciones aplicables, como las versiones permitidas y la compatibilidad. [pip documentation v26.3](https://pip.pypa.io/en/stable/cli/pip_install/)

|Vector|Cómo se distingue|
|---|---|
|**Confusión de dependencias** (_dependency confusion_)|El nombre solicitado es el correcto, pero el instalador puede resolverlo desde un origen distinto del previsto.|
|**Typosquatting**|Un nombre parecido al legítimo aprovecha errores de escritura o de lectura. Si ese nombre existe, una instalación puede terminar correctamente aunque se haya seleccionado otro paquete.|

Para reducir ambos riesgos, conviene revisar los nombres de las dependencias, resolverlas desde fuentes aprobadas y fijar las versiones y los hashes de los artefactos revisados. `pip-audit` ayuda a identificar vulnerabilidades conocidas. La procedencia y la confianza en los paquetes requieren una revisión aparte. [pip documentation v26.3](https://pip.pypa.io/en/stable/topics/secure-installs/)

## 3. Repositorios y procedencia de modelos

Un nombre parecido al de una organización conocida puede inducir a descargar un modelo distinto del esperado. También puede alterarse un repositorio legítimo si se compromete una cuenta con permisos de publicación. En ese caso, el nombre y el historial del repositorio podrían seguir pareciendo normales.

La tarjeta del modelo, la insignia de verificación, la antigüedad y el número de descargas aportan contexto. **La verificación debe llegar al artefacto concreto.** Conviene partir de un enlace publicado por la organización, fijar una revisión concreta del repositorio y comprobar la integridad del archivo frente a una referencia confiable cuando esté disponible. [Hugging Face](https://huggingface.co/docs/huggingface_hub/main/guides/download)

## 4. Riesgos al consumir modelos por API

|Riesgo|Qué puede cambiar|Control defensivo|
|---|---|---|
|**Actualización del modelo**|El comportamiento puede variar aunque el endpoint siga siendo el mismo.|Fijar una versión cuando sea posible, registrar el identificador disponible y ejecutar pruebas de regresión.|
|**Exposición de una clave**|Un tercero puede operar con los permisos de esa clave. El acceso efectivo a datos u operaciones depende de su alcance.|Limitar permisos, almacenar la clave en un gestor de secretos y revocarla ante una exposición.|
|**Alteración de plantillas externas**|Una actualización de la plantilla puede cambiar las instrucciones que usa la aplicación.|Versionar las plantillas y revisar los cambios antes de incorporarlos.|
|**Alteración de datos de entrenamiento**|El modelo puede responder de forma sistemáticamente incorrecta en condiciones que las métricas generales pasan por alto.|Evaluar casos relevantes de forma periódica y mantener revisión humana en decisiones de alto impacto.|

## 5. Implicaciones para AI Security

Los controles deben corresponder al punto donde se toma cada decisión. Un formato de tensores reduce el riesgo asociado a la deserialización, mientras que el control de índices protege la selección de dependencias. Fijar una revisión permite saber exactamente qué artefacto se evaluó. Las pruebas de regresión ayudan a detectar cambios en el comportamiento del sistema que se utiliza en producción. [Hugging Face](https://huggingface.co/docs/safetensors/index)

## Referencias

- [TryHackMe — Supply Chain Attack Vectors](https://tryhackme.com/room/supplychain-attack-vectors)
- [Python — Documentación de `pickle`](https://docs.python.org/3/library/pickle.html) · [PyTorch — `torch.load`](https://docs.pytorch.org/docs/stable/generated/torch.load.html) · [Keras — Capas `Lambda`](https://keras.io/api/layers/core_layers/lambda/)
- [PyPA — Resolución de paquetes con `pip`](https://pip.pypa.io/en/stable/cli/pip_install/) · [Instalaciones seguras](https://pip.pypa.io/en/stable/topics/secure-installs/) · [`pip-audit`](https://github.com/pypa/pip-audit)
- [Hugging Face — SafeTensors](https://huggingface.co/docs/safetensors/) · [GGUF](https://huggingface.co/docs/hub/gguf) · [Descarga por revisión](https://huggingface.co/docs/huggingface_hub/main/guides/download)
- [OWASP — LLM03:2025 Supply Chain](https://genai.owasp.org/llmrisk/llm032025-supply-chain/)
- [HAXOF — Cadena de suministro de IA](ai-supply-chain.md)
