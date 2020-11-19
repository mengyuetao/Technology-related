



##### 常用命令


    pvs
    pvdisplay
    vgs
    vgdisplay
    lvs
    lvdisplay



##### lv 扩容步骤

1. 卸载LV				
2. 扩展LV
3. 检查文件系统
4. 重建文件系统


VBoxManage list hdds
VBoxManage modifyhd "xxx.vdi" --resize 40960
fdisk -l
fdisk /dev/sda   ; n  w 添加一个新的主分区
pvcreate /dev/sda3

partprobe  重新加载分区表

vgdisplay
vgextend centos /dev/sda3

lvdisplay
lvextend /dev/centos/root /dev/sda3
xfs_growfs  /dev/centos/root
resize2fs /dev/centos/root
