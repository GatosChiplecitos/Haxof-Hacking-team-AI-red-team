# Ruta de estudio: Prompt Security y AI Supply Chain Security

- **Autor:** Equipo Haxof (curaduría inicial; atribución individual pendiente)
- **Fecha:** 2026-11-10
- **Última actualización:** 2026-11-10
- **Estado:** Por explorar
- **Etiquetas:** ai-red-team, prompt-security, supply-chain, llm-security, formacion
- **Fuentes:** [TryHackMe](https://tryhackme.com/), [AI Security Bootcamp](https://github.com/AI-Security-Bootcamp/aisb), [PortSwigger](https://portswigger.net/web-security/llm-attacks), [OWASP GenAI](https://genai.owasp.org/), [MITRE ATLAS](https://atlas.mitre.org/), [NIST](https://csrc.nist.gov/pubs/ai/100/2/e2025/final)

## Objetivo y contexto

Conservar las fuentes y los siguientes pasos de aprendizaje técnico para el área **AI Red Team** de Haxof. Esta nota es una **ruta de lectura y práctica pendiente**, no un informe de resultados ni una certificación de que todos los ejercicios externos fueron reproducidos.

La estrategia es: **práctica guiada → profundización técnica → metodología y documentación reproducible**.

## 1. TryHackMe: módulos a documentar

Estas son las diez rooms identificadas para convertir en notas propias de laboratorio. Separar la realización de los ejercicios de la **documentación pendiente**, sin copiar íntegramente el contenido de terceros.

**Prompt Security**

- [Prompt Injection](https://tryhackme.com/room/promptinjectionls)
- [Jailbreaking](https://tryhackme.com/room/jailbreaking)
- [Prompt Defence](https://tryhackme.com/room/promptdefence)
- [LLMborghini](https://tryhackme.com/room/llmborghini)
- [White Rabbit](https://tryhackme.com/room/whiterabbit)

**AI Supply Chain Security**

- [Understanding AI Supply Chains](https://tryhackme.com/room/understanding-ai-supplychains)
- [Supply Chain Attack Vectors](https://tryhackme.com/room/supplychain-attack-vectors)
- [Securing the AI Supply Chain](https://tryhackme.com/room/securing-the-ai-supplychain)
- [Payload](https://tryhackme.com/room/payload)
- [Checkpoint](https://tryhackme.com/room/checkpoint)

**Temas para documentar en este módulo:** límites de confianza, serialización y cargas Pickle, SafeTensors, verificación SHA-256 y firmas, análisis estático con Fickling/ModelScan, capas Lambda de Keras, dependencias y SBOM, monitoreo de comportamiento de API y evaluación en sandbox.

## 2. AI Security Bootcamp (AISB): profundización

- **Repositorio principal:** https://github.com/AI-Security-Bootcamp/aisb
- **Plan de estudios:** https://github.com/AI-Security-Bootcamp/aisb/blob/main/README.md
- **Licencia del material:** CC BY-NC-SA 4.0. Revisar condiciones antes de redistribuir o adaptar contenido.

**Prioridad inicial (a partir de las rooms de THM):**

1. [1.4 — Prompt Injection y RAG Poisoning](https://github.com/AI-Security-Bootcamp/aisb/tree/main/1.4-prompt-injection): fuentes no confiables, jerarquía de instrucciones y fronteras de confianza.
2. [3.2 — Jailbreaking](https://github.com/AI-Security-Bootcamp/aisb/tree/main/3.2-jailbreaking): evaluaciones adversariales de las salvaguardas.
3. [3.3 — Guardrails](https://github.com/AI-Security-Bootcamp/aisb/tree/main/3.3-guardrails): filtros, clasificadores, LLM-as-judge y medición de defensas.

**Segunda fase:**

4. [2.1 — Coding Agents](https://github.com/AI-Security-Bootcamp/aisb/tree/main/2.1-coding-agents): permisos, herramientas, ejecución de código y riesgos de agentes.
5. [4.2 — Backdooring](https://github.com/AI-Security-Bootcamp/aisb/tree/main/4.2-backdooring): envenenamiento, fine-tuning, activadores y controles.
6. [7.1 — Threat Modeling](https://github.com/AI-Security-Bootcamp/aisb/tree/main/7.1-threat-modeling): cadenas de ataque y matrices de adversarios.

**Antes de ejecutar:** revisar [Day 0 / requisitos](https://github.com/AI-Security-Bootcamp/aisb/tree/main/day0-setup); varios laboratorios fueron diseñados para infraestructura remota y requieren ML/PyTorch. No asumir que todos corren cómodamente en equipo local de recursos limitados.

## 3. Laboratorios prácticos adicionales

- **PortSwigger Web Security Academy — Web LLM Attacks:** https://portswigger.net/web-security/llm-attacks
  - Profundizar en prompt injection indirecta, abuso de herramientas, integraciones de API y permisos.
  - Registrar entorno, entrada controlada por atacante, resultado, impacto y defensa.

## 4. Marcos de referencia y fuentes de autoridad

- **OWASP GenAI Security Project:** https://genai.owasp.org/
  - Usar para categorías de riesgo y mitigaciones de LLM y aplicaciones con IA.
- **MITRE ATLAS:** https://atlas.mitre.org/
  - Relacionar técnicas adversarias, precondiciones y cadena de ataque.
- **NIST — Adversarial Machine Learning (AI 100-2):** https://csrc.nist.gov/pubs/ai/100/2/e2025/final
  - Consultar definiciones, taxonomías, límites y mitigaciones.
- **MITRE ATT&CK:** https://attack.mitre.org/
  - Complementar amenazas de infraestructura y cadena de suministro con técnicas convencionales.

## 5. Documentación primaria de herramientas

- **Fickling (Trail of Bits):** https://github.com/trailofbits/fickling — inspección estática de Pickle.
- **ModelScan (Protect AI):** https://github.com/protectai/modelscan — detección de operaciones peligrosas en archivos de modelos.
- **SafeTensors:** https://github.com/huggingface/safetensors — formato seguro para tensores.
- **pip-audit:** https://github.com/pypa/pip-audit — vulnerabilidades conocidas de dependencias Python.
- **Syft (Anchore):** https://github.com/anchore/syft — inventario de componentes y generación de SBOM.
- **Documentación pip (índices de paquetes):** https://pip.pypa.io/en/stable/cli/pip_install/
  - **Observación para verificar/documentar:** `extra-index-url` no crea por sí mismo prioridad segura para paquetes privados; no usarlo como garantía automática contra dependency confusion.

## 6. Flujo de documentación hacia Haxof

Para cada laboratorio, crear un archivo Markdown propio en `areas/hacking/ai-red-team/laboratorios/`; enlazar en el índice correspondiente y desde notas de teoría/herramientas cuando proceda.

Registrar, conforme a `CONTRIBUTING.md`:

1. Objetivo, amenaza y límites de confianza.
2. Fuente original, entorno, versiones y requisitos.
3. Procedimiento reproducible y comandos con explicación.
4. Resultados observados y evidencias (sin credenciales ni información sensible).
5. Interpretación, mitigaciones, límites, dudas y referencias.

**Estados:** «Por explorar» para fuentes pendientes; «En pruebas» para ejercicios en curso; «Validado» solo para hallazgos reproducidos con evidencia, entorno y versión documentados.

## Límites y pendientes

- Priorizar primero el resumen de avances de los módulos de TryHackMe y después las notas individuales.
- Contrastar afirmaciones de cursos con documentación primaria cuando existan discrepancias.
- El repositorio puede estar disponible públicamente: no publicar secretos, datos de clientes, tokens, claves, información interna, ni material sin derechos de redistribución.
- Revisar licencia y atribución de cada fuente; documentar con redacción y hallazgos propios.
