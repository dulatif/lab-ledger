# Lesson 6: Metrics

## Learning Goals

- Confusion Matrix.
- F1-Score.

## 1. Accuracy Paradox

If 99% of emails are NOT spam, a model that predicts "Not Spam" for everything has 99% Accuracy. But it's useless.

## 2. The Confusion Matrix

|                     | Predicted Positive  | Predicted Negative  |
| ------------------- | ------------------- | ------------------- |
| **Actual Positive** | True Positive (TP)  | False Negative (FN) |
| **Actual Negative** | False Positive (FP) | True Negative (TN)  |

## 3. Metrics

- **Precision (TP / TP+FP)**: Of all labeled "Spam", how many actually were? (Avoids False Positives).
- **Recall (TP / TP+FN)**: Of all actual "Spam", how many did we find? (Avoids False Negatives).
- **F1-Score**: Harmonic mean of Precision and Recall.

## Key Takeaways

- Optimize Recall for Cancer Detection (Missed cases are fatal).
- Optimize Precision for YouTube Recommendations (Bad suggestions are annoying, but okay).
