# Lesson 3: Frameworks

## Learning Goals

- PyTorch vs TensorFlow.

## 1. PyTorch (Meta)

- Dynamic Graph (Define by Run).
- Pythonic. Debuggable.
- Preferred by Researchers.

```python
import torch.nn as nn
class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc = nn.Linear(10, 1)

    def forward(self, x):
        return self.fc(x)
```

## 2. TensorFlow / Keras (Google)

- Static Graph (originally).
- Production focused (TFX, TF Lite).
- `model.fit()` is very high level.

## Key Takeaways

- Learn PyTorch first. It helps you understand what is actually happening.
- Keras is great for quick prototypes.
