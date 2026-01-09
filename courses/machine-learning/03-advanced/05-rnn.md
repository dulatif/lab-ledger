# Lesson 5: Sequence Models (RNN)

## Learning Goals

- RNN / LSTM.
- Sequential Data.

## 1. Recurrent Neural Networks

Networks with "Memory". The output depends on the Input AND the Previous Hidden State.
Good for Time Series, Text, Audio.

## 2. The Problem

Vanishing Gradient. RNNs forget long-term dependencies.

## 3. LSTM / GRU

Long Short-Term Memory.
Introduces "Gates" (Forget, Input, Output) to control information flow.
Can remember context over hundreds of steps.

## Key Takeaways

- Use LSTMs for Time-Series forecasting.
- For NLP, Transformers have largely replaced RNNs.
