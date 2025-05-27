# Subtask 2 - Prompts

- [Prompts](#prompts)
- [Metrics](#metrics)
- [Example](./example.json)

## Prompts

### Zero-shot ICL using LLMs

System prompt:

```

Dados los siguientes textos y preguntas, relacione cada pregunta con el texto más relevante y proporcione una respuesta concisa basada únicamente
en la información de los textos. Devuelva el resultado estrictamente como un objeto JSON en el formato

 {"1": "A",
  "2": "B",
   ...},

donde las claves son los números de las preguntas y los valores son las respuestas correspondientes.
No incluyas ninguna explicación ni texto adicional.\n\n {textos}\n{preguntas}"

```

User prompt:

```
Este es el texto con los huecos en blanco:
=============================
{text}
=============================

Estos son los fragmentos de texto que puedes utilizar para rellenar los huecos:
{options}

Para cada hueco en blanco, selecciona el fragmento de texto que mejor lo complete.
Recuerda que no es necesario utilizar todos los fragmentos de texto.
```

### Zero-shot ICL and RAG

System prompt:

```
Eres un experto en lengua castellana. 
Tu tarea es rellenar huecos determinados de un texto con fragmentos de texto que se te proporcionan.
El texto que se te va a presentar es un texto en español. 
Los huecos en blanco están representados por etiquetas <s id=1>, <s id=2>, etc.
Puede haber más fragmentos de texto que huecos en blanco. Por lo tanto, no es necesario utilizar todos los fragmentos de texto.

La respuesta debe ser un formato JSON con la siguiente estructura:
{
    "gap_1": "<option_id>",
    "gap_2": "<option_id>",
    "gap_3": "<option_id>",
}

Este es un ejemplo de respuesta:
{
    "gap_16": "A",
    "gap_17": "G",
    "gap_18": "H"
}
```

User prompt:

```
Este es el texto con los huecos en blanco:
=============================
{text}
=============================

Estos son los fragmentos de texto que puedes utilizar para rellenar los huecos:
{options}

Para cada hueco en blanco, selecciona el fragmento de texto que mejor lo complete.
Recuerda que no es necesario utilizar todos los fragmentos de texto.

A continuación, se te proporciona algo de texto que se ha encontrado automáticamente en Internet a partir de palabras clave del texto original.
Puedes usarlo si lo consideras útil:
=============================
{text_augmentation}
=============================
```

### Zero-shot ICL and Agentic RAG

#### Relevant context decision Agent

System prompt: 

```
Your are a useful assistant that helps the user decice if a context is relevant to complete a fill in the gap exercise.

You are going to be provided with:
- a text that contains gaps to fill with fragments
- a list of fragments to fill the gaps
- a text that contains the context to be used to fill the gaps

The context is automatically retrieved from the web and you have to decide if it is relevant or not.

Your response should be a JSON object with the following structure:
{
    "relevant": true/false
}

The only possible values for the "relevant" field are "true" or "false".
```

User prompt:

```
This is the text with gaps to fill:
{text}

This is the list of fragments to fill the gaps:
{options}

This is the context retrieved from the web:
{context}

Is this context relevant to fill the gaps?
```

#### Enough context decision Agent

System prompt: 

```
Your are a useful assistant that helps the user decice if a context is enough to complete a fill in the gap exercise.

You are going to be provided with:
- a text that contains gaps to fill with fragments
- a list of fragments to fill the gaps
- a text that contains some context to be used to fill the gaps

The contexts are automatically retrieved from the web and you have to decide if it is enough or not.

Your response should be a JSON object with the following structure:
{
    "enough": true/false
}

The only possible values for the "enough" field are "true" or "false".
```

User prompt:

```
This is the text with gaps to fill:
{text}

This is the list of fragments to fill the gaps:
{options}

This is the context retrieved from the web:
{context}

Is this context enough to fill the gaps?
```

#### Predictor

System prompt: 

```
Eres un experto en lengua castellana. 
Tu tarea es rellenar huecos determinados de un texto con fragmentos de texto que se te proporcionan.
El texto que se te va a presentar es un texto en español. 
Los huecos en blanco están representados por etiquetas <s id=1>, <s id=2>, etc.
Puede haber más fragmentos de texto que huecos en blanco. Por lo tanto, no es necesario utilizar todos los fragmentos de texto.

La respuesta debe ser un formato JSON con la siguiente estructura:
{
    "gap_1": "<option_id>",
    "gap_2": "<option_id>",
    "gap_3": "<option_id>",
}

Este es un ejemplo de respuesta:
{
    "gap_16": "A",
    "gap_17": "G",
    "gap_18": "H"
}
```

User prompt:

```
Este es el texto con los huecos en blanco:
=============================
{text}
=============================

Estos son los fragmentos de texto que puedes utilizar para rellenar los huecos:
{options}

Para cada hueco en blanco, selecciona el fragmento de texto que mejor lo complete.
Recuerda que no es necesario utilizar todos los fragmentos de texto.

A continuación, se te proporciona algo de texto que se ha encontrado automáticamente en Internet a partir de palabras clave del texto original.
Puedes usarlo si lo consideras útil:
=============================
{text_augmentation}
=============================
```

## Metrics

| Strategy         | Model                                 | Acc - A1 | Acc - A2 | Acc - B1 | Acc - B2 | Acc - C1 | Acc - C2 | Average Accuracy |
|------------------|-------------------------------------|----------|----------|----------|----------|----------|----------|------------------|
| Embedding        | paraphrase-multilingual-mpnet-base-v2 | 76,19%   | 65,79%   | 64,71%   | 36,17%   | 50,00%   | 56,25%   | 58,41%           |
| Embedding        | Ada (Embeddings OpenAI)              | 90,48%   | 71,05%   | 68,63%   | 63,83%   | 56,25%   | 37,50%   | 68,14%           |
| Embedding        | baai-m3                             | 83,33%   | 55,26%   | 58,82%   | 57,45%   | 34,38%   | 50,00%   | 58,41%           |
| Embedding        | text-embedding-3-small              | 88,10%   | 63,16%   | 60,78%   | 59,57%   | 53,12%   | 37,50%   | 63,27%           |
| Ensemble embedding | ada + baai-m3 + text-embbeding-3  | 90,48%   | 65,79%   | 68,63%   | 65,96%   | 59,38%   | 37,50%   | 68,14%           |
| LLM              | gemini-2.0-flash-thinking-exp-01-21 | 100,00%  | 100,00%  | 100,00%  | 95,74%   | 95,31%   | 93,75%   | 98,11%           |
| LLM              | GPT4-o                             | 97,62%   | 93,18%   | 84,31%   | 95,74%   | 100,00%  | 87,50%   | 93,10%           |
| LLM              | Llama 3.3 - 70B                    | 100,00%  | 92,00%   | 68,63%   | 91,49%   | 96,88%   | 87,50%   | 88,66%           |
| LLM              | DeepSeek R1                       | 100,00%  | 100,00%  | 98,04%   | 95,74%   | 100,00%  | 93,75%   | 98,23%           |
| LLM              | Claude 3.7 Sonnet                 | 100,00%  | 94,74%   | 86,27%   | 95,74%   | 100,00%  | 87,50%   | 94,25%           |
| LLM              | Gemma 3 12B                      | 97,62%   | 89,47%   | 90,20%   | 91,49%   | 90,62%   | 81,25%   | 91,15%           |
| LLM              | Phi 4 14B                       | 100,00%  | 89,47%   | 98,04%   | 85,11%   | 93,75%   | 81,25%   | 92,48%           |
| LLM              | Qwen 2.5 14B Instruct 1M         | 94,29%   | 89,47%   | 90,20%   | 91,49%   | 90,62%   | 87,50%   | 90,87%           |
| LLM              | Ministral 8B Instruct             | 71,43%   | 81,58%   | 88,24%   | 80,85%   | 75,00%   | 68,75%   | 79,20%           |
| Ensemble LLM     | Gemma3 + Phi4 + Qwen2.5          | 100,00%  | 89,47%   | 98,04%   | 85,11%   | 93,75%   | 81,25%   | 92,48%           |
