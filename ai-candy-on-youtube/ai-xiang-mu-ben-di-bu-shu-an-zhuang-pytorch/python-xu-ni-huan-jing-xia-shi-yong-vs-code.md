---
description: Implementing PyTorch with VS code in virtual python environment
---

# 2️⃣ 在Python 虚拟环境下使用 VS Code

1. Install VS Code
2. Install extention (Python, Jupyter) in VS Code
3. Select virtual python environment



```
// Check CUDA

import torch
print("Pytorch CUDA Version is", torch.version.cuda)
print("Whether CUDA is supported by our system:", torch.cuda.is_available())
```