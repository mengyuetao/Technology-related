
yum install openssl-devel
yum install libffi-devel


wget https://www.python.org/ftp/python/3.7.5/Python-3.7.5.tgz
tar xvf
make install


python3 -m venv ev1/
source activate




# conda

https://www.jianshu.com/p/eaee1fadc1e9 Anaconda完全入门指南
https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#viewing-a-list-of-your-environments

https://blog.csdn.net/yunken28/article/details/105035577/ conda安装pytorch总是下载失败，解决方法


```
conda env list  
conda --version
conda create -n learn python=3
activate learn

conda install
conda list
conda env export > environment.yaml

conda info --envs

```
