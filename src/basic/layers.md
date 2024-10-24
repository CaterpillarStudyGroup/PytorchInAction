# Model

```python
class MyModel(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.layer = nn.Sequential([
            nn.Linear(indim=..., outdim=...)
            nn.Dropout(p=...)
        ])

    def forward(self, x):
        x = self.layer(x)
        return x
```

# Dropout

```python
import torch.nn as nn

# Optional dropout
if lora_dropout > 0.:
    self.lora_dropout = nn.Dropout(p=lora_dropout)
else:
    self.lora_dropout = lambda x: x
```