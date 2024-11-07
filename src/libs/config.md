# argparse

```python
import argparse

def parse_args():
    parser = argparse.ArgumentParser()
    parser.add_argument("--src_img", type=str, default="./inputs/src_img.png")
    args = parser.parse_args()
    return args

args = parse_args()
```

# omegaconf

```python
from omegaconf import OmegaConf

config = OmegaConf.load(args.config)

```