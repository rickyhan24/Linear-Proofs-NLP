# Linear-Proofs-NLP
A project in which NLP is used to generate step by step proofs for solving linear equations.

## Introduction
In this project, I collaborated with Robert Perry who has expertise in NLP and computational linguistics.  Inspired by the paper titled "Deep Learning for Symbolic Mathematics", https://arxiv.org/abs/1912.01412, by Guillaume Lample and François Charton, we sought to explore whether we can extend the ability of NLP models to predict the steps in solving a math problem in addition to giving the final solution to the problem.  In the Lample and Charton paper, they applied an NLP model to predict only the final answer to calculus integral problems, among other math problems.  We focused on a very simple type of math problem, the linear equation problem.  We sought to have the NLP model predict the steps along with the algebraic properties used for each step such as the additive inverse property.  
## Data Generation

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

## Building the Model

## Applying the Model to Generate Proofs

## Conclusion
