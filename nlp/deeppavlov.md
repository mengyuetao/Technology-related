wget https://www.python.org/ftp/python/3.7.5/Python-3.7.5.tgz
tar xvf
make install
python3 -m venv pydeeppavlov/

yum install openblas-devel
scl enable devtoolset-7 bash

wget https://support.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.10.5.tar.gz
tar -zxvf hdf5-1.10.5.tar.gz
./configure --prefix=/usr/local/hdf/hdf5
make install

INCLUDE_DIRS=/usr/local/hdf/hdf5/include
pip install deeppavlov


https://blog.csdn.net/wflwn/article/details/78680604  linux虚拟机安装MKL库


pip install tensorflow=1.15

流程
https://towardsdatascience.com/the-bert-based-text-classification-models-of-deeppavlov-a85892f14d61

https://zhuanlan.zhihu.com/p/151397491?from_voters_page=true Deeppavlov 20年DRL前沿新课-《深度强化学习前沿主题》课程分享
