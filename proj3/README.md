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

Preprocessing
- Clarify special characters
- Say what the pretrained tokenizer does 
Forgot to tell which optimizer and Loss function
More clear input output format
More Interpreting results

Grade: 5.25
Preprocessing: Good. Didn't understand your comment about using padding instead of sep token.
Input/output format: Add the info about concatenation and where to put sep token here. What was used to classify?
Model: You confused me here. I thought you mean that your embedding layer has 622 num_embeddings (the first parameter of the nn.Embedding layer), but you're talking about your max sequence length.
Experiments: Good, contains everything we need to know.
Results: Training accuracy very low: lower than majority class baseline. You didn't talk about it at all, but this is super surprising. What happened there?
1e-4 = 1 * 10^(-4)
Say 1 more interesting thing about your results.
Interpretation: Why not select on accuracy?
What can you change when the loss goes down too slowly?
