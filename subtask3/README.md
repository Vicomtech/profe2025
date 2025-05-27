# Subtask 3 - Prompts

## Zero-shot ICL using LLMs

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

## Zero-shot ICL and RAG

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

## Zero-shot ICL and Agentic RAG

### Relevant context decision Agent

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

### Enough context decision Agent

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

### Predictor

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