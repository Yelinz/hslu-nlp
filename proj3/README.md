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
Grade: 4.00
Stage 1 decisions: - Your approach to vocabulary building should be more specific in Stage 1 (tokenization method, vocabulary source [train, validation, test?], unknown token handling, etc.)
Introduction: Good. 
Setup: - Avoid unnecessary imports.
Preprocessing: - "the ordering should not matter as transformers do not have sequential processing like RNNs" -> since you then explicitly add positional embeddings, does that not mean that the ordering actually matters?
- "Case of words is not very important for answering questions, has potential to reduce vocabulary size" -> have you considered information loss like "US" being reduced to "us"? That said, good that you thought about bringing question and passage "closer together in terms of format".
- "Passage does not contain important punctuation" -> Are you entirely sure? What about hyphenated words like "editor-in-chief", which your code preprocesses into "editorinchief"? Or a period in "a fever of 39.7 degrees", a colon in "1:1 match", or a plus sign in "2+2"? You might have considered all those drawbacks, but you should show that this was a deliberate choice.
- The same applies to your filtering out of any token as soon as it has at least one non-ascii characters - which would remove e.g. "Zürich". Again, this may be a considered choice, but it would be good to see you thought about the  consequences in full or at least checked in some detail what you would effectively lose from the dataset.
- The rest of the preprocessing and especially the choices for/against specific operations are well justified.
- It's also good that you padded per batch according to the project feedback.
- If you change something in your approach, e.g. no longer using the Punkt tokenizer, your markdown should reflect that (either delete or use strikethrough). Right now you still say "Use `punkt_tokenize` as we are interested in every word".
- Good that you checked preprocessed example to see whether information was still available.
- Your vocabulary building with build_vocab_from_iterator is generally fine, but you have collected vocabulary tokens from all three datasets (train, validation, test) in your "tokens" list. This is considered data leaking, since you are already preparing for words that your model/setup should not actually be familiar with (if we run your setup on a brand new example with unknown words, you will get an error). Instead, you should have used only the words from your training dataset and prepare your vocabulary to handle unknown tokens, e.g. using an unknown token as special token and using vocab.set_default_index() to it.
Model: - Unfortunately, you did not consider your input/output shapes carefully enough, which resulted in an implementation that scrambles the information in your sequence when using the PositionalEncoding class: 
-- Your batch_collate function uses pad_sequence with default behavior (batch_first=False), resulting in tensors of shape (max_seq_len, batch_size).
-- Your permutation in the model's forward function brings them to a shape of (batch_size, max_seq_len).
-- Your nn.Embedding layer brings them to a shape of (batch_size, max_seq_len, hidden_size).
-- As you see in the StackOverflow source you cited for the PositionalEncoding class, the class you copied expects a tensor of shape "[seq_len, batch_size, embedding_dim]" per the source. But at this point, your data has the shape (batch_size, max_seq_len, hidden_size), which means positional encodings are applied across the wrong dimension, i.e. batch size instead of sequence length. To be clear, this is unlike your class PositionalEmbedding, which expects (batch_size, max_seq_len, hidden_size) and thus matches the input shape.
-- In other words, instead of creating word order, your PositionalEncoding class scrambles the information across the batches.
- In addition, the Stage 1 feedback specifically asked you to consider how to handle the sequence length from your transport output before attaching the classifier. You instead decided to handle the sequence length only *after* the classifier. In other words, your implementation tries to produce per-token prediction logits for every word in the sequence, and averages those logits right before the sigmoid. This means that the classifier never gets to learn to classify a sequence-level representation from a pooled transformer output.
- The way to implement such a pooled transformer output, which the Stage 1 feedback hinted at, is to either average the transformer output itself (while masking out the padding tokens) or to use a special CLS token at the start of the sequence from which you extract the sequence-level representation.
- Regardless of the above, your model description is inconsistent with your code - you say for instance that your embedding dimension is 256, but your code uses 512. There should not be mismatches like this, and in either case, you should justify your choices (why 256/512?).
- Your model description is also incomplete. There are many decisions that you implemented directly in your code but did not mention at all in your markdown. This includes critical choices like setting the feedforward dimension to 4 times the hidden dimension, using average pooling, or using positional embeddings. Decisions like this should be explained and justified already in stage 1 (ideally), and at the latest in stage 2.
- You did not check whether your hidden_size (256, 512) is divisible by your number of attention heads. This is not a problem for the head range you ultimately implemented (8, 16) but would have been for the originally considered range of 4/6/8.
Training: - Your checkpoint "uses loss because it is the most important metric for the model" and you say that "it is not necessary to optimize for accuracy" -> Are you sure? The metric we care about for the model's ultimate deployment is effectively accuracy, not just loss. What if the model gets a lot more confident in correct results (large loss decrease) while at the same time flipping ambiguous results from correct to incorrect (decreasing accuracy, but without a large loss increase) - should that really be rewarded?
- Experiments should not contain old ideas/announcements that you did not implement (e.g. rotary positional embeddings, hyperparameter values or number of experiments, etc.). Either delete or at least strikethrough what is no longer current.
- You should justify your hyperparameter ranges more concretely - why 4/6/8 for the attention heads, why not 16 or 32? Why this learning rate range and not another? You also do not mention the number of epochs in your markdown - let alone justify it.
- Good that you concentrated your metrics on accuracy/loss given the task.
- Your choice of sweep as a method is well justified. However, your early stopping mechanism was not very effective: your best model (pos:embedding/heads:8/lr:0.0001) reached the lowest val_loss of 0.67453 at epoch 8 / step 31, yet continued without improvement until epoch 99 / step 366.
- Good checkpoint names with parameters used and metric achieved - consider adding the epoch number to pinpoint exactly where the model was in the run.
Evaluation: - Your say you want to evaluate "Recall for true labels, specificity for false lables" and that this was "suggested in class", but the project feedback presented in class did not really say this. It only said that recall and precision are not necessarily meaningful for the BoolQ task, and that IF you decide to report precision and recall, then you should do so on all classes and not just the positive class. That being said, you should consider whether these metrics really add something in a task where the true/false categories do not have inherent patterns due to being arbitrarily interchangeable ("Did Maggie Smith pass away?" with label "true" is equivalent to "Is Maggie Smith still alive?" with label "false" -> no meaningful difference in the cost of a false negative vs. false positive). 
- In any event, your markdown says you will implement these metrics with "torchmetrics.functional.recall(preds, target, task='binary')", but then your code actually uses "task='multiclass'". It's very likely that using the "multiclass" parameter leads the function to expect predictions across multiple labels, e.g. [0.3, 0.7] for a binary classification problem, yet your predictions are not expressed as a class vector - they are a single probability for the positive class (e.g. 0.7). This might lead to mismatches in expected dimensions and explain why you get recall and specificity of both exactly 0.
- These metrics should have tipped you off about a problem in your metrics computation. A recall of 0 and a specificity of 0 at the same time means that your model always reaches the wrong answer, i.e. 0% accuracy, which is not consistent with your results.
- Good that you reported your metrics in the markdown as well (not just in the code cells).
Interpretation: - Good setting of expectations given the architecture and good explanation of the likely overfitting models.
- Your best model (pos:embedding/heads:8/lr:0.0001) reached the lowest val_loss of 0.67453 at step 31 of 366, but that loss curve is almost completely flat, as is the accuracy curve, and the accuracy is basically at the baseline majority-class-prediction level (62.2%). When discussing your best model, your interpretation should highlight/discuss these kinds of findings, as it suggests that even the best model was not able to learn anything.
- Good that you considered that the flat curves might result from an implementation problem.
- You also hypothesise that more epochs might have helped, but you do not really address the scope of your gridsearch. Given that you ultimately ran only 2x2x2=8 experiments, is there not a conclusion to draw about the difficulty to find  optimal results from such a narrow parameter range? With discrete upper/lower values as you used, your "best" model will always be at the edge of the range, so your impulse should be about expanding parameters rather than number of epochs - especially when you already use a relatively high number of epoch (100).
- Your interpretation is otherwise broadly good, especially your attempt to draw conclusions from hyperparameter patterns. If you had better results than baseline performance, consider analysing wrong predictions for any patterns therein.
Reproducibility: - Good.
Form: - Good but don't leave things in the markdown that you ended up not implementing - either delete them or, even better, use strikethrough.
- Your restraint in not relying on AI / code-completion tools is a very legitimate approach. That said, consider using tools for targeted sanity-checks, as it seems this could have avoided some of the issues that prevented your models from learning in this project (dimension mismatches, unconventional pooling, etc.).
Experiment Tracking: - Make sure to include a "Weights & Biases view or report" per the project description (not just a link to your workspace)
- Your runs should be sorted by the relevant metric (not by created timestamp).
- Consider the usefulness of some plots - what are you meaningfully trying to show with a "val_loss vs. created" plot?
- Good run naming.

### Things to improve:
- While you put more thought into your preprocessing, you still need to be more careful about e.g. data leaking (like integrating tokens from the validation and test sets into your vocabulary, as you did).
- Consider more rigorously the shape of your input/outputs, including by tracking the shape throughout your batching and forward functions. Writing tests/asserts can help you here.
- Give more thoughts to your architectural decisions (does it make sense to do average pooling *after* the classifier?).
- In any event, do not skip the justification for your model/training choices in the markdown. Show that you have considered the effective consequences.
- Your code testing should include your evaluation code, especially when you get implausible results like a recall and specificity of both 0. This should immediately prompt you to test the functions with simple examples.


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

Showing percentage of false predictions is interesting

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
