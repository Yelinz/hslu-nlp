# Project 2

## Feedback stage 1:
Adding 8 zeros as separator might not work since that is not present as a string in gloVe. Choose something else as a separator embedding.


## Feedback stage 2: 
Grade: 4.75
Stage 1 decisions: overall ok, but some questionable decisions..
Introduction: Good
Setup: Good
Preprocessing: 8000 out-of-vocabulary words is quite substantial. Also, you should not just cut out unknown words. represent them by a preset vector.
Model: You should only pass the last timestep of the rnn to the classifier network.
Training: ok
Evaluation: ok
Interpretation: rather minimal. Why do you see the results your are seeing? Go more into depth.
Reproducibility: good
Form: 
Experiment Tracking: 
Things to improve: 
- Put more thought into preprocessing. Maybe look at some examples in the data and understand what is important for the specific task.
- Justify your decisions and double check your reasoning


## Feedback presentation: 
Grade: 5
Preprocessing: Good. Give an explanation for 8x 0 vectors as separator.
Input/output format: 304*300 = 91200. Reshape is not a dimension reduction. Flattening shouldn't be done on input. We also don't do this in the in-class exercises. This slide should also mention how you combine question and passage. Concatenate [question, 8x 0 vector, passage] for example. Check out my feedback for project 1 for an example. Hidden size is not on this slide, but on the next.
Model: This is not bad, but unfortunately by this point we have no idea what your hidden sizes are. Would be nice to keep the important information together.
Experiments: Good. I also like the selection of values to test.
Results: Good. I didn't realize you mentioned the best values until I looked at this slide again. Why don't you mention the best hidden size when you talk about it in the bullet point?
Is your confusion matrix on the test set? Say that. Would be interesting to see the two parts of the balanced accuracy. Yes accuracy is 80% and no accuracy is 33%? Trying to read it roughly from your confusion matrix.
As I said in class, I prefer the matplotlib confusion matrix. The wandb one is hard to read and doesn't have the exact values.
Interpretation: Good, but you only interpret hyperparameters. What about an interpretation of the result, and your confusion matrix? Comparison to previous project?
