# Lesson 8: Validation Strategies

## Learning Goals

- Cross-Validation.

## 1. The Problem with Train/Test

Your test score depends on _which_ random 20% of data you picked. Maybe you got lucky (or unlucky).

## 2. K-Fold Cross-Validation

1.  Split data into K parts (Folds).
2.  Train on K-1, Test on 1.
3.  Repeat K times rotating the Test fold.
4.  Average the scores.

## Key Takeaways

- CV gives a much more reliable estimate of model performance.
- GridSearch often uses CV to find the best Hyperparameters.
