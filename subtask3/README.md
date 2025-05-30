# Subtask 3

- [Prompts](#prompts)
- [Metrics](#metrics)
- [Dataset distribution](#evaluation-dataset)
- [Example](./example.json)

## Prompts

### Zero-shot ICL using LLMs

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

## Evaluation dataset

| Proficiency level   | B1 | B2 | C1 | C2 | C1/C2 | Total |
|---------------------|----|----|----|----|-------|-------|
| Number of exercises | 7  | 3  | 5  | 3  | 2     | 20    |

## Metrics

The evaluation metric used was accuracy (%).

| Strategy                               | Model                                                       | B1      | B2      | C1     | C2      | C1/C2   | Overall    |
|----------------------------------------|-------------------------------------------------------------|---------|---------|--------|---------|---------|------------|
| Baseline                               | ChatGPT                                                     | ---     | ---     | ---    | ---     | ---     | 43.00%     |
| ICL (zero-shot)                        | o4-mini                                                     | 80.95%  | 88.89%  | 83.33% | 72.22%  | 83.33%  | 81.66%     |
| ICL (zero-shot) + RAG                  | o4-mini                                                     | 88.10%  | 94.44%  | 86.67% | 77.78%  | 75.00%  | 85.84%     |
| ICL (zero-shot)                        | gemini-2.5-pro-exp-03-25                                    | 100.00% | 88.89%  | 90.00% | 100.00% | 100.00% | 95.83%     |
| ICL (zero-shot) + RAG                  | gemini-2.5-pro-exp-03-25                                    | 100.00% | 94.44%  | 93.33% | 94.44%  | 100.00% | 96.44%     |
| ICL (zero-shot)                        | gemini-2.0-flash-thinking-exp-01-21                         | 85.71%  | 94.44%  | 70.00% | 61.11%  | 91.67%  | 80.00%     |
| ICL (zero-shot) + RAG                  | gemini-2.0-flash-thinking-exp-01-21                         | 97.62%  | 94.44%  | 93.33% | 100.00% | 100.00% | **96.67%** |
| ICL (zero-shot) + Agentic RAG          | gemini-2.0-flash-thinking-exp-01-21                         | 92.86%  | 94.44%  | 90.00% | 100.00% | 100.00% | 94.17%     |
| ICL (zero-shot)                        | gemini-2.5-flash-preview-04-17                              | 97.62%  | 94.44%  | 70.00% | 100.00% | 100.00% | 90.83%     |
| ICL (zero-shot) + RAG                  | gemini-2.5-flash-preview-04-17                              | 97.62%  | 94.44%  | 93.33% | 100.00% | 100.00% | **96.67%** |
| ICL (zero-shot)                        | DeepSeek R1                                                 | 92.86%  | 100.00% | 86.67% | 66.67%  | 83.33%  | 87.50%     |
| ICL (zero-shot) + RAG                  | DeepSeek R1                                                 | 90.48%  | 88.88%  | 73.33% | 94.44%  | 100.00% | 87.50%     |
| ICL (zero-shot)                        | Mistral large                                               | 90.48%  | 88.89%  | 60.00% | 72.22%  | 83.33%  | 79.17%     |
| ICL (zero-shot)                        | Claude 3.7 Sonnet (Thinking)                                | 92.86%  | 94.44%  | 93.33% | 100.00% | 100.00% | 95.00%     |
| ICL (zero-shot) + RAG                  | Claude 3.7 Sonnet (Thinking)                                | 92.86%  | 94.44%  | 96.67% | 100.00% | 100.00% | 95.83%     |
| ICL (zero-shot)                        | QwQ-32B                                                     | 76.19%  | 88.89%  | 86.67% | 72.22%  | 100.00% | 82.50%     |
| ICL (zero-shot) + RAG                  | QwQ-32B                                                     | 88.10%  | 88.89%  | 83.33% | 94.44%  | 40.00%  | 83.17%     |
| ICL (zero-shot)                        | Qwen3-32B                                                   | 80.95%  | 94.44%  | 76.67% | 89.89%  | 75.00%  | 82.50%     |
| ICL (zero-shot)                        | Qwen3-30B-A3B                                               | 80.95%  | 83.33%  | 68.00% | 77.78%  | 75.00%  | 77.00%     |
| ICL (zero-shot)                        | Phi 4 14B                                                   | 64.29%  | 72.22%  | 46.67% | 43.33%  | 58.33%  | 57.33%     |
| ICL (zero-shot)                        | DeepSeek R1 Distill Qwen 14B                                | 70.95%  | 55.56%  | 47.33% | 55.56%  | 75.00%  | 60.83%     |
| ICL (zero-shot) + RAG                  | DeepSeek R1 Distill Qwen 14B                                | 76.19%  | 77.78%  | 60.67% | 66.67%  | 33.33%  | 66.83%     |
| ICL (zero-shot) + Agentic RAG          | DeepSeek R1 Distill Qwen 14B                                | 50.00%  | 83.33%  | 43.33% | 44.44%  | 58.33%  | 53.33%     |
| ICL (zero-shot)                        | Gemma 27B                                                   | 69.05%  | 27.78%  | 20.00% | 22.22%  | 50.00%  | 41.67%     |
| ICL (zero-shot)                        | Llama 3.3 70B                                               | 76.19%  | 94.44%  | 60.00% | 77.78%  | 91.67%  | 76.67%     |
| ICL (zero-shot) + RAG                  | Llama 3.3 70B                                               | 73.81%  | 94.44%  | 56.67% | 83.33%  | 66.67%  | 73.33%     |
| Ensamble (max voting), ICL (zero-shot) | Qwen3-32B + Phi-4 + DeepSeek R1 Distill Qwen 14B            | 80.95%  | 94.44%  | 70.00% | 72.22%  | 75.00%  | 78.33%     |
| Embedding similarity                   | sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2 | 43.00%  | 28.00%  | 10.00% | 22.22%  | 50.00%  | 30.05%     |
