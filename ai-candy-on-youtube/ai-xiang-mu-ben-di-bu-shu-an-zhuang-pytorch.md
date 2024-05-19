---
description: '- YouTube 视频 相关文本内容'
---

# 1️⃣ 安装 Pytorch 运行环境

* 视频：[【正确安装驱动，激活GPU】](https://youtu.be/QuRvlo\_woBA)
*   视频：[【构建PyTorch环境, 激活GPU】](https://youtu.be/oc57V1rV7-4)



{% code fullWidth="true" %}
```markup
// Python 环境配置步骤 （AI 项目本地部署）

      -- 显卡驱动更新
      -- 安装 GPU 动态库，(Nvidia 英伟达显卡：CUDA Toolkit )
      -- 安装 Python，Pytorch,  Git,  ffmpeg 等

    -----------------------------------------------------------------------------------

	#. 查 CUDA 和 显卡驱动版本号

            https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html
            
	    nvcc --version   ### CUDA版本, nvidia 系统信息 
	    nvidia-smi       ### CUDA版本, nvidia 系统信息 

	#. 安装（更新）显卡驱动

	     https://www.nvidia.com/  
	
	#. 安装 CUDA Toolkit （含显卡驱动）

	     https://developer.nvidia.com/cuda-toolkit-archive
	       
```
{% endcode %}

{% hint style="danger" %}
正确安装 CUDA Toolkit 和 显卡驱动版本，是能够使用GPU的关键。
{% endhint %}

{% code fullWidth="true" %}
```
// 安装 Python

	------------------- 安装 Python 方法 1 ----------------------------------------------------------------------- 
	
	#. 单独安装 Python

	       https://www.python.org
	       -- 需要将Python路径设置的系统环境里  
	
               #. 安装 pip 

                   https://pypi.org/project/pip
               
               #. 安装 Git  
          
                   https://git-scm.com
               
               #. 安装 ffmpeg     
                 
                   https://ffmpeg.org
	
	------------------- 安装 Python 方法 2 -----------------------------------------------------------------------                
                       
 	#. 直接安装 Anaconda (含Python, pip)

           1. Anaconda:
               https://www.anaconda.com/download
	
	       清华大学镜像：
	          https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/
	
	
           2. Miniconda
	       https://docs.anaconda.com/free/miniconda/
	       -- conda install -y jupyter  (安装Jupyter notebook)
  	                      
        ------------------------------------------------------------------------------------------------------------
```
{% endcode %}

{% code fullWidth="true" %}
```
// PyTorch 环境配置步骤

       
        #. 安装和配置 PyTorch 环境.
		
		1. 进入"Anaconda Prompt".
		
		2. 输入命令 (建立虚拟环境：名为pytorch , python 版本为 3.12）
		
		conda create -n pytorch python=3.12
		conda activate pytorch (进入/激活 名为 pytorch 的虚拟环境）
		
		pip list (显示已安装的Package)
		
		conda install ...... (安装包）
		conda deactivate     （退出虚拟环境）
        
        
        #. 检查GPU是否被激活
        
	       1. 安装 pytorch 
	
	          conda activate pytorch  (注意：需要先进入名为 pytorch 的虚拟环境） 
	
	          https://pytorch.org/get-started/locally/  (选择本机环境，生成命令行，执行安装）
	
	例如： conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia

	       2. 进入 python
	
		  >>import torch    
		  >>torch.cuda.is_available()    ## 查看是否可以使用GPU， 结果为True 就是已经可以激活GPU
		
```
{% endcode %}

{% hint style="info" %}
检查安装的虚拟环境:\
conda info --envs

conda env list
{% endhint %}

{% code fullWidth="true" %}
```
//  安装 Jupyter Notebook / Lab （在 PyTorch 环境中）

	https://jupyter.org/install
	
	pip install jupyterlab  （建议安装Lab）
	pip install notebook     
	
	进入界面：
	jupyter lab
	jupyter notebook 
	
```
{% endcode %}

{% code fullWidth="true" %}
```
//  打开 jupyter lab

  1. 打开 Anaconda 的命令行窗口： anaconda Prompt (miniconda3)
  2. 执行 conda activate pytorch，进入 Pytorch 环境空间   // 能看到前面有（pytorch）
  3. 运行命令 jupyter lab
  
```
{% endcode %}

{% hint style="danger" %}
删除安装的虚拟环境:\
conda remove -n <**env\_name**> --all
{% endhint %}
