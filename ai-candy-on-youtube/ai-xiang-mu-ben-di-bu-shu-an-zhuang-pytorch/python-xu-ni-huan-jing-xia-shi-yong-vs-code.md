---
description: Implementing PyTorch or PyCharm with VS code in virtual python environment
---

# 2️⃣ 在Python虚拟环境下使用 VS Code or PyCharm

## Check NVIDIA Driver and CUDA Version&#x20;

1. cmd
2. nvidia-smi

## VS Code

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
print("CUDA Version is", torch.version.cuda)
print("Pytorch Version is", torch.__version__)
print("Whether CUDA is supported by our system:", torch.cuda.is_available())

device = 'cuda:0' if torch.cuda.is_available() else 'cpu'
print(device)

```

## PyCharm

* Install PyCharm
* Create a new project
* Select Custom environment
* Select existing
* Select Conda Type
* Select Environment

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
