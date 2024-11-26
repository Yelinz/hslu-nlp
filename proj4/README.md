# Project 4
Our task is reading comprehension with the BoolQ dataset. Use a pretrained Transformer encoder of
your choice (from Hugging Face; it must not be finetuned on the BoolQ dataset yet), and a two-layer
classifier with a ReLU. Train the whole network end-to-end. Apply the correct learning rates.

DeBERTA performane comparison small base large

Code to follow
https://colab.research.google.com/github/NielsRogge/Transformers-Tutorials/blob/master/BERT/Fine_tuning_BERT_(and_friends)_for_multi_label_text_classification.ipynb

Tokenizer info
https://github.com/huggingface/transformers/blob/main/src/transformers/models/deberta_v2/tokenization_deberta_v2.py
https://github.com/google/sentencepiece

Change classification head to the desired shape
https://discuss.huggingface.co/t/how-do-i-change-the-classification-head-of-a-model/4720/29

Configure wandb
https://huggingface.co/docs/transformers/v4.46.2/en/main_classes/callback#transformers.integrations.WandbCallback

Log accuracy additonally when training
https://discuss.huggingface.co/t/metrics-for-training-set-in-trainer/2461/7

## Feedback stage 1:
Stage 1 feedback: Very good


## Feedback stage 2: 


## Feedback presentation: 
Grade: 4.5
Preprocessing: Do you lowercase?
Input/output format: Missing.
Model: Good, but a bit confusing: For DeBERTa you say input is 512 (which is length) and output is 768 (which is hidden size).
Experiments: Use exponential steps for the learning rate.
Results: Really low training accuracy and no comment on it. Doesn't the confusion matrix show a minority class classifier?
Interpretation: Good.

