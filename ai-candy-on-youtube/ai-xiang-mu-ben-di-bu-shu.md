# 🖥️ AI项目本地部署

* [https://www.youtube.com/watch?v=QuRvlo\_woBA](https://www.youtube.com/watch?v=QuRvlo\_woBA)

{% code fullWidth="true" %}
```
【AI开源项目，本地部署的基本环境构建】

      -- 显卡驱动更新
      -- 安装 GPU 动态库，(Nvidia 英伟达显卡：CUDA )
      -- 安装 Python，Pytorch,  Git,  ffmpeg 等

-------------------------------------------------------------------------------------------

	#. 安装网卡驱动（更新）

	        https://www.nvidia.com/Download/index.aspx?lang=en-us
	        -- 再按要求，安装 Python, Git, ffmpeg 
	
	#. 安装 CUDA Toolkit （含显卡驱动）

	        https://developer.nvidia.com/cuda-toolkit-archive
	        -- 再按要求，安装 Python, Git, ffmpeg ...

	#. 查 CUDA 和 显卡驱动版本号

	       nvcc --version
	       nvidia 系统信息
		
	#. 直接安装Anaconda (含Python)

	       https://www.anaconda.com/download
	         -- 再按要求，安装 Git, ffmpeg ...
	         -- 需要安装（更新）显卡
	         -- 需要将Python路径设置的系统环境里
	 
	#. 安装Python, pip, Pytorch, Git, ffmpeg

	       https://www.python.org
                       https://pypi.org/project/pip
	       https://pytorch.org
                       https://git-scm.com
                       https://ffmpeg.org
                       
        #. 检查GPU是否被激活
        
	       1. 安装 pytorch
	          https://pytorch.org/get-started/locally/

	       2. 进入 python
		  >>import torch    
		  >>torch.cuda.is_available()    ## 查看是否可以使用GPU， 结果为True 就是已经可以激活GPU
		

	 
```
{% endcode %}
