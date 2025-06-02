# Subtask 1

- [Prompts](#prompts)
- [Dataset distribution](#evaluation-dataset)
- [Metrics](#metrics)
- [Hyperparameters](#fine-tuning-hyperparameters)
- [Example](./example.json)

## Prompts

### Zero-shot ICL using LLMs

User prompt:

```
Given a text, a question, and multiple choice answers, reply with only the letter (e.g., 'A', 'B', etc.) of the correct answer.
Do not include any explanation or additional text — only the single letter.

Text: <text>

Question: <question>

<optionId>) <text>
<optionId>) <text>
<optionId>) <text>

```

## Evaluation dataset

| Proficiency level   | A1 | A2 | B1 | B2 | C1 | C2 | Total |
|---------------------|----|----|----|----|----|----|-------|
| Number of exercises | 6  | 10 | 13 | 6  | 4  | 1  | 40    |

## Metrics

The evaluation metric used was accuracy (%).

| Strategy                                    | Model                                                              | A1    | A2    | B1    | B2     | C1    | C2    | Overall   |
|---------------------------------------------|--------------------------------------------------------------------|-------|-------|-------|--------|-------|-------|-----------|
| ICL (zero-shot)                             | Claude 3.7 Sonnet                                                  | 100   | 96.08 | 98.59 | 94.59  | 79.17 | 60.00 | 95.37     |
| ICL (zero-shot)                             | Qwen 2.5 72B                                                       | 96.97 | 96.08 | 98.59 | 97.30  | 83.33 | 40.00 | 95.83     |
| ICL (zero-shot)                             | DeepSeek R1                                                        | 96.97 | 98.04 | 98.59 | 97.30  | 83.33 | 60.00 | **96.30** |
| ICL (zero-shot)                             | Llama 3.3 70B                                                      | 93.94 | 98.04 | 91.55 | 91.89  | 70.83 | 40.00 | 94.44     |
| ICL (zero-shot)                             | QwQ 32B                                                            | 96.97 | 98.04 | 97.18 | 97.30  | 79.17 | 20.00 | 95.37     |
| ICL (zero-shot)                             | Mistral Large                                                      | 93.94 | 98.04 | 97.18 | 86.49  | 70.83 | 40.00 | 92.13     |
| ICL (zero-shot)                             | Gemma 3 12B                                                        | 96.97 | 98.04 | 97.18 | 94.59  | 70.83 | 40.00 | 93.98     |
| ICL (zero-shot)                             | Phi 4 14B                                                          | 96.97 | 98.08 | 94.37 | 100.00 | 75.00 | 40.00 | 93.98     |
| ICL (zero-shot)                             | DeepSeek R1 Distill Qwen 14B                                       | 84.85 | 92.16 | 91.55 | 78.38  | 70.83 | 20.00 | 86.11     |
| ICL (zero-shot)                             | Qwen 2.5 14B Instruct 1M                                           | 87.88 | 98.04 | 97.18 | 100.00 | 83.33 | 40.00 | 94.91     |
| ICL (zero-shot)                             | Mistral Nemo 12B Instruct                                          | 100   | 98.04 | 95.77 | 89.19  | 54.17 | 20.00 | 91.20     |
| ICL (zero-shot)                             | Qwen 2.5 7B Instruct                                               | 93.94 | 96.08 | 90.14 | 91.89  | 70.83 | 20.00 | 90.28     |
| ICL (zero-shot)                             | Ministral 8B Instruct                                              | 93.94 | 98.04 | 92.96 | 89.19  | 79.17 | 20.00 | 92.13     |
| ICL (zero-shot)                             | Gemma 3 4B                                                         | 81.82 | 90.20 | 83.10 | 78.38  | 75.00 | 20.00 | 82.87     |
| ICL (zero-shot)                             | Phi 4 3B                                                           | 81.82 | 92.16 | 88.73 | 75.68  | 54.17 | 40.00 | 82.41     |
| Fine-tuning (zero-shot)                     | Gemma 3 12B (version 0)                                            | 100   | 90.20 | 92.96 | 81.08  | 70.83 | 40.00 | 88.89     |
| Fine-tuning (zero-shot)                     | Gemma 3 12B (version 1)                                            | 93.94 | 93.14 | 91.55 | 87.84  | 66.67 | 20.00 | 88.43     |
| Ensemble (majority voting), ICL (zero-shot) | Gemma3 12B + Phi4 14B + Qwen2.5 14B + Ministral 8B                 | 96.97 | 98.04 | 97.18 | 100.00 | 75.00 | 40.00 | 95.37     |
| Ensemble (random forest), ICL (zero-shot)   | Gemma3 12B + Phi4 14B + Qwen2.5 14B + Ministral 8B                 | 96.97 | 98.04 | 97.18 | 97.30  | 70.83 | 40.00 | 94.44     |
| NLI (zero-shot)                             | DeBERTa Base Long NLI                                              | 66.67 | 92.16 | 77.47 | 56.76  | 41.67 | 80.00 | 71.95     |
| NLI (zero-shot)                             | DeBERTa v3 Large tasksource NLI                                    | 87.88 | 96.08 | 88.73 | 56.76  | 58.33 | 60.00 | 81.00     |
| NLI (zero-shot)                             | A2T RoBERTa SMFA ACE arg                                           | 48.49 | 37.26 | 35.21 | 51.35  | 45.83 | 20.00 | 41.35     |
| NLI (zero-shot)                             | Longformer Base 4096 BNE ES NLI                                    | 57.58 | 50.98 | 40.85 | 35.14  | 58.33 | 40.00 | 46.94     |
| Ensemble, NLI (zero-shot)                   | DeBERTa Base +    DeBERTa v3 Large + A2T RoBERTa + Longformer Base | 87.88 | 96.08 | 88.73 | 56.76  | 58.33 | 60.00 | 81.00     |


## Fine-tuning hyperparameters

| Parameter                 | Value                   |
|---------------------------|-------------------------|
| Model ID from HuggingFace | `google/gemma-3-12b-it` |
| Batch size                | `4`                     |
| Gradient accumulation     | `8`                     |
| Learning rate             | `2e-4`                  |
| Epochs                    | `20`                    |
| Max sequence length       | `2048`                  |
| LoRA rank (`r`)           | `16`                    |
| LoRA alpha                | `16`                    |
| LoRA dropout              | `0`                     |
| GPUs                      | 4  NVIDIA L40           |
