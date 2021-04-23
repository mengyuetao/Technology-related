沉稳
1 不要随便显露你的情绪
2 不要逢人诉说你的困难和遭遇 （）
3 蜘蛛网理论
4 让别人先讲话（尊重别人，暴露破绽，来不及思考）
5 不要唠叨你的不满 世界上没有人同情别人的




cd ~/kaldi/tools/
./extras/check_dependencies.sh
yum install gcc-c++ automake  sox   subversion  zlib-devel   patch  unzip bzip2 libtool    gcc-gfortran
./extras/install_mkl.s

cd tools
make openfst
make cub
make sclite
make sph2pipe

./extras/install_srilm.sh
./extras/install_irstlm.sh  (没有进一步安装)
./extras/install_irstlm.sh 




#python3
cd  /opt/ 
wget "https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz"
tar -zxvf  Python-3.6.3.tgz
cd Python-3.6.3
mkdir /usr/local/python3
 ./configure --prefix=/usr/local/python3
make
make install
cd /usr/bin/
ln -s /usr/local/python3/bin/python3 /usr/bin/python3






#kaldi
cd ~/kaldi/src/
.configuration
make 
make test
make valgrind
make cudavalgrind






