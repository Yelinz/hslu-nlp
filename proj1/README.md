# Project 1

## Feedback stage 1:
Everything ok

## Feedback stage 2:
Grade: 4.75
Stage 1 decisions: - In stage 1, you show how you plan to do things and why. It needs to be more detailed and should include all decisions, together with justifications. E.g. how do you truncate exactly and what is your reasoning? Why did you not decide to remove any other words that are not stopwords?
- For used features: Do you use the words? Because one could compute additional features like length (not saying you should do this, just to be more precise).
- Input format not clear enough. I assume you want to use single vectors for question/passage, but not certain.
- What's your maximum input length? Is it the same for passage and question?
- Too many layers (compare to the example from the PyTorch exercise from week 1).
- Outputting a probability is useful.
- Why embedding loss when your output has shape 1x1?
- How many experiments do you plan to run exactly? Write that down.
- Unusual to do cross-validation for NN training. Training an NN takes a long time, and CV requires to retrain for every fold.
- Why F1 score. Did you really mean to say area over curve (AOC)?
- Any reasoning for the expectation?
Introduction: Good introduction. Good length.
Setup: Good.
Preprocessing: The idea is that the implementation documentation is already present in your stage 1 submission.
Lemmatization is not used by word embedding methods, so lemmatized words will sometimes not have an associated word vector (fastText will only use n-grams, not the full word).
Focus on clarity in preprocessing and not on speed. Preprocessing will run once, so is never the bottleneck.
Good justification for truncation.
You add a lot (!) of padding to questions if you pad them to max_len. Questions are usually super short in this dataset.
Do you know what max_len is? Would be interesting to print.
In your test of preprocessing, you see the output "question: tokenized: ['iran', 'afghanistan', 'speak', 'language']". Do you think that is sufficient input for a model to identify what the question was? Make sure to look at the output of a test and think if it makes sense.
Model: In your model, you project every query word down to 1 dimension (i.e. a scalar), then in layer 2 you aggregate over the words in the query. Do you think 1 scalar for each input word is enough to capture the meaning for this task?
What was the reason for removing experiments? Time?
Good use of the lightning library.
Good early stopping criterion.
Should not call self.forward() but self() (as discussed in in-class exercises).
Not sure if AUROC is useful here. What's your reason for choosing the ROC curve and not, for example, the precision-recall curve?
Good correctness test.
Training: Again, think about implementation already in stage 1.
Should only save best model checkpoint, otherwise you run into storage limits in later projects.
Nice use of Optuna.
Evaluation: Include a confusion matrix in your notebook (see grading checklist), especially if you reference it.
Describe your results in more detail in the notebook (did you run out of time?). Show the results and describe them. Compare validation and test performance. What is the outcome of your experiments? What setting works best?
If you want, add an error analysis.
Interpretation: Do some interpretation of your results here: What can be expected of word embeddings for this task? Why did they perform well/badly? Interpret the results of your experiments. If you did hyperparameter tuning mostly, what do your results say about the impact of those hyperparameters?
Reflection: The number of layers does not determine the dimensionality of your input. I can reduce a 10-dimensional tensor to 1 dimension with .reshape(-1).
Reproducibility: Can pin the library versions in the setup. Python version not mentioned.
Form: Tools mentioned in setup, but is it complete? No AI help?
Experiment Tracking: Good. All relevant metrics shown. Should give runs a meaningful name. Can use smoothing for very erratic curves (e.g. train_auc or val_accuracy).
Minor point for improvement: Sorting of runs is by val_accuracy (good) but it takes the val_accuracy at the end of training.
Things to improve: - More detailed stage 1 decisions and justifications. Plan your entire project here already.
- Show detailed results and describe them.
- Describe and discuss the outcome of your experiments.
- Bring code and documentation closer together by interleaving code and documentation cells in your notebook.


## Feedback presentation: 
Grade: 4.25
Preprocessing: Lemmatization useful for fastText? Good justification of truncation. Can you include short form of justifications in your slides?
Input/output format: Appears on model slide. Make this a separate slide. Not really clear how question/passage is padded and input into the model exactly.
Model: Didn't understand the squeeze until I looked at the code. But this is a misunderstanding how layers work. Talk to me if this is not clear to you.
What are the layer sizes?
Why did you choose an embedding loss when the output is a single number? No justification given.
Experiments: Optimizer choice and loss choice are hyperparameters, too.
Explicitly show settings of your hyperparameters that you tried and how many experiments you ran.
Results: Try to show any result that you have. Please let me know why you didn't. Not enough time or not satisfied with the results?
Interpretation/learnings: I did not understand your reflection. You said you misunderstood the task but I don't see what the problem was. It seems you did the correct task?
Your future reflections should be about the content of the project. Why did/didn't something work well?


- Present input/ hidden dimensions
- Step by step explanation on how input dimension is achieved
- Show results