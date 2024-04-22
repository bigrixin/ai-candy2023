# 🖥️ AI项目本地部署-安装 Pytorch

* [https://www.youtube.com/watch?v=QuRvlo\_woBA](https://www.youtube.com/watch?v=QuRvlo\_woBA)
*



{% code fullWidth="true" %}
```markup
【AI开源项目，本地部署的基本环境构建】

      -- 显卡驱动更新
      -- 安装 GPU 动态库，(Nvidia 英伟达显卡：CUDA Toolkit )
      -- 安装 Python，Pytorch,  Git,  ffmpeg 等


	#. 安装（更新）显卡驱动

	     https://www.nvidia.com/  
	
	#. 安装 CUDA Toolkit （含显卡驱动）

	     https://developer.nvidia.com/cuda-toolkit-archive
	       
	#. 查 CUDA 和 显卡驱动版本号

	    CUDA版本：    nvcc --version
	    显卡版本：     nvidia 系统信息
		
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
       
        #. 安装和配置 PyTorch环境.
		
		1. "Anaconda Prompt".
		
		2. 输入命令
		
		conda create -n pytorch python=3.11
		conda activate pytorch
		
		pip list (显示已安装的Package)
		
		conda install ......
        
        
       	       https://pytorch.org
       
        #. 检查GPU是否被激活
        
	       1. 安装 pytorch
	          https://pytorch.org/get-started/locally/

	       2. 进入 python
		  >>import torch    
		  >>torch.cuda.is_available()    ## 查看是否可以使用GPU， 结果为True 就是已经可以激活GPU
		

	 
```
{% endcode %}

*
* {% code fullWidth="true" %}
  ```
        -- 显卡驱动更新
        -- 安装 GPU 动态库，(Nvidia 英伟达显卡：CUDA Toolkit )
        -- 安装 Python，Pytorch,  Git,  ffmpeg 等
  ```
  {% endcode %}
