# Linear-Proofs-NLP
A project in which NLP is used to generate step by step proofs for solving linear equations.

## Introduction
In this project, I collaborated with Robert Perry who has expertise in NLP and computational linguistics.  Inspired by the paper titled "Deep Learning for Symbolic Mathematics", https://arxiv.org/abs/1912.01412, by Guillaume Lample and François Charton, we sought to explore whether we can extend the ability of NLP models to predict the steps in solving a math problem in addition to giving the final solution to the problem.  In the Lample and Charton paper, they applied an NLP model to predict only the final answer to calculus integral problems, among other math problems.  We focused on a very simple type of math problem, the linear equation problem.  We sought to have the NLP model predict the steps along with the algebraic properties used for each step such as the additive inverse property. 

You can find the google colab notebook here: https://colab.research.google.com/github/rickyhan24/Linear-Proofs-NLP/blob/main/Linear_Proofs_LSTM.ipynb

## Data Generation
In order to have a dataset of problems and steps leading up to the final solution, we created them from scratch.  A template for the problem/proof statements in infix notation was used to generate variations for the linear equation problem ax+b=d.

Here is what a typical problem and solution looks like:
 
 0. 10 9 8
 1. 10 * x + 9 = 8	Given
 2. 10 * x + 9 - 9 = 8 - 9	subtraction property of equality
 3. 10 * x + 0 = 8 - 9	additive inverse
 4. 10 * x = 8 - 9	additive identity
 5. 10 * x / 10 = ( 8 - 9 ) / 10	division property of equality a != 0
 6. 1 * x = ( 8 - 9 ) / 10	multiplicative inverses
 7. x = ( 8 - 9 ) / 10	multiplicative identity
 8. x = ( 8 - 9 ) / 10	canonical equivalent equation 

## Data Preprocessing
A tokenizer was used to assign tokens to each math symbol, newline symbol, step number, and double newline symbol in our dataset of proofs.  Then a list of tokenized n-grams from each proof was created; these tokenized n-grams were then zero-padded.  These padded proof-line encodings excluding the last symbol were used to define the predictors set, and the last symbols were used to define the label set.  The label set is one-hot encoded using the vocabulary of tokens.  

The predictors and label sets are then split into training and test sets.

## Building the Model
An LSTM model is trained to predict the next symbol given a predictor, using our training predictor and label sets.  After training for upwards of about 45 epochs, we can achieve a training accuracy of 97%.  Testing the model on the test set achieves the same level of accuracy.  This is impressive given that the model has not seen the predictors and labels in the test set, although the tokens that occur in the test set are the same ones that occur in the training set.

## Applying the Model to Generate Proofs
The model only predicts the next symbol given an initial segment of symbols.  So, in order to predict the steps and solution to a problem, we would need to apply the model multiple times to fill out the necessary symbols given the statement of the problem.  To this end, we defined a function that generates the next symbols of the proof given a seed text.  In particular, if we feed the first two lines of the problem/proof into the model, we get the symbol that comes right after it.  We then take this result and feed it back into the model to get the second symbol and so on until we fill out the entire proof.  For example, the seed text could be:
 
 0. 10 9 8
 1. 10 * x + 9 = 8	Given

Then, our proof-generating function would output the problem and the proof as follows:
 
 0. 10 9 8
 1. 10 * x + 9 = 8	Given
 2. 10 * x + 9 - 9 = 8 - 9	subtraction property of equality
 3. 10 * x + 0 = 8 - 9	additive inverse
 4. 10 * x = 8 - 9	additive identity
 5. 10 * x / 10 = ( 8 - 9 ) / 10	division property of equality a != 0
 6. 1 * x = ( 8 - 9 ) / 10	multiplicative inverses
 7. x = ( 8 - 9 ) / 10	multiplicative identity
 8. x = ( 8 - 9 ) / 10	canonical equivalent equation  

We then created a dictionary that has the correct full proof as key and the predicted full proof as value, for each full proof in our entire dataset.  This allows us to see how many full proofs the model got right.  After training the model for about 45 epochs, and obtaining a 97% training accuracy, the model got 100% of the full proofs correct.  This is truly amazing given that the model didn't see the proofs in the test set.  The model 'learned' how to solve linear equations, giving the steps and the associated algebraic properties.

## Conclusion
We have seen that the LSTM model can successfully predict the next symbol in a linear equation proof to a high degree of accuracy.  Furthermore, the model can predict the full proofs given the problem statement with 100% accuracy if we train the model long enough.  This shows that NLP models can not only handle natural language text data such as newspaper articles but can also handle solving math problems such as linear equation problems; and the model can give the steps and properties used to arrive at the result rathern than just giving the result.  
