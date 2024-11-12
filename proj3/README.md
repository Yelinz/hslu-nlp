# Project 3

No truncation if not needed (BERT)
Padding by batch not total

https://www.kaggle.com/code/zhuyuqiang/transformer-encoder-for-classification-in-pytorch

https://debuggercafe.com/text-classification-using-transformer-encoder-in-pytorch/

https://stackoverflow.com/questions/58092004/how-to-do-sequence-classification-with-pytorch-nn-transformer

https://n8henrie.com/2021/08/writing-a-transformer-classifier-in-pytorch/

https://stackoverflow.com/questions/62399243/transformerencoder-with-a-padding-mask

https://www.nltk.org/api/nltk.tokenize.punkt.html

https://pytorch.org/torchtune/0.3/generated/torchtune.modules.RotaryPositionalEmbeddings.html

## Feedback stage 1:
- Carefully consider your input and output shapes for the transformer layers: when you just say "embedding dimension" for both, are you not forgetting another dimension? Think about how you will need to handle this dimension for the next step in your architecture.
- Check whether the Punkt tokenizer is a sentence or word tokenizer.

## Feedback stage 2: 

## Feedback presentation: 
Use cls (first) token for input to classifier, instead of avg before or after classifier
- Information is spread out otherwise, using the first token is standard
When tuning: learning rate and other model parameters (hidden dim, etc) depend on each other, so tune them together

Preprocessing: Clarify special characters 
Forgot to tell which optimizer and Loss function
More clear input output format
More Interpreting results
