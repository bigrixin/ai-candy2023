# 3️⃣ Create python virtual environment in Linux



<pre data-full-width="true"><code><strong>======= Create virtual environment in Linux ===================================
</strong>
mkdir /scratch/mypy                                  // Python root directory
cd  /scratch/mypy       
virtualenv-3 /scratch/mypy                           // Create a virtual environment

======= Use virtual environment ===================================
source /scratch/mypy/bin/activate                    // Start a virtual environment
(venv) python -V
>>>3.6.8

</code></pre>

{% code fullWidth="true" %}
```
=======  create a virtual environment with python 3.9 ===================== ======= 
-----------------------

mkdir /scratch/pytorch                              // Create a directory
cd  /scratch/pytorch  
virtualenv --python python3.9 venv                  // Create a virtual environment with v3.9
source /scratch/pytorch/venv/bin/activate
(venv) python -V
>>>3.9.7
pip list
------------------------------------
Package    Version
---------- -------
pip        24.0
setuptools 69.5.1
wheel      0.43.0
------------------------------------
```
{% endcode %}

{% code fullWidth="true" %}
```
======= Install pytorch in virtual environment ===================================

source /scratch/pytorch/venv/bin/activate            // Entry the virtual environment

(venv) pip3 install torch torchvision torchaudio     // Install Pytorch
(venv) torch.cuda.is_available()
(venv) true

--------------install multi-packages-------
(venv) cat requirements.txt
numpy==1.21.5
pandas==1.3.5

(venv) pip install -r requirements.txt
```
{% endcode %}

{% code fullWidth="true" %}
```
======= Usefull command =================================================

(venv) which python                             // Check current virtual environment location for Python interpreter
(venv) deactivate                               // Exit virtual environment

======== Delete a virtual environment ================================================
rm -rf path/venv-name                           // Delete virtual environment *****


re: https://www.dataquest.io/blog/a-complete-guide-to-python-virtual-environments/
```
{% endcode %}

> <mark style="color:blue;">**Use python environment in VS Code (Linux)**</mark>\
>
>
> * source /scratch/pytorch/venv/bin/activate                       // Entry the virtual environment
> *   (venv) which python
>
>     &#x20;           /scratch/pytorch/venv/bin/python                          //  <mark style="color:red;">copy this</mark>
> * Open VS Code
> * Ctrl +Shift + P - > python: select interpreter select&#x20;
> * Enter interpreter path, entry:\
>   /scratch/cszhai\_pytorch/venv/bin/python

{% code fullWidth="true" %}
```
// Error sovle

Error: Python error message "kernel does not exist"  

    pip uninstall jupyter_client            
    pip uninstall jupyter_core              
    pip uninstall jupyter-events            
    pip uninstall jupyter-lsp               
    pip uninstall jupyter_server            
    pip uninstall jupyter_server_terminals  
    pip uninstall jupyterlab              
    pip uninstall jupyterlab_pygments       
    pip uninstall jupyterlab_server          
    pip uninstall ipykernel
    
    pip install jupyterlab

Error:  Could not determine jupyterlab build status without nodejs

    conda install -c conda-forge nodejs
```
{% endcode %}
