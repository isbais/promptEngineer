# 🚀 Prompting con OpenAI

![OpenAI](https://img.shields.io/badge/Platform-OpenAI-green?style=for-the-badge)
![Documentation](https://img.shields.io/badge/Source-Official%20Docs-blue?style=for-the-badge)

> **📖 Documentación oficial:** [https://platform.openai.com/docs/guides/prompting](https://platform.openai.com/docs/guides/prompting)

**Prompting es el proceso de proporcionar inputs a un modelo.** La calidad de tus outputs dependen de qué tan bien generaste un prompt al modelo.

---

## 📋 Overview

La generación de prompts es tanto **un arte como una ciencia**. OpenAI cuenta con estrategias y decisiones de diseño de API para ayudarte a crear prompts eficaces y obtener resultados consistentemente buenos de un modelo.

---

## 🔧 Prompts in the API

OpenAI proporciona un **objeto de prompt de larga duración**, con versiones y plantillas compartidas por todos los usuarios de un proyecto. Este diseño permite gestionar, probar y reutilizar prompts en todo el equipo, con una definición centralizada para API, SDK y panel de control.

Los **ID de prompts universales** ofrecen flexibilidad para probar y compilar. Las variables y los prompts comparten una prompt base, por lo que al crear una nueva versión, se puede usar para evaluaciones y determinar si un prompt funciona mejor o peor.

---

## 🛠️ Prompting Tools and Techniques

### <span style="color: #FF6B6B;">⚡ Prompt Caching</span>
**Reduce latency by up to 80% and cost by up to 75%**

- **Reduce la latencia y los costes** con el almacenamiento en caché de prompts
- **Model Prompts** suelen contener contenido repetitivo, como prompts del sistema e instrucciones comunes
- **OpenAI enruta** los prompts de API a servidores que procesaron recientemente la misma prompt
- **Beneficios:**
  - 📈 Reduce la latencia hasta en un **80%**
  - 💰 Reduce los costes hasta en un **75%**
  - 🔄 Funciona automáticamente (sin modificar código)
  - 🆓 No tiene costes adicionales
  - ✅ Habilitado para **gpt-4o y posteriores**

### <span style="color: #4ECDC4;">🎯 Prompt Engineering</span>
**Learn strategies, techniques, and tools to construct prompts**

Prompt Engineering es el proceso de **escribir instrucciones efectivas** para un modelo, de modo que genere contenido consistente que cumpla con sus requisitos. Dado que el contenido generado a partir de un modelo no es determinista, la ingeniería de indicaciones para obtener el resultado deseado es **una combinación de arte y ciencia**.

#### 🔄 **Consideraciones importantes:**

Algunas técnicas de ingeniería de indicaciones funcionan con **todos los modelos**, como el uso de roles de mensaje. Sin embargo, los distintos tipos de modelos (como los de **razonamiento** frente a los **modelos GPT**) podrían requerir indicaciones distintas para obtener los mejores resultados.

#### 📋 **Recomendaciones para aplicaciones complejas:**

- ✅ **Fijar las aplicaciones** de producción a instantáneas de modelos específicos (como `gpt-4.1-2025-04-14`) para garantizar un comportamiento consistente
- ✅ **Crear evaluaciones** que midan el comportamiento de las indicaciones para poder supervisar su rendimiento durante la iteración o al cambiar y actualizar las versiones del modelo

---

## 🎛️ Create a Prompt

**Log in and use the OpenAI dashboard** to create, save, version, and share your prompts.

### <span style="color: #96CEB4;">📝 Pasos para crear un prompt</span>

#### 1️⃣ **Start a prompt**
En el **Playground**, llena los campos para crear tu prompt deseado.

#### 2️⃣ **Add prompt variables**
Las **variables** te permiten inyectar valores dinámicos sin cambiar tu prompt. Úsalas en cualquier rol de mensaje usando `{{variable}}`. 

**Ejemplo:** Al crear un prompt de clima local, puedes agregar una variable `city` con el valor `San Francisco`.

#### 3️⃣ **Use the prompt in your Responses API call**
Encuentra tu **prompt ID** y número de versión en la URL, y pásalo como `prompt_id`:

```bash
curl -s -X POST "https://api.openai.com/v1/responses" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $OPENAI_API_KEY" \
-d '{
    "prompt": {
        "prompt_id": "pmpt_123",
        "variables": {
            "city": "San Francisco"
        }
    }
}'
```

### <span style="color: #FECA57;">🔄 Gestión de Versiones</span>

#### **Create a new prompt version**
Las **versiones** te permiten iterar en tus prompts sin sobrescribir detalles existentes. Puedes usar todas las versiones en la API y evaluar su rendimiento entre sí. El prompt ID apunta a la última versión publicada a menos que especifiques una versión.

**Para crear una nueva versión:** edita el prompt y haz click en **Update**. Recibirás un nuevo prompt ID para copiar y usar en tus llamadas de API.

#### **Roll back if needed**
En el **dashboard de prompts**, selecciona el prompt que quieres revertir. En el lado derecho, haz click en **History**. Encuentra la versión que quieres restaurar y haz click en **Restore**.

---

## 🤖 Reasoning Models vs GPT Models

**Compared to GPT models**, our o-series models excel at different tasks and require different prompts. **One model family isn't better than the other—they're just different.**

### <span style="color: #FF6B6B;">🧠 O-Series Models ("The Planners")</span>

We trained our **o-series models** to think longer and harder about complex tasks, making them effective at:
- 🎯 **Strategizing**
- 📊 **Planning solutions** to complex problems
- 🔍 **Making decisions** based on large volumes of ambiguous information
- ⚡ **Executing tasks** with high accuracy and precision

**Ideales para dominios que requieren experiencia humana:**
- 🔢 Matemáticas
- 🔬 Ciencias
- ⚙️ Ingeniería
- 💰 Servicios financieros
- ⚖️ Servicios legales

### <span style="color: #4ECDC4;">⚡ GPT Models ("The Workhorses")</span>

Our **lower-latency, more cost-efficient GPT models** are designed for **straightforward execution**.

### 🔄 **Estrategia de aplicación:**
Una aplicación puede usar **o-series models** para planificar la estrategia para resolver un problema, y usar **GPT models** para ejecutar tareas específicas, particularmente cuando la **velocidad y el costo** son más importantes que la precisión perfecta.

---

## 🎯 How to Choose

| **Criterio** | **GPT Models** | **O-Series Models** |
|--------------|----------------|---------------------|
| <span style="color: #FF6B6B;">**Speed and cost**</span> | ✅ Más rápidos y menos costosos | ❌ Más lentos y costosos |
| <span style="color: #4ECDC4;">**Well-defined tasks**</span> | ✅ Excelentes para tareas explícitas | ❌ Sobreingeniería para tareas simples |
| <span style="color: #45B7D1;">**Accuracy and reliability**</span> | ❌ Buenos pero no perfectos | ✅ Tomadores de decisiones confiables |
| <span style="color: #96CEB4;">**Complex problem-solving**</span> | ❌ Limitados en problemas complejos | ✅ Trabajan con ambigüedad y complejidad |

---

## 🧠 When to Use Our Reasoning Models

### <span style="color: #FECA57;">🎯 Casos de uso ideales para O-Series:</span>

1. **🔍 Navigating ambiguous tasks**
2. **🔎 Finding a needle in a haystack**
3. **📊 Finding relationships and nuance** across a large dataset
4. **🤖 Multistep agentic planning**
5. **👁️ Visual reasoning**
6. **🐛 Reviewing, debugging, and improving code quality**
7. **📈 Evaluation and benchmarking** for other model responses

---

## 🎨 Cómo hacer Prompting Efectivo para Modelos de Reasoning

### ✅ **Mejores Prácticas:**

#### 1️⃣ <span style="color: #FF6B6B;">**Prompts simples y directos**</span>
Estos modelos funcionan mejor con **instrucciones claras y concisas**. No sobrecargues el prompt.

#### 2️⃣ <span style="color: #4ECDC4;">**Evita chain-of-thought prompting**</span>
**No es necesario** decir "think step by step" o "explain your reasoning". El modelo ya razona internamente y este tipo de indicaciones puede incluso **empeorar el resultado**.

#### 3️⃣ <span style="color: #45B7D1;">**Usa delimitadores**</span>
Separa secciones con claridad usando:
- **Títulos**
- **Etiquetas** como `<xml>`
- **Markdown** (`### Sección`)

#### 4️⃣ <span style="color: #96CEB4;">**Empieza con zero-shot, luego few-shot si es necesario**</span>
- 🥇 **Primero prueba sin ejemplos**
- 📚 **Si el resultado no es el esperado**, incluye algunos ejemplos (few-shot)
- ✅ **Asegúrate** de que estén bien alineados con las instrucciones del prompt

#### 5️⃣ <span style="color: #FECA57;">**Indica reglas claras**</span>
Si necesitas **limitar o guiar la respuesta** (ej: "bajo un presupuesto de $500"), pon esas restricciones **explícitamente en el prompt**.

#### 6️⃣ <span style="color: #FF9FF3;">**Define claramente el objetivo final**</span>
Especifica **cómo debe lucir una respuesta exitosa** y anima al modelo a **iterar hasta cumplir** con ese criterio.

#### 7️⃣ <span style="color: #95E1D3;">**Uso de developer messages**</span>
Desde la versión **o1-2024-12-17**, los modelos de reasoning usan **developer messages** en lugar de system messages para seguir la jerarquía de control (chain of command).

#### 8️⃣ <span style="color: #FFA07A;">**Markdown formatting (formato)**</span>
Ya **no se genera markdown por defecto**. Si lo necesitas, agrega la frase **"Formatting re-enabled"** al inicio del developer message.

---

<p align="center">
  <img src="https://img.shields.io/badge/Status-Gu%C3%ADa%20Completa-success?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/Fuente-OpenAI%20Official-blue?style=for-the-badge" alt="Source">
</p>

---

*📅 Última actualización: 2024*  
*👤 Autor: OpenAI Documentation Guide*  
*📧 Contacto: natham@bais.cl*
