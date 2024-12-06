# Project 5
Leads
https://www.datacamp.com/tutorial/fine-tuning-llama-3-1
https://medium.com/@kshitiz.sahay26/fine-tuning-llama-2-for-news-category-prediction-a-step-by-step-comprehensive-guide-to-fine-tuning-48c06dee28a9

## Feedback stage 1:
The project description says that "Since an LLM is a text generation model, make sure to describe how you get predictions from it". Your description in the Stage 1 notebook ("The model should generate either "True" or "False" to the question based on the given input.") is too vague. Think about how you will compute accuracy during in-training evaluation: How do you extract the prediction at the relevant position and avoid leaking the label? Are you using the same input format for validation examples as for training examples? Do you work with generated tokens or with logits? How do you map them to the label? etc.

- You keep optimizing for loss but evaluate on accuracy - show that you have considered the feedback from Project 3 on this discrepancy.

## Feedback stage 2: 


## Feedback presentation: 
