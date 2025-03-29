# FinanceBench Comparative Evaluation

## Overview

This project evaluates and compares the financial question-answering capabilities of Anthropic's Claude 3.5 Haiku and OpenAI's GPT-4o mini using the FinanceBench dataset

## Experiment Setting
### Models
- **Anthropic Claude 3.5 Haiku**
- **OpenAI GPT-4o mini**

### Configurations
- **Temperature**: 0.01
- **Max Tokens**: 512
- **Embedding used in vector store**: OpenAI embeddings
- **Context cutoff for *Long Context* evaluation mode**: 8192

### Evaluation Setup
- **Number of Questions**: Reduced from 150 to 16
- **Number of Distinct PDFs**: Reduced from 84 to 9

### Environment
   - **Python Version**: 3.9
   - **Required Libraries**: Listed in requirements.txt

## Observation

### Inference Time Comparison:
|Configuration | GPT-4o mini | Claude 3.5 Haiku |
|----------|------------|-----------------|
| Closed Book | 59.1s | 48.4s |
| Single Store | 38.3s | 1m5.6s |
| Shared Store | 34.8s | 1m9.4s |
| Long Context (Context-First) | 1m50.8s | 2m4.3s |
| Long Context (Context-Last) | 1m29.8s | 2m4.8s |
| Oracle (Context-First) | 1m15.4s | 1m3.0s |
| Oracle (Context-Last) | 1m16.2s | 1m2.5s |

### Model Performance:
Model | Configuration | Correct Answer | Incorrect Answer | Failed to answer
---------- |----------|-----------|----------|------------|
GPT-4o mini | Closed Book | 1 | 3 | 11
GPT-4o mini | Single Store | 3 | 2 | 10
GPT-4o mini | Shared Store | 2 | 4 | 9
GPT-4o mini | Long Context (Context-First) | 3 | 4 | 8
GPT-4o mini | Long Context (Context-Last) | 4 | 4 | 7
GPT-4o mini | Oracle (Context-First) | 9 | 4 | 2
GPT-4o mini | Oracle (Context-Last) | 9 | 4 | 2
Claude 3.5 Haiku | Closed Book | 1 | 2 | 12
Claude 3.5 Haiku | Single Store | 3 | 2 | 10
Claude 3.5 Haiku | Shared Store | 3 | 2 | 10
Claude 3.5 Haiku | Long Context (Context-First) | 4 | 1 | 10
Claude 3.5 Haiku | Long Context (Context-Last) | 3 | 3 | 9
Claude 3.5 Haiku | Oracle (Context-First) | 12 | 2 | 1
Claude 3.5 Haiku | Oracle (Context-Last) | 11 | 3 | 1

### Challenges Faced:
- **API Limitations**: Both models had limitations with their free-tier usage, resulting in reduced question count and context size. Initially, I attempted to complete the experiment using the free tier, but discovered that with a $0 balance, API calls were not possible.  Eventually, I subscribed to the minimum plan ($5 each for OpenAI and Anthropic), which ultimately cost $0.5 to complete the experiment.
- **Missing Anthropic Tokenizer**: The Anthropic library does not provide a get_tokenizer function, so I skipped the "check Anthropic tokenizer" step. This may have impacted the experiments in the long-context evaluation mode.
- **Answer Verification Challenges**: LLM responses were lengthy, making it difficult to quickly determine if they matched the gold-standard answer. Manual verification required significant effort. Additionally, one of the 16 questions had a gold-standard answer that was too specialized for me to judge the AI’s response accurately, so I ultimately ignored that question. As a result, the final model performance was evaluated on 15 questions.

---

## References

- https://github.com/patronus-ai/financebench
- https://arxiv.org/pdf/2311.11944