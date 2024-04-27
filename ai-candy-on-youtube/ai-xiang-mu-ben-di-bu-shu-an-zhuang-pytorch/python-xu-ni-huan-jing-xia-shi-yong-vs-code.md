---
description: Implementing PyTorch with VS code in virtual python environment
---

# 2️⃣ 在Python 虚拟环境下使用 VS Code

1. Install VS Code
2. Open VS Code
3. Install extention (Python, Jupyter)&#x20;
4. Ctrl + Shift + P
5. Python: Select Interpreter  (Select virtual python environment)
6. Select pytorch (Python 3.11.9)

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>



```
// Check CUDA

import torch
print("Pytorch CUDA Version is", torch.version.cuda)
print("Whether CUDA is supported by our system:", torch.cuda.is_available())
```
