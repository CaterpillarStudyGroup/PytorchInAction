```python
import torch.nn as nn

# Optional dropout
if lora_dropout > 0.:
    self.lora_dropout = nn.Dropout(p=lora_dropout)
else:
    self.lora_dropout = lambda x: x
```