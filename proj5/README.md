# Project 5
Our task is reading comprehension with the BoolQ dataset. Use an LLM (≥ 1B parameters) from Hugging Face. It should not be trained on the BoolQ dataset yet, but sometimes we don’t know.
- Evaluate it directly with 5 diverse prompts.
- Then train it with parameter-efficient fine-tuning (I suggest LoRA, see e.g. the HF blog post or quicktour).
- Use a quantized version as the base model (search for "AWQ" or "GPTQ" in the model name).
    - If that doesn’t work, it may be easier to quantize the model yourself with bitsandbytes.
- Since an LLM is a text generation model, make sure to describe how you get predictions from it.

Leads
https://www.datacamp.com/tutorial/fine-tuning-llama-3-1

https://medium.com/@kshitiz.sahay26/fine-tuning-llama-2-for-news-category-prediction-a-step-by-step-comprehensive-guide-to-fine-tuning-48c06dee28a9
https://wandb.ai/capecape/alpaca_ft/reports/How-to-Fine-tune-an-LLM-Part-3-The-HuggingFace-Trainer--Vmlldzo1OTEyNjMy

## Feedback stage 1:
- The project description says that "Since an LLM is a text generation model, make sure to describe how you get predictions from it". Your description in the Stage 1 notebook ("The model should generate either "True" or "False" to the question based on the given input.") is too vague. Think about how you will compute accuracy during in-training evaluation: How do you extract the prediction at the relevant position and avoid leaking the label? Are you using the same input format for validation examples as for training examples? Do you work with generated tokens or with logits? How do you map them to the label? etc.

- You keep optimizing for loss but evaluate on accuracy - show that you have considered the feedback from Project 3 on this discrepancy.

## Feedback stage 2: 


## Feedback presentation: 
Grade: 5.25
Preprocessing: Good.
Input/output format: Good.
Model: Which 3.2 version did you pick?
Experiments: Nice experiments.
Results: You could still select the best checkpoint based on validation accuracy. No results on just prompting the model.
Interpretation: Interesting.
