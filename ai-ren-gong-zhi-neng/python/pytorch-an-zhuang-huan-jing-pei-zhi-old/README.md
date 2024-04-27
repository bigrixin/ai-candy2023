# Pytorch 安装环境配置 (old)

### **建立环境**

1.  建立一个新环境 name = pytorch

    conda create -n pytorch python
2. 进入 pytorch 工作环境 conda activate pytorch
3. 查看安装包 pip list
4.  查询显卡型号:

    任务管理器中，查看 Geforce GTX 950M https://developer.nvidia.com/cuda-gpus

    查看驱动版本： C:\Program Files\NVIDIA Corporation\NVSMI>nvidia-smi.exe

    Driver Version: 388.57 ==>需要升级驱动 https://developer.nvidia.com/drive/downloads

> <mark style="color:red;">**查看驱动版本**</mark>：check graphic card and driver both suppot the CUDA varsion
>
> \
> &#x20;   cd C:\Program Files\NVIDIA Corporation\NVSMIconda       //windows
>
> > nvidia-smi.exe
>
> ```
>    NVIDIA-SMI version: 512.15
>    Card type: NVIDIA GeForce 
>    CUDA Version: 11.6
> ```
>
> <mark style="color:orange;">**Check Compute Capabilities**</mark><mark style="color:blue;">**:**</mark>
>
> [https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)
>
> ```
> ### CUDA Toolkit version need match Card Driver Version 
> 	
> 	CUDA 12.0.x     >=527.41 (Windows Driver)
> 	CUDA 11.6.x     >=452.39 (Windows Driver)
> ```

&#x20;\| NVIDIA-SMI 512.15 Driver Version: 512.15 === CUDA Version: 11.6 |



### 安装CUDA Toolkit and NVIDIA Driver

&#x20;    NVIDIA Driver Downloads: [https://www.nvidia.com/download/index.aspx](https://www.nvidia.com/download/index.aspx)

&#x20;    CUDA Toolkit Downloads: [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)



### **安装 pytorch**&#x20;

1.  选择系统，软件包，下载版本，产生下载安装命令\
    https://pytorch.org/get-started/locally/   &#x20;



Anaconda Prompt (anaconda3)

```
conda install pytorch torchvision torchaudio pytorch-cuda=11.6 -c pytorch -c nvidia
```

Why pip install does not working?

```
pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu117
```

\
<mark style="color:blue;">**查询是否有torch**</mark>

<pre class="language-html"><code class="lang-html">// Run Anaconda Prompt 
 (base) conda activate pytorch      ## activate pytorch env
 (pytorch) python
 >>import torch                    ## 查看是否可以使用GPU 
 >>torch.cuda.is_available() 
    <a data-footnote-ref href="#user-content-fn-1">True</a>
 >>torch.version.cuda
    <a data-footnote-ref href="#user-content-fn-2">11.6</a>
</code></pre>

{% hint style="info" %}
<mark style="color:orange;">**The solution of GPU is not available**</mark> [<mark style="color:orange;">**(**</mark>_<mark style="color:blue;background-color:yellow;">if torch.version.cuda return none</mark>_](#user-content-fn-3)[^3]<mark style="color:orange;">**)**</mark>



* [x] Check CUDA Toolkit version match driver version
* [x] conda uninstall pytorch
* [x] conda uninstall cpuonly
* [ ] select to install pytroch v1.14 or v2.0
  * conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia (v1.14)
  * [https://pytorch.org/get-started/pytorch-2.0/](https://pytorch.org/get-started/pytorch-2.0/)
{% endhint %}

{% hint style="info" %}
[<mark style="color:red;">**Run Jupyter lab with GPU**</mark>](#user-content-fn-4)[^4]

Method 1: Run Anaconda Navigator, Launch JupyterLab

Method 2: Run Anaconda Prompt (Anaconda3) -- entry command, \
&#x20;                 then run jupyterlab.bat\
\
**jupyterlab.bat:**\
_D:\ProgramData\Anaconda3\python.exe d:\ProgramData\Anaconda3\cwp.py d:\ProgramData\Anaconda3 d:\ProgramData\Anaconda3\python.exe d:\ProgramData\Anaconda3\Scripts\jupyter-lab-script.py "%USERPROFILE%/"_
{% endhint %}

> **Install pytorch genmetric**\
> [https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html](https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html)
>
> ```
> pip install torch-scatter torch-sparse torch-cluster torch-spline-conv torch-geometric -f https://data.pyg.org/whl/torch-1.13.0+cu117.html
> ```

```
torch.cuda.device_count()      // 
torch.cuda.current_device() 
torch.cuda.device(0) 
torch.cuda.get_device_name(0)
torch.zeros(1).cuda()      // 
```



```
# setting device on GPU if available, else CPU
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
print('Using device:', device)
print()

#Additional Info when using cuda
if device.type == 'cuda':
    print(torch.cuda.get_device_name(0))
    print('Memory Usage:')
    print('Allocated:', round(torch.cuda.memory_allocated(0)/1024**3,1), 'GB')
    print('Cached:   ', round(torch.cuda.memory_reserved(0)/1024**3,1), 'GB')
```

&#x20;\## check torch version //v1.11

&#x20;   print(torch.**version**)                // in jupyter \
&#x20;   python -c "import torch; print(torch.**version**)"

&#x20;\##To move tensors to the respective device:    \
&#x20;   torch.rand(10).to(device)&#x20;

&#x20; \##To create a tensor directly on the device:     \
&#x20;   torch.rand(10, device=device)

[<mark style="color:blue;">**安装其他包**</mark>](#user-content-fn-5)[^5]

```
// install torch geometric

   refer to:  https://pytorch-geometric.readthedocs.io/en/latest/notes/installation.html
   conda install pyg -c pyg    //conda
   pip install torch-scatter torch-sparse torch-cluster torch-spline-conv torch-geometric -f https://data.pyg.org/whl/torch-1.11.0+cu115.html
```



<pre><code>// install jurpyter in pytorch enveriment
   <a data-footnote-ref href="#user-content-fn-6">conda list</a>                //check installed parkage
   conda install jupyter
</code></pre>

```
Does PyTorch see any GPUs?		torch.cuda.is_available()
Are tensors stored on GPU by default?	torch.rand(10).device
Set default tensor type to CUDA:	torch.set_default_tensor_type(torch.cuda.FloatTensor)
Is this tensor a GPU tensor?		my_tensor.is_cuda
Is this model stored on the GPU?	all(p.is_cuda for p in my_model.parameters())
```



[^1]: is available

[^2]: torch cuda version

[^3]: 

[^4]: 

[^5]: 

[^6]: 
