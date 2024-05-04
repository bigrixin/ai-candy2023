---
description: Implementing PyTorch or PyCharm with VS code in virtual python environment
---

# 2️⃣ 在Python虚拟环境下使用 VS Code or PyCharm

## Check NVIDIA Driver and CUDA Version&#x20;

1. cmd
2. nvidia-smi

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## VS Code

1. Install VS Code
2. Open VS Code
3. Install extention (Python, Jupyter)&#x20;
4. Ctrl + Shift + P
5. Python: Select Interpreter  (Select virtual python environment)
6. Select pytorch (Python 3.11.9)

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## PyCharm

* Install PyCharm
* Create a new project
* Select Custom environment
* Select existing
* Select Conda Type
* Select Environment

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

## Check version and GPU (<mark style="color:red;">batch file</mark>)

<pre><code>## check_v.py

import torch
import sys

print("Python Version is",sys.version)
print("CUDA Version is", torch.version.cuda)
print("Pytorch Version is", torch.__version__)
print("Whether CUDA is supported by our system:", torch.cuda.is_available())

device_id =  torch.cuda.current_device()
print("Current Device name:", torch.cuda.get_device_name(device_id))

<strong>cuda_id = "GPU available for cuda:"+str(device_id)
</strong>device = cuda_id if torch.cuda.is_available() else 'Only CPU can be used'
print(device)

</code></pre>

```
## entryTorch.bat

call conda activate pytorch
python check_v.py
```



### Install cuDNN

1. [https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)
2. Download cuDNN Library
3. Select Windows -> x86\_64 -> Tarball -> 12 (CUDA  Version)
4. Download and Unzip
5. go to C:\Program Files\NVIDIA Corporation
6. Create new folder: CUDNN
7. Create a new folder, name it is v9.0 which in the folder CUDNN
8. copy unzip files into the v9.x folder
9. add path C:\Program Files\NVIDIA Corporation\CUDNN\v9.x\bin into **Environment Variables**
