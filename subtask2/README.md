# Subtask 2

- [Prompts](#prompts)
- [Dataset distribution](#evaluation-dataset)
- [Metrics](#metrics)
- [Example](#example)

## Prompts

### Zero-shot ICL using LLMs

User prompt:

```

Dados los siguientes textos y preguntas, relacione cada pregunta con el texto más relevante y proporcione una respuesta concisa basada 
únicamente en la información de los textos. Devuelva el resultado estrictamente como un objeto JSON en el formato

 {"1": "A",
  "2": "B",
   ...},

donde las claves son los números de las preguntas y los valores son las respuestas correspondientes.
No incluyas ninguna explicación ni texto adicional.\n\n {textos}\n{preguntas}

```

## Evaluation dataset

| Proficiency level   | A1 | A2 | B1 | B2 | C1 | C2 | Total |
|---------------------|----|----|----|----|----|----|-------|
| Number of exercises | 7  | 4  | 8  | 5  | 4  | 2  | 30    |

## Metrics

The evaluation metric used was accuracy (%).

| Strategy                       | Model                                 | A1      | A2      | B1      | B2     | C1      | C2     | Overall    |
|--------------------------------|---------------------------------------|---------|---------|---------|--------|---------|--------|------------|
| Embedding similarity           | paraphrase-multilingual-mpnet-base-v2 | 76,19%  | 65,79%  | 64,71%  | 36,17% | 50,00%  | 56,25% | 58,41%     |
| Embedding similarity           | Ada-002                               | 90,48%  | 71,05%  | 68,63%  | 63,83% | 56,25%  | 37,50% | 68,14%     |
| Embedding similarity           | bge-m3                                | 83,33%  | 55,26%  | 58,82%  | 57,45% | 34,38%  | 50,00% | 58,41%     |
| Embedding similarity           | text-embedding-3-small                | 88,10%  | 63,16%  | 60,78%  | 59,57% | 53,12%  | 37,50% | 63,27%     |
| Ensemble, embedding similarity | ada-002 + bge-m3 + text-embbeding-3   | 90,48%  | 65,79%  | 68,63%  | 65,96% | 59,38%  | 37,50% | 68,14%     |
| ICL (zero-shot)                | gemini-2.0-flash-thinking-exp-01-21   | 100,00% | 100,00% | 100,00% | 95,74% | 95,31%  | 93,75% | 98,11%     |
| ICL (zero-shot)                | GPT4-o                                | 97,62%  | 93,18%  | 84,31%  | 95,74% | 100,00% | 87,50% | 93,10%     |
| ICL (zero-shot)                | Llama 3.3 - 70B                       | 100,00% | 92,00%  | 68,63%  | 91,49% | 96,88%  | 87,50% | 88,66%     |
| ICL (zero-shot)                | DeepSeek R1                           | 100,00% | 100,00% | 98,04%  | 95,74% | 100,00% | 93,75% | **98,23%** |
| ICL (zero-shot)                | Claude Sonnet  3.7                    | 100,00% | 94,74%  | 86,27%  | 95,74% | 100,00% | 87,50% | 94,25%     |
| ICL (zero-shot)                | Gemma 3 12B                           | 97,62%  | 89,47%  | 90,20%  | 91,49% | 90,62%  | 81,25% | 91,15%     |
| ICL (zero-shot)                | Phi 4 14B                             | 100,00% | 89,47%  | 98,04%  | 85,11% | 93,75%  | 81,25% | 92,48%     |
| ICL (zero-shot)                | Qwen 2.5 14B Instruct 1M              | 94,29%  | 89,47%  | 90,20%  | 91,49% | 90,62%  | 87,50% | 90,87%     |
| ICL (zero-shot)                | Ministral 8B Instruct                 | 71,43%  | 81,58%  | 88,24%  | 80,85% | 75,00%  | 68,75% | 79,20%     |
| Ensemble, ICL (zero-shot)      | Gemma3 + Phi4 + Qwen2.5               | 100,00% | 89,47%  | 98,04%  | 85,11% | 93,75%  | 81,25% | 92,48%     |

## Example

There are two different formats for these exercises:

- [Example 1](./example1.json), where there are more source texts than target texts.

- [Example 2](./example2.json), where there are more target texts than source texts.
