## overview

First: leo 2015-5



## kvm服务器配置

#### 服务器环境
操作系统: CentOS 7   mini 最小化安装
虚拟系统 KVM
	 yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install


在安装启动的时候，加入 nomodeset 参数。
如果你已经安装完毕，则可以修改 vi /etc/sysconfig/grub，加入 nomodeset 参数：
GRUB_CMDLINE_LINUX=”rd.md=0 rd.dm=0 KEYTABLE=us SYSFONT=True
 rd.lvm.lv=vg/lv_root rd.luks=0 rd.lvm.lv=vg/lv_swap LANG=en_US.UTF-8
 rhgb quiet nomodeset”
然后执行：
grub2-mkconfig -o /boot/grub2/grub.cfg

2 磁盘分区情况

	Filesystem               Size  Used Avail Use% Mounted on
	/dev/mapper/centos-root   50G  1.2G   49G   3% /
	/dev/sda2                497M   98M  400M  20% /boot


	/dev/mapper/centos-home  467G   33M  467G   1% /home
	/dev/mapper/data-data1   128G   33M  128G   1% /data1


3 文件系统

	 mkdir /data1/vm-images
	semanage fcontext --add -t virt_image_t '/vm-images(/.*)?'
	restorecon -R -v /vm-images


3 网络环境配置
 桥接网卡 br0 配置如下

 1. /etc/sysconfig/network-scripts/ifcfg-em1  
BRIDGE=br0
 2.  /etc/sysconfig/network-scripts/ifcfg-br0

	DEVICE="br0"
	BOOTPROTO="dhcp"
	IPV6INIT="yes"
	IPV6_AUTOCONF="yes"
	ONBOOT="yes"
	TYPE="Bridge"
	DELAY="0"



3. /etc/sysctl.conf:

xxx

	net.ipv4.ip_forward = 1
	sysctl -p /etc/sysctl.conf


xxxx

 	 virt-install \
	 --network bridge:virbr0 \
 	--name win2003_vm1 \
	--ram=16384 \
	--vcpus=12 \
	--disk path=/data2/vm-images/win2003_vm1.img,size=128 \
	--vnc \
	--cdrom=/data1/vm-images/2003.iso





## 快速新建虚拟机

#### 创建磁盘
qemu-img create -f qcow2 /home/data1/images/centos6.5-vm2.qcow2 16G


#### 修改UUID
 uuidgen
01330e4b-ed7a-4825-ba81-55387600e9f6


#### 修改mac

MACADDR="52:54:$(dd if=/dev/urandom count=1 2>/dev/null | md5sum | sed 's/^\(..\)\(..\)\(..\)\(..\).*$/\1:\2:\3:\4/')"; echo $MACADDR


#### 定义虚拟机
virsh define /home/data1/vm-images/centos6.5-vm2.xml

#### 列出全部虚拟机
virsh list --all

#### 启动虚拟机
 virsh start centos6.5-vm2

#### 显示VNC端口
virsh vncdisplay centos6.5-vm2

#### 建立隧道
 ssh -l root -L5900:127.0.0.1:5900  192.168.2.150

#### 使用
realvnc 连接隧道

#### 安装完后
 <boot dev='cdrom'/> 改为 <boot dev='hd'/>


#### 客隆 clone

virt-clone \
--connect qemu:///system \
--original centos6.5-vm1 \
--name centos6.5-vm3 \
--file /home/data1/vm-images/centos6.5-vm3.qcow2




#### 网卡问题:

 /etc/udev/rules.d/70-persistent-net.rules

/etc/sysconfig/network-scripts
DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.150.10
NETMASK=255.255.255.0

#### 主机名称

hostnamectl set-hostname kvm2


## 复制使用的镜像

virsh managedsave centos6.5-LB1

## 修改镜像文件


   Guestfish 参考  http://libguestfs.org/guestfs-recipes.1.html。

   guestfish --rw -a centos63_desktop.img
   <fs> run

   mount /dev/vg_centosbase/lv_root /


## qemu-img 命令

http://blog.csdn.net/tantexian/article/details/40044453


### 格式转换

qemu-img convert -f raw -O qcow2 centos6.5-manage1.raw centos6.5-manage1.qcow2
